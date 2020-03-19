# Rust 字符串

Rust 语言提供了两种字符串
- 字符串字面量 **&str**。它是 Rust 核心内置的数据类型。
- 字符串对象 **String**。它不是 Rust 核心的一部分，只是 Rust 标准库中的一个 **公开 pub 结构体**。

## 字符串字面量 &str

字符串字面量 &str 就是在 编译时 就知道其值的字符串类型，是 Rust 语言核心的一部分。
字符串字面量 &str 是字符的集合，被硬编码赋值给一个变量。

```
let name1 = "你好，零基础教程 简单编程";
```

字符串字面量的核心代码可以在模块 _std::str_ 中找到，如果你有兴趣，可以阅读一二。

Rust 中的字符串字面量被称之为 **字符串切片**。因为它的底层实现是 **切片**。

### 范例

下面的代码，我们定义了两个字符串字面量 company 和 location

```
fn main() {
   let company:&str="零基础教程";
   let location:&str = "中国";
   println!("公司名 : {} 位于 :{}",company,location);
}
```

字符串字面量模式是 **静态** 的。 这就意味着字符串字面量从创建时开始会一直保存到程序结束。

因为默认是 **静态** 的，我们也可以主动添加 static 关键字。只不过语法格式有点怪，所以日常使用还是忽略吧。

```
fn main() {
   let company:&'static str = "零基础教程";
   let location:&'static str = "中国";
   println!("公司名 : {} 位于 :{}",company,location);
}
```

编译运行以上 Rust 代码，输出结果如下

```
公司名 : 零基础教程 位于 :中国
```

## 字符串对象

字符串对象是 Rust 标准库提供的内建类型。

与字符串字面量不同的是：字符串对象并不是 Rust 核心内置的数据类型，它只是标准库中的一个 公开 pub 的结构体。

字符串对象在标准库中的定义语法如下

```
pub struct String
```

字符串对象是是一个 长度可变的集合，它是 **可变** 的而且使用 UTF-8 作为底层数据编码格式。

字符串对象在 **堆 heap** 中分配，可以在运行时提供字符串值以及相应的操作方法。

### 创建字符串对象的语法

要创建一个字符串对象，有两种方法：

1. 一种是创建一个新的空字符串，使用 String::new() 静态方法

```
String::new()
```

2. 另一种是根据指定的字符串字面量来创建字符串对象，使用 String::from() 方法

```
String::from()
```

### 范例

下面，我们分别使用 String::new() 方法和 String::from() 方法创建字符串对象，并输出字符串对象的长度

```
fn main(){
   let empty_string = String::new();
   println!("长度是 {}",empty_string.len());

   let content_string = String::from("零基础教程");
   println!("长度是 {}",content_string.len());
}
```

编译运行以上 Rust 代码，输出结果如下

```
长度是 0
长度是 12
```

The above example creates two strings − an empty string object using the new method and a string object from string literal using the from method.

## 字符串对象常用的方法

| 方法 |	原型 | 说明 |
| -- | -- | -- |
|new() |	pub const fn new() -> String |	创建一个新的字符串对象 |
|to_string() |	fn to_string(&self) -> String	| 将字符串字面量转换为字符串对象 |
|replace()	pub fn replace<'a, P>(&'a self, from: P, to: &str) -> String |	搜索指定模式并替换 |
|as_str() |	pub fn as_str(&self) -> &str |	将字符串对象转换为字符串字面量 |
|push() |	pub fn push(&mut self, ch: char) |	再字符串末尾追加字符 |
|push_str() |	pub fn push_str(&mut self, string: &str) |	再字符串末尾追加字符串 |
|len() |	pub fn len(&self) -> usize |	返回字符串的字节长度
|trim() |	pub fn trim(&self) -> &str |	去除字符串首尾的空白符
|split_whitespace() |	pub fn split_whitespace(&self) -> SplitWhitespace |	根据空白符分割字符串并返回分割后的迭代器
|split() |	pub fn split<'a, P>(&'a self, pat: P) -> Split<'a, P> |	根据指定模式分割字符串并返回分割后的迭代器。模式 P 可以是字符串字面量或字符或一个返回分割符的闭包
|chars() |	pub fn chars(&self) -> Chars |	返回字符串所有字符组成的迭代器

## 创建一个新的空字符串对象 new()

如果要创建一个新的空字符串对象，我们可以调用 new() 方法。

```
fn main(){
   let mut z = String::new();
   z.push_str("零基础教程 简单编程");
   println!("{}",z);
}
```

编译运行以上 Rust 代码，输出结果如下

