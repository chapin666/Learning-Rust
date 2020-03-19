# Rust Cargo 包管理器

Rust 内置了一个包管理器 **cargo**。它会随着 Rust 的安装而安装。

cargo 类似于 Python 中的 pip 或 Ruby 中的 RubyGems 或 Node.js 中的 NPM。

当然了，cargo 不仅仅是一个包管理器，它还是 Rust 的项目管理利器。

## 检查 cargo 是否安装和安装的版本

打开终端或命令行提示符或 Shell，输入下面的命令然后回车

```
cargo --version
```

如果已经安装，则输出结果类似于

```
cargo 1.35.0
```

cargo 包管理器的版本和 Rust 语言是同步的。

## cargo 的帮助信息

如果想要查看 cargo 的帮助信息或查看 cargo 提供了哪些命令和功能，可以在 终端 中输入 cargo 或 cargo -h 然后回车

输出结果类似于

```
Rust's package manager

USAGE:
    cargo [OPTIONS] [SUBCOMMAND]

OPTIONS:
    -V, --version           Print version info and exit
        --list              List installed commands
        --explain <CODE>    Run `rustc --explain CODE`
    -v, --verbose           Use verbose output (-vv very verbose/build.rs output)
    -q, --quiet             No output printed to stdout
        --color <WHEN>      Coloring: auto, always, never
        --frozen            Require Cargo.lock and cache are up to date
        --locked            Require Cargo.lock is up to date
    -Z <FLAG>...            Unstable (nightly-only) flags to Cargo, see 'cargo -Z help' for details
    -h, --help              Prints help information

Some common cargo commands are (see all commands with --list):
    build       Compile the current package
    check       Analyze the current package and report errors, but don't build object files
    clean       Remove the target directory
    doc         Build this package's and its dependencies' documentation
    new         Create a new cargo package
    init        Create a new cargo package in an existing directory
    run         Run a binary or example of the local package
    test        Run the tests
    bench       Run the benchmarks
    update      Update dependencies listed in Cargo.lock
    search      Search registry for crates
    publish     Package and upload this package to the registry
    install     Install a Rust binary. Default location is $HOME/.cargo/bin
    uninstall   Uninstall a Rust binary

See 'cargo help <command>' for more information on a specific command.
```

## cargo 提供的命令

正如上面 cargo 帮助信息中所描述的那样，如果我们想要查看 cargo 提供的所有命令，可以直接在终端里输入

```
cargo --list
```

输出结果如下

```
Installed Commands:
    bench                Execute all benchmarks of a local package
    build                Compile a local package and all of its dependencies
    check                Check a local package and all of its dependencies for errors
    clean                Remove artifacts that cargo has generated in the past
    clippy-preview       Checks a package to catch common mistakes and improve your Rust code.
    doc                  Build a package's documentation
    fetch                Fetch dependencies of a package from the network
    fix                  Automatically fix lint warnings reported by rustc
    generate-lockfile    Generate the lockfile for a package
    git-checkout         Checkout a copy of a Git repository
    init                 Create a new cargo package in an existing directory
    install              Install a Rust binary. Default location is $HOME/.cargo/bin
    locate-project       Print a JSON representation of a Cargo.toml file's location
    login                Save an api token from the registry locally. If token is not specified, it will be read from stdin.
    metadata             Output the resolved dependencies of a package, the concrete used versions including overrides, in machine-readable format
    new                  Create a new cargo package at <path>
    owner                Manage the owners of a crate on the registry
    package              Assemble the local package into a distributable tarball
    pkgid                Print a fully qualified package specification
    publish              Upload a package to the registry
    read-manifest        Print a JSON representation of a Cargo.toml manifest.
    run                  Run a binary or example of the local package
    rustc                Compile a package and all of its dependencies
    rustdoc              Build a package's documentation, using specified custom flags.
    search               Search packages in crates.io
    test                 Execute all unit and integration tests and build examples of a local package
    uninstall            Remove a Rust binary
    update               Update dependencies as recorded in the local lock file
    verify-project       Check correctness of crate manifest
    version              Show version information
    yank                 Remove a pushed crate from the index
```

命令很多，我们就不逐一介绍了，挑几个常用的命令介绍下

