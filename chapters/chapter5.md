# Rust 变量定义

现实生活中，我们肯定不会使用 隔壁邻居的儿子、隔壁的隔壁邻居的女儿 、隔壁的隔壁的隔壁邻居的儿子这种称呼，多难记啊。

我们都习惯于给别人取一个小名，比如 小明 = 隔壁邻居的儿子、小红 = 隔壁的隔壁邻居的女儿、小王 = 隔壁的隔壁的隔壁邻居的儿子。

在计算机中，在编程时也是一样的。

我们肯定不会使用 内存位置1、内存位置2、内存位置3 这种形式来记录内存中存储的数据。

我们喜欢对内存进行标记，比如 name = 内存位置1、age = 内存位置2、address = 内存位置3

我们把 name、age 和 address 这种标记称之为 变量

变量 是对 内存 的标记。

因为 内存中存储的数据是有数据类型的，所以 变量也是有数据类型的。

变量 的数据类型不仅用于标识内存中存储的数据类型，还用于标识内存中存储的数据的大小。同时也标识内存中存储的数据可以进行的操作。

## Rust 语言中变量的命名规则

Rust 中的变量名并不是随便什么字符都可以的，它遵循着一套规则

1. 变量名中可以包含 字母、数字 和 下划线。也就只能是 abcdefghijklmnopqrstuvwxyz013456789_ 以及大写字母。

2. 变量名必须以 字母 或 下划线 开头。

3. 也就是不能以 数字 开头。

4. 变量名是 区分大小 写的。也就是大写的 A 和小写的 a 是两个不同的字符

## Rust 中变量的声明语法

Rust 语言中声明变量时的 数据类型 是可选的，也就是可以忽略的。如果忽略了，那么 Rust 就会自动通过 值 来推断变量的类型。

就像我们平时说 1 ，大家默认都会把它当作数字看待那样。

Rust 语言中声明变量的语法格式如下
```
let variable_name = value;            // 不指定变量类型
let variable_name:dataType = value;   // 指定变量类型
```

### 范例
```
fn main() {
   let fees = 25_000;
   let salary:f64 = 35_000.00;
   println!("fees is {} and salary is {}",fees,salary);
}
```
编译运行以上 Rust 代码，输出结果如下
```
fees is 25000 and salary is 35000
```

## let 变量的不可变性

默认情况下，Rust 语言中的变量是不可变的。

Rust 语言中使用 let 声明的变量，在第一次赋值之后，是不可变更不可重新赋值的，变成了 只读 状态。

就像我们身份证上的名字，不是轻易能够改变的，不是自己涂涂写写就能改变的。

这点不同于很多语言，倒是跟 Erlang 有点像。

如果你觉得有点难以理解，那么上代码来的更直接

```
fn main() {
   let fees = 25_000;
   println!("fees is {} ",fees);
   fees = 35_000;
   println!("fees changed is {}",fees);
}
```

编译上面这段 Rust 代码，是会报错的

```
error[E0384]: re-assignment of immutable variable `fees`
 --> main.rs:6:3
   |
 3 | let fees = 25_000;
   | ---- first assignment to `fees`
...
 6 | fees=35_000;
   | ^^^^^^^^^^^ re-assignment of immutable variable

error: aborting due to previous error(s)
```

上面这段错误消息告诉我们：我们不能对变量 fees 进行第二次赋值。

这估计是 Rust 语言中最大的变更了。也是并发编程安全性的重要组成部分。

但也是 Rust 程序员最容易出错的地方。

## 可变变量

let 关键字声明的变量默认情况下是 不可变更 的，在定义了一个变量之后，默认情况下我们是不能通过赋值再改变它的值的。

```
fn main() {
   let fees:i32 = 25_000;
   println!("fees is {} ",fees);
   fees = 35_000;
   println!("fees changed is {}",fees);
}
```

上面这段代码，编译会报错的。

```
error[E0384]: re-assignment of immutable variable `fees`
 --> main.rs:6:3
   |
 3 | let fees = 25_000;
   | ---- first assignment to `fees`
...
 6 | fees=35_000;
   | ^^^^^^^^^^^ re-assignment of immutable variable

error: aborting due to previous error(s)
```

但有时候，我们又需要给一个变量重新赋值，这要怎么做呢 ？

Rust 语言提供了 mut 关键字表示 可变更。 我们可以在变量名的前面加上 mut 关键字来告诉编译器这个变量是可以重新赋值的。

## 可更变量的声明语法
```
let mut variable_name:dataType = value;
```
或者让编译器自动推断类型
```
let mut variable_name = value;
```
### 范例

我们使用 mut 关键字定义一个变量 fees 然后再重新赋值
```
fn main() {
   let mut fees:i32 = 25_000;
   println!("fees is {} ",fees);
   fees = 35_000;
   println!("fees changed is {}",fees);
}
```
编译运行以上 Rust 代码，输出结果如下
```
fees is 25000
fees changed is 35000
```