```
零基础教程 简单编程
```

## 字符串字面量转换为字符串对象 to_string()

字符串字面量是没有任何操作方法的，它仅仅只保存了 字符串 本身。

其实是有的，就是 to_string()

如果要对字符串进行一些操作，就必须将字符串转换为字符串对象。而这个转换过程，可以通过调用 to_string() 方法

```
fn main(){
   let name1 = "你好，零基础教程
   简单编程".to_string();
   println!("{}",name1);
}
```

编译运行以上 Rust 代码，输出结果如下

```
你好，零基础教程
   简单编程
```

## 字符串替换 replace()

如果要一个字符串对象中的指定字符串子串替换成另一个字符串，可以调用 **replace()** 方法。

replace() 方法接受两个参数：

- 第一个参数是要被替换的字符串子串或模式。
- 第二个参数是新替换的字符串。
> 注意: replace() 会搜索和替换所有要被替换的字符串子串或模式。

### 范例

下面的代码，我们搜索字符串中所有的 程 字，并替换成 www.badu.com

```
fn main(){
   let name1 = "零基础教程 简单编
   程".to_string(); //原字符串对象
   let name2 = name1.replace("程","www.badu.com");    // 查找并替换
   println!("{}",name2);
}
```

编译运行以上 Rust 代码，输出结果如下

```
简单教www.badu.com 简单编
   www.badu.com
```

## 将字符串对象转换为字符串字面量 as_str()

字符串字面量就是字符串那些字符，比如

```
let name1 = "零基础教程"; 
```

name1 是一个字符串字面量，它只包含 零基础教程 四个字本身。

字符串字面量只包含字符串本身，并没有提供相应的操作方法。

如果要返回一个字符串对象的 **字符串** 字面量，则可以调用 **as_str()** 方法

```
fn main() {
   let example_string = String::from("零基础教程");
   print_literal(example_string.as_str());
}
fn print_literal(data:&str ){
   println!("显示的字符串字面量是: {}",data);
}
```

编译运行以上 Rust 代码，输出结果如下

```
显示的字符串字面量是: 零基础教程
```

## 原字符串后追加字符 push()

如果要在一个字符串后面追加字符则首先需要将该字符串声明为 **可变** 的，也就是使用 mut 关键字。然后再调用 **push()** 方法。

push() 是在原字符上追加，而不是返回一个新的字符串

```
fn main(){
   let mut company = "零基础教程".to_string();
   company.push('t');
   println!("{}",company);
}
```

编译运行以上 Rust 代码，输出结果如下

```
零基础教程t
```

## 原字符串后追加字符串 push_str()

如果要在一个字符串后面追加字符串则首先需要将该字符串声明为 **可变** 的，也就是使用 mut 关键字。然后再调用 **push_str()** 方法。

push_str() 是在原字符上追加，而不是返回一个新的字符串

```
fn main(){
   let mut company = "零基础教程".to_string();
   company.push_str(" 简单编程");
   println!("{}",company);
}
```

编译运行以上 Rust 代码，输出结果如下

```
零基础教程 简单编程
```

## 获取字符串长度 len()

如果要返回字符串中的总字节数可以使用 **len()** 方法。

len() 方法会统计所有的字符，包括空白符。

空白符是指 制表符 \t、空格 、回车 \r、换行 \n 和回车换行 \r\n 等等。

```
fn main() {
   let fullname = " 零基础教程 简单编程 www.badu.com";
   println!("length is {}",fullname.len());
}
```

编译运行以上 Rust 代码，输出结果如下

```
length is 38
```

## 去除字符串头尾的空白符 trim()

空白符是指 制表符 \t、空格 、回车 \r、换行 \n 和回车换行 \r\n 等等。

如果要去掉字符串头尾的空白符，可以使用 **trim()** 方法。

该方法并不会去掉不是头尾的空白符，而且该方法会返回一个新的字符串。

```
fn main() {
   let fullname = " \t简单\t教程 \r\n简单
   编程\r\n     ";
   println!("Before trim ");
   println!("length is {}",fullname.len());
   println!();
   println!("After trim ");
   println!("length is {}",fullname.trim().len());
   println!("string is :{}",fullname.trim());
}
```

编译运行以上 Rust 代码，输出结果如下

```
Before trim 
length is 41

After trim 
length is 32
string is :简单  教程 
简单
   编程
```

## 使用空白符分割字符串 split_whitespace()

空白符是指 制表符 \t、空格 、回车 \r、换行 \n 和回车换行 \r\n 等等。

