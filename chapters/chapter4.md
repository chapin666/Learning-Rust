# Rust 数据类型

类型系统 对于任何一门语言都是重中之重，因为它体现了语言所支持的不同类型的值。

类型系统 也是 IT 初学者最难啃的三座大山之一，而类型系统之所以难以理解，主要是没有合适的现成的参考体系。

举个例子，比如我们说 **类型系统** 存在的目的，就是 程序在存储或操作某个数之前检查这个数的有效性。

就这一句话，简简单单，看起来每个文字都懂，如果把它们放在一起，就有那么一点天书的味道了。

类型系统在程序存储或操作之前检查所提供值的有效性。这可以保证程序运行时给变量提供的数据不会发生类型错误。

更近一步说，类型系统可以允许编辑器时时报告错误或者自动提示。

Rust 是一个静态的严格数据类型的语言。每个值都有唯一的数据类型，要么是整型，要么是浮点型等等。

Rust 语言在赋值时并不强制要求指定变量的数据类型，Rust 编译器可以根据分配给它的值自动推断变量的数据类型。

## 声明/定义一个变量

Rust 语言使用 **let** 关键字来声明和定义一个变量。

Rust 语言中声明变量的语法格式如下

```
let variable_name = value;
```

这里我们只会粗略带过变量的定义和声明，后面的章节我们会详细介绍。

下面的代码演示了如何定义变量

```
fn main() {
   let company_string = "TutorialsPoint";  // string 字符串类型
   let rating_float = 4.5;                 // float 类型
   let is_growing_boolean = true;          // boolean 类型
   let icon_char = '♥';                    //unicode character 类型

   println!("company name is:{}",company_string);
   println!("company rating on 5 is:{}",rating_float);
   println!("company is growing :{}",is_growing_boolean);
   println!("company icon is:{}",icon_char);
}
```

上面的代码中，我们并没有为每一个变量指定它们的数据类型。Rust 编译器会自动从 **等号=** 右边的值中推断出该变量的类型。例如 Rust 会自动将 _双引号_ 阔起来的数据推断为 **字符串**，把 _没有小数点的数字_ 自动推断为 **整型**。把 _true_ 或 _false_ 值推断为 **布尔类型**。

println!() 是一个宏，而不是一个函数，区分函数和宏的唯一办法，就是看函数名/宏名最后有没有感叹号 !. 如果有感叹号则是宏，没有则是函数。

println!() 宏接受两个参数：

* 第一个参数是格式化符，一般是 {}，如果是复杂类型，则是 {:?}。
* 第二个参数是变量名或者常量名。

编译运行以上 Rust 代码，输出结果如下

```
company name is: TutorialsPoint
company rating on 5 is:4.5
company is growing: true
company icon is: ♥
```

## 标量数据类型

标量数据类型 又称为 基本数据类型。标量数据类型只能存储单个值，例如 10 或 3.14 或 c。

Rust 语言中有四种标量数据类型：
* 整型
* 浮点型
* 布尔类型
* 字符类型

接下来我们会对每种标量数据类型做一个简单的介绍。

## 整型

整数就是没有小数点的数字，比如说 0，1，-1，-2，9999999 等，但是 0.0 、1.0 和 -1.11 等都不是整数。

**整型** 能够囊括所有的数字，虽然不可能无穷大，但已经大到足够使用了。

最大的整型为 340282366920938463463374607431768211455，由 std::u128:MAX 定义。

最小的整型为 -170141183460469231731687303715884105728，由 std::i128:MIN 定义。

它们看起来是不是一个天文数字？

整型可以进一步分为 **有符号整型** 和 **无符号整型** 两种:
- 有符号整型，英文 signed，既可以存储正数，也可以存储负数。
- 无符号整型，因为 unsigned，只能存储正数。

按照存储空间来说，整型可以进一步划分为 1字节、2字节、4字节、8字节、16字节。

1 字节 = 8 位，每一位能只能存储二进制 0 或 1，因此每一个字节能够存储的最大数字是 256，而最小数字则是 -127。

有点复杂了，详细的看 C 语言的数据可能会更简单。

下表列出了整型所有的细分类型：

