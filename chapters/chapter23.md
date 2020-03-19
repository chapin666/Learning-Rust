# 二十三、Rust IO

本章节我们来讲讲 Rust 中的重要内容 **IO 输入输出**。也就是 Rust 语言如何从标准输入例如键盘中读取数据并将读取的数据显示出来。

本章节我们会简单的介绍 Rust 语言 IO 输入输出的三大块内容：**从标准输入读取数据**、**把数据写入标准输出**、**命令行参数**。

## 23.1 读取 和 写入 类型

Rust 标准库通过两个特质 ( Trait ) 来组织 IO 输入输出

- Read 特质用于 读

- Write 特质用于 写

|特质|	说明|	范例|
|--|--|--|
|Read|	包含了许多方法用于从输入流读取字节数据|	Stdin,File|
|Write|	包含了许多方法用于向输出流中写入数据，包含字节数据和 UTF-8 数据两种格式|	Stdout,File|

## 23.2 Read Trait 特质 / 标准输入流

Read 特质是一个用于从输入流读取字节的组件。输入流包括 标准输入、键盘、鼠标、命令行、文件等等。

Read 特质中的 _read_line()_ 方法用于从输入流中读取一行的字符串数据。它的相关信息如下

|Trait 特质|	方法|	说明|
|--|--|--|
|Read	read_line(&mut line)->Result|	从输入流中读取一行的字符串数据并存储在 line 参数中。返回 Result 枚举，如果成功则返回读取的字节数。|

### 23.2.1 范例：从命令行/标准输入流 stdin() 中读取数据

Rust 程序可以在运行时接收用户输入的数据。

接收的方式就是从标准输入流 （一般是键盘） 中读取用户输入的内容。

下面的代码，从标准输入流/命令行 中读取用户的输入，并显示出来。

```
fn main(){
   let mut line = String::new();
   println!("请输入你的名字:");
   let b1 = std::io::stdin().read_line(&mut line).unwrap();
   println!("你好 , {}", line);
   println!("读取的字节数为：{}", b1);
}
```

编译运行以上 Rust 代码，输出结果如下

```
请输入你的名字:
从零蛋开始教程
你好 , 从零蛋开始教程

读取的字节数为：13
```

标准库提供的 std::io::stdin() 会返回返回当前进程的标准输入流 stdin 的句柄。

而 read_line() 则是标准入流 stdin 的句柄上的一个方法，用于从标准输入流读取一行的数据。

read_line() 方法的返回值值一个 Result 枚举，而 unwrap() 则是一个帮助方法，用于简化可恢复错误的处理。它会返回 Result 中存储的实际值。

>注意: read_line() 方法会自动删除行尾的换行符 \n

## 23.3 Write Trait 特质 / 标准输出流

Write 特质是一个用于向输出流写入字节的组件。输出流包括 标准输出、命令行、文件 等等。

Write 特质中的 write() 方法完成实际的输出操作。它的相关信息如下

|Trait 特质	|方法	|说明|
|Write|	write(&buf) -> Result|	把参数 buf 中的全部或部分字节写入底层的流。
返回 Result 枚举，如果成功则返回写入的字节数。|

>注意: write() 方法并不会输出结束时自动追加换行符 \n。如果需要换行符则需要手动添加

### 23.3.1 范例： 使用 stdout() 向标准输出写入内容

我们之前用过的无数次的 print!() 宏和 println!() 宏都可以向标准输出写入内容。

不过我们这次来一点不一样的，我们使用标准库 std::io 提供的 stdout().write() 方法来输出内容。

```
use std::io::Write;
fn main() {
   let b1 = std::io::stdout().write("从零蛋开始教程 ".as_bytes()).unwrap();
   let b2 = std::io::stdout().write(String::from("www.baidu.com").as_bytes()).unwrap();
   std::io::stdout().write(format!("\n写入的字节数为：{}\n",(b1+b2)).as_bytes()).unwrap();
}
```

编译运行以上 Rust 代码，输出结果如下

```
从零蛋开始教程 www.baidu.com
写入的字节数为：24
```

标准库提供的 std::io::stdout() 会返回返回当前进程的标准输出流 stdout 的句柄。

