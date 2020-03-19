# Rust 文件读写

Rust 标准库提供了大量的模块和方法用于读写文件。

Rust 语言使用结构体 **File** 来描述/展现一个文件。

结构体 **File** 有相关的成员变量或函数用于表示程序可以对文件进行的某些操作和可用的操作方法。

所有对结构体 **File** 的操作方法都会返回一个 **Result** 枚举。

下表列出了一些常用的文件读写方法。

|模块|	方法/方法签名|	说明|
|--|--|--|
|std::fs::File|	open() / pub fn open(path: P) -> Result|	静态方法，以 只读 模式打开文件|
|std::fs::File|	create() / pub fn create(path: P) -> Result	|静态方法，以 可写 模式打开文件。如果文件存在则清空旧内容。如果文件不存在则新建|
|std::fs::remove_file|	remove_file() / pub fn remove_file(path: P) -> Result<()>|从文件系统中删除某个文件|
|std::fs::OpenOptions|	append() / pub fn append(&mut self, append: bool) -> &mut OpenOptions	|设置文件模式为 追加|
|std::io::Writes|	write_all() / fn write_all(&mut self, buf: &[u8]) -> Result<()> |	将 buf 中的所有内容写入输出流 |
|std::io::Read|	read_to_string() / fn read_to_string(&mut self, buf: &mut String) -> Result |	读取所有内容转换为字符串后追加到 buf 中|

## Rust 打开文件

Rust 标准库中的 std::fs::File 模块提供了静态方法 open() 用于打开一个文件并返回文件句柄。

open() 函数的原型如下

```
pub fn open(path: P) -> Result
```

open() 函数用于以只读模式打开一个已经存在的文件，如果文件不存在，则会抛出一个错误。如果文件不可读，那么也会抛出一个错误。

### 范例

下面的范例，我们使用 open() 打开当前目录下的文件，已经文件已经存在，所以不会抛出任何错误

```
fn main() {
    let file = std::fs::File::open("data.txt").unwrap();
    println!("文件打开成功：{:?}",file);
}
```
编译运行以上 Rust 代码，输出结果如下

```
文件打开成功：File { fd: 3, path: "/Users/Admin/Downloads/guess-game-app/src/data.txt", read: true, write: false }
```

如果文件 data.txt 不存在，则会抛出以下错误

```
thread 'main' panicked at 'called `Result::unwrap()` on an `Err` value: Os { code: 2, kind: NotFound, message: "No such file or directory" }', src/libcore/result.rs:997:5
note: Run with `RUST_BACKTRACE=1` environment variable to display a backtrace.
```

## Rust 创建文件

Rust 标准库中的 std::fs::File 模块提供了静态方法 create() 用于创建一个文件并返回创建的文件句柄。

create() 函数的原型如下

```
pub fn create(path: P) -> Result
```

create() 函数用于创建一个文件并返回创建的文件句柄。如果文件已经存在，则会内部调用 open() 打开文件。如果创建失败，比如目录不可写，则会抛出错误

### 范例

下面的代码，使用 create() 函数创建文件 data.txt。

```
fn main() {
   let file = std::fs::File::create("data.txt").expect("create failed");
   println!("文件创建成功:{:?}",file);
}
```

编译运行以上 Rust 代码，输出结果如下

```
文件创建成功:File { fd: 3, path: "/Users/Admin/Downloads/guess-game-app/src/data.txt", read: false, write: true }
```

## Rust 写入文件

Rust 语言标准库 std::io::Writes 提供了函数 write_all() 用于向输出流写入内容。

因为文件流也是输出流的一种，所以该函数也可以用于向文件写入内容。

write_all() 函数在模块 std::io::Writes 中定义，它的函数原型如下

```
fn write_all(&mut self, buf: &[u8]) -> Result<()>
```

write_all() 用于向当前流写入 buf 中的内容。如果写入成功则返回写入的字节数，如果写入失败则抛出错误

### 范例

下面的代码，我们使用 write_all() 方法向文件 data.txt 写入一些内容

```
use std::io::Write;
fn main() {
   let mut file = std::fs::File::create("data.txt").expect("create failed");
   file.write_all("从零蛋开始教程".as_bytes()).expect("write failed");
   file.write_all("\n简单编程".as_bytes()).expect("write failed");
   println!("data written to file" );
}
```

编译运行以上 Rust 范例，输出结果如下

```
从零蛋开始教程
简单编程
```

> 注意： write_all() 方法并不会在写入结束后自动写入换行符 \n。

