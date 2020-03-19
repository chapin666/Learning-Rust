# 十七、Rust 结构体 Struct

数组永远只能保存相同类型的元素。从某些方面说，数组是相同类型元素的集合。

但世界并不是那么美好的，永远都是复制克隆类的东西，很多东西往往比较复杂。

比如我们要描述一个人，它有着年龄，有着姓名，有着居住地址。如果我们要存储这些东西，数组是指望不上了。

为了解决这里问题，语言的开发者们想到了另一种可以让用户自定义类型的方法，那就是 **结构体（struct）**。

结构体，其实就是 **结构**，日常生活中，当我们提及某某结构的时候，都会分析说它是由什么什么组成的。比如当我们分析一张 桌子 的时候，我们会说它有 4 条腿，有一个桌面，有几个横杠...

结构体 就是可以组合不同类型的数据项，包括另一个结构。

## 17.1 定义一个结构体

几乎所有有结构体这个概念的语言，定义结构体的关键字都是一样的，那就是struct。

因为结构体是一个集合，也就是复合类型。结构体中的所有元素/字段也必须明确指明它的数据类型。

定义一个结构体的语法格式如下

```
struct Name_of_structure {
   field1:data_type,
   field2:data_type,
   field3:data_type
}
```

定义一个结构体时：

- 结构体名 Name_of_structure 和元素/字段名 fieldN 遵循普通变量的命名语法。
- 结构体中中的每一个元素/字段都必须明确指定数据类型。可以是基本类型，也可以是另一个结构体。

### 17.1.1 范例

下面的代码，我们定义了一个结构体 Employee ，它有着三个元素/字段，分别是姓名、年龄、公司。

```
struct Employee {
   name:String,
   company:String,
   age:u32
}
```

## 17.2 创建结构体的实例（也称为结构体初始化）

创建结构体的实例或者说结构体初始化本质上就是创建一个变量。使用 let 关键字创建一个变量。

创建结构体的一个实例和定义结构体的语法真的很类似。但说起来还是有点复杂，我们先看具体的语法

### 17.2.1 结构体初始化语法

```
let instance_name = Name_of_structure {
   field1:value1,
   field2:value2,
   field3:value3
}; 
```

从语法中可以看出，初始化结构体时的等号右边，就是把定义语法中的元素类型换成了具体的值。

结构体初始化，其实就是对结构体中的各个元素进行赋值。

> 注意: 千万不要忘记结尾的分号 ;

### 17.2.2 访问结构体实例元素的语法

如果要访问结构体实例的某个元素，我们可以使用 元素访问符，也就是 点号 ( . )

具体的访问语法格式如下

```
struct_name_instance.field_name
```

例如，如果要访问 Employee 的实例 emp1 中的 name 元素，可以如下使用

```
emp1.name
```

### 17.2.3 范例

下面的代码，我们定义了一个有三个元素的结构体 Employee，然后初始化一个实例 emp1，最后通过元素访问符来访问 emp1 的三个元素。

```
struct Employee {
   name:String,
   company:String,
   age:u32
}

fn main() {
   let emp1 = Employee {
      company:String::from("TutorialsPoint"),
      name:String::from("Mohtashim"),
      age:50
   };
   println!("Name is :{} company is {} age is {}",emp1.name,emp1.company,emp1.age);
}
```

编译运行以上 Rust 代码，输出结果如下

```
Name is :Mohtashim company is TutorialsPoint age is 50
```

## 17.3 修改结构体实例

修改结构体实例就是对结构体的个别元素 **重新赋值**。

结构体实例默认是 **不可修改的**，因为结构体实例也是一个使用 let 定义的变量。

如果要修改结构体实例，就必须在创建时添加 mut 关键字，让它变成可修改的。

因为没啥新的知识内容，我们就直接上范例吧。

### 17.3.1 范例

下面的范例给 Employee 的实例 emp1 添加了 mut 关键字，因此我们可以修改 emp1 的内部元素。


```
struct Employee {
   name:String,
   company:String,
   age:u32
}

let mut emp1 = Employee {
   company:String::from("TutorialsPoint"),
   name:String::from("Mohtashim"),
   age:50
};
emp1.age = 40;
println!("Name is :{} company is {} age is 
{}",emp1.name,emp1.company,emp1.age);
```

编译运行以上 Rust 代码，输出结果如下

```
Name is :Mohtashim company is TutorialsPoint age is 40
```

## 17.4 结构体作为函数的参数

结构体的用途之一就是可以作为参数传递给函数。

