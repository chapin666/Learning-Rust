# Rust Collections

> collection 翻译成中文是 集合 的意思，set 翻译成中文也是集合的意思。这要如何区分啊？
> 在 V2EX 上问了下，马上就有好心人来告诉我，可以把 collection 翻译成 容器，谢谢了

Rust 语言的容器标准库提供了最常见的通用的数据结构的实现。包括 **向量（Vector）**、**哈希表（ HashMap ）**、**哈希集合（ HashSet ）** 等等。

Rust 容器库提供的数据结构没有 C++ 或 Java 那么多那么细致，但上面三个也足够使用了。

本章节我们将会详细的介绍上面提到的三个数据结构。

## 向量 Vector

前面的Rust数组章节中，我们有提到数组是相同数据类型的值的集合，但数组有一个缺点，就是它的长度是在编译时就确定的，一旦定义就永不可更改。

数组是各个语言所共通的，任何一个语言都不可能为了修复长度不可变这个 BUG 而改变数组长度不可变这个通识。

因此，急需要一个新的数据结构，它的元素布局方式和数组一样，但是长度可以在运行时随意变更。

也就是说，我们需要一个 **长度可变的数组**。于是，向量 Vector 就被提上日程了。

**向量** 是一个长度可变的数组。它和数组一样，在内存上开辟一段 **连续的内存块** 用于存储元素。

从某些方面说，**向量** 既有数组的特征，又有自己独有的特征：

- 向量的长度是可变的，可以在运行时增长或者缩短。

- 向量也是相同类型元素的集合。

- 向量以特定顺序（添加顺序）将数据存储为元素序列。

- 向量中的每个元素都分配有唯一的索引号。
索引从 0 开始并自增到 n-1，其中 n 是集合的大小。
例如集合有 5 个元素，那么第一个元素的下标是 0，最后一个元素的下标是 4。

- 元素添加到向量时会添加到向量的末尾。这个操作类似于 **栈 （ stack )**，因此可以用来实现 **栈** 的功能。

- 向量的内存在 **堆 ( heap )** 上存储，因此长度动态可变。

### 创建向量的语法

Rust 在标准库中定义了结构体 **Vec** 用于表示一个向量。同时提供了 **new()** 静态方法用于创建一个结构体 Vec 的实例。

因此，向量的创建语法格式如下

```
let mut instance_name = Vec::new();
```

除了提供 **new()** 静态方法创建向量之外， Rust 标准库还提供了 **vec!()** 宏来简化向量的创建。

```
let vector_name = vec![val1,val2,val3]
```

结构体 Vec 包含了大量的方法用于操作向量和向量中的元素，我们逻辑几个常见的于下表，并在后面做一个简单的介绍。

|方法|	签名|	说明|
|--|--|--|
|new()|	pub fn new()->Vec|	创建一个空的向量的实例|
|push()|	pub fn push(&mut self, value: T)|	将某个值 T 添加到向量的末尾|
|remove()|	pub fn remove(&mut self, index: usize) -> T|	删除并返回指定的下标元素。|
|contains()|	pub fn contains(&self, x: &T) -> bool|	判断向量是否包含某个值|
|len()|	pub fn len(&self) -> usize|	返回向量中的元素个数|

### 使用 **Vec::new()** 静态方法创建向量

创建向量的一般通过调用 Vec 结构的 **new()** 静态方法来创建。

当有了向量的一个实例后，再通过 **push()** 方法像向量添加元素

```
fn main() {
   let mut v = Vec::new();
   v.push(20);
   v.push(30);
   v.push(40);

   println!("size of vector is :{}",v.len());
   println!("{:?}",v);
}
```

运行以上 Rust 代码，输出结果如下

```
size of vector is :3
[20, 30, 40]
```

上面的代码中，我们使用结构体 **Vec** 提供的静态方法 **new()** 创建向量的一个实例。

有了向量时候之后，使用 **push(val)** 方法像实例添加元素。

**len()** 方法用于获取向量的元素个数。

### 使用 vec! 宏创建向量

使用 **Vec::new()** 方法创建一个向量的实例，然后在使用 **push()** 方法添加元素的操作看起来有点复杂。

为了使创建向量看起来像创建数组那么简单，Rust 标准库提供了 **vect!** 用于简化向量的创建。

