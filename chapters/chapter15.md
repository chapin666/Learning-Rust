# Rust 借用 Borrowing

上一章节我们学习了 **所有权 （ ownership ）** 这个改变，知道了在 **堆（ heap ）**上分配的变量都有所有权。

上一章节，我们也以一种艰难的方式将具有所有权的变量，比如字符串变量、向量变量作为参数传递给函数，同时为了保证函数调用之后变量仍然具有所有权，又在函数内返回变量。

这样的过程，不但没有减轻我们的负担，反而觉得越来越难以使用的感觉。

使用的过程中，我就一直在想，为什么不多支持一个 **借用所有权** 或者 **租借所有权** 的概念呢 ？

把具有所有权的变量传递给函数作为参数时，就是临时出租所有权，当函数执行完后就会自动收回所有权。就像现实生活中，我可以把某个工具临时借用给其它人，当他们使用完了之后还给我们就可以了。

随着对 Rust 的深入了解，觉得 Rust 语言的开发者也是不笨的，他们也想到了 借用所有权 这个概念。

Rust 支持对所有权的 **出借 borrowing**。当把一个具有所有权的变量传递给函数时，就是把所有权借用给函数的参数，当函数返回后则自动收回所有权。

下面的代码，我们并没有使用上一章节的 所有权 转让规则收回所有权，所以程序会报错

```
fn main(){

    let v = vec![10,20,30]; // 声明一个向量，变量 v 具有数据的所有权
    print_vector(v);
    println!("{}",v[0]);    // 这行会报错
}

fn print_vector(x:Vec<i32>){
    println!("Inside print_vector function {:?}",x);
}
```

上面这段代码中，我们定义了两个函数 main() 和 print_vector() ，前者是应用程序的入口函数，而后者则用于输出一个 向量。

我们在 main() 函数中定义了一个向量，同时将这个向量传递给 print_vector() 作为参数。 因为参数的传递会触发所有权的转让。因此将 v 传递给 print_vector() 函数时，数据的所有权就从 v 转让到了 参数 x 上。

但函数返回时我们并没有把 x 对数据的所有权转让回 v 变量，因此上面这段代码编译的时候编译的时候就会报错了。

```
error[E0382]: borrow of moved value: `v`
 --> src/main.rs:5:18
  |
3 |    let v = vec![10,20,30]; // 声明一个向量，变量 v 具有数据的所有权
  |        - move occurs because `v` has type `std::vec::Vec<i32>`, which does not implement the `Copy` trait
4 |    print_vector(v);
  |                 - value moved here
5 |    println!("{}",v[0]);    // 这行会报错
  |                  ^ value borrowed here after move

error: aborting due to previous error
```

重复的讲解这个例子，并不是为了凑字数，而是我们会有一种更好的解决方案，这个方案只要修改一点点就能让程序运行。

## 什么是借用 Borrowing ?

借用 Borrowing 或者说 出借 应该不用我再详细解释了吧，很简单的，就是 临时性的把东西借给别人，当别人用完了之后就要还回来。

这里的重点有两个字： 借 和 还。

- 借： 把东西借给他人后，自己就暂时性的失去了东西的所有权（现实中是失去了使用权）。
- 还： 借了别人的东西要主动还，这应该养成一个良好的习惯，如果不还，就是 占为己有 了。

了解了 借用、借、还 的概念后，对 Rust 语言的 **借用 Borrowing** 概念就很清晰了。

Rust 语言中，借用 就是一个函数中将一个变量传递给另一个函数作为参数暂时使用。

同时，Rust 也引用了自动 还 的概念，就是要求函数的参数离开其作用域时需要将 所有权 还给当初传递给他的变量，这个过程，我们需要将函数的参数定义为 &variable_name，同时传递参数时，需要传递 &variable_name。

站在 C++ 语言的角度考虑，就是将 函数的参数定义为引用，同时传递变量的引用。

