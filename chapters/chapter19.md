# 十九、Rust 模块 Modules

模块 **Module** 用于将函数或结构体按照功能分组。我们通常把相似的函数或者实现相同功能的或者共同实现一个功能的函数和结构体划分到一个模块中。

例如 network 模块包含了所有和网络有关的函数和结构体，graphics 模块则包含了所有和图形处理有关的函数和结构体。

Rust 中的模块，类似于其它语言中的模块或者包的概念，例如 C++ 语言中的命名空间，例如 Java 语言中的包。

比模块更高级别的分组是 **crate**，我们可以将多个模块放到一个 **crate** 下面。crate 是 Rust 语言的基本编译单元。Rust 中的可执行二进制文件程序或者一个库就是一个 carate。

可执行二进制文件程序和库的最大区别，就是 **可执行二进制程序** 有一个包含 main() 方法作为程序入口。

而一个库 (library crate) 是一组可以在其他项目中重用的组件。与二进制包不同，库包没有入口函数（main() 方法）。

Rust 内置了 cargo 作为包管理器，类似于 Python 语言中的 pip 。Rust 官方同时提供了 crates.io 用作所有第三方包的中央存储服务器。你可以使用 cargo install 命令从 crates.io 上下载你的程序所需要的 crate。

上面我们提到了很多新的术语，我们将它们罗列于下表中

|术语|	中文|	说明|
|--|--|--|
|crate|	crate|	Rust 中的基本编译单元，可以被编译为可执行文件或库|
|cargo|	cargo|	Rust 官方出品的 Rust 包管理器|
|module|	模块|	一个 crate 中有相互逻辑的代码|
|crates.io|	crates.io|	Rust 第三方扩展包的中央官方仓库|

crate 翻译为中文是 **箱|板条** 的意思，个人觉得还不如翻译为 **项目** 来的实在。

比如交流的时候说 创建一个板条箱，估计没人能听懂

## 19.1 Rust 中模块的定义语法

Rust 提供了 mod 关键字用于定义一个模块，定义模块的语法格式如下

```
mod module_name {
   fn function_name() {
      // 具体的函数逻辑
   }
   fn function_name() {
      // 具体的函数逻辑
   }
}
```

module_name 必须是一个合法的标识符，它的格式和函数名称一样。

### 19.1.1 公开的模块和公开的函数

Rust 语言默认所有的模块和模块内的函数都是私有的，也就是只能在模块内部使用。

如果一个模块或者模块内的函数需要导出为外部使用，则需要添加 pub 关键字。

定义一个公开的模块和模块内公开的函数的语法如下

```
//公开的模块
pub mod a_public_module {
   pub fn a_public_function() {
      // 公开的方法
   }
   fn a_private_function() {
      // 私有的方法
   }
}
//私有的模块
mod a_private_module {

   // 私有的方法
   fn a_private_function() {
   }
}
```

模块 Module 可以是公开可访问的 pub，也可以是私有的。

如果一个模块不添加 pub 关键字，那么它就是私有的，私有的模块不能为外部其它模块或程序所调用。

Rust 语言中的模块默认是私有的。

如果一个模块添加了 pub 关键字，那么它就是公开对外可访问的。

不过需要注意的是，私有模块的所有函数都必须是私有的，而公开的模块，则即可以有公开的函数也可以有私有的函数。

模块中的函数默认都是私有的，如果一个函数没有添加 pub 关键字，那么它就是私有的，相反，添加了 pub 关键字的函数则是公开的。

> 简直和绕口令差不多了....

### 19.1.2 范例： 定义一个模块

我们已经学习了 Rust 中模块的基本知识和定义语法，接下来我们尝试定义一个模块 movies，它包含一个单独的方法 play()。

play() 方法接受一个单独的字符串参数，然后输出这个字符串。

```
pub mod movies {
   pub fn play(name:String) {
      println!("Playing movie {}",name);
   }
}

fn main(){
   movies::play("Herold and Kumar".to_string());
}
```

运行以上 Rust 代码，输出结果如下

```
Playing movie Herold and Kumar
```

## 19.2 use 关键字

每次调用外部的模块中的函数或结构体都要添加 **模块限定**，这样似乎有点啰嗦了。

我们能不能在文件头部先把需要调用的函数/结构体引用进来，然后调用的时候就可以省去 **模块限定** 呢 ？

答案是可以的。

Rust 从 C++ 借鉴了 use 关键字。

use 关键字用于文件头部预先引入需要用到的外部模块中的函数或结构体。

### 19.2.1 use 关键字的使用语法
```
use public_module_name::function_name;
```

### 19.2.2 范例

有了 use 关键字，我们就可以预先引入外部模块中的函数和结构体而不用在使用时使用 **全限定模块**。

```
pub mod movies {
   pub fn play(name:String) {
      println!("Playing movie {}",name);
   }
}

use movies::play;

fn main(){
   play("Herold and Kumar ".to_string());
}
```