|命令|	说明|
|--|--|
|cargo build|	编译当前项目|
|cargo check|	分析当前项目并报告项目中的错误，但不会编译任何项目文件|
|cargo run	|编译并运行文件 src/main.rs|
|cargo clean|	移除当前项目下的 target 目录及目录中的所有子目录和文件|
|cargo update|	更新当前项目中的 Cargo.lock 文件列出的所有依赖|
|cargo new	|在当前目录下新建一个 cargo 项目|

如果你对某个命令不甚熟悉，可以直接使用 cargo help <command> 语法显示命令的帮助信息。

比如要详细了解 new 命令，可以直接输入 cargo help new，输出结果如下

```
cargo-new 
Create a new cargo package at <path>

USAGE:
    cargo new [OPTIONS] <path>

OPTIONS:
    -q, --quiet                  No output printed to stdout
        --registry <REGISTRY>    Registry to use
        --vcs <VCS>              Initialize a new repository for the given version control system (git, hg, pijul, or
                                 fossil) or do not initialize any version control at all (none), overriding a global
                                 configuration. [possible values: git, hg, pijul, fossil, none]
        --bin                    Use a binary (application) template [default]
        --lib                    Use a library template
        --edition <YEAR>         Edition to set for the crate generated [possible values: 2015, 2018]
        --name <NAME>            Set the resulting package name, defaults to the directory name
    -v, --verbose                Use verbose output (-vv very verbose/build.rs output)
        --color <WHEN>           Coloring: auto, always, never
        --frozen                 Require Cargo.lock and cache are up to date
        --locked                 Require Cargo.lock is up to date
    -Z <FLAG>...                 Unstable (nightly-only) flags to Cargo, see 'cargo -Z help' for details
    -h, --help                   Prints help information

ARGS:
    <path>  
```

## cargo 创建 Rust 项目

作为包管理器，cargo 可以帮助我们下载第三方库。但这仅仅是 cargo 功能的冰山一角。

我们还可以使用 cargo 来构建自己的 库，然后发布到 Cargo 的官方仓库中。

cargo 可以创建两种类型的项目：**可执行的二进制程序** 和 **库**。

如果要创建一个 可执行的二进制程序，可以使用下面的 cargo new 命令创建项目

```
cargo new project_name --bin
```

如果要创建一个 库，则可以使用下面的 cargo new 命令。

```
cargo new project_name --lib
```

## 范例： 使用 cargo 创建并构建一个完整的二进制可执行程序项目

创建项目、安装依赖、编译项目是 cargo 作为包管理器的三个最重要的功能。

下面我们就以一个简单的游戏：猜数字 来演示下如何使用 cargo 创建并编译项目。

### 一、 创建项目目录

打开终端或命令行提示符或 Shell 并进入到你想放项目的目录下。

然后输入命令 cargo new guess-game-app --bin，如果不出意外就会输出下面的提示

```
$ cargo new guess-game-app --bin
     Created binary (application) `guess-game-app` package
```

出现上面的提示说明项目创建成功，同时该命令会在当前目录下创建一个新的目录 guess-game-app 并创建一些必要的文件

```
$ tree guess-game-app/
guess-game-app/
├── Cargo.toml
└── src
    └── main.rs
```

通过上面的篇幅可知：cargo new 命令用于创建一个新项目，--bin 选项用于指示当前项目是一个 可执行的二进制程序 项目。

> 在 Cargo 中，通常把一个项目称之为 crates，but 我也不理解为啥...

Rust 官方为 cargo 包管理器提供了中央托管网站。网站地址为 https://crates.io/。 我们可以从该网站上找到一些我们需要的第三方库或者一些有用的第三方项目。

### 二、添加项目需要的第三方依赖库

我们的 **猜数字** 游戏需要生成 随机数。 但是 Rust 语言核心和 Rust 标准库都没有提供生成随机数的的方法或结构体。

我们只能借助于 https://crates.io/ 上其它开发者提供的第三方库或 crates。

我们可以打开网站 crates.io 并在顶部的搜索中输入 rand 然后回车来查找第三方随机数生成库。

从搜索的结果来看，由很多和随机数生成的相关库，不过我们只使用 rand。

点击搜索结果中的 rand 会跳转到 https://crates.io/crates/rand。

下图是我们需要的 **随机数生成库** rand 的基本信息截图。

![](/images/rand.png)

了解了 rand 的基本信息后，我们就可以修改 Cargo.toml 添加 rand 依赖了。

准确的说就是在 [dependencies] 节中添加 rand = "0.6.5"。

编辑完成后的文件内容如下

