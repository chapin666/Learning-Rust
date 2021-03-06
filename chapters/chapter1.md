# 一、Rust 基础教程

## 1.1 Rust 语言简介

Rust 是一门系统级别的编程语言。

Rust 由 Graydon Hoare 开发并在被 Mozilla 实验室收购后发扬光大。

## 1.2 应用程序及所采用的语言

总所周知，Java 和 C# 语言 一般用于构建面向用户服务的软件。比如电子表格，文字处理器，Web 应用程序或移动应用程序等业务应用程序。

至于面向机器的那些系统级别应用程序，因为对性能和并发由极高的要求，系统、软件以及软件平台所采用的语言几乎都是清一色的 C 语言 和 C++ 语言，比如它们操作系统，游戏引擎，编译器等。这些编程语言需要很大程度的硬件交互。

系统级别和应用程序级别编程语言面临两个主要问题：

- 难以写出高安全的语言，尤其是 C/C++ 指针带来的悬空指针，缓冲区溢出和内存泄漏等问题
- 缺少语言级别的高并发特性。

## 1.3 为什么选择 Rust ?

为什么选择 Rust ?

估计我们能说出一大堆理由来，然而，对我来说，最大的理由就是 字节跳动公司的飞聊团队已经用上了 Rust 语言了。这意味着学好 Rust 语言就有机会找到高薪的工作。

此外，正如 Rust 语言自己说的那样，Rust 语言有三大愿景：

- 高安全
- 高性能
- 高并发

Rust 语言旨在以简单的方式开发高度可靠和快速的软件。

Rust 语言支持用现代语言特性来写一些系统级别乃至机器级别的程序。

### 1.3.1 高性能

高性能是所有语言的最高追求，Rust 也不例外。

为了追求极致的性能，Rust 抛弃了 C/C++ 之外的语言都有的 垃圾回收器（ Garbage Collector (GC)）。

也就是消除了垃圾回收机制带来的性能损耗。

### 1.3.2 编译时内存安全

Rust 虽然也有指针的概念，但这个概念被大大的弱化，因此它没有 C/C++ 那种悬空指针，缓冲区溢出和内存泄漏等等问题。

### 1.3.3 天生多线程安全运行程序

Rust 是为多线程高并发而设计的系统级别语言

Rust 的 **拥有者(ownership)** 概念和 **内存安全** 规则使得它天生支持高并发，而且是支持没有数据竞争的高并发。

### 1.3.4 Rust 语言支持 Web Assembly (WASM) 语言

Rust 的目标是成为高并发且高安全的系统级语言，但这并不代表它就不能开发 Web 应用。

Rust 支持通过把代码编译成 **Web Assembly (WASM)** 语言从而能够在浏览器端以实现快速，可靠的运行。

**Web Assembly (WASM)** 语言是被设计用来在浏览器端/嵌入式设别上运行的，用于执行 CPU 计算密集型的语言。

也就是说 **Web Assembly (WASM) 语言** 的目标是和 Javascript 一样能够在浏览器里运行，但因为是编译型，所以更高效。