运行以上 Rust 代码，输出结果如下

```
Playing movie Herold and Kumar
```

## 19.3 嵌套模块 / 多级模块

Rust 允许一个模块中嵌套另一个模块，换种说法，就是允许多层级模块。

嵌套模块的语法格式很简单，如下

```
pub mod movies {
   pub mod english {
      pub mod comedy {
         pub fn play(name:String) {
            println!("Playing comedy movie {}",name);
         }
      }
   }
}
```

上面这段代码中，movies 模块内嵌了 english 模块，english 模块内嵌了 comedy 模块，而 comedy 模块内才含有真正的函数 play()。

调用或使用嵌套模块的方法也很简单，只要使用两个冒号 (::) 从左到右拼接从外到内的模块即可，例如上面的模块，使用方式如下

```
use movies::english::comedy::play; 
```

### 19.3.1 范例

下面的范例是对上面代码的补充

```
pub mod movies {
   pub mod english {
      pub mod comedy {
         pub fn play(name:String) {
            println!("Playing comedy movie {}",name);
         }
      }
   }
}
use movies::english::comedy::play; // 导入公开的模块

fn main() {
   // 短路径语法
   play("Herold and Kumar".to_string());
   play("The Hangover".to_string());

   // 全路径语法
   movies::english::comedy::play("Airplane!".to_string());
}
```

运行以上 Rust 代码，输出结果如下

```
[www.badu.com]$ cargo run
   Compiling guess-game-app v0.1.0 (/Users/Admin/Downloads/guess-game-app)
    Finished dev [unoptimized + debuginfo] target(s) in 1.73s
     Running `target/debug/guess-game-app`
Playing comedy movie Herold and Kumar
Playing comedy movie The Hangover
Playing comedy movie Airplane!
```

## 19.4 范例：创建一个库 crate 并编写一些用例

上面大篇幅我们一直都在说模块的一些用法，估计把大家绕的一头雾水了。

下面我们来点真格的，从零开始创建一个库并编写一些测试用例。

首先我们来理一理我们要创建的库的一些基本信息。

|元数据|	值|
|--|--|
|crate 名|	movie_lib|
|模块名字|	movies|

Rust 项目一般使用 cargo 作为包管理器，我们也不例外，我们会用它来管理我们的 crate ，权当复习复习。

### 19.4.1 第一步 - 创建 movie_lib 库 crate

在你的工作目录下创建一个项目目录叫做 movie_app，创建项目的命令如下

```
$ mkdir movie_app
$ ls
movie_app
```

> 如果你是 Windows 电脑，可以直接点击右键新建目录。

我的工作目录是 /Users/Admin/Downloads/rust。

> 你的工作目录不需要和我的一样，因为只要 movie_app 是一样的即可。另一方面，你用的可能是 Windows 电脑，这个和我的 Mac 苹果电脑路径也不一样。如果有问题，请联系。

接下来在 movie_app 目录下新建一个目录 movie_lib。

然后在 movie_lib 目录下新建文件 Cargo.toml 和 src 目录。

最后在 movie_lib/src 目录下新建 lib.rs 文件和 movies.rs 文件。

创建完成后的目录结构如下

```
movie_app
   movie_lib/
      -->Cargo.toml
      -->src/
         lib.rs
         movies.rs
```

Cargo.toml 文件主要用于保存库 crate 的一些基本信息/元数据，比如库 crate 的版本号、库名称、作者信息等等

上面三个步骤可以在 movie-app 目录下运行 cargo new movie_lib --lib 命令一键创建。

```
$ cd movie_app/
$ cargo new movie_lib --lib
     Created library `movie_lib` package
$ ls
movie-lib
$ tree ../movie-app/
../movie_app/
└── movie_lib
    ├── Cargo.toml
    └── src
        └── lib.rs

2 directories, 2 files

$ touch movie_lib/src/movies.rs
$ tree ../movie_app/
../movie_app/
└── movie_lib
    ├── Cargo.toml
    └── src
        ├── lib.rs
        └── movies.rs

2 directories, 3 files
```

### 19.4.2 第二步 - 编辑 Cargo.toml 文件修改库 crate 的一些基本信息

修改之前，我们先来看看 Cargo.toml 文件里的原内容是啥

```
$ cat movie_lib/Cargo.toml 
```

输出为

```
[package]
name = "movies_lib"
version = "0.1.0"
authors = ["****<noreply@xx.com>"]
edition = "2018"

[dependencies]
```

如果你不是使用 cargo new 命令创建 movie_lib ，那么这个文件的内容可能是空的。

其实也没啥好改的，因为我们创建的时候信息都挺正确的，如果你要将 crate 名字改成其它的，则可以直接修改 name

```
[package]
name = "movies_lib"
version = "0.1.0"
authors = ["****<noreply@xx.com>"]
edition = "2018"

[dependencies]
```

### 19.4.3 第三步 - 编辑 lib.rs 文件

lib.rs 文件 用于指定 库 crate 有哪些公开的模块可用。

