+++
date = '2026-06-06T17:00:00+08:00'
draft = false
title = 'Rust 语言新手入门教程'
tags = ['Rust', '编程入门', '教程']
+++

Rust 是 Mozilla 发起、现由 Rust 基金会维护的系统级编程语言，连续多年被评为 Stack Overflow "最受喜爱语言"。它以**零成本抽象**和**内存安全**著称，在编译器层面就杜绝了空指针、悬垂指针和数据竞争。

<!--more-->

---

## 为什么选 Rust？

- **内存安全** — 所有权系统在编译期检查，不需要垃圾回收
- **零成本抽象** — 高级抽象不产生运行时开销
- ** fearless concurrency** — 编译器防止数据竞争
- **出色的错误信息** — 编译器提示精确到应该写什么代码
- **广泛使用** — Linux 内核、Firefox、Windows 驱动都用 Rust

---

## 一、安装

推荐使用 `rustup` 安装：

```bash
# Windows 访问 https://rustup.rs 下载，或：
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# Linux/macOS
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

验证安装：

```bash
rustc --version
# rustc 1.95.0 (2026)
cargo --version
# cargo 1.95.0
```

国内用户设置镜像：

```bash
# 编辑 ~/.cargo/config.toml
[source.crates-io]
replace-with = "ustc"
[source.ustc]
registry = "sparse+https://mirrors.ustc.edu.cn/crates.io-index/"
```

---

## 二、第一个程序

```rust
fn main() {
    println!("你好，世界！");
}
```

编译运行：

```bash
rustc main.rs && ./main
# 你好，世界！
```

### 使用 Cargo（推荐）

```bash
cargo new hello_rust
cd hello_rust
cargo run
```

Cargo 会创建如下结构：

```
hello_rust/
├── Cargo.toml    # 项目配置和依赖
└── src/
    └── main.rs   # 源代码
```

---

## 三、基础语法

### 变量与可变性

```rust
// 默认不可变（immutable）
let x = 5;
// x = 6;  // ❌ 编译错误

// mut 关键字使其可变
let mut y = 5;
y = 6;  // ✅

// 常量（必须标注类型）
const MAX_POINTS: u32 = 100_000;

// 变量遮蔽（shadowing）
let name = "小明";
let name = name.len();  // 同名新变量，类型可变
```

### 基本类型

```rust
// 标量类型
let i: i32 = -42;       // 有符号整数（i8/i16/i32/i64/i128）
let u: u32 = 42;        // 无符号整数（u8/u16/u32/u64/u128）
let f: f64 = 3.14;      // 浮点数（f32/f64）
let b: bool = true;     // 布尔
let c: char = '中';     // 字符（Unicode，4字节）

// 复合类型
let tup: (i32, f64, u8) = (500, 6.4, 1);
let (x, y, z) = tup;    // 解构
let first = tup.0;      // 索引访问

let arr: [i32; 3] = [1, 2, 3];
let slice = &arr[0..2]; // 切片
```

### 控制流

```rust
// if 表达式（返回值，无需括号）
let score = 85;
let grade = if score >= 90 { "A" }
            else if score >= 80 { "B" }
            else { "C" };

// loop 循环（可返回值）
let mut counter = 0;
let result = loop {
    counter += 1;
    if counter == 10 { break counter * 2; }
};

// while
while counter > 0 { counter -= 1; }

// for（最常用）
for i in 0..5 { println!("{}", i); }          // 0,1,2,3,4
for i in 0..=5 { println!("{}", i); }         // 0,1,2,3,4,5
for item in &vec { println!("{}", item); }    // 遍历集合
```

---

## 四、所有权（Ownership）— Rust 的核心

这是 Rust 最难但也最精髓的概念。记住三条规则：

1. **每个值在 Rust 中都有一个所有者（owner）**
2. **同一时间只能有一个所有者**
3. **当所有者离开作用域，值被自动释放**

```rust
// 移动（Move）
let s1 = String::from("hello");
let s2 = s1;           // s1 的所有权移动到 s2
// println!("{}", s1); // ❌ 编译错误，s1 已失效

// 克隆（Clone）— 深拷贝
let s1 = String::from("hello");
let s2 = s1.clone();   // 复制堆上的数据
println!("{} {}", s1, s2); // ✅

// 引用（Reference）— 不转移所有权
fn calc_len(s: &String) -> usize {  // 不可变引用
    s.len()
}  // s 离开作用域，但不释放 String
let s1 = String::from("hello");
let len = calc_len(&s1);
println!("{} 的长度是 {}", s1, len); // ✅ s1 仍然有效

