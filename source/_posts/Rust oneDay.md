# 一、简介

## 1、Rust 文档链接

官网地址：https://www.rust-lang.org

中文文档：https://kaisery.github.io/trpl-zh-cn/

优势：在“系统层面”的工作，设计内存管理、并发等底层的细节。在给程序员造成了很大困扰，Rust消除了这些陷阱，更妙的是，它的语言设计的本身自然而言的引导你编写出可靠的代码，并且运行速度和内存使用很高效。

# 二、入坑指南

## 1、安装

通过 `rustup` 下载 Rust，这是一个管理 Rust 版本和相关工具的命令行工具。下载时需要联网。

> 注意：如果你出于某些理由倾向于不使用 `rustup`，请到 [Rust 的其他安装方法页面](https://forge.rust-lang.org/infra/other-installation-methods.html) 查看其它安装选项。

### 1.1、在 Linux 和 macOS 安装 rustup

打开终端，输入命令：

```shell
curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
```

此命令下载一个脚本并开始安装 `rustup` 工具，这会安装最新稳定版 Rust。过程中可能会提示你输入密码。如果安装成功，将会出现如下内容：

```shell
Rust is installed now. Great!
```

在 macOS 上，你可以通过运行以下命令获得 C 语言编译器(很可能你已经安装了)：

```shell
xcode-select --install
```

出现下边警告是已经安装了

```shell
xcode-select: error: command line tools are already installed, use "Software Update" in System Settings to install updates
```

### 1.2、Windows 安装

安装包下载地址：[安装 Rust - Rust 程序设计语言 (rust-lang.org)](https://www.rust-lang.org/zh-CN/tools/install) 傻瓜式安装



## 2、编写 Helle world

打开终端并输入如下命令创建 *projects* 目录

对于 Linux、macOS 和 Windows PowerShell，输入：

```shell
mkdir ~/projects
cd ~/projects
mkdir hello_world
cd hello_world
```

对于 Windows CMD，输入：

```doscon
> mkdir "%USERPROFILE%\projects"
> cd /d "%USERPROFILE%\projects"
> mkdir hello_world
> cd hello_world
```

编写程序：

创建一个文件（Linux 和 maxOS）：

```rust
vim main.rs
fn main() {
    println!("Hello, world!");
}
```

编译运行：

```rust
rustc main.rs
./main
Hello, world!
```

Windows 创建一个 .rs 后缀的文件，添加代码：

```rust
fn main() {
    println!("Hello, world!");
}
```

运行如果：

```rust
rustc main.rs
.\main.exe
Hello world!!
```

对于 Windows 的坑，如果编译存在报错下边处理后重新执行：

```dockerfile
error: linker `link.exe` not found
  |
  = note: program not found

note: the msvc targets depend on the msvc linker but `link.exe` was not found

note: please ensure that Visual Studio 2017 or later, or Build Tools for Visual Studio were installed with the Visual C++ option.

note: VS Code is a different product, and is not sufficient.

error: aborting due to previous error
```

错误信息很明显：依赖于微软的 `msvc linker.exe`。

一般让去下载VS，不过可以直接用rust命令解决。

```
rustup toolchain install stable-x86_64-pc-windows-gnu
rustup default stable-x86_64-pc-windows-gnu
```

### 3、Cargo 编程管理

Cargo 是 Rust 的构建系统和包管理器。大多数 Rustacean 们使用 Cargo 来管理他们的 Rust 项目，因为它可以为你处理很多任务，比如构建代码、下载依赖库并编译这些库。（我们把代码所需要的库叫做 **依赖**（*dependencies*））。

终端检查是否安装 Cargo ：

```rust
cargo --version
```

使用 Cargo 创建项目

```rust
cargo new hello_cargo
cd hello_cargo
```

第一行命令新建了名为 *hello_cargo* 的目录和项目。我们将项目命名为 *hello_cargo*，同时 Cargo 在一个同名目录中创建项目文件。

进入 *hello_cargo* 目录并列出文件。将会看到 Cargo 生成了两个文件和一个目录：一个 *Cargo.toml* 文件，一个 *src* 目录，以及位于 *src* 目录中的 *main.rs* 文件。

这也会在 *hello_cargo* 目录初始化了一个 git 仓库，以及一个 *.gitignore* 文件。如果在一个已经存在的 git 仓库中运行 `cargo new`，则这些 git 相关文件则不会生成；可以通过运行 `cargo new --vcs=git` 来覆盖这些行为。

> 注意：Git 是一个常用的版本控制系统（version control system，VCS）。可以通过 `--vcs` 参数使 `cargo new` 切换到其它版本控制系统（VCS），或者不使用 VCS。运行 `cargo new --help` 参看可用的选项。

请自行选用文本编辑器打开 *Cargo.toml* 文件。它应该看起来如示例 1-2 所示：

文件名：Cargo.toml

```toml
[package]
name = "hello_cargo"
version = "0.1.0"
edition = "2021"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
```

示例 1-2: *cargo new* 命令生成的 *Cargo.toml* 的内容

这个文件使用 [*TOML*](https://toml.io/) (*Tom's Obvious, Minimal Language*) 格式，这是 Cargo 配置文件的格式。

第一行，`[package]`，是一个片段（section）标题，表明下面的语句用来配置一个包。随着我们在这个文件增加更多的信息，还将增加其他片段（section）。

接下来的三行设置了 Cargo 编译程序所需的配置：项目的名称、项目的版本以及要使用的 Rust 版本。[附录 E](https://kaisery.github.io/trpl-zh-cn/appendix-05-editions.html) 会介绍 `edition` 的值。

最后一行，`[dependencies]`，是罗列项目依赖的片段的开始。在 Rust 中，代码包被称为 *crates*。这个项目并不需要其他的 crate，不过在第二章的第一个项目会用到依赖，那时会用得上这个片段。



- 可以使用 `cargo new` 创建项目。
- 可以使用 `cargo build` 构建项目。
- 可以使用 `cargo run` 一步构建并运行项目。
- 可以使用 `cargo check` 在不生成二进制文件的情况下构建项目来检查错误。
- 有别于将构建结果放在与源码相同的目录，Cargo 会将其放到 *target/debug* 目录。

通常 `cargo check` 要比 `cargo build` 快得多，因为它省略了生成可执行文件的步骤。如果你在编写代码时持续的进行检查，`cargo check` 可以让你快速了解现在的代码能不能正常通过编译！为此很多 Rustaceans 编写代码时定期运行 `cargo check` 确保它们可以编译。当准备好使用可执行文件时才运行 `cargo build`。

使用 Cargo 的一个额外的优点是，不管你使用什么操作系统，其命令都是一样的。所以从现在开始本书将不再为 Linux 和 macOS 以及 Windows 提供相应的命令。

发布 （release）构建

当项目最终准备好发布时，可以使用 `cargo build --release` 来优化编译项目。这会在 *target/release* 而不是 *target/debug* 下生成可执行文件。这些优化可以让 Rust 代码运行的更快，不过启用这些优化也需要消耗更长的编译时间。这也就是为什么会有两种不同的配置：一种是为了开发，你需要经常快速重新构建；另一种是为用户构建最终程序，它们不会经常重新构建，并且希望程序运行得越快越好。如果你在测试代码的运行时间，请确保运行 `cargo build --release` 并使用 *target/release* 下的可执行文件进行测试。



对于简单项目，Cargo 并不比 `rustc` 提供了更多的优势，不过随着开发的深入，终将证明其价值。一旦程序壮大到由多个文件组成，亦或者是需要其他的依赖，让 Cargo 协调构建过程就会简单得多。

即便 `hello_cargo` 项目十分简单，它现在也使用了很多在你之后的 Rust 生涯将会用到的实用工具。其实，要在任何已存在的项目上工作时，可以使用如下命令通过 Git 检出代码，移动到该项目目录并构建：

```console
git clone example.org/someproject
cd someproject
cargo build
```

