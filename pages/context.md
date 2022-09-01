- > 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/qq_24447809/article/details/123902722)
### 文章目录

*   *   [context 原理](#context_1)
  *   *   [Context 基本结构](#Context_4)
      *   [WithCancel](#WithCancel_17)
      *   [WithTimeout，WithDeadline](#WithTimeoutWithDeadline_112)
      *   [WithValue](#WithValue_151)
  *   [代码实现](#_193)

context 原理
----------

context 主要用来在 goroutine 之间传递上下文信息，包括取消信号（WithCancel）、超时时间（WithTimeout）、截止时间（WithDeadline）、键值对 key-value（WithValue）  
![](https://img-blog.csdnimg.cn/caacbfbc4371474e884f0c11ea7ea4eb.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATXIu56a-,size_20,color_FFFFFF,t_70,g_se,x_16)
### Context 基本结构

源码中 Context 接口如下：

```
type Context interface {
	Deadline() (deadline time.Time, ok bool)   //超时设置
	Done() <-chan struct{}			//接收取消信号的管道，对应cancel
	Err() error                 
	Value(key interface{}) interface{}   //上下文键值对
}

```

`WithCancel`，`WithTimeout`，`WithValue`等函数返回的都是实现了这些方法的对应结构体，如图所示  
![](https://img-blog.csdnimg.cn/80797172de184393afca012172c1c5e8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATXIu56a-,size_20,color_FFFFFF,t_70,g_se,x_16)
### WithCancel

协程 A 中初始化 ctx 代码：

```
ctx1, cancel := context.WithCancel(context.Background())

```

将 ctx 传给子协程，并监听管道信号：

```
select {
case <-ctx.Done():
	fmt.Println("goroutine recv")
	return
default:
	time.Sleep(time.Second * 1)
}

```

如图，在 A 协程中调用`cancel()`函数将会关闭所有子协程（B,C,D,E,F,G），下面详细分析这一原理。  
首先看下`WithCancel`的实现：

```
func WithCancel(parent Context) (ctx Context, cancel CancelFunc) {
	if parent == nil {
panic("cannot create context from nil parent")
	}
	c := newCancelCtx(parent)   //返回一个cancelCtx结构体
	propagateCancel(parent, &c)   //绑定父子context关系
	return &c, func() { c.cancel(true, Canceled) }
}

```

go 源码中`cancelCtx`的结构，`Context` 是自己的父 context，`done`就是接收取消信号的管道，`children` 包含了自己路径下的所有子 context 集合。

```
type cancelCtx struct {
	Context   

	mu       sync.Mutex            // protects following fields
	done     chan struct{}         // created lazily, closed by first cancel call
	children map[canceler]struct{} // set to nil by the first cancel call
	err      error                 // set to non-nil by the first cancel call
}

```

`ctx.Done()`其实就是返回一个管道，也就是上面`cancelCtx` 结构体中的`done`

```
func (c *cancelCtx) Done() <-chan struct{} {
	c.mu.Lock()
	if c.done == nil {
c.done = make(chan struct{})
	}
	d := c.done
	c.mu.Unlock()
	return d
}

```

再来看看什么时候往这个管道中发送取消信号，分析`cancelCtx`的`cancel`方法，可以发现取消信号就是关闭`cancelCtx`的`done`管道，因此只要父协程触发`cancel`，所有子协程中监听的`ctx.Done()`就会收到关闭管道的信号进入可读状态，从而执行`selct`下面的`case`语句。

```
var closedchan = make(chan struct{})
func init() {
	close(closedchan)   //预先初始化一个已经关闭的管道，下面cancel函数会用到
}
func (c *cancelCtx) cancel(removeFromParent bool, err error) {
	if err == nil {
panic("context: internal error: missing cancel error")
	}
	c.mu.Lock()
	if c.err != nil {
c.mu.Unlock()
return // already canceled
	}
	c.err = err
	//重点在这，取消信号其实就是关闭通道，结合上面的Done方法来理解
	if c.done == nil {
c.done = closedchan    //如果在子协程的Done()方法调用之前就调用了父协程的cancel函数，
					  //c.done还没赋值，将其置为一个关闭管道，子协程接收端收到关闭信号
	} else {
close(c.done)         //如果子协程调用Done()方法之后再调用父协程的cancel函数，就直接关闭管道
                     //ps：看出来两次调用cancel函数就会关闭已经关闭的管道，触发panic
	}
	for child := range c.children {
// NOTE: acquiring the child's lock while holding parent's lock.
child.cancel(false, err)   //所有子context发送取消信号
	}
	c.children = nil
	c.mu.Unlock()

	if removeFromParent {
removeChild(c.Context, c)
	}
}

```

这段代码注意体会这个预先初始化且已经关闭的管道 closedchan。
### WithTimeout，WithDeadline

`WithTimeout`底层实现是调用的`WithDeadline`，所以只需关注`WithDeadline`的代码实现

```
func WithTimeout(parent Context, timeout time.Duration) (Context, CancelFunc) {
	return WithDeadline(parent, time.Now().Add(timeout))
}

```

分析`WithDeadline`源代码，其实就是添加了`timer`计时器，`time.AfterFunc`中计时条件满足时自动触发`cancel`函数

```
func WithDeadline(parent Context, d time.Time) (Context, CancelFunc) {
	if parent == nil {
panic("cannot create context from nil parent")
	}
	if cur, ok := parent.Deadline(); ok && cur.Before(d) {
// The current deadline is already sooner than the new one.
return WithCancel(parent)
	}
	c := &timerCtx{
cancelCtx: newCancelCtx(parent),
deadline:  d,
	}
	propagateCancel(parent, c)  //绑定父子context关系
	dur := time.Until(d)
	if dur <= 0 {           //时间参数检查
c.cancel(true, DeadlineExceeded) // deadline has already passed
return c, func() { c.cancel(false, Canceled) }
	}
	c.mu.Lock()
	defer c.mu.Unlock()
	if c.err == nil {
c.timer = time.AfterFunc(dur, func() {   //重点在这，绑定dur和c.cancel函数
	c.cancel(true, DeadlineExceeded)     //时间满足后自动触发
})
	}
	return c, func() { c.cancel(true, Canceled) }
}

```
### WithValue

WithValue 的源码更为简单，返回一个带有键值对的`valueCtx`结构体

```
func WithValue(parent Context, key, val interface{}) Context {
	if parent == nil {
panic("cannot create context from nil parent")
	}
	if key == nil {
panic("nil key")
	}
	if !reflectlite.TypeOf(key).Comparable() {
panic("key is not comparable")
	}
	return &valueCtx{parent, key, val}
}

```

`valueCtx`结构体表示如下：

```
type valueCtx struct {
	Context
	key, val interface{}
}

```

可以发现每个`valueCtx`结构体只能存一对键值对，但有趣的是，在查找 value 过程中会向自己的父 contex 递归查找，`Value`方法源码如下:

```
func (c *valueCtx) Value(key interface{}) interface{} {
	if c.key == key {
return c.val    //如果找到直接返回value值
	}
	return c.Context.Value(key)   //如果没找到直接向上递归查找
}

```

递归总要有个头，当查找到最初的父 context，即初始化时由`Background()`得到的`emptyCtx`时就会结束

```
func (*emptyCtx) Value(key interface{}) interface{} {
	return nil
}

```

代码实现
----

```
package main

import (
	"context"
	"fmt"
	"sync"
	"time"
)
var wg sync.WaitGroup
type contextString string

func go_withCancel(ctx context.Context) {
	defer wg.Done()
LABEL:
	for {
select {
case <-ctx.Done():
	fmt.Println("go_withCancel recv 1s")
	break LABEL
default:
	time.Sleep(time.Second * 1)
}
	}
}

func go_withTimeOut(ctx context.Context) {
	defer wg.Done()
LABEL:
	for {
select {
case <-ctx.Done():
	fmt.Println("go_withTimeOut recv 2s")
	break LABEL
default:
	time.Sleep(time.Second * 1)
}
	}
}

func go_WithDeadline(ctx context.Context) {
	defer wg.Done()
LABEL:
	for {
select {
case <-ctx.Done():
	fmt.Println("go_WithDeadline recv 3s")
	break LABEL
default:
	time.Sleep(time.Second * 1)
}
	}
}

func go_WithValue(ctx context.Context) {
	defer wg.Done()
	fmt.Println("go_WithValue get go_value = ", ctx.Value(contextString("go_value")))
}
func main() {
	ctx1, cancel1 := context.WithCancel(context.Background())
	ctx2, _ := context.WithTimeout(context.Background(), time.Second*2)                  //2s后自动结束
	ctx3, _ := context.WithDeadline(context.Background(), time.Now().Add(time.Second*3)) //3s后自动结束
	ctx4 := context.WithValue(context.Background(), contextString("go_value"), "hello")  //设置上下文键值对
	wg.Add(4)
	go go_withCancel(ctx1)
	go go_withTimeOut(ctx2)
	go go_WithDeadline(ctx3)
	go go_WithValue(ctx4)
	time.Sleep(time.Second * 1)
	cancel1() //1s后调用cancel结束go_withCancel协程
	wg.Wait()
}


```

**测试结果如下：**  
![](https://img-blog.csdnimg.cn/2c0fcb668edf4273a866ab38ad6bebfe.png)