根据空白符分割字符串是最常用的操作之一，为此，Rust 语言为字符串提供了 **split_whitespace()** 用于根据空白符 分割一个字符串并返回一个迭代器。

我们可以使用这个迭代器来访问分割后的字符串。

```
fn main(){
   let msg = "零基础教程 简单编程 www.badu.com
   https://www.badu.com".to_string();
   let mut i = 1;

   for token in msg.split_whitespace(){
      println!("token {} {}",i,token);
      i+=1;
   }
}
```

编译运行以上 Rust 代码，输出结果如下

```
token 1 零基础教程
token 2 简单编程
token 3 www.badu.com
token 4 https://www.badu.com
```

## 根据指定模式分割字符串 split()

如果要将字符串根据某些指定的 字符串子串 分割，则可以使用 **split()** 方法。

split() 会根据传递的指定 模式 （字符串分割符） 来分割字符串，并返回分割后的字符串子串组成的切片上的迭代器。我们可以通过这个迭代器来迭代分割的字符串子串。

split() 方法最大的缺点是不可重入迭代，也就是迭代器一旦使用，则需要重新调用才可以再用。

但我们可以先在迭代器上调用 collect() 方法将迭代器转换为 向量 Vector ，这样就可以重复使用了。

```
fn main() {
   let fullname = "李白，诗仙，唐朝";

   for token in fullname.split("，"){
      println!("token is {}",token);
   }

   // 存储在一个向量中
   println!("\n");
   let tokens:Vec<&str>= fullname.split("，").collect();
   println!("姓名 is {}",tokens[0]);
   println!("称号 {}",tokens[1]);
   println!("朝代 {}",tokens[2]);
}
```

编译运行以上 Rust 代码，输出结果如下

```
token is 李白
token is 诗仙
token is 唐朝

姓名 is 李白
称号 诗仙
朝代 唐朝
```

## 将字符串打散为字符数组 chars()

如果要将一个字符串打散为所有字符组成的数组，可以使用 **chars()** 方法。

从某些方面说，如果我们要迭代字符串中的每一个字符，则必须首先将它打散为字符数组，然后才能遍历。

```
fn main(){
   let n1 = "零基础教程 www.badu.com".to_string();

   for n in n1.chars(){
      println!("{}",n);
   }
}
```
编译运行以上 Rust 代码，输出结果如下
```
简
单
教
程

w
w
w
.
t
w
l
e
.
c
n
```

## 字符串连接符 +

将一个字符串追加到另一个字符串的末尾，创建一个新的字符串，我们将这种操作称之为 **连接**，有时候也称之为 **拼接** 或 **差值**。

**连接** 的结果是创建一个新的字符串对象。

Rust 语言使用 **加号 +** 来完成这种 **连接**，我们称之为 **字符串连接符**。

### 字符串连接符 + 的内部实现

字符串连接符 **+** 的内部实现，其实是重写了 **add()** 方法。该方法接受两个参数，第一个参数是当前的字符串对象 self ，也就是 + 左边的字符串，第二个参数是一个引用，它指向了 + 右边的字符串。

具体的方法原型如下

```
//字符串拼接符的实现
add(self,&str)->String { 
   // 返回一个新的字符串对象
}
```

### 范例

下面的代码，我们使用 字符串拼接符 + 将连个字符串变量拼接成一个新的字符串

```
fn main(){
   let n1 = "零基础教程".to_string();
   let n2 = "简单编程".to_string();

   let n3 = n1 + &n2; // 需要传递 n2 的引用
   println!("{}",n3);
}
```

编译运行以上 Rust 代码，输出结果如下

```
零基础教程 简单编程
```

## 类型转换 to_string()

如果需要将其它类型转换为字符串类型，可以直接调用 **to_string()** 方法。

例如可以调用一个数字类型的变量的 to_string() 方法将当前变量转换为字符串类型。

```
fn main(){
   let number = 2020;
   let number_as_string = number.to_string(); 

   // 转换数字为字符串类型
   println!("{}",number_as_string);
   println!("{}",number_as_string=="2020");
}
```

编译运行以上 Rust 代码，输出结果如下

```
2020
true
```

## 格式化宏 format!

如果要把不同的变量或对象拼接成一个字符串，我们可以使用 **格式化宏 ( format! )**

格式化宏 format! 的使用方法如下

```
fn main(){
   let n1 = "零基础教程".to_string();
   let n2 = "简单编程".to_string();
   let n3 = format!("{} {}",n1,n2);
   println!("{}",n3);
}
```

编译运行以上 Rust 代码，输出结果如下

```
零基础教程 简单编程
```