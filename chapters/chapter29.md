# 二十九、Rust 多线程并发编程

随着电脑等电子产品全面进入多核时代，并发编程已经是程序不可或缺的功能之一。

并发编程就是同时运行两个或多个任务，就像宅男宅女的我们，一边吃零食还能一边吃饭，顺带还能调侃下男女朋友。

并发编程的一个重要思想就是 **程序不同的部分可以同时独立运行互不干扰**。

## 29.1 多线程

现代的操作系统，是一个多任务操作系统，系统可以管理多个程序的运行，一个程序往往有一个或多个进程，而一个进程则有一个或多个线程。

让一个进程可以运行多个线程的机制叫做多线程编程。

一个进程一定有一个主线程，主线程之外创建出来的线程称之为 **子线程**

多线程编程，其实就是在主线程之外创建子线程，让子线程和主线程同时运行，完成各自的任务。

Rust 语言支持多线程并发编程。

## 29.2 创建线程

Rust 语言标准库中的 std::thread 模块用于支持多线程编程。

std::thread 提供很很多方法用于创建线程、管理线程和结束线程。

创建一个新线程，可以使用 std::thread::spawn() 方法。

spawn() 函数的原型如下

```
pub fn spawn<F, T>(f: F) -> JoinHandle<T> 
```

参数 f 是一个闭包（closure ） 是线程要执行的代码。

### 29.2.1 范例

下面的范例，我们使用 spawn() 函数创建了一个线程，用于输出数字 1 到 10

```
use std::thread;           // 导入线程模块
use std::time::Duration;   // 导入时间模块

fn main() {
   //创建一个新线程
   thread::spawn(|| {
      for i in 1..10 {
         println!("hi number {} from the spawned thread!", i);
         thread::sleep(Duration::from_millis(1));
      }
   });
   // 主线程要执行的代码
   for i in 1..5 {
      println!("hi number {} from the main thread!", i);
      thread::sleep(Duration::from_millis(1));
   }
}
```

编译运行以上 Rust 范例，输出结果如下

```
hi number 1 from the main thread!
hi number 1 from the spawned thread!
hi number 2 from the main thread!
hi number 2 from the spawned thread!
hi number 3 from the main thread!
hi number 3 from the spawned thread!
hi number 4 from the spawned thread!
hi number 4 from the main thread!
```

咦，执行结果好像出错了？ 是吗？

嗯，好像是错了，但好像又不是。

> 注意: 当主线程执行结束，那么就会自动关闭创建出来的衍生子线程。

上面的代码，我们调用 **thread::sleep()** 函数强制线程休眠一段时间，这就允许不同的线程交替执行。

虽然某个线程休眠时会自动让出 cpu，但并不保证其它线程会执行。这取决于操作系统如何调度线程。

这个范例的输出结果是随机的，主线程一旦执行完成程序就会自动退出，不会继续等待子线程。这就是子线程的输出结果为什么不全的原因。

## 29.3 加入线程句柄 join()

默认情况下，主线程并不会等待子线程执行完毕。如果主线程创建完衍生线程就立即退出的话，那么子线程可能根本没机会开始运行或者执行完毕，

为了避免这种情况，我们可以让主线程等待子线程执行完毕然后再继续执行。

Rust 标准库提供了 **join()** 方法用于把子线程加入主线程等待队列。

**join()** 方法的原型如下

```
spawn<F, T>(f: F) -> JoinHandle<T>
```

### 29.3.1 范例

下面的范例，我们使用 **join()** 方法把衍生线程加入主线程等待队列，这样主线程会等待子线程执行完毕才能退出。

```
use std::thread;
use std::time::Duration;

fn main() {
   let handle = thread::spawn(|| {
      for i in 1..10 {
         println!("hi number {} from the spawned thread!", i);
         thread::sleep(Duration::from_millis(1));
      }
   });
   for i in 1..5 {
      println!("hi number {} from the main thread!", i);
      thread::sleep(Duration::from_millis(1));
   }
   handle.join().unwrap();
}
```

编译运行以上 Rust 范例，输出结果如下

```
hi number 1 from the main thread!
hi number 1 from the spawned thread!
hi number 2 from the spawned thread!
hi number 2 from the main thread!
hi number 3 from the spawned thread!
hi number 3 from the main thread!
hi number 4 from the main thread!
hi number 4 from the spawned thread!
hi number 5 from the spawned thread!
hi number 6 from the spawned thread!
hi number 7 from the spawned thread!
hi number 8 from the spawned thread!
hi number 9 from the spawned thread!
```

从输出结果来看，主线程和子线程交替执行。

> 注意: 主线程会等待子线程执行完毕是因为调用了 join() 方法。