| 大小 | 有符号 |	无符号 |
| -- | -- | -- |
| 8bit | i8 |	u8 |
| 16bit |	i16 |	u16 |
| 32bit |	i32 |	u32 |
| 64bit |	i64 |	u64 |
| 128bit | i128 |	u128 |
| Arch | isize |	usize |

i32 是默认的整型，如果我们直接说出一个数字而不说它的数据类型，那么它默认就是 i32 。

整型的长度还可以是 arch。arch 是由 CPU 构架决定的大小的整型类型。大小为 arch 的整数在 x86 机器上为 32 位，在 x64 机器上为 64 位。

Arch 整型通常用于表示容器的大小或者数组的大小，或者数据在内存上存储的位置。

### 范例：如何定义各种整型的变量

定义整型变量的时候要注意每种整型的最大值和最小值，如果超出可能会赋值失败，也有可能结果不是我们想要的。

```
fn main() {
   let result = 10;    // i32 默认
   let age:u32 = 20;
   let sum:i32 = 5-15;
   let mark:isize = 10;
   let count:usize = 30;
   println!("result value is {}",result);
   println!("sum is {} and age is {}",sum,age);
   println!("mark is {} and count is {}",mark,count);
}
```

编译运行以上 Rust 代码，输出结果如下
```
result value is 10
sum is -10 and age is 20
mark is 10 and count is 30
```

整型只能保存整数，不能保存有小数点的数字，哪怕小数点后面全是 0。如果把一个有小数的数字赋值给整型，会报编译错误

```
fn main() {
   let age:u32 = 20.0;
}
```

编译运行以上 Rust 代码，报错信息如下

```
error[E0308]: mismatched types
 --> src/main.rs:2:18
  |
2 |    let age:u32 = 20.0;
  |                  ^^^^ expected u32, found floating-point number
  |
  = note: expected type `u32`
             found type `{float}`
```

从报错信息中可以看出，Rust 语言将 20.0 这种有小数点的数字称为 浮点数。使用 float 来表示。

我们不能将一个 float 类型的数字赋值给 u32 类型。

### 整型范围

每种整型并不都是能存储任意数字的，每种整型只能装下固定大小的数字，但总体上，大的整型能装下小的整型。