定一个结构体参数和定义其它类型的参数的语法是一样的。我们这里就不多介绍了，直接看范例

下面的代码定义了一个函数 display ，它接受一个 Employee 结构体实例作为参数并输出结构体的所有元素

```
fn display( emp:Employee) {
   println!("Name is :{} company is {} age is 
   {}",emp.name,emp.company,emp.age);
}
```

完整的可运行的实例代码如下

```
//定义一个结构体
struct Employee {
   name:String,
   company:String,
   age:u32
}
fn main() {
   //初始化结构体
   let emp1 = Employee {
      company:String::from("TutorialsPoint"),
      name:String::from("Mohtashim"),
      age:50
   };
   let emp2 = Employee{
      company:String::from("TutorialsPoint"),
      name:String::from("Kannan"),
      age:32
   };
   //将结构体作为参数传递给 display
   display(emp1);
   display(emp2);
}

// 使用点号(.) 访问符访问结构体的元素并输出它么的值
fn display( emp:Employee){
   println!("Name is :{} company is {} age is 
   {}",emp.name,emp.company,emp.age);
}
```

编译运行以上 Rust 代码，输出结果如下

```
Name is :Mohtashim company is TutorialsPoint age is 50
Name is :Kannan company is TutorialsPoint age is 32
```

## 17.5 结构体实例作为函数的返回值

Rust 中的结构体不仅仅可以作为函数的参数，还可以作为 函数的返回值。

函数返回结构体实例需要实现两个地方：
1. 在 箭头 -> 后面指定结构体作为一个返回参数。
2. 在函数的内部返回 结构体的实例


### 17.5.1 结构体实例作为函数的返回值的语法格式

```
struct My_struct {}

fn function_name([parameters]) -> My_struct {
   // 其它的函数逻辑
   return My_struct_instance;
}
```

### 17.5.2 范例

下面的代码，我们首先定义一个结构体 Employee ，接着定义一个方法 who_is_elder ，传入两个结构体 Employee 作为参数并返回年龄个大的那个。

````
fn main() {

   let emp1 = Employee{
      company:String::from("TutorialsPoint"),
      name:String::from("Mohtashim"),
      age:50
   };
   let emp2 = Employee {
      company:String::from("TutorialsPoint"),
      name:String::from("Kannan"),
      age:32
   };
   let elder = who_is_elder(emp1,emp2);
   println!("elder is:");

   display(elder);
}

//接受两个 Employee 的实例作为参数并返回年长的那个
fn who_is_elder (emp1:Employee,emp2:Employee)->Employee {
   if emp1.age>emp2.age {
      return emp1;
   } else {
      return emp2;
   }
}

// 显示结构体的所有元素
fn display( emp:Employee) {
   println!("Name is :{} company is {} age is {}",emp.name,emp.company,emp.age);
}

// 定义一个结构体
struct Employee {
   name:String,
   company:String,
   age:u32
}
````

编译运行以上 Rust 代码，输出结果如下

```
elder is:
Name is :Mohtashim company is TutorialsPoint age is 50
```

## 17.6 结构体中的方法

Rust 中的结构体可以定义方法 (method)。

方法 (method) 是一段代码的逻辑组合，用于完成某项特定的任务或实现某项特定的功能。

方法（method） 和 函数（function) 有什么不同之处呢？

简单的说：
1. 函数（ function） 没有属主，也就是归属于谁，因此可以直接调用。
2. 方法（ method ） 是有属主的，调用的时候必须指定 属主。
3. 函数（ function） 没有属主，同一个程序不可以出现两个相同签名的函数。
4. 方法（ method ） 有属主，不同的属主可以有着相同签名的方法。

定义方法时需要使用 fn 关键字。

> fn 关键字是 function 取头尾两个字母的缩写。

结构体方法的 **作用域** 仅限于 **结构体内部**。

与 C++ 语言中的结构体的方法不同的是，Rust 中的结构体方法只能定义在结构体的外面。

在定义结构体方法时需要使用 impl 关键字，语法格式如下

```
struct My_struct {}

impl My_struct { 
   // 属于结构体的所有其它代码
}
```

impl 关键字最重要的作用，就是定义上面我们所说的方法的属主。所有被 impl My_struct 块包含的代码，都只属于 My_struct 这个结构。

impl 关键字是 implement 的前 4 个字母的缩写。意思是 **实现**。

结构体的普通方法（后面我们还会学到其它方法）时，第一个参数永远是 &self 关键字。self 是"自我"的意思，&self 永远表示着当前的结构体的一个实例。

