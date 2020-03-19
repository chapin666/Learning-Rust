# 二十二、Rust 泛型

**泛型** 就是可以在运行时指定数据类型的机制。

**泛型** 最大的好处就是一套代码可以应用于多种类型。比如我们的 **向量**，可以是整型向量，也可以是字符串向量。

**泛型** 既能保证数据安全和类型安全，同时还能减少代码量。所以，现代语言，没有范型简直就是鸡肋。

嘿，说的就是你，隔壁的 XX 语言。

Rust 语言中的泛型主要包含 **泛型集合**、**泛型结构体**、**泛型函数**、**范型枚举** 和 **特质** 几个方面。

## 22.1 Rust 语言中的泛型

Rust 使用使用 <T> 语法来实现泛型的东西指定数据类型。 其中 _T_ 可以是任意数据类型。

例如下面的两个类型

```
age: i32
age: String
```

使用泛型，则可以直接声明为

```
age:T
```

然后在使用过程中指定 _T_ 的类型为 _i32_ 或 _String_ 即可。

## 22.2 泛型集合

日常使用过程中，我们最常见的泛型应用莫过于 **集合 ( collection )** 了。

比如我们一直在使用的向量 Vec。它可以是一个字符串向量，也可以是一个整形向量。

下面的代码，我们声明了一个整型向量

```
fn main(){
   let mut vector_integer: Vec<i32> = vec![20,30];
   vector_integer.push(40);
   println!("{:?}",vector_integer);
}
```

编译运行以上 Rust 范例，输出结果如下

```
[20, 30, 40]
```

如果向整型向量传递一个非整型参数，则会报错

```
fn main() {
   let mut vector_integer: Vec<i32> = vec![20,30];
   vector_integer.push(40);
   vector_integer.push("hello"); 
   //error[E0308]: 类型不匹配
   println!("{:?}",vector_integer);
}
```

上面的代码中可以看出，整型的向量只能添加整型的参数，如果尝试添加一个字符串参数则会报错。

从某些方面说，**泛型** 让集合更通用，也更安全。

## 22.3 泛型结构体

我们也可以把某个结构体声明为泛型的，**泛型结构体** 主要是结构体的成员类型可以是泛型。

泛型结构体的定义语法如下

```
struct struct_name<T> {
   field:T,
}
```

例如我们可以定义一个泛型结构体 Data，它只有一个成员 value，类型是一个泛型。

```
struct Data<T> {
   value:T,
}
```

把我们的泛型结构体语法和泛型结构体实例 Data 放在一起对比下，很容易区分泛型结构体和普通结构体的不同。

### 22.3.1 范例

下面的范例，我们使用了刚刚定义的泛型结构体 Data，演示了如何使用泛型结构体

```
fn main() {
   // 泛型为 i32
   let t:Data<i32> = Data{value:350};
   println!("value is :{} ",t.value);
   // 泛型为 String
   let t2:Data<String> = Data{value:"Tom".to_string()};
   println!("value is :{} ",t2.value);
}

struct Data<T> {
   value:T,
}
```

编译运行以上 Rust 范例，输出结果如下

```
value is :350
value is :Tom
```

## 22.4 特质 Traits

Rust 语言中没有 **类 ( class )** 这个概念，于是顺带把 **接口 (interface )** 这个概念也取消了。

可是，没有了 **接口** 我们要如何证明两个结构体之间的关系呢 ？

为此，Rust 提供了 **特质 Traits** 这个蛋炒饭的概念。

说蛋炒饭，是因为很多语言都有特质这个概念。

Rust 提供了 **特质 Traits** 这个概念，用于跨多个结构体实现一组标准行为（方法）。

从某些方面来说，虽然 Rust 语言没有接口的概念，但 特质 Traits 实打实的就是接口啊。

### 22.4.1 特质的定义语法

Rust 语言提供了 trait 关键字用于定义 **特质**。

特质 其实就是一组方法原型。它的语法格式如下

```
trait some_trait {

   // 没有任何实现的虚方法
   fn method1(&self);

   // 有具体实现的普通方法
   fn method2(&self){
      //方法的具体代码
   }
}
```

从语法格式来看，特征可以包含具体方法（带实现的方法）或抽象方法（没有具体实现的方法）

如果想让某个方法的定义在实现了特质的结构体所共享，那么推荐使用具体方法。

如果想让某个方法的定义由实现了特质的结构体自己定义，那么推荐使用抽象方法。

即使某个方法是具体方法，结构体也可以对该方法进行重写。

### 22.4.2 实现特质 impl for

前面我们说过了，Rust 中的 特质 相当于其它语言的 **接口 （ interface ）**。

其它语言，比如 Java 实现接口使用的是 implement 关键字。

但 Rust 却不这么做，而是提供了一个更加语义化的词组 **impl for**。

如果要为某个结构体 （ struct ）实现某个特质，需要使用 **impl for** 语句。

impl 是 implement 的缩写。impl for 语义已经很明显了，就是为 xxx 实现 xxx 的意思。

impl for 语句的语法格式如下

```
impl some_trait for structure_name {
   // 实现 method1() 的具体代码
   fn method1(&self ){
   }
}
```

> 注意: 特质的方法是结构体的成员方法，因此第一个参数是 &self。

### 22.4.3 范例: impl for 的简单使用

下面的范例，我们首先定义了一个结构体 Book 和一个特质 Printable。

然后我们使用 impl for 语句为 Book 实现特质 Printable。

```
fn main(){
   //创建结构体 Book 的实例
   let b1 = Book {
      id:1001,
      name:"Rust in Action"
   };
   b1.print();
}

//声明结构体
struct Book {
   name:&'static str,
   id:u32
}
// 声明特质
trait Printable {
   fn print(&self);
}
// 实现特质
impl Printable for Book {
   fn print(&self){
      println!("Printing book with id:{} and name {}",self.id,self.name)
   }
}
```

编译运行以上 Rust 范例，输出结果如下

```
Printing book with id:1001 and name Rust in Action
```

## 22.5 泛型函数

泛型也可以用在函数中，我们称使用了泛型的函数为 泛型函数。

泛型函数 主要是指 函数的参数 是泛型。

> 注意: 泛型函数并不要求所有参数都是泛型，而是某个参数是泛型。

泛型函数的定义语法如下

```
fn function_name<T[:trait_name]>(param1:T, [other_params]) {

   // 函数实现代码
}
```

例如我们定义一个可以打印输出任意类型的泛型函数 print_pro

```
fn print_pro<T:Display>(t:T){
   println!("Inside print_pro generic function:");
   println!("{}",t);
}
```

对比语法和范例，泛型函数的定义就很简单了。

### 22.5.1 范例

下面的范例我们使用刚刚定义的泛型函数 print_pro()，该函数会打印输出传递给它的参数。

传递的参数可以是实现了 Display 特质的任意类型。

```
use std::fmt::Display;

fn main(){
   print_pro(10 as u8);
   print_pro(20 as u16);
   print_pro("Hello TutorialsPoint");
}

fn print_pro<T:Display>(t:T){
   println!("Inside print_pro generic function:");
   println!("{}",t);
}
```

编译运行以上 Rust 范例，输出结果如下

```
Inside print_pro generic function:
10
Inside print_pro generic function:
20
Inside print_pro generic function:
Hello TutorialsPoint
```