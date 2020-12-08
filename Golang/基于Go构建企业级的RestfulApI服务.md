### 基于 Go 语言构建企业级的 RESTful API 服务

本小册为了演示，构建了一个账号系统（后面统称为`apiserver`），功能如下：

- API 服务器状态检查
- 登录用户
- 新增用户
- 删除用户
- 更新用户
- 获取指定用户的详细信息
- 获取用户列表

#### RESTful API 介绍

###### 什么是 API

- API（Application Programming Interface，应用程序编程接口）是一些预先定义的函数或者接口，目的是提供应用程序与开发人员基于某软件或硬件得以访问一组例程的能力，而又无须访问源码，或理解内部工作机制的细节。

  要实现一个 API 服务器，首先要考虑两个方面：**API 风格和媒体类型**。Go 语言中常用的 API 风格是 **RPC 和 REST**，常用的媒体类型是 **JSON、XML 和 Protobuf**。在 Go API 开发中常用的组合是 gRPC+Protobuf 和 REST+JSON。

###### REST简介

- REST 代表**表现层状态转移**（REpresentational State Transfer），由 Roy Fielding 在他的 [论文](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm) 中提出。**REST 是一种架构风格，指的是一组架构约束条件和原则**。满足这些约束条件和原则的应用程序或设计就是 RESTful。**REST 规范把所有内容都视为资源，网络上一切皆资源**。REST 架构对资源的操作包括**获取、创建、修改和删除**，资源的操作正好对应 HTTP 协议提供的 GET、POST、PUT 和 DELETE 方法。HTTP 动词与 REST 风格 CRUD 对应关系：
- ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a095d726d67a45dbae63a7d1480d6a27~tplv-k3u1fbpfcp-zoom-1.image)

###### RPC简介

- RPC 全称 Remote Procedure Call（远程过程调用），是一种**进程间通信方式**。它允许程序调用另一个地址空间（通常是共享网络的另一台机器上）的过程或函数，而不用程序员显式编码这个远程调用的细节。即程序员无论是调用本地的还是远程的，本质上编写的调用代码基本相同。
- RPC 这个概念在 20 世纪 80 年代由 Bruce Jay Nelson 提出。在 Nelson 的论文 "Implementing Remote Procedure Calls" 中我们了解到 RPC 具有如下优点：
  1. 简单：RPC 概念的语义十分清晰和简单，这样建立分布式计算就更容易
  2. 高效：过程调用看起来十分简单而且高效
  3. 通用：在单机计算中过程往往是不同算法部分间最重要的通信机制

- 简单地说，就是一般程序员对于本地的过程调用很熟悉，我们把 RPC 做成和本地调用完全类似，那么就更容易被接受，使用起来毫无障碍。

###### RPC 结构：

Nelson 的论文中指出实现 RPC 的程序包括 5 个部分：

- User
- User-stub
- RPCRuntime
- Server-stub
- Server

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/78d54556e16845b7b7ceaf376eec9ceb~tplv-k3u1fbpfcp-zoom-1.image)

- 这里 User 就是 client 端，当 User 想发起一个远程调用时，它实际是通过本地调用 User-stub。User-stub 负责将调用的接口、方法和参数通过约定的协议规范进行编码并通过本地的 RPCRuntime 实例传输到远端的实例。远端 RPCRuntime 实例收到请求后交给 Server-stub 进行解码后发起本地端调用，调用结果再返回给 User 端。

###### REST vs RPC

在做 API 服务器开发时，很多人都会遇到这个问题 —— 选择 REST 还是 RPC。RPC 相比 REST 的优点主要有 3 点：

- RPC+Protobuf 采用的是 **TCP** 做传输协议，REST 直接使用 **HTTP** 做应用层协议，这种区别导致 **REST 在调用性能上会比 RPC+Protobuf 低**