```
[package]
name = "guess-game-app"
version = "0.1.0"
authors = ["** <noreply@xxx.com>"]
edition = "2018"

[dependencies]
rand = "0.6.5"
```

### 三、 先预编译项目

对于 Rust / C / C++ 这种静态语言，先预编译一下应该是一种习惯。

因为预编译的时候会告诉我们项目配置是否正确，同时会下载第三方库，这样就可以 自动提示 了。

使用 cargo 创建的项目，我们可以在终端里输入 cargo build 来预编译下项目。

输出一般如下
```
    Updating crates.io index
  Downloaded rand v0.6.5
  Downloaded libc v0.2.58
  Downloaded rand_core v0.4.0
  Downloaded rand_hc v0.1.0
  Downloaded rand_chacha v0.1.1
  Downloaded rand_isaac v0.1.1
  Downloaded rand_jitter v0.1.4
  Downloaded rand_os v0.1.3
  Downloaded rand_xorshift v0.1.1
  Downloaded rand_pcg v0.1.2
  Downloaded autocfg v0.1.4
  Downloaded rand_core v0.3.1
   Compiling libc v0.2.58
   Compiling autocfg v0.1.4
   Compiling rand_core v0.4.0
   Compiling rand_core v0.3.1
   Compiling rand_isaac v0.1.1
   Compiling rand_xorshift v0.1.1
   Compiling rand_hc v0.1.0
   Compiling rand_pcg v0.1.2
   Compiling rand_chacha v0.1.1
   Compiling rand v0.6.5
   Compiling rand_os v0.1.3
   Compiling rand_jitter v0.1.4
   Compiling guess-game-app v0.1.0 (/Users/Admin/Downloads/guess-game-app)
    Finished dev [unoptimized + debuginfo] target(s) in 45.77s
```

从预编译输出的信息中我们可以看到：rand 第三方库和相关的依赖都会自动被下载。

### 四、 理解 猜数字 游戏逻辑

动手码代码之前，我们先来分析下 猜数字 游戏的游戏逻辑。

> 这是一种良好的编程习惯。

1. 游戏开始时先 随机 生成一个 数字 N。

2. 然后输出一些信息提示用户，并让用户猜并输入生成的数字 N 是多少。

3. 如果用户输入的数字小于随机数，则提示用户 Too low 然后让用户继续猜。

4. 如果用户输入的数字大于随机数，则提示用于 Too high 然后让用户继续猜。

5. 如果用户输入的数字正好是随机数，则游戏结束并输出 You go it ..

### 五、 修改 main.rs 文件完善游戏逻辑

猜数字 游戏逻辑很简单，前面我们已经详细的分析过了，接下来我们只要实现上面的逻辑即可。

代码我们就不做做介绍了，直接复制粘贴就好

```
use std::io;
extern crate rand; // 导入当前项目下的 rand 第三方库

use rand::random;
fn get_guess() -> u8 {
   loop {
      println!("Input guess") ;
      let mut guess = String::new();
      io::stdin().read_line(&mut guess)
         .expect("could not read from stdin");
      match guess.trim().parse::<u8>(){ // 需要去除输入首尾的空白
         Ok(v) => return v,
         Err(e) => println!("could not understand input {}",e)
      }
   }
}
fn handle_guess(guess:u8,correct:u8)-> bool {
   if guess < correct {
      println!("Too low");
      false

   } else if guess> correct {
      println!("Too high");
      false
   } else {
      println!("You go it ..");
      true
   }
}
fn main() {
   println!("Welcome to no guessing game");

   let correct:u8 = random();
   println!("correct value is {}",correct);
   loop {
      let guess = get_guess();
      if handle_guess(guess,correct){
         break;
      }
   }
}
```

### 六、编译项目并运行可执行文件

我们可以在 终端 中输入命令 cargo run 编译并运行我们的猜数字游戏。

如果没有出现任何错误，那么我们就可以简单的来玩几盘，哈哈

```
   Compiling guess-game-app v0.1.0 (/Users/Admin/Downloads/guess-game-app)
    Finished dev [unoptimized + debuginfo] target(s) in 0.67s
     Running `target/debug/guess-game-app`
Welcome to no guessing game
correct value is 145
Input guess
20
Too low
Input guess
100
Too low
Input guess
97
Too low
Input guess
145
You go it ..
```

哈哈，其实我们是有点作弊嫌疑，因为不提示结果，要猜中得很长的逻辑