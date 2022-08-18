# mutex

C++11中新增了<mutex>，它是C++标准程序库中的一个头文件，定义了C++11标准中的一些互斥访问的类与方法等。其中std::mutex就是lock、unlock。std::lock_guard与std::mutex配合使用，把锁放到lock_guard中时，mutex自动上锁，lock_guard析构时，同时把mutex解锁。mutex又称互斥量。

 C++11标准库定义了4个互斥类：

 (1)、std::mutex：该类表示普通的互斥锁, 不能递归使用。

 (2)、std::timed_mutex：该类表示定时互斥锁，不能递归使用。std::time_mutex比std::mutex多了两个成员函数：

 A、try_lock_for()：函数参数表示一个时间范围，在这一段时间范围之内线程如果没有获得锁则保持阻塞；如果在此期间其他线程释放了锁，则该线程可获得该互斥锁；如果超时(指定时间范围内没有获得锁)，则函数调用返回false。

 B、try_lock_until()：函数参数表示一个时刻，在这一时刻之前线程如果没有获得锁则保持阻塞；如果在此时刻前其他线程释放了锁，则该线程可获得该互斥锁；如果超过指定时刻没有获得锁，则函数调用返回false。

 (3)、std::recursive_mutex：该类表示递归互斥锁。递归互斥锁可以被同一个线程多次加锁，以获得对互斥锁对象的多层所有权。例如，同一个线程多个函数访问临界区时都可以各自加锁，执行后各自解锁。std::recursive_mutex释放互斥量时需要调用与该锁层次深度相同次数的unlock()，即lock()次数和unlock()次数相同。可见，线程申请递归互斥锁时，如果该递归互斥锁已经被当前调用线程锁住，则不会产生死锁。此外，std::recursive_mutex的功能与 std::mutex大致相同。

 (4)、std::recursive_timed_mutex：带定时的递归互斥锁。

 互斥类的最重要成员函数是lock()和unlock()。在进入临界区时，执行lock()加锁操作，如果这时已经被其它线程锁住，则当前线程在此排队等待。退出临界区时，执行unlock()解锁操作。

 用于互斥锁的RAII的类模板：更好的办法是采用”资源分配时初始化”(RAII)方法来加锁、解锁，这避免了在临界区中因为抛出异常或return等操作导致没有解锁就退出的问题。极大地简化了程序员编写Mutex相关的异常处理代码。C++11的标准库中提供了std::lock_guard类模板做mutex的RAII。

 std::mutex类是一个同步原语，可用于保护共享数据被同时由多个线程访问。std::mutex提供独特的，非递归的所有权语义。

        std::mutex是C++11中最基本的互斥量，std::mutex对象提供了独占所有权的特性，不支持递归地对std::mutex对象上锁。

        std::mutex成员函数：

        (1)、构造函数：std::mutex不支持copy和move操作，最初的std::mutex对象是处于unlocked状态。

        (2)、lock函数：互斥锁被锁定。线程申请该互斥锁，如果未能获得该互斥锁，则调用线程将阻塞(block)在该互斥锁上；如果成功获得该互诉锁，该线程一直拥有该互斥锁直到调用unlock解锁；如果该互斥锁已经被当前调用线程锁住，则产生死锁(deadlock)。

        (3)、unlock函数：解锁，释放调用线程对该互斥锁的所有权。

        (4)、try_lock：尝试锁定互斥锁。如果互斥锁被其他线程占有，则当前调用线程也不会被阻塞，而是由该函数调用返回false；如果该互斥锁已经被当前调用线程锁住，则会产生死锁。其中std::mutex就是lock、unlock。std::lock_guard与std::mutex配合使用，把锁放到lock_guard中时，mutex自动上锁，lock_guard析构时，同时把mutex解锁。
         (5)、native_handle：返回当前句柄。

         std::mutex: A mutex is a lockable object that is designed to signal when critical sections of code need exclusive access, preventing other threads with the same protection from executing concurrently and access the same memory locations. std::mutex objects provide exclusive ownership and do not support recursivity (i.e., a thread shall not lock a mutex it already owns).

 下面是从其它文章中copy的std::mutex测试代码，详细内容介绍可以参考对应的reference：

```C++
#include "mutex.hpp"
#include <iostream>
#include <mutex>
#include <thread>
 
namespace mutex_ {
 
///
// refrence: http://www.cplusplus.com/reference/mutex/mutex/
std::mutex mtx;           // mutex for critical section
 
static void print_block(int n, char c)
{
	// critical section (exclusive access to std::cout signaled by locking mtx):
	mtx.lock();
	for (int i = 0; i<n; ++i) { std::cout << c; }
	std::cout << '\n';
	mtx.unlock();
}
 
int test_mutex_1()
{
	std::thread th1(print_block, 50, '*');
	std::thread th2(print_block, 50, '$');
 
	th1.join();
	th2.join();
 
	return 0;
}
 
//
// reference: http://www.cplusplus.com/reference/mutex/mutex/lock/
static void print_thread_id(int id)
{
	// std::mutex::lock: The calling thread locks the mutex
	// std::mutex::unlock: Unlocks the mutex, releasing ownership over it.
	// critical section (exclusive access to std::cout signaled by locking mtx):
	mtx.lock();
	std::cout << "thread #" << id << '\n';
	mtx.unlock();
}
 
int test_mutex_2()
{
	std::thread threads[10];
	// spawn 10 threads:
	for (int i = 0; i<10; ++i)
		threads[i] = std::thread(print_thread_id, i + 1);
 
	for (auto& th : threads) th.join();
 
	return 0;
}
 
/
// reference: http://www.cplusplus.com/reference/mutex/mutex/try_lock/
volatile int counter(0); // non-atomic counter
 
static void attempt_10k_increases()
{
	// std::mutex::try_lock: Lock mutex if not locked,
	// true if the function succeeds in locking the mutex for the thread, false otherwise.
	for (int i = 0; i<10000; ++i) {
		if (mtx.try_lock()) {   // only increase if currently not locked:
			++counter;
			mtx.unlock();
		}
	}
}
 
int test_mutex_3()
{
	std::thread threads[10];
	// spawn 10 threads:
	for (int i = 0; i<10; ++i)
		threads[i] = std::thread(attempt_10k_increases);
 
	for (auto& th : threads) th.join();
	std::cout << counter << " successful increases of the counter.\n";
 
	return 0;
}
 
} // namespace mutex_
```

[原文链接：](https://blog.csdn.net/fengbingchun/article/details/73521630)
