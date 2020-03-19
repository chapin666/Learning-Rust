# Rust 枚举 Enum

长大的时候回去想想小时候，觉得有时候特么的太傻逼了，比如下面这道选择题

下面哪个不是水果？

```
    A. 香蕉   B. 梨   C. 橘子  D. 茄子
```

我特么的能选择 A。 原因是其它三个都吃过，可是香蕉，我家那种野香蕉树，香蕉就只有拇指大小，还特别涩，哪能吃啊。

长大了之后，也会觉得过得很苦逼，比如填表格的时候

请问你的婚姻状况是？

```

    A. 未婚  B 已婚   C 离异
```

都填了二十几年的表了，选择永远是 A 。

上面两个问题有啥共同点么 ？

都是很傻逼.... ?

哈哈，不是的，它们的共同点都是提供了 ABCD 让我们选择。

编程时我们要如何保存 A B C D 这种选择题和答案呢 ？

如果只利用我们之前的所学知识，大概是

```
let option_a = "香蕉";
let option_b = "梨";
let option_c = "橘子";
let option_d = "茄子";
```

大家有没有发现什么问题？ 我们为啥要傻傻的定义四个变量啊，用一个数组就搞定了

```
let option = ["香蕉","梨","橘子","茄子"];
```

看起来不错的样子，可另一个问题出现了，一般做选择的时候我们都会回答**香蕉**或**茄子** ，肯定不会回答**0**或**3**。因为出题的人看不懂啊。

这个说法不恰当，但目前没想到更好的。

## 枚举

像这种万里挑一的问题，从众多个选项中选择一个的问题，像这种众多选项，Rust 提供了一个新的数据类型用来表示它们。

这个新的数据类型就是 **枚举** ，英文 **enum**。

也就是说，枚举 用于从众多的可变列表中选择一个。

## 枚举定义

Rust 语言提供了 enum 关键字用于定义枚举。

定义枚举的语法格式如下

```
enum enum_name {
   variant1,
   variant2,
   variant3
}
```

例如上面我们的 香蕉橘子选项，我们可以定义一个枚举 Fruits

```
enum Fruits {
    Banana,     // 香蕉
    Pear,       // 梨
    Mandarin,   // 橘子
    Eggplant    // 茄子
}
```

## 使用枚举

枚举定义好了之后我们就要开始用它了，枚举的使用方式很简单，就是 枚举名::枚举值。语法格式如下

```
enum_name::variant
```

例如上面的枚举，比如我选择了 香蕉，那么赋值的语法如下

```
let selected = Fruits::Banana;
```

如果需要明确指定类型，可以如下

```
let selected: Fruits = Fruits::Banana;
```

### 范例

下面的范例，演示了枚举类型的基本使用方法和案例。

我们首先定义了一个枚举 Fruits ，它有四个枚举值，分别是 Banana、Pear、 Mandarin 和 Eggplant。

println!() 用于输出枚举。

> 注意： 关于枚举前面的 #[derive(Debug)] 我们后面会介绍

```
#[derive(Debug)]
enum Fruits {
    Banana,     // 香蕉
    Pear,       // 梨
    Mandarin,   // 橘子
    Eggplant    // 茄子
}

fn main() {
   let selected = Fruits::Banana;
   println!("{:?}",selected);
}
```

编译运行以上 Rust 代码，输出结果如下

```
Banana
```

## #[derive(Debug)] 注解

想必大家看到了 enum Fruits 前面的 #[derive(Debug)]。

这个 #[derive(Debug)] 语句的作用是啥呢 ？

我先不解释，我先把 #[derive(Debug)] 去掉看看

```
enum Fruits {
    Banana,     // 香蕉
    Pear,       // 梨
    Mandarin,   // 橘子
    Eggplant   // 茄子
}

fn main() {
   let selected = Fruits::Banana;
   println!("{:?}",selected);
}
```

编译上面的 Rust 代码，或报错，错误信息如下

```
error[E0277]: `Fruits` doesn't implement `std::fmt::Debug`
  --> src/main.rs:11:20
   |
11 |    println!("{:?}",selected);
   |                    ^^^^^^^^ `Fruits` cannot be formatted using `{:?}`
   |
   = help: the trait `std::fmt::Debug` is not implemented for `Fruits`
   = note: add `#[derive(Debug)]` or manually implement `std::fmt::Debug`
   = note: required by `std::fmt::Debug::fmt`
