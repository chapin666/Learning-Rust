# Rust 智能指针

Rust 是一门系统级的语言，至少，它自己是这么定位的，它所对标的语言是 C++。

作为系统级别的语言，抛弃指针是完全不现实的，提供有限的指针功能还是能够做得到的。

Rust 语言是一门现代的语言。一门现代语言会尽可能的抛弃指针，也就是默认会把所有的数据都存储在 栈 上。

如果要把数据存储在 **堆** 上，就要在 **堆** 上开辟内存，这时候就要使用到 **指针**。

作为系统级的语言， Rust 提供了在 堆 上存储数据的能力。只不过它把这些能力弱化并封装到了 **Box** 中。

这种把 **栈** 上数据搬到 **堆** 上的能力，我们称之为 **装箱**。

Rust 语言中的某些类型，如 **向量 Vector** 和 **字符串对象 String** 默认就是把数据存储在 **堆** 上的。

Rust 语言把指针封装为以下两大 **特质 trait**。当一个结构体实现了下面的接口后，它们就不再是普通的结构体了。

|特质名|	包|	Description|
|--|--|--|
|Deref|	std::ops::Deref|	用于创建一个只读智能指针，例如 *v|
|Drop|	std::ops::Drop|	智能指针超出它的作用域范围时会回调该特质的 **drop()** 方法，类似于其它语言的 析构函数|

本章节，我们就来学习 Rust 中那功能少的可怜的智能指针，准确的说是学习 **Box** 这个智能指针装箱器。

## Box 指针
Box 指针也称之 **装箱（ box ）**，允许我们将数组存储在 **堆 （ heap ）** 上而不是 **栈（ stack ）** 上。

但即使把数据存储在 **堆 （ heap ）** 上，**栈（ stack ）** 仍然包含了指向**堆数据的指针**。

**Box** 指针没有任何额外的其它开销，因为它仅仅只是把数据存储在 **堆 （ heap ）** 而已。

说起来很拗口，我们直接就看代码

```
fn main() {

   let var_i32 = 5;           // 默认数组保存在 栈 上
   let b = Box::new(var_i32); // 使用 Box 后数据会存储在堆上
   println!("b = {}", b);

}
```
编译运行上面的 Rust 代码，输出结果如下

```
b = 5
```

### 访问 Box 指针存储的数据

当我们使用 Box::new() 把一个数据存储在堆上之后，为了访问存储的具体数据，我们必须 **解引用**。

**解引用** 需要使用操作符 **星号**，因此 **星号** 也称之为 **解引用操作符**。

这一点和 C++ 一样的。

下面这段代码，为了访问数据 y，我们需要使用 *y 。

```
fn main() {
   let x = 5;           // 值类型数据
   let y = Box::new(x); // y 是一个智能指针，指向堆上存储的数据 5 

   println!("{}",5==x);
   println!("{}",5==*y); // 为了访问 y 存储的具体数据，需要解引用
}
```

编译运行上面的 Rust 代码，输出结果如下

```
true
true
```

上面的代码中，因为 5 是一个基础数据类型，所以当使用 5 == x 的时候会返回 true，因为基础类型只会比较值相同与否。

而另一个变量 y，它是一个智能指针，是一个引用类型，直接使用 5 == y 会返回 false。为了访问 y 指向的具体的值，我们需要对 y 解引用。

### Deref Trait

Deref 是由 Rust 标准库提供的一个 **特质 ( trait )**。

实现 Deref 特质需要我们实现 deref() 方法。

deref() 方法从某些方面说用于借用 self 对象并返回一个指向内部数据的指针。

也就是说 deref() 方法返回一个指向结构体内部数据的指针。

### 范例

下面的代码有点长，我们的范型结构体 **MyBox** 实现了 **Deref** 特质。

我们可以通过 **dedef()** 方法返回的结构体实例的引用来访问 **堆 heap** 上的数据。

```
use std::ops::Deref;

struct MyBox<T>(T);

impl<T> MyBox<T> { 
   // 范型方法
   fn new(x:T)-> MyBox<T> {
      MyBox(x)
   }
}

impl<T> Deref for MyBox<T> {
   type Target = T;
   fn deref(&self) -> &T {
      &self.0 // 返回数据
   }
}

fn main() {
   let x = 5;
   let y = MyBox::new(x);  // 调用静态方法 new() 返回创建一个结构体实例

   println!("5==x is {}",5==x);
   println!("5==*y is {}",5==*y);  // 解引用 y
   println!("x==*y is {}",x==*y);  // 解引用 y
}
```

编译运行上面的 Rust 代码，输出结果如下

```
5==x is true
5==*y is true
x==*y is true
```

### 删除特质 Drop Trait

> Drop Trait 我将它翻译为 删除特质 ，但总感觉怪怪的。

> Drop Trait 翻译的有点坑爹，因为我不知道要如何翻译才能确切的表达那个意思。

**Drop Trait** 只有一个方法 **drop()**。

当实现了 Drop Trait 的结构体在离开了它的作用域范围时会触发调用 drop() 方法。

一些其它语言中，比如 C++，智能指针每次使用完了之后都必须手动释放相关内存或资源。

而在 Rust 语言中，我们可以把释放内存和资源的操作交给 Drop trait。

具体的，我们直接看代码就好

```
use std::ops::Deref;

struct MyBox<T>(T);

impl<T> MyBox<T> {
   fn new(x:T)->MyBox<T>{
      MyBox(x)
   }
}

impl<T> Deref for MyBox<T> {
   type Target = T;
      fn deref(&self) -< &T {
      &self.0
   }
}

impl<T> Drop for MyBox<T>{
   fn drop(&mut self){
      println!("dropping MyBox object from memory ");
   }
}
fn main() {
   let x = 50;
   MyBox::new(x);
   MyBox::new("Hello");
}
```

编译运行上面的 Rust 代码，输出结果如下

```
dropping MyBox object from memory
dropping MyBox object from memory
```

输出两次结果是因为我们在 **堆（ heap ）** 上创建了两个对象。