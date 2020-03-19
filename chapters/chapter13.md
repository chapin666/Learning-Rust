# Rust 数组

虽然我们看到的绝大多数变量都是基本数据类型。虽然这些基本数据类型的能够满足大部分的工作，但它们不是万能的。

基本数据类型的变量也有它们的局限性。

- 基本数据类型的变量本质上是 标量。这意味着每个基本数据类型的变量一次只能存储一个值。
因此，当我们需要存储多个值的时候，我们不得不重复定义多个变量。比如 a1、a2、a3 ....
如果我们要存储的值非常多，成百上千，这种重复定义变量的方法是行不通的。

- 基本数据类型的变量的值在内存中的位置是随机的。多个按照顺序定义的变量，它们在内存中的存储位置不一定是连续的。因此我们可能按照变量的声明顺序来获取它们的值。

数组 是用来存储一系列数据，但它往往被认为是一系列相同类型的变量，也就是说，数组 是可以存储一个固定大小的相同类型元素的顺序集合。

数组的声明并不是声明一个个单独的变量，比如 number0、number1、...、number99，而是声明一个数组变量，比如 numbers，然后使用 numbers[0]、numbers[1]、...、numbers[99] 来代表一个个单独的变量。 数组中的特定元素可以通过索引访问

数组可以理解为相同数据类型的值的集合。

## 数组的特性

- 数组的定义其实就是为分配一段 **连续的相同数据类型** 的内存块。

- 数组是静态的。这意味着一旦定义和初始化，则永远不可更改它的长度。

- 数组的元素有着相同的数据类型，每一个元素都独占者数据类型大小的内存块。
也就是说。数组的内存大小等于数组的长度乘以数组的数据类型。

- 数组中的每一个元素都按照顺序依次存储，这个顺序号既代表着元素的存储位置，也是数组元素的唯一标识。我们把这个标识称之为 数组下标 。
注意，数组下标从 0 开始。

- 填充数组中的每一个元素的过程称为 **数组初始化**。也就是说 数组初始化 就是为数组中的每一个元素赋值。

- 可以更新或修改数组元素的值，但不能删除数组元素。如果要删除功能，你可以将它的值赋值为 0 或其它表示删除的值。

## 声明和初始化数组

Rust 语言为数组的声明和初始化提供了 3 中语法

1. 最基本的语法，指定每一个元素的初始值

```
let variable_name:[dataType;size] = [value1,value2,value3];
```

例如

```
let arr:[i32;4] = [10,20,30,40];
```

2. 省略数组类型的语法

因为指定了每一个元素的初始值，所以可以从初始值中推断出数组的类型

```
let variable_name = [value1,value2,value3];
```

例如

```
let arr = [10,20,30,40];
```

指定默认初始值的语法，这种语法有时候称为 默认值初始化。

如果不想为每一个元素指定初始值，则可以为所有元素指定一个默认的初始值。

```
let variable_name:[dataType;size] = [default_value_for_elements,size];
```

例如下面的代码为每一个元素指定初始值为 -1

```
let arr:[i32;4] = [-1;4];
```

### 数组初始化：简单的数组

数组初始化的语法一般如下

```
let variable_name = [value1,value2,value3];
```

这种一种最基本的初始化方法，也是字符最长的初始化方法，除了明确指定了数组的类型外，还未每一个数组元素指定了初始值。

数组是一个复合类型，因此输出数组的时候需要使用 **{:?}** 格式符。

Rust 还提供了 len() 方法则用于返回数组的长度，也就是元素的格式。

```
fn main(){
   let arr:[i32;4] = [10,20,30,40];
   println!("array is {:?}",arr);
   println!("array size is :{}",arr.len());
}
```

编译运行以上 Rust 代码，输出结果如下

```
array is [10, 20, 30, 40]
array size is :4
```

### 数组初始化：忽略元素数据类型

数组初始化时如果为每一个元素都指定了它的初始值，那么在定义数组时可以忽略数组的数据类型。

因为这时候，编译器可以通过元素的数据类型自动推断出数组的数据类型。

例如下面的代码，我们的数组长度为 4，因为初始化的时候为 4 个元素都指定了初始值为整型，那么声明数组变量的时候就可以忽略数组的数据类型。

数组的 len() 函数用于返回数组的长度。

```
fn main(){
   let arr = [10,20,30,40];
   println!("array is {:?}",arr);
   println!("array size is :{}",arr.len());
}
```

编译运行以上 Rust 范例，输出结果如下

```
array is [10, 20, 30, 40]
array size is :4
```

## 数组默认值

在数组初始化时，如果不想为数组中的每个元素指定值，我们可以为数组设置一个默认值，也就是使用 默认值初始化 语法。

当使用 默认值初始化 语法初始化数组时，数组中的每一个元素都被设置为默认值。

例如下面的代码，我们将数组中所有的元素的值初始化为 -1

```
fn main() {
   let arr:[i32;4] = [-1;4];
   println!("array is {:?}",arr);
   println!("array size is :{}",arr.len());
}
```

编译运行以上 Rust 代码，输出结果如下

```
array is [-1, -1, -1, -1]
array size is :4
```

## 数组长度 len()

Rust 为数组提供了 **len()** 方法用于返回数组的长度。

len() 方法的返回值是一个整型。

例如下面的代码，我们使用 len() 求数组的长度

```
fn main() {
   let arr:[i32;4] = [-1;4];
   println!("array size is :{}",arr.len());
}
```

编译运行以上 Rust 代码，输出结果如下

```
array size is :4
```

