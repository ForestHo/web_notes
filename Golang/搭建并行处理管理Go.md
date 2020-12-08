### 搭建并行处理管道，感受Go语言的魅力

#### Go语言的项目

###### 完全使用Go语言

- Docker
- Kubernetes
- Caddy
- CockroachDB

###### 部分使用Go语言

- MongoDB/Couchbase
- Dropbox
- Uber
- Google

Go语言的历史

- 2009年开始开源项目
- 2012年发布1.0版
- 2015年发布1.5版，自编译、重写垃圾回收器，更好的并发
- 现在, 1.9版本

Google内部的"标准"编程语言

- C++:必须有性能保障的部分，如搜索引擎
- Java:复杂业务逻辑，如adwords，google docs
- Python:大量内部工具
- Go: 新的内部工具，及其他业务模块，如dl.google.com

###### Go语言的设计初衷

- 如果有一门语言，有像C/C++那样的性能，可以做系统开发
- 但是没有繁琐的类型系统，有简单统一化的模块依赖管理，编译速度飞快
- 如果有一门语言，像Java那样拥有垃圾回收
- 但是更快，对业务的影响更小
- 如果有一门语言，像Python那样简单易学，拥有灵活的类型，支持函数式编程，异步IO
- 但是有编译器进行静态类型检查
- 如果有一门语言，针对上述痛点进行设计，并加入并发编程
- 这就是Go语言

#### Go语言的归类

- 类型检查: 编译时
- 运行环境: 编译成机器代码直接运行
- 编程范式: 面向接口，函数式编程，并发编程

Go语言并发编程

-  采用CSP(Communication Sequential Process)模型
- 不需要锁，不需要callback
- 并发编程 vs 并行计算

Go语言的安装与开发环境

- 下载 https://studygolang.com/dl
- 开发环境:vi,emacs,idea,eclipse,vs,sublime....+go插件
- IDE:Jetbean Goglang(https://www.jetbrains.com/go/)
- 本课程使用Idea+go插件,https://tech.souyunku.com/?p=30970

#### go demo

- go语言没有引用类型，只有值类型和指针类型

- ```go
  package main
  
  import (
  	"fmt"
  	"net/http"
  )
  
  func main() {
  	http.HandleFunc("/", func(
  		writer http.ResponseWriter,
  		request *http.Request) {
  		fmt.Fprintf(writer,"<h1>hello world %s </h1>",request.FormValue("name"))
  	})
  	http.ListenAndServe(":8888",nil)
  }
  ```

- 

- ![image-20201205233614014](/Users/joseph/WebstormProjects/web_notes/Golang/搭建并行处理管理Go.assets/image-20201205233614014.png)