// 可变引用（同一时间只能有一个）
fn change(s: &mut String) {
    s.push_str(" world");
}
let mut s = String::from("hello");
change(&mut s);
```

### 借用规则

- 可以有**任意多个**不可变引用（`&T`）
- 只能有**一个**可变引用（`&mut T`）
- 不可变引用和可变引用**不能同时存在**

```rust
let mut s = String::from("hello");
let r1 = &s;      // ✅
let r2 = &s;      // ✅
// let r3 = &mut s; // ❌ 已有不可变引用
println!("{} {}", r1, r2);
let r3 = &mut s;  // ✅ r1, r2 不再使用
```

---

## 五、结构体与枚举

```rust
// 结构体
struct User {
    name: String,
    age: u8,
    email: String,
}

let u = User {
    name: String::from("小明"),
    age: 25,
    email: String::from("ming@example.com"),
};

// 结构体方法
impl User {
    fn greet(&self) -> String {
        format!("你好，我是{}", self.name)
    }

    fn new(name: &str, age: u8) -> Self {
        User {
            name: String::from(name),
            age,
            email: String::new(),
        }
    }
}

// 枚举
enum WebEvent {
    PageLoad,
    KeyPress(char),
    Click { x: i64, y: i64 },
}

// Option 和 Result（Rust 没有 null）
// Option<T>：值存在或不存在
fn divide(n: f64, d: f64) -> Option<f64> {
    if d == 0.0 { None }
    else { Some(n / d) }
}

// Result<T, E>：可恢复的错误
fn read_file(path: &str) -> Result<String, std::io::Error> {
    std::fs::read_to_string(path)
}
```

---

## 六、错误处理

```rust
// 用 match 处理 Result
let file = match std::fs::File::open("hello.txt") {
    Ok(f) => f,
    Err(e) => panic!("打开文件失败: {}", e),
};

// ? 运算符（简洁传播错误）
fn read_username() -> Result<String, std::io::Error> {
    let mut content = String::new();
    std::fs::File::open("user.txt")?.read_to_string(&mut content)?;
    Ok(content)
}
```

---

## 七、常用工具：Cargo

```bash
cargo new project    # 新建项目
cargo build          # 编译（debug）
cargo build --release # 编译（release，优化后）
cargo run            # 执行
cargo test           # 运行测试
cargo check          # 快速检查能否编译（不生成二进制）
cargo add serde      # 添加依赖（Rust 1.95+）
cargo doc --open     # 生成文档并在浏览器打开
```

### Cargo.toml 示例

```toml
[package]
name = "my_app"
version = "0.1.0"
edition = "2024"

[dependencies]
serde = { version = "1", features = ["derive"] }
reqwest = "0.12"
tokio = { version = "1", features = ["full"] }

[dev-dependencies]
criterion = "0.5"
```

---

## 八、新手常犯错误

1. **借用和生命周期** — 最常见的编译错误来源，多写几次就习惯了
2. **忘记 `mut`** — 默认不可变，需要修改记得加 `mut`
3. **字符串类型混淆** — `&str`（字符串切片）和 `String`（拥有型字符串）不同
4. **unwrap 过度** — 学习阶段可用 `unwrap()`，但正式代码应优雅处理错误
5. **试图同时借用可变和不可变** — 编译器会阻止，通常需要缩小借用范围

---

## 九、学习资源

| 资源 | 地址 |
|------|------|
| The Book（官方教材） | [doc.rust-lang.org/book](https://doc.rust-lang.org/book/) |
| Rust by Example | [doc.rust-lang.org/rust-by-example](https://doc.rust-lang.org/rust-by-example/) |
| Rustlings（交互练习） | [github.com/rust-lang/rustlings](https://github.com/rust-lang/rustlings) |
| Rust 语言圣经（中文） | [course.rs](https://course.rs/) |
| Rust 标准库文档 | [doc.rust-lang.org/std](https://doc.rust-lang.org/std/) |

---

**总结：** Rust 的学习曲线确实比较陡峭，尤其所有权和生命周期是其他语言没有的概念。但一旦掌握了这些，你会发现自己写出的代码质量显著提升。建议先通读 The Book 前 6 章，然后用 Rustlings 做练习巩固。

Rust 编译器的错误提示是最好的老师 — 遇到错误时，先仔细看编译器说了什么，它往往已经告诉了你正确答案。