## for in 循环遍历数组

在其它语言中，一般使用 for 循环来遍历数组，Rust 语言也可以，只不过时使用 for 语句的变种 for ... in .. 语句。

因为数组的长度在编译时就时已知的，因此我们可以使用 for ... in 语句来遍历数组。

注意 for in 语法中的左闭右开法则。

```
fn main(){
   let arr:[i32;4] = [10,20,30,40];
   println!("array is {:?}",arr);
   println!("array size is :{}",arr.len());

   for index in 0..4 {
      println!("index is: {} & value is : {}",index,arr[index]);
   }
}
```

编译运行以上 Rust 代码，输出结果如下

```
array is [10, 20, 30, 40]
array size is :4
index is: 0 & value is : 10
index is: 1 & value is : 20
index is: 2 & value is : 30
index is: 3 & value is : 40
```

## 迭代数组 iter()

我们可以使用 **iter()** 函数为数组生成一个迭代器。

然后就可以使用 for in 语法来迭代数组。
```
fn main(){

   let arr:[i32;4] = [10,20,30,40];
   println!("array is {:?}",arr);
   println!("array size is :{}",arr.len());

   for val in arr.iter(){
      println!("value is :{}",val);
   }
}
```

编译运行以上 Rust 代码，输出结果如下

```
array is [10, 20, 30, 40]
array size is :4
value is :10
value is :20
value is :30
value is :40
```

## 可变数组

使用 let 声明的变量，默认是只读的，数组也不例外。也就是说，默认情况下，数组是不可变的。

```
fn main(){
   let arr:[i32;4] = [10,20,30,40];
   arr[1] = 0;
   println!("{:?}",arr);
}
```

上面的代码运行会出错，错误信息如下

```
error[E0594]: cannot assign to `arr[_]`, as `arr` is not declared as mutable
 --> src/main.rs:3:4
  |
2 |    let arr:[i32;4] = [10,20,30,40];
  |        --- help: consider changing this to be mutable: `mut arr`
3 |    arr[1] = 0;
  |    ^^^^^^^^^^ cannot assign

error: aborting due to previous error
```

数组的不可变，表现为两种形式：变量不可重新赋值为其它数组、数组的元素不可以修改。

如果要让数组的元素可以修改，就需要添加 mut 关键字。例如

```
let mut arr:[i32;4] = [10,20,30,40];
```

我们将刚刚错误的代码修改下

```
fn main(){
   let mut arr:[i32;4] = [10,20,30,40];
   arr[1] = 0;
   println!("{:?}",arr);
}
```

就可以正常运行，输出结果如下

```
[10, 0, 30, 40]
```

## 数组作为函数参数

数组可以作为函数的参数。而传递方式有 **传值传递** 和 **引用传递** 两种方式。

**传值传递** 就是传递数组的一个副本给函数做参数，函数对副本的任何修改都不会影响到原来的数组。

**引用传递** 就是传递数组在内存上的位置给函数做参数，因此函数对数组的任何修改都会影响到原来的数组。

### 范例：传值传递

下面的代码，我们使用传值方式将数组传递给函数做参数。函数对参数的任何修改都不会影响到原来的数组。

```
fn main() {
   let arr = [10,20,30];
   update(arr);

   println!("Inside main {:?}",arr);
}
fn update(mut arr:[i32;3]){
   for i in 0..3 {
      arr[i] = 0;
   }
   println!("Inside update {:?}",arr);
}
```

编译运行以上 Rust 代码，输出结果如下

```
Inside update [0, 0, 0]
Inside main [10, 20, 30]
```

### 范例2: 引用传递

下面的代码，我们使用引用方式将数组传递给函数做参数。函数对参数的任何修改都会影响到原来的数组

```
fn main() {
   let mut arr = [10,20,30];
   update(&mut arr);
   println!("Inside main {:?}",arr);
}
fn update(arr:&mut [i32;3]){
   for i in 0..3 {
      arr[i] = 0;
   }
   println!("Inside update {:?}",arr);
}
```

编译运行以上 Rust 代码，输出结果如下

```
Inside update [0, 0, 0]
Inside main [0, 0, 0]
```

## 数组声明和常量

数组 ( array ) 有一个唯一的弱点，它的长度必须在编译时就是固定的已知的。

声明数组时长度必须指定为整数字面量或者整数常量。

如果数组长度是一个变量，则会报编译错误。例如下面的代码

```
fn main() {
   let N: usize = 20;
   let arr = [0; N]; //错误: non-constant used with constant
   print!("{}",arr[10])
}
```

编译上面的 Rust 代码报错

```
error[E0435]: attempt to use a non-constant value in a constant
 --> main.rs:3:18
  |
3 |    let arr = [0; N]; //错误: non-constant used with constant
  |                  ^ non-constant value

error: aborting due to previous error

For more information about this error, try `rustc --explain E0435`.
```

报错的原因是： N 不是一个常量。

注意，虽然 N 默认是只读的，但它仍然是一个变量，只不过是一个只读变量而已，只读变量不是常量。因为变量的值是在运行时确定的，而常量的值是在编译器确定的。

变量不可用做数组的长度。

如果我们将 let 关键字修改为 const 关键字，编译就能通过了。

```
fn main() {
   const N: usize = 20; 
   // 固定大小
   let arr = [0; N];

   print!("{}",arr[10])
}
```

编译运行以上 Rust 代码，输出结果如下

```
0
```
usize 是一个指针所占用的大小。它的实际大小取决于你编译程序的 cpu 体系结构。