而 write() 则是标准输出流 stdout 的句柄上的一个方法，用于向标准输出流写入字节流内容。

write() 方法的返回值值一个 Result 枚举，而 unwrap() 则是一个帮助方法，用于简化可恢复错误的处理。它会返回 Result 中存储的实际值。

> 注意: 和文件相关的 IO 我们会在下一个章节介绍。

## 23.4 命令行参数

命令行参数是程序执行前就通过终端或命令行提示符或 Shell 传递给程序的参数。

命令行参数有点类似于传递给函数的实参。

比如我们之前见过的无数次的 main.exe 。之前我们运行它的时候没有传递任何参数

```
./main.exe
```

但实际上我们是可以传递一些参数的，不管程序使用与否，比如我们可以传递 2019 和 从零蛋开始教程 作为参数

./main.exe 2019 从零蛋开始教程
如果要传递多个参数，多个参数之间必须使用 空格（ ' ' ） 分隔。如果参数里有空格，则参数必须使用 双引号（"）包起来。

```
./main.exe 2019 "从零蛋开始教程 简单编程"
```

Rust 语言在标准库中内置了 std::env::args() 函数返回所有的命令行参数。

std::env::args() 返回的结果包含了程序名。例如上面的命令，std::env::args() 中存储的结果为

```
["./main.exe","2019","从零蛋开始教程 简单编程"]
```

### 23.4.1 范例：输出命令行传递的所有参数

下面的代码，我们通过迭代 std::env::args() 来输出所有传递给命令行的参数。

```
main.rs
// main.rs
fn main(){
   let cmd_line = std::env::args();
   println!("总共有 {} 个命令行参数",cmd_line.len()); // 传递的参数个数
   for arg in cmd_line {
      println!("[{}]",arg); // 迭代输出命令行传递的参数
   }
}
```
编译运行以上 Rust 代码，输出结果如下

```
$ cargo build
   Compiling guess-game-app v0.1.0 (/Users/Admin/Downloads/guess-game-app)
    Finished dev [unoptimized + debuginfo] target(s) in 0.32s
$ cargo run 1 从零蛋开始教程 3 简单编程
    Finished dev [unoptimized + debuginfo] target(s) in 0.04s
     Running `target/debug/guess-game-app 1 '从零蛋开始教程' 3 '简单编程'`
总共有 5 个命令行参数
[target/debug/guess-game-app]
[1]
[从零蛋开始教程]
[3]
[简单编程]
```

上面的程序，使用 cargo build 命令编译之后，就会生成一个可执行文件 main.exe 或 main。

Rust 语言命令行参数和其它语言的命令行参数一样，都使用 空格（ ' ' ） 来分割所有的参数。例如上面我们传递的 1 从零蛋开始教程 3 简单编程 会通过空格分割成 ["1","从零蛋开始教程","3"," "简单编程"] 4 个参数。

但实际上，我们的 std::env::args() 存储者 5 个参数，第一个参数是当前的程序名

程序名是相当于当前的执行目录的。

### 23.4.2 范例：从命令行读取多个参数

下面的代码从命令行读取多个以空格分开的参数，并统计这些参数的总和。

```
fn main(){
   let cmd_line = std::env::args();
   println!("总共有 {} 个命令行参数",cmd_line.len()); // 传递的参数个数

   let mut sum = 0;
   let mut has_read_first_arg = false;

   //迭代所有参数并计算它们的总和

   for arg in cmd_line {
      if has_read_first_arg { // 跳过第一个参数，因为它的值是程序名
         sum += arg.parse::<i32>().unwrap();
      }
      has_read_first_arg = true; // 设置跳过第一个参数，这样接下来的参数都可以用于计算
   }
   println!("和值为:{}",sum);
}
```

编译运行以上 Rust 代码，输出结果如下

````
$ cargo build
   Compiling guess-game-app v0.1.0 (/Users/Admin/Downloads/guess-game-app)
    Finished dev [unoptimized + debuginfo] target(s) in 1.59s
$ cargo run 1 2 3 4
    Finished dev [unoptimized + debuginfo] target(s) in 0.04s
     Running `target/debug/guess-game-app 1 2 3 4`
总共有 5 个命令行参数
和值为:10
```