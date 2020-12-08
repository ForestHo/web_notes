# Golang 学习笔记

#### Go 语言的特点

- 静态类型、编译型的开源语言

- 运行效率和开发效率高，自带 GC

- 执行性能好，(1.9 版本开始接近 Java)

- 语言层面原生支持并发，易于利用多核实现并发

- 脚本化的语法，支持多种编程范式(函数式&&面向对象)

- 内置 runtime 运行时(性能监控，GC 等)

- 简单易学，丰富的标准库，强大的网络库，部署简单

- 内置强大的工具(gofmt)，跨平台编译，内嵌 C 支持

#### Go 语言有哪些应用

- 服务器编程，如处理日志、数据打包、虚拟机处理、文件系统等
- 分布式系统，数据库代理器，中间件等
- 网络编程，web 应用、API 应用等
- 云平台，目前云平台逐步采用 Go 实现

#### Go 语言的优势

- 静态类型+编译型，程序运行速度快(相对于动态类型+解释型)
- 原生的支持并发编程(降低开发维护成本，程序更好的执行)

#### Go 语言的劣势

- 语法糖并没有 Python 和 Ruby 那么多
- 目前的程序运行速度还不及 C，赶超 C++和 Java
- 第三方函数库暂时没有主流的编程语言那么多

#### Go 语言发展前景

- 天生支持并发

- 执行性能好

- 真正的企业级编程语言(Java、Go)

- 语法简单易学

- 项目转型首选语言

- 21 世纪的 C 语言

- 开发效率高

### Go 语言开发环境搭建

- 安装 Go 开发包

- 下载，Go 镜像站 https://golang.google.cn/dl/

- goroot,gopath 环境变量配置

- go 语言目录结构

### Go 语言命令行工具

- go build:用于编译源码文件、代码包、依赖包
- go run:可以编译并运行 Go 源码文件
- go get:主要用来动态获取远程代码包
- 支持跨平台编译 ,如:CGO_ENABLED=0 GOOS=windows GOARCH=amd64 go build main.go

### Go语言的设计初衷

- 针对其他语言的痛点进行设计
- 并加入并发编程
- 为大数据，微服务，并发而生的通用编程语言

Go语言与转型

- 项目转型首选语言
- 软件工程师转型，添加技术栈的首选语言
- 这是一门为转型量身定制的课程

Go语言很特别

- 没有"对象"，没有继承多态、没有泛型，没有try/catch
- 有接口，函数式编程，CSP并发模型(goroutine+channel)
- 学习Go语言很简单，因为语法简单
- 用好Go语言不容易，因为要调整三观

![image-20201205213413775](/Users/joseph/WebstormProjects/web_notes/Golang/golang.assets/image-20201205213413775.png)

![image-20201205213537427](/Users/joseph/WebstormProjects/web_notes/Golang/golang.assets/image-20201205213537427.png)

### 实战项目

- 从0开始，使用Go语言自主搭建简单分布式爬虫
- 爬取相亲网站资料

![image-20201205213824286](/Users/joseph/WebstormProjects/web_notes/Golang/golang.assets/image-20201205213824286.png)

![image-20201205213847255](/Users/joseph/WebstormProjects/web_notes/Golang/golang.assets/image-20201205213847255.png)

单任务版=>并发版=>分布式版

### 编写第一个 go 程序

```go
package main
//导入包
import (
	"fmt"
)
func main(){
	fmt.Print("hello world")
}
//main函数没有参数和返回值
```

