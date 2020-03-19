# 二十一、Rust 错误处理

人人都会犯错，编程业务不例外。

而犯的每个错误，有的可以挽回，有的不可挽回，可挽回的都能轻松处理，不可挽回的就是永久创伤。

编程里也有两种错误，一种错误可以被捕捉到，然后轻松处理，另一种错误，就没办法了，只能导致程序崩溃退出。

Rust 语言也有错误这个概念，而且把错误分为两大类：**可恢复** 和 **不可恢复**，相当于其它语言的 **异常** 和 **错误**。

|Name|	描述|	范例|
|--|--|--|
|Recoverable|	可以被捕捉，相当于其它语言的异常 Exception|	Result 枚举|
|UnRecoverable|	不可捕捉，会导致程序崩溃退出|	panic 宏|

> 可恢复 （ Recoverable ）
- 可恢复 （ Recoverable ）错误就是那些可被捕捉的错误，因为可以被捕捉，所以可以被矫正，让程序继续运行。
一旦捕捉到了可恢复的错误，程序就可以通过不断尝试之前那个失败的操作或者选择一个备用的操作。

- 可恢复 （ Recoverable ） 错误的一个典型范例就是：读取不存在文件时的 File Not Found 错误。

> 不可恢复 （ UnRecoverable ）
- 不可恢复 （ UnRecoverable ） 错误就是那些致命的会导致程序崩溃的错误。这些错误一旦发生，就会让程序立即停止。

- 不可恢复 （ UnRecoverable ） 错误的一个典型范例是 数组越界。

与其它语言不同，Rust 语言没有 **异常（Exception）** 这个概念，而是 **可恢复 （ Recoverable ）** 错误。

Rust 语言遇到可恢复错误时会返回一个 **Result<T, E>** 的枚举。如果遇到不可恢复的错误，则会自动调用 panic() 宏。

> panic!() 宏或导致程序立即退出。

## 21.1 panic!() 宏和不可恢复错误

panic!() 会导致程序立即退出，并在退出时向它的调用者反馈退出原因。

panic!() 宏的语法格式如下

```
panic!( string_error_msg )
```

字符串类型的 string_error_msg 用于向调用者传递程序退出的原因。

一般情况下，当遇到不可恢复错误时，程序会自动调用 panic!()。

但我们也可以通过手动调用 panic!() 以达到让程序退出的目的。

> panic!() 不要乱用，除非遇到不可挽救的错误。

### 21.1.1 范例1

下面的范例，因为 panic!() 会导致程序立即退出，所以后面的 println!() 宏就不会运行。

```
fn main() {
   panic!("Hello");
   println!("End of main"); // 不可能执行的语句
}
```

编译运行以上 Rust 代码，输出结果如下

```
thread 'main' panicked at 'Hello', main.rs:3
```

### 21.1.2 范例2: 数组越界错误

下面的代码，因为数组的最大下标是 2 ，远远小于 10，因此会触发数组越界错误。

```
fn main() {
   let a = [10,20,30];
   a[10]; // 因为 10 超出了数组的长度，所以会触发不可恢复错误
}
```

编译运行以上 Rust 代码，输出结果如下

```
warning: this expression will panic at run-time
--> main.rs:4:4
  |
4 | a[10];
  | ^^^^^ index out of bounds: the len is 3 but the index is 10

$main
thread 'main' panicked at 'index out of bounds: the len 
is 3 but the index is 10', main.rs:4
note: Run with `RUST_BACKTRACE=1` for a backtrace.
```

### 21.1.3 范例 3 ：手动出发 panic!() 让程序退出

如果程序执行过程中违反了既定的业务规则，可以手动调用 panic!() 宏让程序退出。

例如下面的代码，因为 13 是奇数，违反了要求偶数的规则。

```
fn main() {
   let no = 13; 
   // 测试奇偶
   if no % 2 == 0 {
      println!("Thank you , number is even");
   } else {
      panic!("NOT_AN_EVEN"); 
   }
   println!("End of main");
}
```

编译运行以上 Rust 代码，输出结果如下

```
thread 'main' panicked at 'NOT_AN_EVEN', main.rs:9
note: Run with `RUST_BACKTRACE=1` for a backtrace.
```

## 21.2 Result 枚举和可恢复错误

一些比较古老的语言，比如 C 通过设置全局变量 errno 来告诉程序发生了什么错误，而其它的语言，比如 Java 在返回类型的基础上还要通过指定可捕捉的异常来达到程序可恢复的目的，而比较现代的语言，比如 Go 则是通过将错误和正常值一起返回来达到可恢复的目的。

Rust 在可恢复错误（ Recoverable ）上更大胆。它使用一个 Result 枚举来封装正常返回的值和错误信息。 带来的好处就是只要一个变量就能接收正常值和错误信息，又不会污染全局空间。

Result<T,E> 枚举被设计用于处理可恢复错误。

Result 枚举的定义如下

```
enum Result<T,E> {
   OK(T),
   Err(E)
}
```

Result<T,E> 枚举包含了两个值：OK 和 Err。

T 和 E 则是两个范型参数：

- T 用于当 Result 的值为 OK 时作为正常返回的值的数据类型。
- E 用于当 Result 的值为 Err 时作为错误返回的错误的类型。

### 21.2.1 范例1：Result 枚举的简单使用

下面的范例，我们通过打开一个不存在的文件来演示下 Result 枚举的使用

```
use std::fs::File;
fn main() {
   let f = File::open("main.jpg"); //文件不存在，因此值为 Result.Err
   println!("{:?}",f);
}
```