使用 **vect!** 宏创建向量时，向量的数据类型由第一个元素自动推断出来。

```
fn main() {
   let v = vec![1,2,3];
   println!("{:?}",v);
}
```

运行以上 Rust 代码，输出结果如下

```
[1, 2, 3]
```

向量也是相同类型元素的集合。

因此，如果给向量传递了不同数据类型的值则会引发错误 error[E0308]: mismatched types 。

下面的代码，编译会报错

```
fn main() {
   let v = vec![1,2,3,"hello"];
   println!("{:?}",v);
}
```

错误信息为

```
error[E0308]: mismatched types
 --> src/main.rs:2:23
  |
2 |    let v = vec![1,2,3,"hello"];
  |                       ^^^^^^^ expected integer, found reference
  |
  = note: expected type `{integer}`
             found type `&'static str`

error: aborting due to previous error
```

### 追加元素到向量中 push()

**push()** 方法可以将指定的值添加到向量的末尾

```
fn main() {
   let mut v = Vec::new();
   v.push(20);
   v.push(30);
   v.push(40);

   println!("{:?}",v);
}
```

运行以上 Rust 代码，输出结果如下

```
[20, 30, 40]
```

### 删除向量中的某个元素 remove()

**remove()** 方法移除并返回向量中指定的下标索引处的元素，将其后面的所有元素移到向左移动一位。

```
fn main() {
   let mut v = vec![10,20,30];
   v.remove(1);
   println!("{:?}",v);
}
```

运行以上 Rust 代码，输出结果如下

```
[10, 30]
```

### 判断向量是否包含某个元素

**contains()** 用于判断向量是否包含某个值。

如果值在向量中存在则返回 _true_，否则返回 _false_。

```
fn main() {
   let v = vec![10,20,30];
   if v.contains(&10) {
      println!("found 10");
   }
   println!("{:?}",v);
}
```

运行以上 Rust 代码，输出结果如下

```
found 10
[10, 20, 30]
```

### 获取向量的长度

len() 方法可以获取向量的长度，也就是向量元素的个数。

```
fn main() {
   let v = vec![1,2,3];
   println!("size of vector is :{}",v.len());
}
```

运行以上 Rust 代码，输出结果如下
```
size of vector is :3
```

### 访问向量元素的方法

向量既然被称为是可变的数组，那么它的元素当然可以使用 **下标** 语法来访问。

也就是可以使用 **索引号** 来访问向量的每一个元素。

例如下面的代码，我们可以使用 _v[0]_ 来访问第一个元素 20，使用 _v[1]_ 来访问第二个元素。

```
fn main() {
   let mut v = Vec::new();
   v.push(20);
   v.push(30);

   println!("{:?}",v[0]);
}
```

运行以上 Rust 代码，输出结果如下

```
20
```

### 迭代/遍历向量

向量本身就实现了迭代器特质，因此可以直接使用 for in 语法来遍历向量

```
fn main() {
   let mut v = Vec::new();
   v.push(20);
   v.push(30);
   v.push(40);
   v.push(500);

   for i in v {
      println!("{}",i);
   }

   // println!("{:?}",v); // 运行出错，因为向量已经不可用
}
```

编译运行以上 Rust 代码，输出结果如下

```
20
30
40
500
```

如果把上面代码中的注释去掉，则会报编译错误

```
fn main() {
   let mut v = Vec::new();
   v.push(20);
   v.push(30);
   v.push(40);
   v.push(500);

   for i in v {
      println!("{}",i);
   }

   println!("{:?}",v); // 运行出错，因为向量已经不可用
}
```

编译出错

```
error[E0382]: borrow of moved value: `v`
  --> src/main.rs:12:20
   |
2  |    let mut v = Vec::new();
   |        ----- move occurs because `v` has type `std::vec::Vec<i32>`, which does not implement the `Copy` trait
