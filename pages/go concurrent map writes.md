- ![image.png](../assets/image_1700640501170_0.png){:height 525, :width 408}
- ```
  type Action struct {
  	Uuid    string
  	Version int32
  	TaskID  string
  }
  
  type ActionsCache struct {
  	sync.RWMutex
  	Actions map[string]Action
  }
  
  var ac ActionsCache
  
  func init() {
  	ac = ActionsCache{
  		Actions: make(map[string]Action), // key = actionUuid
  	}
  }
  func (p ActionsCache) Update(a Action) {
  	p.Lock()
  	defer p.Unlock()
  	p.Actions[a.Uuid] = a
  }
  ```
- 问题出在Update方法的receiver是值类型而不是指针类型，那么receiver的锁是原始锁的副本，因此导致锁失效，无法保护对map的并发写入操作。
- 解决办法是将receiver设置为指针类型或者将sync.RWMutex设置为指针。