```

这段错误的意思，就是我们的 enum Fruits 枚举并没有实现 std::fmt::Debug 特质 （ trait ）。

> 关于特质 trait 我们会在后面介绍，这里你只要把特质当作接口 interface 看待就好。

为了让编译能通过，我们需要将我们的枚举派生自或衍生自一个已经实现了 std::fmt::Debug 特质的东西。这个东西比较常见的就是 Debug 。

因此 #[derive(Debug)] 注解的作用，就是让 Fruits 派生自 Debug。

但，其实，即使添加了 #[derive(Debug)] 注解注解仍然会有警告

```
#[derive(Debug)]
enum Fruits {
    Banana,     // 香蕉
    Pear,       // 梨
    Mandarin,   // 橘子
    Eggplant   // 茄子
}

fn main() {
   let selected = Fruits::Banana;
   println!("{:?}",selected);
}
```

编译结果如下

```
warning: variant is never constructed: `Pear`
 --> main.rs:4:5
  |
4 |     Pear,       // 梨
  |     ^^^^
  |
  = note: #[warn(dead_code)] on by default

warning: variant is never constructed: `Mandarin`
 --> main.rs:5:5
  |
5 |     Mandarin,   // 橘子
  |     ^^^^^^^^

warning: variant is never constructed: `Eggplant`
 --> main.rs:6:5
  |
6 |     Eggplant   // 茄子
  |     ^^^^^^^^
```

大概的意思是说，那些我们没用到的枚举都还没有被构造呢。

```
variant is never constructed: Eggplant 具体是啥意思，以后有空再来 YY 吧。
```

## 结构类型 struct 和枚举类型 enum

好了，到目前为止，我们已经学习了两个可以自定义类型的东西了，
- 一个是 结构类型 struct
- 另一个是 枚举类型 enum

它们之间有什么关系和关联呢？

但是是它们是八杆子都打不着的东西。

如果说有那么一丁点儿关系，那就是 枚举类型 enum 可以作为结构体的成员变量的数据类型。

### 范例

下面的代码，我们定义了一个枚举 GenderCategory 用于表示性别，枚举值有 Male 和 Female 分别表示男和女。

> 还有第三种性别？ 这里忽略吧。

同时，我们又定义了一个结构体 Person 用于描述一个人。这个 Person 结构体使用 GenderCategory 枚举作为其成员变量 gender 的数据类型

因此，Person 结构体的 gener 成员变量就只能有两个值: Male 和 Female

```
// 添加 #[derive(Debug)] 省的报错
#[derive(Debug)]
enum GenderCategory {
   Male,Female
}

// 添加 #[derive(Debug)] 省的报错
#[derive(Debug)]
struct Person {
   name:String,
   gender:GenderCategory
}

fn main() {
   let p1 = Person {
      name:String::from("零基础教程"),
      gender:GenderCategory::Female
   };
   let p2 = Person {
      name:String::from("Admin"),
      gender:GenderCategory::Male
   };
   println!("{:?}",p1);
   println!("{:?}",p2);
}
```

编译运行以上 Rust 代码，输出结果如下

```
Person { name: "零基础教程", gender: Female }
Person { name: "Admin", gender: Male }
```

## Option 枚举

Rust 语言核心和标准库内置了很多枚举，其中有一个枚举我们会经常和它打交道，那就是 Option 枚举。

Option 枚举代表了那种 可有可无 的选项。它有两个枚举值 None 和 Some(T)。

- None 表示可有可无中的 无。
- Some(T) 表示可有可无中的 有，既然有，那么就一定有值，也就是一定有数据类型，那个 T 就表示有值时的值数据类型。

### Option 枚举的定义代码如下

```
enum Option<T> {
   Some(T),      // 用于返回一个值 used to return a value
   None          // 用于返回 null ，虽然 Rust 并不支持 null 
   the null keyword
}
```

Rust 语言并不支持 null 关键字，取而代之的是使用 None 作为没有的意思。

Option 枚举经常用在函数中作为返回值，因为它可以表示有返回且有值，也可以用于表示有返回但没有值。

如果函数有返回值，那么可以返回 Some(data)，如果函数没有返回值，则可以返回 None

如果你还不理解，那么就直接看范例吧

### 范例

下面的范例，我们定义了一个函数 is_even()，使用 Option 枚举作为它的返回值类型。

如果传递给 is_even() 函数的参数是个偶数，则返回传递的参数，如果是奇数则返回 None。

```
fn main() {
   let result = is_even(3);
   println!("{:?}",result);
   println!("{:?}",is_even(30));
}
fn is_even(no:i32)->Option<bool> {
   if no %2 == 0 {
      Some(true)
   } else {
      None
   }
}
```

编译运行以上 Rust 代码，输出结果如下

```
None
Some(true)
```

## match 语句和枚举

枚举的另一个重要操作就是判断枚举值。判断一个枚举值，== 比较运算符是不起作用的。

准确的说会报错

```
#[derive(Debug)]
enum Fruits {
    Banana,     // 香蕉
    Pear,       // 梨
    Mandarin,   // 橘子
    Eggplant   // 茄子
}

