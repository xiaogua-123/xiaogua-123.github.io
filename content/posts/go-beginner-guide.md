+++
date = '2026-06-06T16:30:00+08:00'
draft = false
title = 'Go 语言新手入门教程'
tags = ['Go', '编程入门', '教程']
+++

Go（又称 Golang）是 Google 开发的静态类型、编译型编程语言，以其简洁的语法、高效的并发模型和快速的编译速度而闻名。2026 年，Go 已发展到 1.27 版本。

<!--more-->

---

## 为什么选 Go？

- **语法极简** — 只有 25 个关键字，几小时即可上手
- **编译极快** — 大型项目秒级编译
- **天生并发** — goroutine + channel，比线程轻量得多
- **静态类型** — 编译时检查类型错误
- **标准库强大** — 内置 HTTP 服务、JSON 解析、加密等

---

## 一、安装

### 下载安装

访问 [go.dev/dl](https://go.dev/dl) 下载对应系统的安装包。

验证安装：

```bash
go version
# go version go1.27.0 windows/amd64
```

### 配置环境

```bash
# 查看环境
go env

# 设置 GOPROXY（国内用户推荐）
go env -w GOPROXY=https://goproxy.cn,direct
```

---

## 二、第一个程序

创建文件 `hello.go`：

```go
package main

import "fmt"

func main() {
    fmt.Println("你好，世界！")
}
```

运行：

```bash
go run hello.go
# 你好，世界！
```

编译为可执行文件：

```bash
go build hello.go
# 生成 hello.exe（Windows）或 hello（Linux/macOS）
```

---

## 三、基础语法速览

### 变量与常量

```go
// 变量声明
var name string = "小明"
var age = 25           // 类型推断
count := 10            // 短声明（只能在函数内）

// 常量
const PI = 3.14159

// 多变量声明
var x, y int = 1, 2
```

### 基本类型

```go
bool           // true / false
string         // 字符串
int / int8~64  // 整数
uint / uint8~64// 无符号整数
float32 / float64  // 浮点数
byte           // uint8 别名
rune           // int32 别名，代表 Unicode 码点
```

### 控制流

```go
// if
if score >= 60 {
    fmt.Println("及格")
} else {
    fmt.Println("不及格")
}

// for（Go 没有 while，都用 for）
for i := 0; i < 5; i++ {
    fmt.Println(i)
}
// 类似 while
sum := 1
for sum < 100 {
    sum += sum
}

// switch（不用写 break，默认自动跳出）
switch day {
case "周一", "周二": fmt.Println("工作日")
case "周六", "周日": fmt.Println("周末")
default: fmt.Println("普通日")
}
```

### 数组与切片

```go
// 数组（长度固定）
var arr [3]int = [3]int{1, 2, 3}

// 切片（动态数组，更常用）
nums := []int{1, 2, 3}
nums = append(nums, 4)     // 追加
fmt.Println(nums[1:3])     // 切片 [2, 3]

// 用 make 创建切片
s := make([]int, 5, 10)    // 长度5，容量10
```

### 映射（Map）

```go
scores := map[string]int{
    "小明": 90,
    "小红": 85,
}
scores["小刚"] = 88
delete(scores, "小红")
score, exists := scores["小明"]   // 检查键是否存在
```

### 函数

```go
// 多返回值（Go 的特色）
func divide(a, b int) (int, error) {
    if b == 0 {
        return 0, fmt.Errorf("除数不能为0")
    }
    return a / b, nil
}

// 命名返回值
func sum(nums ...int) (total int) {  // 可变参数
    for _, n := range nums {
        total += n
    }
    return  // 自动返回 total
}
```

---

## 四、结构体与方法

```go
// 定义结构体
type User struct {
    Name string
    Age  int
    Email string
}

// 创建实例
u := User{Name: "小明", Age: 25, Email: "ming@example.com"}
u.Age = 26  // 访问字段

// 方法（在结构体外定义）
func (u User) Greet() string {
    return "你好，我是" + u.Name
}

// 指针接收者（可修改原值）
func (u *User) Birthday() {
    u.Age++
}
```

---

## 五、并发：goroutine 与 channel

这是 Go 最强大的特性。

```go
// goroutine：在函数前加 go 关键字即可
go func() {
    fmt.Println("并发执行")
}()

// channel：goroutine 之间的通信管道
ch := make(chan string)

go func() {
    ch <- "来自goroutine的消息"
}()

msg := <-ch
fmt.Println(msg)  // 来自goroutine的消息
```

### 实际例子：并发下载

```go
func fetch(url string, ch chan<- string) {
    resp, _ := http.Get(url)
    ch <- fmt.Sprintf("%s: %d", url, resp.StatusCode)
}

func main() {
    urls := []string{"https://go.dev", "https://golang.org"}
    ch := make(chan string)

    for _, url := range urls {
        go fetch(url, ch)  // 同时请求多个 URL
    }

    for range urls {
        fmt.Println(<-ch)  // 按完成顺序接收结果
    }
}
```

---

## 六、接口（Interface）

```go
// 定义接口
type Animal interface {
    Speak() string
}

// 实现接口（无需显式声明，只要实现方法即可）
type Dog struct{}
func (d Dog) Speak() string { return "汪汪！" }

type Cat struct{}
func (c Cat) Speak() string { return "喵喵！" }

// 使用接口
func MakeSound(a Animal) {
    fmt.Println(a.Speak())
}
```

---

## 七、常用标准库

| 包 | 用途 |
|----|------|
| `fmt` | 格式化输入输出 |
| `net/http` | HTTP 客户端和服务端 |
| `encoding/json` | JSON 编解码 |
| `io/ioutil` | 文件读写 |
| `time` | 时间和定时器 |
| `sync` | 并发同步（Mutex, WaitGroup）|
| `strings` | 字符串处理 |
| `database/sql` | 数据库操作 |

### 5 分钟写一个 HTTP 服务

```go
package main

import (
    "fmt"
    "net/http"
)

func main() {
    http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
        fmt.Fprintf(w, "你好，世界！")
    })
    http.ListenAndServe(":8080", nil)
}
```

---

## 八、新手常犯错误

1. **未使用的变量和 import** — Go 编译器会报错，必须使用或删除
2. **循环中捕获变量** — 旧版本中 goroutine 内的循环变量需复制
3. **忽略错误返回值** — `_` 或显式处理，不要装作没看见
4. **切片容量误解** — 切片扩容可能产生意外的共享底层数组
5. **map 不是并发安全的** — 并发读写需要加锁或用 `sync.Map`

---

## 九、学习资源

| 资源 | 地址 |
|------|------|
| 官方教程 | [go.dev/learn](https://go.dev/learn) |
| A Tour of Go（交互） | [go.dev/tour](https://go.dev/tour) |
| Effective Go | [go.dev/doc/effective_go](https://go.dev/doc/effective_go) |
| 标准库文档 | [pkg.go.dev/std](https://pkg.go.dev/std) |
| Learn Go with Tests | [github.com/quii/learn-go-with-tests](https://github.com/quii/learn-go-with-tests) |

---

**总结：** Go 是"少即是多"的典范。几个下午就能上手，但它的并发模型和设计哲学值得长期钻研。如果你要写网络服务、CLI 工具或云原生应用，Go 绝对是最佳选择之一。