...
8  |    for i in v {
   |             - value moved here
...
12 |    println!("{:?}",v); // 运行出错，因为向量已经不可用
   |                    ^ value borrowed here after move
```

出错原因我们在 Rust 所有权 Ownership 章节已经提到过了，这里就不做详细介绍了。

修复的方式，就是在使用使用 _for in_ 来迭代向量的一个引用

```
fn main() {
   let mut v = Vec::new();
   v.push(20);
   v.push(30);
   v.push(40);
   v.push(500);

   for i in &v {
      println!("{}",i);
   }
   println!("{:?}",v);
}
```

编译运行以上 Rust 代码，输出结果如下

```
20
30
40
500
[20, 30, 40, 500]
```

## 哈希表 HashMap

哈希表 HashMap 就是 **键值对** 的集合。哈希表中不允许有重复的键，但允许不同的键有相同的值。

从另一方面说，哈希表有点像 **查找表**。键用于查找值。

Rust 语言使用 HashMap 结构体来表示哈希表。

HashMap 结构体在 Rust 语言标准库中的 _std::collections_ 模块中定义。

使用 HashMap 结构体之前需要显式导入 _std::collections_ 模块。

### 创建哈希表的语法

Rust 语言标准库 _std::collections_ 的结构体 HashMap 提供了 _new()_ 静态方法用于创建哈希表的一个实例。

使用 _HashMap::new()_ 创建哈希表的语法格式如下

```
let mut instance_name = HashMap::new();
```

_new()_ 方法会创建一个空的哈希表。但这个空的哈希表是不能立即使用的，因为它还没指定数据类型。当我们给哈希表添加了元素之后才能正常使用。

结构体 HashMap 同时提供了大量的方法用于操作哈希表中的元素，我们将常用的几个方法罗列于此

|方法|	方法签名|	说明|
|--|--|--|
|insert()|	pub fn insert(&mut self, k: K, v: V) -> Option|	插入/更新一个键值对到哈希表中，如果数据已经存在则返回旧值，如果不存在则返回 None|
|len()|	pub fn len(&self) -> usize|	返回哈希表中键值对的个数|
|get()|	pub fn get<Q: ?Sized>(&lself, k: &Q) -> Option<&V>|	根据键从哈希表中获取相应的值|
|iter()|	pub fn iter(&self) -> Iter<K, V>|	返回哈希表键值对的无序迭代器，迭代器元素类型为 (&'a K, &'a V)|
|contains_key()|	pub fn contains_key<Q: ?Sized>(&self, k: &Q) -> bool|	如果哈希表中存在指定的键则返回 true 否则返回 false|
|remove()|	pub fn remove_entry<Q: ?Sized>(&mut self, k: &Q) -> Option<(K, V)>|	从哈希表中删除并返回指定的键值对|


### 插入/更新一个键值对到哈希表中 insert()

insert() 方法用于插入或更新一个键值对到哈希表中。

如果键已经存在，则更新为新的简直对，并则返回旧的值。

如果键不存在则执行插入操作并返回 None。

```
use std::collections::HashMap;
fn main(){
   let mut stateCodes = HashMap::new();
   stateCodes.insert("name","从零蛋开始教程");
   stateCodes.insert("site","https://www.baidu.com");
   println!("{:?}",stateCodes);
}
```

编译运行以上 Rust 代码，输出结果如下

```
{"name": "从零蛋开始教程", "site": "https://www.baidu.com"}
```

### 获取哈希表中键值对的个数 len()

len() 方法用于获取哈希表的长度，也就是哈希表中键值对的个数。

```
use std::collections::HashMap;
fn main() {
   let mut stateCodes = HashMap::new();
   stateCodes.insert("name","从零蛋开始教程");
   stateCodes.insert("site","https://www.baidu.com");
   println!("size of map is {}",stateCodes.len());
}
```

编译运行以上 Rust 代码，输出结果如下

```
size of map is 2
```

### 根据键从哈希表中获取相应的值 get()

get() 方法用于根据键从哈希表中获取相应的值。

如果值不存在，也就是哈希表不包含参数的键则返回 None。

如果值存在，则返回值的一个引用。

```
use std::collections::HashMap;
fn main() {
   let mut stateCodes = HashMap::new();
   stateCodes.insert("name","从零蛋开始教程");
   stateCodes.insert("site","https://www.baidu.com");
   println!("size of map is {}",stateCodes.len());
   println!("{:?}",stateCodes);

   match stateCodes.get(&"name") {
      Some(value)=> {
         println!("Value for key name is {}",value);
      }
      None => {
         println!("nothing found");
      }
   }
}
```

编译运行以上 Rust 代码，输出结果如下
```
size of map is 2
{"name": "从零蛋开始教程", "site": "https://www.baidu.com"}
Value for key name is 从零蛋开始教程
```

### 迭代哈希表 iter()

iter() 方法会返回哈希表中 键值对的引用 组成的无序迭代器。

迭代器元素的类型为 (&'a K, &'a V)。

```
use std::collections::HashMap;
fn main() {
   let mut stateCodes = HashMap::new();
    stateCodes.insert("name","从零蛋开始教程");
    stateCodes.insert("site","https://www.baidu.com");

   for (key, val) in stateCodes.iter() {
      println!("key: {} val: {}", key, val);
   }
}
```

编译运行以上 Rust 代码，输出结果如下

```
key: site val: https://www.baidu.com
key: name val: 从零蛋开始教程
```

### 是否包含指定的键 contains_key()

contains_key() 方法用于判断哈希表中是否包含指定的 **键值对**。

如果包含指定的键，那么会返回相应的值的引用，否则返回 None。

```
use std::collections::HashMap;
fn main() {
    let mut stateCodes = HashMap::new();
    stateCodes.insert("name","从零蛋开始教程");
    stateCodes.insert("site","https://www.baidu.com");
    stateCodes.insert("slogn","从零蛋开始教程，简单编程");

    if stateCodes.contains_key(&"name") {
        println!("found key");
    }
}
```

编译运行以上 Rust 代码，输出结果如下

```
found key
```

### 从哈希表中删除指定键值对 remove()

_remove()_ 用于从哈希表中删除指定的键值对。

如果键值对存在则返回删除的键值对，返回的数据格式为 (&'a K, &'a V)。

如果键值对不存在则返回 None

```
use std::collections::HashMap;
fn main() {
    let mut stateCodes = HashMap::new();
    stateCodes.insert("name","从零蛋开始教程");
    stateCodes.insert("site","https://www.baidu.com");
    stateCodes.insert("slogn","从零蛋开始教程，简单编程");

    println!("length of the hashmap {}",stateCodes.len());
    stateCodes.remove(&"site");
    println!("length of the hashmap after remove() {}",stateCodes.len());
}
```

编译运行以上 Rust 代码，输出结果如下

```
length of the hashmap 3
length of the hashmap after remove() 2
```

## 哈希集合 HashSet

哈希集合 **HashSet**，简称为 **集合 (set)**，是没有重复值的相同数据类型的值的集合。

集合的最大特征就是没有重复值。

Rust 语言标准库 _std::collections_ 中定义了结构体 HashSet 用于描述集合。

std::collections 模块中同时包含了大量的方法用于创建、访问和操作集合。

### 创建集合的语法

Rust 语言标准库 std::collections 的结构体 HashSet 提供了 new() 静态方法用于创建集合的一个实例。

使用 HashSet::new() 创建集合的语法格式如下

```
let mut hash_set_name = HashSet::new();
```

new() 方法会创建一个空的集合。但这个空的集合是不能立即使用的，因为它还没指定数据类型。当我们给集合添加了元素之后才能正常使用。

结构体 HashSet 同时提供了大量的方法用于操作集合中的元素，我们将常用的几个方法罗列于此

|方法|	方法原型|	描述|
|--|--|--|
|insert()|	pub fn insert(&mut self, value: T) -> bool|	插入一个值到集合中, 如果集合已经存在值则插入失败|
|len()|	pub fn len(&self) -> usize|	返回集合中的元素个数|
|get()|	pub fn get<Q:?Sized>(&self, value: &Q) -> Option<&T>|	根据指定的值获取集合中相应值的一个引用|
|iter()|	pub fn iter(&self) -> Iter|	返回集合中所有元素组成的无序迭代器, 迭代器元素的类型为 &'a T|
|contains_key()|	pub fn contains<Q: ?Sized>(&self, value: &Q) -> bool|	判断集合是否包含指定的值|
|remove()|	pub fn remove<Q: ?Sized>(&mut self, value: &Q) -> bool|	从结合中删除指定的值|

### 插入一个值到集合中 insert()

insert() 用于插入一个值到集合中。

insert() 方法的函数原型如下

```
pub fn insert(&mut self, value: T) -> bool
```

insert() 用于插入一个值到集合中，如果集合中已经存在指定的值，则返回 false，否则返回 true。

> 注意： 集合中不允许出现重复的值，因此如果集合中已经存在相同的值，则会插入失败。

```
use std::collections::HashSet;

fn main() {
    let mut languages = HashSet::new();
    languages.insert("Python");
    languages.insert("Rust");
    languages.insert("Ruby");
    languages.insert("PHP");

    languages.insert("Rust"); // 插入失败但不会引发异常

    println!("{:?}",languages);
}
```

编译运行以上 Rust 代码，输出结果如下

```
{"Python", "PHP", "Rust", "Ruby"}
```

### 获取集合的长度 len()

len() 方法用于获取集合的长度，也就是集合中元素的个数。

len() 方法的函数原型如下

```
pub fn len(&self) -> usize
```

注意： usize 是一个指针长度类型，这个由编译时的电脑 CPU 的构架决定。

### 范例

```
use std::collections::HashSet;
fn main() {
   let mut languages = HashSet::new();
   languages.insert("Python");
   languages.insert("Rust");
   languages.insert("Ruby");
   languages.insert("PHP");
   println!("size of the set is {}",languages.len());
}
```

编译运行以上 Rust 代码，输出结果如下

```
size of the set is 4
```

### 返回集合所有元素创建的迭代器 iter()

iter() 方法用于返回集合中所有元素组成的无序迭代器。

iter() 方法的函数原型如下

```
pub fn iter(&self) -> Iter
```

> 注意: 迭代器元素的顺序是无序的，毫无规则的。而且每次调用 iter() 返回的元素顺序都可能不一样。

```
use std::collections::HashSet;
fn main() {
   let mut languages = HashSet::new();
   languages.insert("Python");
   languages.insert("Rust");
   languages.insert("Ruby");
   languages.insert("PHP");

   for language in languages.iter() {
      println!("{}",language);
   }
}
```

编译运行以上 Rust 代码，输出结果如下

```
PHP
Python
Rust
Ruby
```

### 获取集合中指定值的一个引用 get()

get() 方法用于获取集合中指定值的一个引用。

get() 方法的原型如下

```
pub fn get<Q:?Sized>(&self, value: &Q) -> Option<&T>
```

如果值 value 存在于集合中则返回集合中的相应值的一个引用，否则返回 None。

```
use std::collections::HashSet;
fn main() {
   let mut languages = HashSet::new();
   languages.insert("Python");
   languages.insert("Rust");
   languages.insert("Ruby");
   languages.insert("PHP");

   match languages.get(&"Rust"){
      Some(value)=>{
         println!("found {}",value);
      }
      None =>{
         println!("not found");
      }
   }
   println!("{:?}",languages);
}
```

编译运行以上 Rust 代码，输出结果如下

```
found Rust
{"Python", "Ruby", "PHP", "Rust"}
```

### 判断集合是否包含某个值 contains()

contains() 方法用于判断集合是否包含指定的值。

contains() 方法的函数原型如下

```
pub fn contains<Q: ?Sized>(&self, value: &Q) -> bool
```

如果值 value 存在于集合中则返回 true ，否则返回 false。

```
use std::collections::HashSet;

fn main() {
   let mut languages = HashSet::new();
   languages.insert("Python");
   languages.insert("Rust");
   languages.insert("Ruby");

   if languages.contains(&"Rust") {
      println!("found language");
   }  
}
```

编译运行以上 Rust 代码，输出结果如下

```
found language
```

### 删除集合元素 remove()

remove() 方法用于从集合中删除指定的值。

remove() 方法的原型如下

```
pub fn remove(&mut self, value: &Q) -> bool
```

删除之前如果值 value 存在于集合中则返回 true，如果不存在则返回 false。

```
use std::collections::HashSet;

fn main() {
   let mut languages = HashSet::new();
   languages.insert("Python");
   languages.insert("Rust");
   languages.insert("Ruby");
   println!("length of the Hashset: {}",languages.len());
   languages.remove(&"Kannan");
   println!("length of the Hashset after remove() : {}",languages.len());
}
```

编译运行以上 Rust 代码，输出结果如下

```
length of the Hashset: 3
length of the Hashset after remove() : 3
```