fn main() {
    let selected = Fruits::Banana;
    if selected == Fruits::Banana {
        println!("你选择了香蕉");
    } else {
        println!("你选择了其它");
    }
   println!("{:?}",selected);
}
```

编译错误

```
  --> src/main.rs:11:17
   |
11 |     if selected == Fruits::Banana {
   |        -------- ^^ -------------- Fruits
   |        |
   |        Fruits
   |
   = note: an implementation of `std::cmp::PartialEq` might be missing for `Fruits`
```

判断一个枚举变量的值，唯一能用的操作符就是 match 语句。

match 语句我们之间已经学过了，我们就不介绍了。我们直接上范例，看看如何使用 match 语句来判断枚举值

### 范例

下面的代码，我们定义了一个枚举 CarType，同时定义了一个函数 print_size() ，它接受 CarType 枚举类型的变量，并使用 match 语句来判断枚举变量的值。

```
enum CarType {
   Hatch,
   Sedan,
   SUV
}
fn print_size(car:CarType) {
   match car {
      CarType::Hatch => {
         println!("Small sized car");
      },
      CarType::Sedan => {
         println!("medium sized car");
      },
      CarType::SUV =>{
         println!("Large sized Sports Utility car");
      }
   }
}
fn main(){
   print_size(CarType::SUV);
   print_size(CarType::Hatch);
   print_size(CarType::Sedan);
}
```

编译运行以上 Rust 代码，输出结果如下

```
Large sized Sports Utility car
Small sized car
medium sized car
```

## match 语句和 Option 类型

既然 match 语句可以用于比较和判断枚举变量的值，那么对于上面提到的 Option 枚举，match 语句也使用。

事实上，match 语句和 Option 类型相结合才能体现出 Option 枚举的强大之处

```
fn main() {
   match is_even(5) {
      Some(data) => {
         if data==true {
            println!("Even no");
         }
      },
      None => {
         println!("not even");
      }
   }
}
fn is_even(no:i32)->Option<bool> {
   if no%2 == 0 {
      Some(true)
   } else {
      None
   }
}
```

编译运行以上 Rust 代码，输出结果如下

```
not even
```

## match 语句和带数据类型的枚举

Rust 中的枚举值可以有它们自己的数据类型。这种枚举值带数据类型的语法，彻底把 match 语句和枚举推向了史无前例的高度。

更厉害的是，每一个枚举值可以有不同的数据类型。

枚举值带数据类型的语法格式很简单，如下

```
enum enum_name {
   variant1(data_type1),
   variant2(data_type2),
   variant3(data_type1)
}
```

例如下面的枚举

```
enum GenderCategory {
   Name(String),
   Usr_ID(i32)
}
```

枚举 GenderCategory 有两个枚举值 Name 和 User_ID，它们有着不同的数据类型：String 和 i32。

### 范例

下面的范例，我们定义了一个带数据类型的枚举 GenderCategory。 然后演示了带数据类型枚举值的初始化和 match 语句的判断

```
// 省的报错
#[derive(Debug)]
enum GenderCategory {
   Name(String),Usr_ID(i32)
}
fn main() {
   let p1 = GenderCategory::Name(String::from("Mohtashim"));
   let p2 = GenderCategory::Usr_ID(100);
   println!("{:?}",p1);
   println!("{:?}",p2);

   match p1 {
      GenderCategory::Name(val)=> {
         println!("{}",val);
      }
      GenderCategory::Usr_ID(val)=> {
         println!("{}",val);
      }
   }
}
```

编译运行以上 Rust 代码，输出结果如下

```
Name("Mohtashim")
Usr_ID(100)
Mohtashim
```