每种 **有符号整型** 能够存储的最小值为 **-(2^(n-1)**，能够存储的最大值为 **2^(n-1) -1**。

每种 **无符号整型** 能够存储的最小值为 **0**，能够存储的最大值为 **2^n - 1**。

其中 n 是指数据类型的大小。

例如一个 8 位有符号整型 i8，它能够存储的最小值为 -(2^(8-1)) = -128。最大值为 (2^(8-1)-1) = 127。

例如一个 8 位无符号整型 u8，它能够存储的最小值为 0，能够存储的最大值为 2^8-1 = 255。

### 整型溢出

每种整型并不都是能存储任意数字的，每种整型只能装下固定大小的数字。如果给予的数字超出了整型的范围则会发生溢出。

比如一个 i8 能够存储的最小值是 0，如果我们让它来存储 -1 则会发生溢出。

当发生数据溢出时，Rust 抛出一个错误指示数据溢出。

例如下面的代码，编译会报错。

```
fn main() {
   let age:u8 = 255;

   // u8 只能存储 0 to 255 
   let weight:u8 = 256;   // 溢出值为 0
   let height:u8 = 257;   // 溢出值为 1
   let score:u8 = 258;    // 溢出值为 2

   println!("age is {} ",age);
   println!("weight is {}",weight);
   println!("height is {}",height);
   println!("score is {}",score);
}
```

编译运行以上代码，报错信息如下

```
error: literal out of range for u8
 --> src/main.rs:5:20
  |
5 |    let weight:u8 = 256;   // 溢出值为 0
  |                    ^^^
  |
  = note: #[deny(overflowing_literals)] on by default

error: literal out of range for u8
 --> src/main.rs:6:20
  |
6 |    let height:u8 = 257;   // 溢出值为 1
  |                    ^^^

error: literal out of range for u8
 --> src/main.rs:7:19
  |
7 |    let score:u8 = 258;    // 溢出值为 2
  |                   ^^^
```

从错误信息来看，三个溢出的地方都报错了。提示赋值的数字超出了 u8 的范围。

## 浮点型：f32 和 f64

前面我们提到过，整型只能保存没有小数点的数字。而对于有小数点的数字，Rust 提供了浮点型。

Rust 区分整型和浮点型的唯一指标就是 **有没有小数点**。

Rust 中的整型和浮点型是严格区分的，不能相互转换。

也就是说，我们不能将 0.0 赋值给任意一个整型，也不能将 0 赋值给任意一个浮点型。

按照存储大小，我们把浮点型划分为 **f32** 和 **f64**。其中 f64 是默认的浮点类型。
- f32 又称为 单精度浮点型。
- f64 又称为 双精度浮点型，它是 Rust 默认的浮点类型.

### 范例：如何定义各种浮点型的变量

定义浮点型变量的时候要注意每种浮点型的最大值和最小值，如果超出可能会赋值失败，也有可能结果不是我们想要的。

```
fn main() {
   let result = 10.00;        // 默认是 f64 
   let interest:f32 = 8.35;
   let cost:f64 = 15000.600;  // 双精度浮点型

   println!("result value is {}",result);
   println!("interest is {}",interest);
   println!("cost is {}",cost);
}
```

编译运行以上 Rust 代码，报错信息如下

```
interest is 8.35
cost is 15000.6
```

## 不允许类型自动转换

Rust 中的数字类型与 C/C++ 中不同的是 Rust 语言不允许类型自动转换。

例如，把一个 _整型_ 赋值给一个 _浮点型_ 是会报错的。

### 范例

```
fn main() {
   let interest:f32 = 8;   // integer assigned to float variable
   println!("interest is {}",interest);
}
```

编译上面的代码，会抛出 mismatched types error 错误
```
error[E0308]: mismatched types
   --> main.rs:2:22
   |
 2 | let interest:f32=8;
   |    ^ expected f32, found integral variable
   |
   = note: expected type `f32`
      found type `{integer}`
error: aborting due to previous error(s)
```

## 数字可读性分隔符 _

为了方便阅读超大的数字，Rust 语言允许使用一个 *虚拟的分隔符* 也就是 *下划线（ _ ）* 来对数字进行可读性分隔符。

比如为了提高 50000 的可读性，我们可以写成 50_000 。

Rust 语言会在编译时移除数字可读性分隔符 _

#### 范例

我们写几个例子来演示下 数字分隔符，从结果中可以看出，分隔符对数字没有造成任何影响。

```
fn main() {
   let float_with_separator = 11_000.555_001;
   println!("float value {}",float_with_separator);

   let int_with_separator = 50_000;
   println!("int value {}",int_with_separator);
}
```

编译运行上面的代码，输出结果如下

```
float value 11000.555001
int value 50000
```

## 布尔类型 bool

布尔类型 只有两个可能的取值 true 或 false 。

Rust 使用 bool 关键字来声明一个 **布尔类型** 的变量。

### 范例
```
fn main() {
   let isfun:bool = true;
   println!("Is Rust Programming Fun ? {}",isfun);
}
```
编译运行上面的代码，输出结果如下
```
Is Rust Programming Fun ? true
```

## 字符类型 char

字符，简单的来说，就是字符串的基本组成部分，也就是单个字符或字。

Rust 使用 char 作为 字符数据类型。这点可谓是继承了 C / C++。

但与 C / C++ 不同的是：Rust 使用 UTF-8 作为底层的编码 ，而不是常见的使用 ASCII 作为底层编码。

也就是说，Rust 中的 **字符数据类型** 包含了数字、字母、Unicode 和 其它特殊字符。

Rust 选用 UTF-8 作为底层编码可谓是顺应时代的潮流。因为编程和互联网早就不极限于拉丁语系的国家，像中国、印度、日本等国家都有大量的程序员和网民。

Unicode 编码的标量值的范围从 U+0000 到 U+D7FF， U+E000 到 U+10FFFF（含）

### 范例
我们输出几个不同语系的字符来演示下 Rust 中的 char 字符类型。
```
fn main() {
   let special_character = '@'; //default
   let alphabet:char = 'A';
   let emoji:char = 'c'; // 笑脸的那个图

   println!("special character is {}",special_character);
   println!("alphabet is {}",alphabet);
   println!("emoji is {}",emoji);
}
```
编译运行上面的代码，输出结果如下
```
special character is @
alphabet is A
emoji is c
```