这是不是可以带来其它的结构体方法的解释：结构体的方法就是用来操作当前结构体的一个实例的。

### 17.6.1 结构体方法的定义语法

定义结构体方法的语法格式如下

```
struct My_struct {}
impl My_struct { 

   // 定义一个结构体的普通方法
   fn method_name(&self[,other_parameters]) { 
      //方法的具体逻辑代码
   }

}
```

&self 是结构体普通方法固定的第一个参数，其它参数则是可选的。

即使结构体方法不需要传递任何参数，&self 也是固定的，必须存在的。像下面这种定义方法是错误的

```
struct My_struct {}
impl My_struct { 

   //定义一个结构体的普通方法
   fn method_name([other_parameters]) { 
      //方法的具体逻辑代码
   }
}
```

### 17.6.2 结构体方法内部访问结构体元素

因为我们在定义方法时固定传递了 &self 关键字。而 &self 关键字又代表了当前方法的属主。

因此我们可以在方法内部使用 self, 来访问结构体的元素。

详细的语法格式如下

```
struct My_struct {
   age: u32
}

impl My_struct { 

   //定义一个结构体的普通方法
   fn method_name([other_parameters]) {

      self.age = 28;

      println!("{}",self.age);

      //其它的具体逻辑代码
   }
}
```

### 17.6.3 结构体方法的调用语法

因为结构体的方法是有属主的，所以调用的时候必须先指定 **属主**，调用格式为 **属主.方法名(方法参数)**。

详细的调用语法格式为

```
My_struct.method_name([other_parameters])
```

> 注意
虽然定义方法时需要固定 &self 作为第一个参数，但在调用的时候是 不需要也不能 传递的。这个参数的传递 Rust 编译器会 偷偷的 帮我们完成。

### 17.6.4 范例

下面的代码，我们首先定义了一个结构体 Rectangle 用于表示一个 长方形，它有宽高 两个元素 width 和 height。

然后我们又为 结构体 Rectangle 定义了一个方法 area 用于计算当前 长方形实例 的面积。

```
// 定义一个长方形结构体
struct Rectangle {
   width:u32, height:u32
}

// 为长方形结构体定义一个方法，用于计算当前长方形的面积
impl Rectangle {
   fn area(&self)->u32 {
      // 在方法内部，可以使用点号 `self.` 来访问当前结构体的元素。use the . operator to fetch the value of a field via the self keyword
      self.width * self.height
   }
}

fn main() {
   // 创建 Rectangle 结构体的一个实例
   let small = Rectangle {
      width:10,
      height:20
   };

   //计算并输出结构体的面积
   println!("width is {} height is {} area of Rectangle 
   is {}",small.width,small.height,small.area());
}
```

编译运行以上 Rust 代码，输出结果如下

```
width is 10 height is 20 area of Rectangle is 200
```

## 17.7 结构体的静态方法

Rust 中的结构体还可以有静态方法。

静态方法可以直接通过结构体名调用而无需先实例化。

结构体的静态方法定义方式和普通方法类似，唯一的不同点是 **不需要使用 &self** 作为参数。

### 17.7.1 定义静态方法的语法

结构体静态方法的定义语法格式如下

```
impl Structure_Name {

   // Structure_Name 结构体的静态方法
   fn method_name(param1: datatype, param2: datatype) -> return_type {
      // 方法内部逻辑
   }

}
```

静态方法和其它普通方法一样，参数是可选的。也就是可以没有参数

### 17.7.2 调用静态方法的语法

静态方法可以直接通过结构体名调用，而无需先实例化。

结构体的静态方法需要使用 structure_name:: 语法来访问，详细的语法格式如下

```
structure_name::method_name(v1,v2)
```
 
## 17.3 范例

下面的范例，我们为结构体 Point 定义了一个静态方法 getInstance()。

```
getInstance() 是一个 工厂方法，它初始化并返回结构体 Point 的实例。

//声明结构体 Point
struct Point {
   x: i32,
   y: i32,
}

impl Point {
   // 用于创建 Point 实例的静态方法
   fn getInstance(x: i32, y: i32) -> Point {
      Point { x: x, y: y }
   }
   // 用于显示结构体元素的普通方法
   fn display(&self){
      println!("x = {} y = {}",self.x,self.y );
   }
}
fn main(){

   // 调用静态方法
   let p1 = Point::getInstance(10,20);
   p1.display();

}
```

编译运行以上 Rust 代码，输出结果如下

```
x = 10 y = 20
```