如果你使用 cargo new 创建 movie_lib，那么 lib.rs 里是有一些内容的

```
#[cfg(test)]
mod tests {
    #[test]
    fn it_works() {
        assert_eq!(2 + 2, 4);
    }
}
```

这些内容以后我们有空再回来讲讲，现在，我们专注于这个 movie_lib。

因为这是一个 库 crate ，因此我们需要在 lib.rs 文件中指定我们的需要导出的模块名字。

语法格式为

```
pub mod [库名字];
```

例如我们这个库的名字是 movies，因此 lib.rs 文件的内容为

```
pub mod movies;
```

### 19.4.4 第四步 - 编辑 movies.rs 文件

我们的 movie_lib 库只有一个模块 movies，而这个模块只有一个功能，就是在 movies.rs 中放置一个函数 play() 。

play() 函数用于输出传递过来的字符串参数

```
pub fn play(name:String){
   println!("Playing movie {} :movies_app",name);
}
```

因为 play() 函数需要导出给外部使用因此需要添加 pub 关键字。

### 19.4.5 第五步 - 编译 movie_lib 库 crate

我们可以在 movie_lib 目录下使用 cargo build 命令来构建/编译项目。这个命令除了编译作用外，还会检查我们的 crate 结构是否正确。

如果构建成功，则输出结果类似于

```
$ cargo build
   Compiling movie_lib v0.1.0 (/Users/Admin/Downloads/rust/movie_app/movie_lib)
    Finished dev [unoptimized + debuginfo] target(s) in 0.89s
```

### 19.4.6 第六步 - 创建测试应用程序

在 movie_app 目录下新建一个目录 movie_lib_test。

然后在 movie_lib_test 目录下新建文件 Cargo.toml 和 src 目录。

最后在 movie_lib_test/src 目录下新建 main.rs 文件。

创建完成后的目录结构如下

```
../movie_app/
├── movie_lib
│   ├── Cargo.lock
│   ├── Cargo.toml
│   └── src
│       ├── lib.rs
│       └── movies.rs
└── movie_lib_test
    ├── Cargo.toml
    └── src
        └── main.rs

4 directories, 6 files
```

上面三个步骤可以在 movie_app 目录下运行 cargo new movie_lib_test --bin 命令一键创建。

movie_lib_test 是一个测试应用程序，它的主要作用就是测试我们刚刚写的 movie_lib。因为是一个可执行二进制项目，因此 movie_lib_test/src/main.rs 中必须包含 main() 作为入口函数。

### 19.4.7 第七步 - 编辑 Cargo.toml 添加本地依赖

打开 Cargo.toml 文件并在 [dependencies] 节点下添加

```
movies_lib = { path = "../movie_lib" }
```

修改完成后的代码如下

```
[package]
name = "movie_lib_test"
version = "0.1.0"
authors = ["*** <noreply@xxx.com>"]
edition = "2018"

[dependencies]
movies_lib = { path = "../movie_lib" }
```

> 注意: 请你特别留意 movies_lib 依赖项的文件位置。

下面的图例标明了 movies_lib 和 测试应用程序 两个项目的目录结构

![](/images/crate.png)

### 19.4.8 第八步 - 添加一些使用范例代码到 src/mian.rs 文件中

打开 src/main.rs 文件并复制粘贴一下内容

```
extern crate movies_lib;

use movies_lib::movies::play;

fn main() {
   println!("inside main of test ");
   play("零基础教程".to_string());
}
```

上面的代码中

- extern crate movies_lib; 用于引入我们刚刚创建的模块 movies_lib。这个是必须的

- use movies_lib::movies::play; 用于引入 movies_lib::movies::play。如果没有这句，那么使用 play() 函数就要明确指定模块和模块下的文件。

- play("零基础教程".to_string()); 调用我们 play() 函数并传递字符串参数。

> 注意 1: 如果你不清楚当前项目的 crate 名字，可以打开 Cargo.toml 文件查询。

> 注意 2: 请仔细阅读 use movies_lib::movies::play; 中 :: 隔开的每一部分，它的组成其实是 crate 名字 + 库名字 + 函数名/结构体名/常量名

### 19.4.9 第九步 - 使用 cargo build 编译或使用 cargo run 运行

终于，激动人心的时刻来临了，我们可以在 movie_lib_test 目录下运行 cargo build 构建项目，当然了，如果仅仅是想看看运行结果，可以直接输入 cargo run 运行，这个命令会先执行 cargo build ，输出结果下:

```
$ cargo run
   Compiling movies_lib v0.1.0 (/Users/Admn/Downloads/rust/movie_app/movie_lib)
   Compiling movie_lib_test v0.1.0 (/Users/Admin/Downloads/rust/movie_app/movie_lib_test)
    Finished dev [unoptimized + debuginfo] target(s) in 1.70s
     Running `target/debug/movie_lib_test`
inside main of test 
Playing movie  :movies_app
```