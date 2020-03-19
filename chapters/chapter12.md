# Rust 元组 tuple

Rust 支持元组 tuple。而且元组是一个 **复合类型** 。

## 标量类型 vs 复合类型

1. 基础类型/标量类型 只能存储一种类型的数据。例如一个 i32 的变量只能存储一个数字。
2. **复合类型** 可以存储多个不同类型的数据。

复合类型就像我们的菜篮子，里面可以放各种类型的菜。

## 元组

元组有着固定的长度。而且一旦定义，就不能再增长或缩小。

元组的下标从 0 开始。

### 元组定义语法

Rust 语言中元组的定义语法格式如下

```
let tuple_name:(data_type1,data_type2,data_type3) = (value1,value2,value3);
```

定义元组时也可以忽略数据类型

```
let tuple_name = (value1,value2,value3);
```

Rust 中元组的定义很简单，就是使用一对小括号 () 把所有元素放在一起，元素之间使用逗号 , 分隔。

定义元组数据类型的时候也是一样的。

但需要注意的是，如果显式指定了元组的数据类型，那么数据类型的个数必须和元组的个数相同，否则会报错。

### 范例 1

如果要输出元组中的所有元素，必须使用 {:?} 格式化符。

```
fn main() {
   let tuple:(i32,f64,u8) = (-325,4.9,22);
   println!("{:?}",tuple);
}
```

编译运行以上 Rust 代码，输出结果如下

```
(-325, 4.9, 22)
```

仅仅使用下面的输出语句是不能输出元组中的元素的。

```
println!("{ }",tuple)
```

因为元组是一个 **复合类型**，要输出复合类型的数据，必须使用 println!("{:?}", tuple_name)

### 访问元组中的单个元素

我们可以使用 **元组名.索引数字** 来访问元组中相应索引位置的元素。索引从 0 开始。

例如下面这个拥有 3 个元素的元组
```
let tuple:(i32,f64,u8) = (-325,4.9,22);
```

我们可以通过下面的方式访问各个元素

```
tuple.0  // -325

tuple.1  // 4.9

tuple.2  // 22
```

### 范例

下面的范例演示了如何通过 **元组名.索引数字** 方式输出元组中的各个元素

```
fn main() {
   let tuple:(i32,f64,u8) = (-325,4.9,22);
   println!("integer is :{:?}",tuple.0);
   println!("float is :{:?}",tuple.1);
   println!("unsigned integer is :{:?}",tuple.2);
}
```

编译运行以上 Rust 代码，输出结果如下

```
integer is :-325
float is :4.9
unsigned integer is :2
```

## 元组也可以作为函数的参数

Rust 语言中，元组也可以作为函数的参数。

函数参数中元组参数的声明语法和声明一个元素变量是相似的

```
fn function_name(tuple_name:(i32,bool,f64)){}
```

### 范例

下面这段代码，我们声明一个函数 print，它接受一个元组作为参数并打印元组中的所有元素

```
fn main(){
   let b:(i32,bool,f64) = (110,true,10.9);
   print(b);
}

// 使用元组作为参数
fn print(x:(i32,bool,f64)){
   println!("Inside print method");
   println!("{:?}",x);
}
```

编译运行以上 Rust 代码，输出结果如下

```
Inside print method
(110, true, 10.9)
```

### 元组解构赋值 ( destructing ) =

解构赋值 ( destructing ) 就是把元组 ( tuple ) 中的每一个元素按照顺序一个一个赋值给变量。

### 元组解构赋值 ( destructing ) 语法格式

元组解构赋值 ( destructing ) 的语法格式如下

```
(age,is_male,cgpa) = (30,true,7.9);
```

上面这种赋值操作称之为 元组解构赋值，它会把等号 ( = ) 右边的元组的元素按照顺序一个一个赋值给等号左边元组里的变量。

赋值完成后，左边的各个变量的值为

```
age      = 30;
is_male  = true;
cgpa     = 7.9;
```

解构 操作是 Rust 语言的一个特性，最新的 JavaScript 语言也有解构操作。

### 范例
```
fn main(){
   let b:(i32,bool,f64) = (30,true,7.9);
   print(b);
}
fn print(x:(i32,bool,f64)){
   println!("Inside print method");
   let (age,is_male,cgpa) = x; //assigns a tuple to 
   distinct variables
   println!("Age is {} , isMale? {},cgpa is 
   {}",age,is_male,cgpa);
}
```

编译运行以上 Rust 代码，输出结果如下

```
Inside print method
Age is 30 , isMale? true,cgpa is 7.9
```