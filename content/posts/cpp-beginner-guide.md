+++
date = '2026-06-06T17:30:00+08:00'
draft = false
title = '现代 C++ 新手入门教程'
tags = ['C++', '编程入门', '教程']
+++

C++ 是 1985 年诞生的系统级编程语言，至今仍是性能要求最高的应用场景的首选。2026 年，C++ 已经发展到 **C++26** 标准，现代 C++（C++11 及以后）与旧版 C++ 几乎是两种语言。

<!--more-->

---

## 为什么选 C++？

- **极致性能** — 零开销抽象，手动内存管理，适合游戏引擎、高频交易、嵌入式等
- **硬件控制** — 指针、位运算、内联汇编，极致底层
- **庞大生态** — Qt、Unreal Engine、LLVM、TensorFlow 等都用 C++
- **跨平台** — 一次编写，随处编译
- **与 C 兼容** — 可以直接调用 C 库

---

## 一、安装

### 编译器选择

| 平台 | 推荐编译器 | 安装方式 |
|------|-----------|---------|
| Windows | MSVC (Visual Studio) | 安装 Visual Studio Community，选择"使用 C++ 的桌面开发" |
| Windows | MinGW-w64 (GCC) | `winget install mingw` 或下载 [msys2](https://www.msys2.org/) |
| macOS | Clang | 安装 Xcode Command Line Tools：`xcode-select --install` |
| Linux | GCC | `sudo apt install g++` (Ubuntu) / `sudo dnf install gcc-c++` (Fedora) |

验证安装：

```bash
g++ --version
# g++ (GCC) 14.0.0
```

---

## 二、第一个程序

```cpp
#include <iostream>  // 输入输出库

int main() {
    std::cout << "你好，世界！" << std::endl;
    return 0;  // 返回 0 表示成功
}
```

编译运行：

```bash
g++ -std=c++26 hello.cpp -o hello
./hello
# 你好，世界！
```

也可以用 C++20 标准编译：

```bash
g++ -std=c++20 hello.cpp -o hello
```

---

## 三、基础语法

### 变量与类型

```cpp
#include <iostream>

int main() {
    // 基本类型
    int age = 25;                // 整数（4字节）
    double price = 99.99;        // 双精度浮点
    float pi = 3.14159f;         // 单精度浮点（需 f 后缀）
    char grade = 'A';            // 字符
    bool is_ok = true;           // 布尔
    unsigned int count = 100;    // 无符号整数

    // 自动类型推断（C++11）
    auto result = 3.14 * 2;      // double

    // constexpr（编译期常量，C++11）
    constexpr int MAX_SIZE = 100;

    // 输出
    std::cout << "年龄: " << age << std::endl;

    return 0;
}
```

### 字符串

现代 C++ 强烈推荐使用 `std::string` 而非 C 风格字符串。

```cpp
#include <string>

std::string name = "小明";
std::string greeting = name + "你好";   // 拼接
std::cout << name.length();           // 长度
std::cout << name[0];                 // 字符访问

// 字符串字面量（C++14）
auto s = "Hello World"s;             // s 后缀表示 std::string
```

### 控制流

```cpp
// if
if (score >= 60) {
    std::cout << "及格";
} else if (score >= 90) {
    std::cout << "优秀";
} else {
    std::cout << "不及格";
}

// switch
switch (day) {
    case 1: std::cout << "周一"; break;
    case 2: std::cout << "周二"; break;
    default: std::cout << "其他";
}

// for
for (int i = 0; i < 5; i++) {
    std::cout << i << " ";
}

// 范围 for（C++11）
for (const auto& item : vec) {
    std::cout << item << " ";
}

// while
int i = 0;
while (i < 5) { i++; }
```

---

## 四、容器（STL）

C++ 标准模板库提供了丰富的容器。现代 C++ 中**优先使用 STL 容器**，不要自己造轮子。

```cpp
#include <vector>
#include <map>
#include <set>
#include <unordered_map>

// vector（动态数组）
std::vector<int> nums = {1, 2, 3};
nums.push_back(4);          // 末尾添加
nums.pop_back();            // 末尾删除
nums.size();                // 大小

// map（有序映射）
std::map<std::string, int> scores;
scores["小明"] = 90;
scores["小红"] = 85;

// unordered_map（哈希表，更快）
std::unordered_map<std::string, int> cache;
cache["key"] = 42;

// set（有序集合）
std::set<int> unique_nums = {3, 1, 4, 1, 5};  // {1, 3, 4, 5}
```

---

## 五、函数

```cpp
// 普通函数
int add(int a, int b) {
    return a + b;
}

// 引用传参（避免拷贝）
void change(std::string& s) {
    s += " world";
}

// const 引用（只读，不拷贝）
void print(const std::vector<int>& v) {
    for (auto x : v) std::cout << x;
}

// 默认参数
void greet(std::string name = "匿名") {
    std::cout << "你好，" << name;
}

// Lambda 表达式（C++11）
auto square = [](int x) { return x * x; };
std::cout << square(5);  // 25

// 泛型 Lambda（C++14）
auto add = [](auto a, auto b) { return a + b; };
```

---

## 六、类与面向对象

```cpp
class Student {
private:
    std::string name;
    int age;

public:
    // 构造函数
    Student(const std::string& n, int a)
        : name(n), age(a) {}  // 初始化列表

    // 成员函数
    void say_hello() const {  // const 表示不修改成员
        std::cout << "你好，我是" << name << std::endl;
    }

    // getter/setter
    std::string get_name() const { return name; }
    void set_age(int a) { age = a; }

    // 静态成员
    static int count;
};
int Student::count = 0;
```

### 现代 C++ 特性

```cpp
// 智能指针（C++11）— 自动管理内存，避免 new/delete
#include <memory>

// unique_ptr：独占所有权
auto ptr = std::make_unique<int>(42);

// shared_ptr：共享所有权（引用计数）
auto ptr1 = std::make_shared<int>(42);
auto ptr2 = ptr1;  // 引用计数 +1

// 移动语义（C++11）
std::vector<int> create_vec() {
    std::vector<int> v = {1, 2, 3};
    return v;  // 自动移动而非拷贝（NRVO / move）
}

// 可选值（C++17）
#include <optional>
std::optional<int> find_user(int id) {
    if (id == 1) return 42;
    return std::nullopt;  // 无值
}

// 结构化绑定（C++17）
auto [name, age] = std::make_pair("小明", 25);
```

---

## 七、新手常见陷阱

### 1. 使用 new/delete（❌ 不要这样做）

```cpp
// ❌ 旧风格：手动管理
auto ptr = new int(42);
delete ptr;

// ✅ 现代 C++：智能指针
auto ptr = std::make_unique<int>(42);
```

### 2. 原始数组

```cpp
// ❌ 旧风格
int arr[10];

// ✅ 现代 C++：std::array 或 std::vector
std::array<int, 10> arr;      // 固定大小
std::vector<int> vec;          // 动态大小
```

### 3. 传递大对象的值拷贝

```cpp
// ❌ 拷贝整个向量
void process(std::vector<int> v);

// ✅ const 引用
void process(const std::vector<int>& v);
```

### 4. 未定义行为

```cpp
int* p = nullptr;
*p = 42;  // ❌ 解引用空指针

int arr[5];
arr[10] = 1;  // ❌ 越界访问

int& ref = *new int(42);
delete &ref;
// ref 现在是悬垂引用 ❌
```

### 5. 头文件重复包含

```cpp
// ✅ 使用头文件保护
#ifndef MY_HEADER_H
#define MY_HEADER_H
// ...
#endif

// 或更简单的（大多数编译器支持）
#pragma once
```

---

## 八、推荐的编译标志

```bash
# 推荐开启的警告
g++ -std=c++26 -Wall -Wextra -Wpedantic -Werror \
    -O2 -fsanitize=address main.cpp -o main
```

- `-Wall -Wextra -Wpedantic`：开启警告
- `-Werror`：把警告当错误
- `-O2`：优化级别
- `-fsanitize=address`：检测内存错误（调试时）

---

## 九、构建工具

现代 C++ 项目建议使用 CMake：

```cmake
cmake_minimum_required(VERSION 3.28)
project(my_project)

set(CMAKE_CXX_STANDARD 26)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_executable(my_app main.cpp)
```

```bash
cmake -B build
cmake --build build
./build/my_app
```

---

## 十、学习资源

| 资源 | 地址 |
|------|------|
| cppreference.com（权威参考） | [cppreference.com](https://cppreference.com) |
| LearnCpp（最佳教程网站） | [learncpp.com](https://learncpp.com) |
| C++ 核心指南 | [isocpp.github.io/CppCoreGuidelines](https://isocpp.github.io/CppCoreGuidelines/) |
| C++ 之旅（Bjarne Stroustrup 著） | 经典入门书 |
| Effective Modern C++（Scott Meyers 著） | 进阶必读 |

---

**总结：** C++ 是一门大而全的语言，学习曲线很陡。建议先从 C++20/23 的现代特性入手，**不要**先学 C 风格的那一套（裸指针、C 风格字符串、宏）。用好 `std::vector`、`std::string`、智能指针，你就能写出比 C++98 时代安全得多的代码。

记住：**现代 C++ 的核心原则是 RAII（资源获取即初始化）** — 用对象的构造函数获取资源，析构函数自动释放。这比手动 `new/delete` 安全 100 倍。
