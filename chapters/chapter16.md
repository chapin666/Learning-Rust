# 十六、Rust 切片 Slice

一个 **切片（slice）** 就是指向一段内存的指针。

因此切片可用于访问内存块中连续区间内的数据。

一般情况下，能够在内存中连续区间存储数据的数据结构有: 数组 array、向量 vector、字符串 string。

也就是说，**切片** 可以和数组、向量、字符串一起使用，它使用 数字索引 （类似于数组的下标索引）来访问它所指向的数据。

例如，**切片** 可以和字符串一起使用，切片 可以指向字符串中连续的一部分。这种 字符串切片 实际上是指向字符串的指针。因为是指向字符串的连续区间，所以我们要指定字符串的开始和结束位置。

访问切片内容的时候，下标索引是从 0 开始的。

**切片** 的大小是运行时才可知的，并不是数组那种编译时就必须告知的。

## 16.1 定义切片的语法

经过上面晦涩难懂的解释，我们知道切片就是指向数组、向量、字符串的连续区间。

定义一个切片的语法格式如下

```
let sliced_value = &data_structure[start_index..end_index]
```

定义切片时的几个注意事项：

1. [start_index..end_index] 是一个左闭又开区间 [start_index,end_index)

2. 要注意区间的语法两个点 .. 。

3. start_index 的最小值为 0，这是因为数组、向量、字符串它们的下标访问方式也是从 0 开始的。

4. end_index 的最大值是数组、向量、字符串的长度。

5. end_index 所表示的索引的字符并不包含在切片里面。

## 16.2 切片图例

数组、向量、字符串在内存中是连续存储的，一个字符串 Tutorials 在内存中的存储类似于下图

[String Tutorials](/images/string_tutorials.jpg)

从图中很直观的可以看出，字符串 Tutorials 的长度为 9，第一个字符的下标索引为 0，最后一个字符的下标索引为 8。

如果我们想访问字符串 Tutorials 中第 4 个字符开始的连续 5 个字符，使用切片，我们可以这么做

```
fn main() {
   let n1 = "Tutorials".to_string();
   println!("length of string is {}",n1.len());
   let c1 = &n1[4..9]; 

   // fetches characters at 4,5,6,7, and 8 indexes
   println!("{}",c1);
}
```

编译运行以上 Rust 代码，输出结果如下

```
length of string is 9
rials
```

## 16.3 切片作为函数参数

切片还可以作为函数的参数。

使用切片可以把数组、向量、字符串中的连续子集通过引用的方式传递给函数。

我们先来看一段简单的代码

```
fn main(){
   let data = [10,20,30,40,50];
   use_slice(&data[1..4]);
   //this is effectively borrowing elements for a while
}
fn use_slice(slice:&[i32]) { 
   // is taking a slice or borrowing a part of an array of i32s
   println!("length of slice is {:?}",slice.len());
   println!("{:?}",slice);
}
```

编译运行以上 Rust 代码，输出结果如下

```
length of slice is 3
[20, 30, 40]
```

上面这段代码中

1. 声明了两个函数 main() 和 use_slice()，后者接受一个切片并打印切片的长度。
2. 首先在 main() 函数中声明了一个有 5 个元素的数组。
3. 然后调用函数 use_slice() 并把数组的一个切片（ 从下标 1 开始到下标 3 之间的元素 ）作为参数。

## 16.4 可变更切片

默认情况下 **切片** 是不可变更的。

虽然，看起来切片是指向原数据，但是默认情况下我们并不能改变切片的元素。

也就说默认情况下不能通过更改切片的元素来影响原数据。

但这不是绝对的，如果我们声明的原数据是可变的，同时定义切片的时候添加了 &mut 关键字，那么我们就可以通过更改切片的元素来影响原数据。

```
fn main(){
   let mut data = [10,20,30,40,50];
   use_slice(&mut data[1..4]);
   // passes references of 
   20, 30 and 40
   println!("{:?}",data);
}
fn use_slice(slice:&mut [i32]) {
   println!("切片的长度为：{:?}",slice.len());
   println!("{:?}",slice);
   slice[0] = 1010; // replaces 20 with 1010
}
```

编译运行以上 Rust 代码，输出结果如下

```
切片的长度为：3
[20, 30, 40]
[10, 1010, 30, 40, 50]
```

从上面的代码中可以看出，只要原数据是可变的，且切片声明时添加了 &mut 关键字，那么切片就是可变更的。