## Rust 读取文件

Rust 读取内容的一般步骤为:

1. 使用 open() 函数打开一个文件
2. 然后使用 read_to_string() 函数从文件中读取所有内容并转换为字符串。

open() 函数我们前面已经介绍过了，这次我们主要来讲讲 read_to_string() 函数。

read_to_string() 函数用于从一个文件中读取所有剩余的内容并转换为字符串。

read_to_string() 函数的原型如下

```
fn read_to_string(&mut self, buf: &mut String) -> Result
```

read_to_string() 函数用于读取文件中的所有内容并追加到 buf 中，如果读取成功则返回读取的字节数，如果读取失败则抛出错误。

### 范例

我们首先在当前目录下新建一个文件 data.txt 并写入以下内容

```
从零蛋开始教程
简单编程
```

然后我们写一个小范例，使用 read_to_string 函数从文件中读取内容。

```
use std::io::Read;

fn main(){
   let mut file = std::fs::File::open("data.txt").unwrap();
   let mut contents = String::new();
   file.read_to_string(&mut contents).unwrap();
   print!("{}", contents);
}
```

编译运行以上 Rust 代码，输出结果如下

```
从零蛋开始教程
简单编程
```

## 追加内容到文件末尾

Rust 核心和标准库并没有提供直接的函数用于追加内容到文件的末尾。

但提供了函数 append() 用于将文件的打开模式设置为 追加。

当文件的模式设置为 追加 之后，写入文件的内容就不会代替原先的旧内容而是放在旧内容的后面。

函数 append() 在模块 std::fs::OpenOptions 中定义，它的函数原型为

```
pub fn append(&mut self, append: bool) -> &mut OpenOptions
```

### 范例

下面的范例，我们使用 append() 方法修改文件的打开模式为 追加。

```
use std::fs::OpenOptions;
use std::io::Write;

fn main() {
   let mut file = OpenOptions::new().append(true).open("data.txt").expect(
      "cannot open file");
   file.write_all("www.baidu.com".as_bytes()).expect("write failed");
   file.write_all("\n从零蛋开始教程".as_bytes()).expect("write failed");
   file.write_all("\n简单编程".as_bytes()).expect("write failed");
   println!("数据追加成功");
}
```

编译运行以上 Rust 代码，输出结果如下

```
数据追加成功
```

打开 data.txt 文件，可以看到内容如下

```
从零蛋开始教程
简单编程www.baidu.com
从零蛋开始教程
简单编程
```

## Rust 删除文件

Rust 标准库 std::fs 提供了函数 remove_file() 用于删除文件。

remove_file() 函数的原型如下

```
pub fn remove_file<P: AsRef>(path: P) -> Result<()>
```
> 注意，删除可能会失败，即使返回结果为 OK，也有可能不会立即就删除。

### 范例

下面的代码，我们使用 remove_file() 方法删除 data.txt。

expect() 方法当删除出错时用于输出自定义的错误消息。

```
use std::fs;
fn main() {
   fs::remove_file("data.txt").expect("could not remove file");
   println!("file is removed");
}
```

编译运行以上范例，输出结果如下

```
file is removed
```
打开当前目录，我们可以发现文件已经被删除了。

## Rust 复制文件

Rust 标准库没有提供任何函数用于复制一个文件为另一个新文件。

但我们可以使用上面提到的函数和方法来实现文件的复制功能。

下面的代码，我们模仿简单版本的 copy 命令

```
copy old_file_name  new_file_name
```

具体的 Rust 代码如下

```
use std::io::Read;
use std::io::Write;

fn main() {
   let mut command_line: std::env::Args = std::env::args();
   command_line.next().unwrap();

   // 跳过程序名
   // 原文件
   let source = command_line.next().unwrap();

   // 新文件
   let destination = command_line.next().unwrap();
   let mut file_in = std::fs::File::open(source).unwrap();
   let mut file_out = std::fs::File::create(destination).unwrap();
   let mut buffer = [0u8; 4096];
   loop {
      let nbytes = file_in.read(&mut buffer).unwrap();
      file_out.write(&buffer[..nbytes]).unwrap();
      if nbytes < buffer.len() { break; }
   }
}
```

首先编译上面的代码

```
$ rustc main.rs 
```

然后使用下面的命令来运行

windows 电脑
```
$ ./main.exe data.txt data_new.txt
```

linux / mac 电脑
```
$ ./main data.txt data_new.txt
```

其中

data.txt 为我们想要复制的原文件路径
data_new.txt 为我们想要的新文件路径