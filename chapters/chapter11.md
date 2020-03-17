# Rust 函数 fn

函数是一组一起执行一个任务的语句块。

函数是一段可读，可维护和可重用代码的多条语句。

每个 Rust 程序都至少有一个函数，即主函数 main()。

除了使用 Rust 核心和标准库提供的函数外，我们还可以自己定义自己的函数。

### 划分代码到函数中

我们可以把代码划分到不同的函数中，这样可以使得代码可读性更强，逻辑更简单。

虽然划分代码到不同的函数中没有一个统一的规范，但实践证明，在逻辑上，划分的标准是每个函数执行一个特定的任务的

函数声明就是告诉编译器一个函数的名称、变量、和返回值类型。这三个合在一起组成了函数的签名，函数签名的作用就是防止出现两个相同的函数。

函数定义，就是提供了函数的具体实现

|术语 |	说明|
|--|--|
|函数定义|	函数定义其实就是定义一个任务要以什么方式来完成|
|函数调用|	函数只有被调用才会运行起来|
|函数返回值|	函数在执行完成后可以返回一个值给它的调用者|
|函数参数|	函数参数用于携带外部的值给函数内部的代码|

## 函数定义

函数定义其实就是定义一个任务要以什么方式来完成。

因此，定义函数时首先想的并不是我要定义一个函数，而是我这个任务要怎么做，要定义哪些函数来完成。

函数也不会凭空出现的，在使用一个函数前，我们必须先定义它。

定义函数时必须以 fn 关键字开头，fn 关键字是 function 的缩写。

函数内部必须包含函数要执行的具体代码，我们把这些代码称之为 函数体。

函数名称的命名规则和变量的命名规则一致。

### 定义函数的语法

定义函数的语法如下，定义函数时必须使用 fn 关键字开头，后面跟着要定义的函数名。

```
fn function_name([param1:data_type1,param2..paramN]) {
   // 函数代码
}
```

参数用于将值传递给函数内部的语句。函数定义时可以自由选择包含参数与否。

### 范例：定义一个简单的函数

下面的代码，我们定义了一个函数名为 fn_hello 的函数，用于输出一些信息

```
// 函数定义
fn fn_hello(){
   println!("hello from function fn_hello ");
}
```

## 函数调用

为了运行一个函数首先必须调用它。函数不像普通的语句，写完了会自动执行，函数需要调用才会被执行。否则看起来就像是多余的没有用的代码。

让函数运行起来的过程我们称之为 函数调用。

如果函数定义了参数，那么在 函数调用 时必须传递指定类型的参数。

在一个函数 fn1 内部调用其它函数 fn2，那么这个 fn1 函数就称为 调用者函数，简称为 调用者。

调用者函数有点拗口，我们一般都称呼为 函数调用者。

### 函数调用的语法格式

```
function_name(val1,val2,valN)
```

例如我们在 main() 函数内部调用函数 fn_hello()

```
fn main(){
   //调用函数
   fn_hello();
}
```

这时候，函数 main() 就是 调用者函数，也就是 调用者。

### 范例

下面的代码，我们定义了一个函数 fn_hello() 用于输出一些信息。 然后我们在 main() 函数对 fn_hello() 进行调用

```
fn main(){
   // 调用函数
   fn_hello();
}


// 定义函数
fn fn_hello(){
   println!("hello from function fn_hello ");
}
```

编译运行以上 Rust 代码，输出结果如下

```
hello from function fn_hello
```

## 函数返回值

函数可以返回值给它的调用者。我们将这些值称为 函数返回值。

也就是说，函数在代码执行完成后，除了将控制权还给调用者之外，还可以携带值给它的调用者。

如果一个函数需要返回值给它的调用者，那么我们在函数定义时就需要明确中函数能够返回什么类型的值。

### 函数返回值语法格式

Rust 语言的返回值定义语法与其它语言有所不同，它是通过在 小括号后面使用 箭头 ( -> ) 加上数据类型 来定义的。

同时在函数的代码中，可以使用 return 关键字指定要返回的值。

如果函数代码中没有使用 return 关键字，那么函数会默认使用最后一条语句的执行结果作为返回值。

因此，千万注意，return 中返回的值或最后一条语句的执行结果 必须和函数定义时的返回数据类型一样，不然会编译会出错

具有返回值的函数的完整定义语法如下

1. 有 return 语句

```
function function_name() -> return_type {
   // 其它代码

   // 返回一个值
   return value;
}
```

2. 没有 return 语句则使用最后一条语句的结果作为返回值

函数中最后用于返回值的语句不能有 分号 ; 结尾，否则就不会时返回值了。

function function_name() -> return_type {
   // 其它代码

   value // 没有分号则表示返回值
}

### 范例

下面的代码，我们定义了两个相同功能的 get_pi() 和 get_pi2() 函数，一个使用 return 语句返回值，另一个则使用没有分号的最后一条语句作为返回值。

```
fn main(){
   println!("pi value is {}",get_pi());
   println!("pi value is {}",get_pi2());
}

fn get_pi()->f64 {
   22.0/7.0
}

fn get_pi2()->f64 {
   return 22.0/7.0;
}
```

编译运行以上 Rust 代码，输出结果如下

```
pi value is 3.142857142857143
pi value is 3.142857142857143
```

### 范例 2

我们修改下上面的代码，在没有 return 的那个函数的最后一条语句添加一个分号，看看执行结果如何