有了 借用 Borrowing 也就是引用的概念后，我们只要修改两处就能让上面的代码运行起来

```
fn print_vector(x:&Vec<i32>){  // 1. 第一步，定义参数接受一个引用
    println!("Inside print_vector function {:?}",x);
}

fn main(){

    let v = vec![10,20,30];     // 声明一个向量，变量 v 具有数据的所有权
    print_vector(&v);           // 第二步，传递变量的引用给函数
    println!("{}",v[0]);        // 这行会报错
}
```

编译运行以上 Rust 代码，输出结果如下

```
Inside print_vector function [10, 20, 30]
Printing the value from main() v[0] = 10
```

## 可变引用

借用 Borrowing 或者说引用默认情况下是只读的，也就是我们不能修改引用的的变量的值。

例如下面的代码

```
fn add_one(e: &i32) {
   *e+= 1;
}

fn main() {
   let i = 3;
   println!("before {}",i);
   add_one(&i);
   println!("after {}", i);
}
```

编译运行以上 Rust 代码，输出结果如下

```
error[E0594]: cannot assign to `*e` which is behind a `&` reference
 --> src/main.rs:2:4
  |
1 | fn add_one(e: &i32) {
  |               ---- help: consider changing this to be a mutable reference: `&mut i32`
2 |    *e+= 1;
  |    ^^^^^^ `e` is a `&` reference, so the data it refers to cannot be written

error: aborting due to previous error
```

我们尝试在函数 add_one() 将引用的变量 +1 但却编译失败了。

而失败的原因，就像错误信息里说的那样，引用 默认情况下是不可编辑的。

Rust 中，要让一个变量可编辑，唯一的方式就是给他加上 mut 关键字。

因此，我们可以将上面的代码改造下，改成下面这样

```
fn add_one(e: &mut i32) {
   *e+= 1;
}

fn main() {
   let mut i = 3;
   println!("before {}",i);
   add_one(&mut i);
   println!("after {}", i);
}
```

编译运行以上 Rust 代码，输出结果如下

```
before 3
after 4
```

从上面的代码中可以看出： 借用 Borrowing 或者说引用的变量如果要变更，必须符合满足三个要求：

1. 变量本身是可变更的，也就是定义时必须添加 mut 关键字。
2. 函数的参数也必须定义为可变更的，加上 借用 Borrowing 或者说引用，也就是必须添加 &mut 关键字。
3. 传递 借用 Borrowing 或者说引用也必须声明时 可变更传递，也就是传递参数时必须添加 &mut 关键字。

以上三个条件，任意一个不满足，都会报错。比如第三个条件不满足时

```
fn add_one(e: &mut i32) {
   *e+= 1;
}

fn main() {
   let mut i = 3;
   println!("before {}",i);
   add_one(& i);
   println!("after {}", i);
}
```

报错信息如下

```
error[E0308]: mismatched types
 --> src/main.rs:8:12
  |
8 |    add_one(& i);
  |            ^^^ types differ in mutability
  |
  = note: expected type `&mut i32`
             found type `&{integer}`

error: aborting due to previous error
```
> 注意: 可变引用只能操作可变变量

### 范例：字符串的可变引用

上面的范例，我们操作的是基础的数据类型，如果是堆上分配的变量又会怎么样呢 ？ 比如字符串。

其实堆上分配的变量的 借用 Borrowing 或者说引用跟基础类型的变量一样，我们来看一段代码

```
fn main() {
   let mut name:String = String::from("TutorialsPoint");
   display(&mut name);  // 传递一个可变引用
   println!("The value of name after modification is:{}",name);
}

fn display(param_name:&mut String){
   println!("param_name value is :{}",param_name);
   param_name.push_str(" Rocks"); // 修改字符串，追加一些字符
}
```

编译运行以上 Rust 代码，输出结果如下

```
param_name value is :TutorialsPoint
The value of name after modification is:TutorialsPoint Rocks
```