- RPC 不像 REST 那样，**每一个操作都要抽象成对资源的增删改查**，在实际开发中，**有很多操作很难抽象成资源**，比如**登录操作**。所以在实际开发中并不能严格按照 REST 规范来写 API，RPC 就不存在这个问题

- RPC 屏蔽网络细节、易用，和本地调用类似

- ```json
  这里的易用指的是调用方式上的易用性。在做 RPC 开发时，开发过程很烦琐，需要先写一个 DSL 描述文件，然后用代码生成器生成各种语言代码，当描述文件有更改时，必须重新定义和编译，维护性差。
  ```

但是 REST 相较 RPC 也有很多优势：

- 轻量级，简单易用，维护性和扩展性都比较好
- REST 相对更规范，更标准，更通用，无论哪种语言都支持 HTTP 协议，可以对接外部很多系统，只要满足 HTTP 调用即可，更适合对外，RPC 会有语言限制，不同语言的 RPC 调用起来很麻烦
- JSON 格式可读性更强，开发调试都很方便
- 在开发过程中，如果严格按照 REST 规范来写 API，API 看起来更清晰，更容易被大家理解

其实业界普遍采用的做法是，**内部系统之间调用用 RPC，对外用 REST**，因为内部系统之间可能调用很频繁，需要 RPC 的高性能支撑。对外用 REST 更易理解，更通用些。当然以现有的服务器性能，如果两个系统间调用不是特别频繁，对性能要求不是非常高，以笔者的开发经验来看，REST 的性能完全可以满足。本小册不是讨论微服务，所以不存在微服务之间的高频调用场景，此外 REST 在实际开发中，能够满足绝大部分的需求场景，所以 RPC 的性能优势可以忽略，相反基于 REST 的其他优势，笔者更倾向于用 REST 来构建 API 服务器，本小册正是用 REST 风格来构建 API 的。

### 媒体类型选择

媒体类型是独立于平台的类型，设计用于分布式系统间的通信，媒体类型用于传递信息，一个正式的规范定义了这些信息应该如何表示。HTTP 的 REST 能够提供多种不同的响应形式，常见的是 XML 和 JSON。JSON 无论从形式上还是使用方法上都更简单。相比 XML，JSON 的内容更加紧凑，数据展现形式直观易懂，开发测试都非常方便，所以在媒体类型选择上，选择了 **JSON** 格式，这也是很多大公司所采用的格式。



### 小结

本小节介绍了软件架构中 API 的实现方式，并简单介绍了相应的技术，通过对比，得出本小册所采用的实现方式：**API 风格采用 REST，媒体类型选择 JSON**。通过本小节的学习，读者可以了解小册所构建 API 服务器核心技术的选型和原因。

### API 流程和代码结构

为了使读者在开始实战之前对 API 开发有个整体的了解，这里选择了两个流程来介绍：

- HTTP API 服务器启动流程
- HTTP 请求处理流程

本小节也提前给出了程序代码结构图，让读者从宏观上了解将要构建的 API 服务器的功能。

#### HTTP API 服务器启动流程

![image-20201208221123530](/Users/joseph/WebstormProjects/web_notes/Golang/基于Go构建企业级的RestfulApI服务.assets/image-20201208221123530.png)

如上图，在启动一个 API 命令后，API 命令会首先加载配置文件，根据配置做后面的处理工作。通常会将日志相关的配置记录在配置文件中，在解析完配置文件后，就可以加载日志包初始化函数，来初始化日志实例，供后面的程序调用。接下来会初始化数据库实例，建立数据库连接，供后面对数据库的 CRUD 操作使用。在建立完数据库连接后，需要设置 HTTP，通常包括 3 方面的设置：

- **设置 Header**
- **注册路由**
- **注册中间件**

之后会调用 `net/http` 包的 `ListenAndServe()` 方法启动 HTTP 服务器。

在启动 HTTP 端口之前，程序会 go 一个**协程**，来ping HTTP 服务器的 `/sd/health` 接口，如果程序成功启动，ping 协程在 timeout 之前会成功返回，如果程序启动失败，则 ping 协程最终会 timeout，并终止整个程序。