```
fn main(){
   println!("pi value is {}",get_pi());
}

fn get_pi()->f64 {
   22.0/7.0;
}
```

编译运行上面的代码，会报错，错误信息如下

```
error[E0308]: mismatched types
 --> src/main.rs:5:14
  |
5 | fn get_pi()->f64 {
  |    ------    ^^^ expected f64, found ()
  |    |
  |    this function's body doesn't return
6 |    22.0/7.0;
  |            - help: consider removing this semicolon
  |
  = note: expected type `f64`
             found type `()`
```

从错误信息中可以看出，函数定义了返回值，但我们却没有返回值。也就是说，函数的返回值必须没有 分号 结尾。

### 范例： 函数返回值接收变量

我们还可以将函数的返回值赋值给一个变量。

例如下面的代码，我们定义了变量 pi 来接收函数的返回值

```
fn main(){
   let pi = get_pi();
   println!("pi value is {}",pi);
}

fn get_pi()->f64 {
   22.0/7.0
}
```

编译运行以上 Rust 代码，输出结果如下

```
pi value is 3.142857142857143
```

## 函数参数

函数参数 是一种将外部变量和值带给函数内部代码的一种机制。函数参数是函数签名的一部分。

函数签名的最大作用，就是防止定义两个相同的签名的函数。

当一个函数定义了函数参数，那么在调用该函数的之后就可以把变量/值传递给函数。

我们把函数定义时指定的参数名叫做 形参。同时把调用函数时传递给函数的变量/值叫做 实参。

除非特别指定，函数调用时传递的 实参 数量和类型必须与 形参 数量和类型必须相同。 否则会编译错误。

函数参数有两种传值方法，一种是把值直接传递给函数，另一种是把值在内存上的保存位置传递给函数。

### 传值调用

传值调用 就是简单的把传递的变量的值传递给函数的 形参，从某些方面说了，就是把函数参数也赋值为传递的值。
因为是赋值，所以函数参数和传递的变量其实是各自保存了相同的值，互不影响。因此函数内部修改函数参数的值并不会影响外部变量的值。

### 范例

我们定义了一个函数 mutate_no_to_zero()，它接受一个参数并将参数重新赋值为 0 。

我们还定义了一个变量 no 并初始化它的值为 5。然后将该变量传递给函数 mutate_no_to_zero()。

虽然我们在函数中将变量的值改成了 0，但当调用完成后，我们的 no 变量的值仍然是 5。

这是因为传值调用传递的是变量的一个副本，也就是重新创建了一个变量。

```
fn main(){
   let no:i32 = 5;
   mutate_no_to_zero(no);
   println!("The value of no is:{}",no);
}

fn mutate_no_to_zero(mut param_no: i32) {
   param_no = param_no*0;
   println!("param_no value is :{}",param_no);
}
```

编译运行以上 Rust 代码，输出结果如下

```
param_no value is :0
The value of no is:5
```

### 引用调用

值传递变量导致重新创建一个变量。但引用传递则不会，引用传递把当前变量的内存位置传递给函数。

对于引用传递来说，传递的变量和函数参数都共同指向了同一个内存位置。

引用传递需要函数定义时在参数类型的前面加上 & 符号，语法格式如下

```
fn function_name(parameter: &data_type) {
   // 函数的具体代码
}
```

### 范例

我们对刚刚传值调用的代码做一些修改。

我们定义了一个函数 mutate_no_to_zero()，它接受一个可变引用作为参数，并把传递的引用变量重新赋值为 0 。

同时定义了一个 可变变量 no 并初始化它的值为 5。然后将该变量的一个引用传递给函数 mutate_no_to_zero()。

当函数执行完成后，可变变量 no 的值就变成 0 了。

```
fn main() {
   let mut no:i32 = 5;
   mutate_no_to_zero(&mut no);
   println!("The value of no is:{}",no);
}
fn mutate_no_to_zero(param_no:&mut i32){
   *param_no = 0; //解引用操作
}
```

编译运行以上 Rust 代码，输出结果如下

```
The value of no is 0.
```

上面的代码中，星号（*） 用于访问变量 param_no 指向的内存位置上存储的变量的值。这种操作也称为 解引用。 因此 星号（*） 也称为 解引用操作符。

## 传递复合类型给函数做参数

对于复合类型，比如字符串，如果按照普通的方法传递给函数后，那么该变量将不可再访问

例如下面的代码编译会报错

```
fn main(){
   let name:String = String::from("TutorialsPoint");
   display(name); 
   println!("after function name is: {}",name);
}

fn display(param_name:String){
   println!("param_name value is :{}",param_name);
}
```

编译上面的代码会出错，错误信息如下

```
error[E0382]: borrow of moved value: `name`
 --> src/main.rs:4:42
  |
2 |    let name:String = String::from("TutorialsPoint");
  |        ---- move occurs because `name` has type `std::string::String`, which does not implement the `Copy` trait
3 |    display(name); 
  |            ---- value moved here
4 |    println!("after function name is: {}",name);
  |                                          ^^^^ value borrowed here after move
```

修复的方法之一就是去掉后面的 println!() 语句

```
fn main(){
   let name:String = String::from("TutorialsPoint");
   display(name);
}

fn display(param_name:String){
   println!("param_name value is :{}",param_name);
}
```

编译运行以上 Rust 代码，输出结果如下

```
param_name value is :TutorialsPoint
````

后面的章节，我们会讨论如何解决这个问题，本章节这不是重点。