编译运行以上 Rust 代码，输出结果如下

```
Err(Error { repr: Os { code: 2, message: "No such file or directory" } })
```

上面的代码，如果 main.jpg 存在则结果为 OK(File)。 如果文件不存在则结果为 Err(Error)。

上面的代码仅仅是输出错误信息，这只能是演示目的，正常情况下我们要根据结果类型作出不同的选择。

### 21.2.2 范例 2: 捕捉错误信息并恢复程序运行

下面的代码，我们使用 match 对 Result 的不同值作出不同的处理。

```
use std::fs::File;
fn main() {
   let f = File::open("main.jpg");   // main.jpg 文件不存在
   match f {
      Ok(f)=> {
         println!("file found {:?}",f);
      },
      Err(e)=> {
         println!("file not found \n{:?}",e);   // 处理错误
      }
   }
   println!("end of main");
}
```

编译运行以上 Rust 代码，输出结果如下

```
file not found
Os { code: 2, kind: NotFound, message: "The system cannot find the file specified." }
end of main
```

> 注意: 上面的代码，不管 f 的值是什么，最后的 println!("end of main"); 都会运行。

### 21.2.3 范例 3 ： 实战演练函数返回错误
下面的代码，我们定义了一个函数 is_even()。如果传递的参数不是偶数则会抛出一个可恢复错误。

```
fn main(){
   let result = is_even(13);
   match result {
      Ok(d)=>{
         println!("no is even {}",d);
      },
      Err(msg)=>{
         println!("Error msg is {}",msg);
      }
   }
   println!("end of main");
}
fn is_even(no:i32)->Result<bool,String> {
   if no%2==0 {
      return Ok(true);
   } else {
      return Err("NOT_AN_EVEN".to_string());
   }
}
```

编译运行以上 Rust 代码，输出结果如下

```
Error msg is NOT_AN_EVEN
end of main
```

## 21.3 unwrap() 函数和 expect() 函数

上面的 Result<T,E> ，用 match 语句处理起来蛮不错的样子，但写多了就会有 Go 语言那种漫天飞舞 if err != nil 的赶脚。

有的时候我们不想处理或者让程序自己处理 Err， 有时候我们只要 OK 的具体值就可以了。

针对这两种处女座诉求， Rust 语言的开发者们在标准库中定义了两个帮助函数 _unwrap()_ 和 _expect()_。

|方法|	原型|	说明|
|--|--|--|
|unwrap|	unwrap(self):T|	如果 self 是 Ok 或 Some 则返回包含的值。否则会调用宏 panic!() 并立即退出程序|
|expect|	expect(self,msg:&str):T|	如果 self 是 Ok 或 Some 则返回包含的值。否则调用panic!() 输出自定义的错误并退出|

expect() 函数用于简化不希望事情失败的错误情况。而 unwrap() 函数则在返回 OK 成功的情况下，提取返回的实际结果。

> unwrap() 和 expect() 不仅能够处理 Result <T,E> 枚举，还可以用于处理 Option <T> 枚举。

## 21.4 unwrap() 函数

unwrap() 函数返回操作成功的实际结果。如果操作失败，它会调用 panic!() 并输出默认的错误消息。

unwrap() 函数的原型如下

```
unwrap(self):T
```

实际上，unwrap() 函数内部的实现就是我们上面见过的 match 语句。

### 21.4.1 范例1

下面代码，我们使用 unwrap() 函数对判断偶数的那个范例进行改造，看起来是不是舒服多了

```
fn main(){
   let result = is_even(10).unwrap();
   println!("result is {}",result);
   println!("end of main");
}
fn is_even(no:i32)->Result<bool,String> {
   if no%2==0 {
      return Ok(true);
   } else {
      return Err("NOT_AN_EVEN".to_string());
   }
}
```

编译运行以上 Rust 代码，输出结果如下

```
result is true
end of main
```

### 21.4.2 范例 2

如果我们将 is_even(10) 传递的 10 改成 13 则会退出程序并输出错误消息

```
fn main(){
   let result = is_even(10).unwrap();
   println!("result is {}",result);
   println!("end of main");
}
fn is_even(no:i32)->Result<bool,String> {
   if no%2==0 {
      return Ok(true);
   } else {
      return Err("NOT_AN_EVEN".to_string());
   }
}
```

编译运行以上 Rust 代码，输出结果如下

```
thread 'main' panicked at 'called `Result::unwrap()` on 
an `Err` value: "NOT_AN_EVEN"', libcore\result.rs:945:5
note: Run with `RUST_BACKTRACE=1` for a backtrace

```

## 21.5 函数 expect()

函数 expect() 当 self 是 Ok 或 Some 则返回包含的值。否则调用panic!() 输出自定义的错误并退出程序。

函数 expect() 的原型如下

```
expect(self,msg:&str):T
```

函数 expect() 和 unwrap() 一样，唯一的不同点是 当错误发生时，expect() 会输出自定义的错误消息而不是默认的错误消息。

### 21.5.1 范例

下面的代码，我们使用 expect() 函数改造下上面那个文件不存在的 match 范例

```
use std::fs::File;
fn main(){
   let f = File::open("pqr.txt").expect("File not able to open"); // 文件不存在
   println!("end of main");
}
```
编译运行以上 Rust 代码，输出结果如下

```
thread 'main' panicked at 'File not able to open: Error { repr: Os 
{ code: 2, message: "No such file or directory" } }', src/libcore/result.rs:860
note: Run with `RUST_BACKTRACE=1` for a backtrace.
```