```html
解析配置文件、初始化 Log 、初始化数据库的顺序根据自己的喜好和需求来排即可。
```

Go 协程 (goroutine) 是指在后台中运行的轻量级执行线程，go 协程是 Go 中实现并发的关键组成部分。

### HTTP 请求处理流程

![image-20201208222025766](/Users/joseph/WebstormProjects/web_notes/Golang/基于Go构建企业级的RestfulApI服务.assets/image-20201208222025766.png)

一次完整的 HTTP 请求处理流程如上图所示。（图片出自[《HTTP 权威指南》](https://book.douban.com/subject/10746113/)，推荐想全面理解 HTTP 的读者阅读此书。）

1.**建立连接**

客户端发送 HTTP 请求后，服务器会根据域名进行域名解析，就是将网站名称转变成 IP 地址：localhost -> 127.0.0.1，Linux hosts文件、DNS 域名解析等可以实现这种功能。之后通过发起 TCP 的三次握手建立连接。TCP 三次连接请参考 [TCP 三次握手详解及释放连接过程](https://blog.csdn.net/oney139/article/details/8103223)，建立连接之后就可以发送 HTTP 请求了。

2.**接收请求**

**HTTP 服务器软件进程**，这里指的是 API 服务器，在接收到请求之后，首先根据 HTTP **请求行**的信息来解析到 **HTTP 方法和路径**，在上图所示的报文中，方法是 `GET`，路径是 `/index.html`，之后根据 **API 服务器注册的路由信息**（大概可以理解为：**HTTP 方法 + 路径和具体处理函数的映射**）找到具体的处理函数。

3.**处理请求**

在接收到请求之后，API 通常会解析 **HTTP 请求报文**获取**请求头**和**消息体**，然后根据这些信息进行**相应的业务处理**，HTTP 框架一般都有**自带的解析函数**，只需要输入 HTTP 请求报文，就可以解析到需要的请求头和消息体。通常情况下，**业务逻辑处理**可以分为两种：**包含对数据库的操作**和**不包含对数据的操作**。大型系统中通常两种都会有：

- **包含对数据库的操作**：需要访问数据库（增删改查），然后获取指定的数据，对数据处理后构建指定的响应结构体，返回响应包。数据库通常用的是 MySQL，因为免费，功能和性能也都能满足企业级应用的要求。
- **不包含对数据库的操作**：进行业务逻辑处理后，构建指定的响应结构体，返回响应包。

4.**记录事务处理过程**

在业务逻辑处理过程中，需要记录一些关键信息，方便后期 Debug 用。在 Go 中有各种各样的日志包可以用来记录这些信息。

### HTTP 请求和响应格式介绍

一个 **HTTP 请求报文**由**请求行（request line）、请求头部（header）、空行和请求数据**四部分组成，下图是请求报文的一般格式。

![image-20201208222923112](/Users/joseph/WebstormProjects/web_notes/Golang/基于Go构建企业级的RestfulApI服务.assets/image-20201208222923112.png)

- 第一行必须是一个请求行（request line），用来说明请求类型、要访问的资源以及所使用的 HTTP 版本

- 紧接着是一个头部（header）小节，用来说明服务器要使用的附加信息

- 之后是一个空行

- 再后面可以添加任意的其他数据（称之为主体：body）

  ```
  HTTP 响应格式跟请求格式类似，也是由 4 个部分组成：状态行、消息报头、空行和响应数据。
  ```

### 目录结构

```markdown
├── admin.sh                     # 进程的start|stop|status|restart控制文件
├── conf                         # 配置文件统一存放目录
│   ├── config.yaml              # 配置文件
│   ├── server.crt               # TLS配置文件
│   └── server.key
├── config                       # 专门用来处理配置和配置文件的Go package
│   └── config.go                 
├── db.sql                       # 在部署新环境时，可以登录MySQL客户端，执行source db.sql创建数据库和表
├── docs                         # swagger文档，执行 swag init 生成的
│   ├── docs.go
│   └── swagger
│       ├── swagger.json
│       └── swagger.yaml
├── handler                      # 类似MVC架构中的C，用来读取输入，并将处理流程转发给实际的处理函数，最后返回结果
│   ├── handler.go
│   ├── sd                       # 健康检查handler
│   │   └── check.go 
│   └── user                     # 核心：用户业务逻辑handler
│       ├── create.go            # 新增用户
│       ├── delete.go            # 删除用户
│       ├── get.go               # 获取指定的用户信息
│       ├── list.go              # 查询用户列表
│       ├── login.go             # 用户登录
│       ├── update.go            # 更新用户
│       └── user.go              # 存放用户handler公用的函数、结构体等
├── main.go                      # Go程序唯一入口
├── Makefile                     # Makefile文件，一般大型软件系统都是采用make来作为编译工具
├── model                        # 数据库相关的操作统一放在这里，包括数据库初始化和对表的增删改查
│   ├── init.go                  # 初始化和连接数据库
│   ├── model.go                 # 存放一些公用的go struct
│   └── user.go                  # 用户相关的数据库CURD操作
├── pkg                          # 引用的包
│   ├── auth                     # 认证包
│   │   └── auth.go
│   ├── constvar                 # 常量统一存放位置
│   │   └── constvar.go
│   ├── errno                    # 错误码存放位置
│   │   ├── code.go
│   │   └── errno.go
│   ├── token
│   │   └── token.go
│   └── version                  # 版本包
│       ├── base.go
│       ├── doc.go
│       └── version.go
├── README.md                    # API目录README
├── router                       # 路由相关处理
│   ├── middleware               # API服务器用的是Gin Web框架，Gin中间件存放位置
│   │   ├── auth.go 
│   │   ├── header.go
│   │   ├── logging.go
│   │   └── requestid.go
│   └── router.go
├── service                      # 实际业务处理函数存放位置
│   └── service.go
├── util                         # 工具类函数存放目录
│   ├── util.go 
│   └── util_test.go
└── vendor                         # vendor目录用来管理依赖包
    ├── github.com
    ├── golang.org
    ├── gopkg.in
    └── vendor.json
```

Go API 项目中，一般都会包括这些功能项：**Makefile 文件、配置文件目录、RESTful API 服务器的 handler 目录、model 目录、工具类目录、vendor 目录，以及实际处理业务逻辑函数所存放的 service 目录**。这些都在上述的代码结构中有列出，新加功能时将代码放入对应功能的目录/文件中，可以使整个项目代码结构更加清晰，非常有利于后期的查找和维护。

### 小结

本小节通过介绍 **API 服务器启动流程**和 **HTTP 请求处理流程**，来让读者对 API 服务器中的**关键流程**有个宏观的了解，更好地理解 API 服务器是如何工作的。API 服务器源码结构也非常重要，一个好的源码结构通常能让逻辑更加清晰，编写更加顺畅，后期维护更加容易，本小册介绍了笔者倾向的源码组织结构，供读者参考。

## Go API 开发环境配置

### Go 命令安装

Go 有多种安装方式，比如 Go 源码安装、Go 标准包安装、第三方工具（yum、apt-get 等）安装。本小册 API 运行在 Linux 服务器上，选择通过标准包来安装 Go 编译环境。Go 提供了每个平台打好包的一键安装，这些包默认会安装到如下目录：`/usr/local/go`。当然你可以改变它们的安装位置，但是改变之后你必须在你的环境变量中设置如下两个环境变量：

- GOROOT：GOROOT 就是 Go 的安装路径
- GOPATH：GOPATH 是作为**编译后二进制的存放目的地和 import 包时的搜索路径**

假定你想要安装 Go 的目录为 `$GO_INSTALL_DIR`，后面替换为相应的目录路径，安装步骤如下。

1.下载安装包

安装包下载地址为 [golang.org](https://golang.org/dl/)，如果打不开可以使用这个地址：[golang.google.cn](https://golang.google.cn/dl/)。

Linux 版本选择 goxxxxx.linux-amd64.tar.gz 格式的安装包，这里在 Linux 服务器上直接用 `wget` 命令下载：

```shell
wget https://dl.google.com/go/go1.10.2.linux-amd64.tar.gz
```

2.设置安装目录

```shell
export GO_INSTALL_DIR=$HOME
```

这里我们安装到用户主目录下。

3.解压Go安装包

```shell
tar -xvzf go1.10.2.linux-amd64.tar.gz -C $GO_INSTALL_DIR
```

4.设置环境变量

```shell
$ export GO_INSTALL_DIR=$HOME
$ export GOROOT=$GO_INSTALL_DIR/go
$ export GOPATH=$HOME/mygo
$ export PATH=$GOPATH/bin:$PATH:$GO_INSTALL_DIR/go/bin
```

如果不想每次登录系统都设置一次环境变量，可以将上面 4 行追加到 `$HOME/.bashrc` 文件中。

5.执行 go version 检查 Go 是否成功安装

```shell
$ go version
go version go1.10.2 linux/amd64
```

6.创建 $GOPATH/src 目录

$GOPATH/src 是 Go 源码存放的目录，所以在正式开始编码前要先确保 $GOPATH/src 目录存在，执行命令：

```shell
mkdir -p $GOPATH/src
```

### Vim **配置**

因为 Vim 是 Linux 下开发的最基本工具，为了通用这里基于 Vim 来配置开发环境。如果要配置一个 Vim IDE 有很多步骤需要一步一步去做，笔者调研了很多 Go vim ide 的配置方法，编写了一个安装工具，这里直接用该工具来配置，具体配置步骤如下。

1.下载 Vim 配置工具

```shell
git clone https://github.com/lexkong/lexVim
```

2.进入 lexVim 目录，下载 go ide 需要的二进制文件：

```shell
$ cd lexVim
$ git clone https://github.com/lexkong/vim-go-ide-bin
```

都是二进制文件，大概有 141MB，请耐心等待 :-)

3.启动安装脚本：

```shell
$ ./start_vim.sh
```

启动后，会进入一个交互环境，依次输入： `1 -> yourname -> youremail@qq.com`，脚本最后输出 `this vim config is success !`说明安装成功。很简单，只需 3 个选择即可安装成功，配置 IDE so easy。

### Vim IDE 常用功能

在 Go 项目开发中最常用的功能是：

- `gd` 或者`ctrl + ]` 跳转到对应的函数定义处
- `ctrl + t` 标签退栈
- `ctrl + o` 跳转到前一个位置
- `<F4>` 最近文件列表
- `<F5>` 在 Vim 的上面打开文件查找窗口
- `<F9>` 生成供函数跳转的 tag
- `<F2>` 打开目录窗口，再按会关闭目录窗口
- `<F6>` 添加函数注释

在代码间跳来跳去，将光标放在某个函数调用上，按 `ctl + ]` 就会跳到函数的定义处，按 `ctrl + o` 就会跳回来。

更多 Go vim ide 功能请参考 [Vim IDE 功能](https://github.com/lexkong/lexVim/blob/master/doc/ide.md)。

### 小结

“工欲善其事，必先利其器。”在开始 Go 开发之前，需要安装基本的 Go 编译工具，设置基本的环境变量。如果有一个顺手的开发工具就更好了。该小节向读者介绍了：

1. 如何安装 Go 编译环境
2. 如何配置 Vim IDE

开头的这 4 小节介绍了 API 开发的一些基本的知识，并做了开发前的准备工作，接下来开始 API 开发实战，一步一步教你构建一个账号管理的 API 服务，满满的干货等你来 Get。