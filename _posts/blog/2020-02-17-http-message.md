---
layout: post
title: HTTP 报文
categories: HTTP
description: http 中有关 HTTP 报文的一些知识的学习笔记
keywords: http,https,http 权威指南,http 报文
---

http 权威指南中关于 HTTP 报文 章节的总结学习。  

# HTTP 权威指南

## 第三章 HTTP 报文

### 3.1 报文流

HTTP 报文是在 HTTP 应用程序之间发送的数据块。

#### 3.1.1 报文流入源端服务器

HTTP 使用术语流入（inbound） 和流出（outbound）来描述事务处理（transaction）的方向。报文流入源端服务器，工作完成之后，会流回用户的 Agent 代理中。

#### 3.1.2 报文向下游流动

不管是请求报文还是响应报文，所有报文都会向下游（downstream）流动。所有报文的发送者都在接受者的上游（upstream）。

### 3.2 报文的组成部分

HTTP 报文是简单的格式化数据块。由三个部分组成：

- 对报文进行描述的**起始行**（start line）
- 包含属性的**首部块** （head）
- 可选的，包含数据的**主体**（body）

起始行和首部是由行分隔的 ASCII 文本。每行都以一个由两个字符组成的行终止序列作为结束，其中包括一个回车符（ASCII 码13）和一个换行符（ASCII码10）。这个行终止序列可以写作 CRLF。  

实体的主体或报文的主体是一个可选的数据块。主体中可以包含文本或二进制数据，也可以为空。  

#### 3.2.1 报文的语法

请求报文的格式

```
<method> <request-URL> <version>
<headers>
<entity-body>
```

响应报文的格式：（注意，只有起始行的语法有所不同）

```
<version> <status> <reason-phrase>
<headers>
<entity-body>
```

简要描述：

- 方法 method

    客户端希望服务器对资源执行的动作。  

- 请求 URL request-URL

    命名了所请求资源，或者 URL 路径组件的完整 URL。  

- 版本 version

    报文所使用的 HTTP 版本，其格式：

    HTTP/<major>·<minor>

- 状态吗 status-code

    这三位数字描述了请求过程中所发生的情况。每个状态码的第一位数字都用于描述状态的一般类别。

- 原因短语 reson-phrase

    数字状态码的可读版本，包含行终止序列之前的所有文本。

- 首部 header

    可以有零个或多个首部，每个首部都包含一个名字，后面跟着一个冒号(:)，然后是以恶搞可选的空格，接着是一个值，最后是一个 CRLF。首部是由一个空行 CRLF 结束的，表示了首部列表的结束和实体主体部分的开始。

- 实体的主体部分 entity-body

    实体的主体部分包含一个由任意数据组成的数据块。

#### 3.2.2 起始行

所有 HTTP 报文都以一个起始行作为开始。请求报文的起始行说明了**要做些什么**，响应报文的起始行说明**发生了什么**

##### 1.请求行

请求报文是请求服务器对资源进行一些操作。

##### 2. 响应行

响应报文承载了状态信息和操作产生的所有结果数据。

##### 3. 方法

方法用来告知服务器要做什么。

常用的 HTTP 方法：

|  方法   | 描述                                               | 是否包含主体 |
| :-----: | -------------------------------------------------- | ------------ |
|   GET   | 从服务器获取一份文档                               | 否           |
|  HEAD   | 只从服务器获取文档的首部                           | 否           |
|  POST   | 向服务器发送需要处理的数据                         | 是           |
|   PUT   | 将请求的主体部分存储在服务器上                     | 是           |
|  TRACE  | 对可能经过代理服务器传送到服务器上去的报文进行追踪 | 否           |
| OPTIONS | 决定可以在服务器上执行哪些方法                     | 否           |
| DELETE  | 从服务器上删除一份文档                             | 否           |



##### 4. 状态码

方法是用来告诉服务器做什么事情，状态码则用来告诉客户端，发生了什么事情。    

状态码的分类：

|  整体范围  | 已定义范围 | 分类       |
| :--------: | ---------- | ---------- |
| 100 ～ 199 | 100～101   | 信息提示   |
| 200 ～ 299 | 200～206   | 成功       |
| 300 ～ 399 | 300～305   | 重定向     |
| 400 ～ 499 | 400～415   | 客户端错误 |
| 500 ～ 599 | 500～505   | 服务端错误 |



##### 5. 原因短句

为状态码提供了文本形式的解释。

##### 6. 版本号

为 HTTP 应用程序提供了一种将自己所遵循的协议版本告知对方的形式。

#### 3.2.3 首部

跟在起始行后面的就是零个、一个或多个 HTTP 首部字段。  

HTTP 首部字段向请求和响应报文中添加了一些附加信息。

##### 1. 首部分类

- 通用首部：在请求报文和响应报文中均可出现
- 请求首部：提供更多有关请求的信息
- 响应首部：提供更多有关响应的信息
- 实体首部：用于应对实体主体部分的首部
- 扩展首部：规范中没有定义的新首部

##### 2. 首部延续行

将长的首部行分为多行可以提高可读性，多出来的每行前面至少要有一个空格或制表符（tab）

```
HTTP/1.0 200 OK
Content-Type: image/gif
Content-Length: 8572
Server: Test Server
	Version 1.0
```

Server 首部的完整值为 Test Server Version 1.0

#### 3.2.4 实体的主体部分

实体的主体是 HTTP 报文的负荷。就是 HTTP 要传输的内容。  

### 3.3 方法

见上文。  

### 3.4 状态码

| 状态码    | 原因短语                                      | 含义                                                     |
| --------- | --------------------------------------------- | -------------------------------------------------------- |
| 100       | Continue 继续                                 | 收到了请求的起始部分，客户端应该继续请求                 |
| 101       | Switching Protocols 切换协议                  |                                                          |
| 200       | OK                                            | 服务器已成功处理请求                                     |
| 201       | Created 创建                                  | 对那些要服务器创建对象的请求来说，资源已经创建完毕       |
| 202       | Accepted 已接受                               | 请求已接受，但服务器尚未处理                             |
| 203       | Non-Authoritative Information 非权威信息      |                                                          |
| 204       | No Content 没有内容                           | 响应报文包含一些首部和一个状态行，但不包括实体的主体部分 |
| 205       | Reset Content 重置内容                        | 浏览器应该重置当前页面上所有的 HTML 表单                 |
| 206       | Partial Content 部分内容                      | 部分请求成功                                             |
| 300       | Multiple Choices 多项选择                     |                                                          |
| 301       | Moved Permanently 永久搬离                    | 请求的 URL 已经移走了                                    |
| 302       | Found 已找到                                  | 与 301 类似，但是搬离是临时的。                          |
| 303       | See Other 参见其它                            | 告诉客户端应该用另一个 URL 获取资源                      |
| 304       | Not Modified 未修改                           | 说明资源未发生过变化。                                   |
| 305       | Use Proxy 使用代理                            | 必须使用代理访问资源                                     |
| 306       | 未用                                          |                                                          |
| 307       | Temporary Redirect 临时重定向                 | 和状态码 301 类似                                        |
| 400       | Bad request 坏请求                            | 告诉客户端它发送了一个异常请求                           |
| 401       | Unauthorized 未授权                           |                                                          |
| 402       | Payment Required 要求付款                     |                                                          |
| 403       | Forbidden 禁止                                | 服务器拒绝了请求                                         |
| 404       | Not Found 未找到                              |                                                          |
| 405 - 417 | 略                                            |                                                          |
| 500       | Internal Server Error 内部服务器错误          | 服务器遇到了一个错误                                     |
| 501       | Not Implemented 未实现                        |                                                          |
| 502       | Bad Gateway 网关故障                          |                                                          |
| 503       | Service Unavailable 未提供此服务              |                                                          |
| 504       | Gateway Timeout 网关超时                      |                                                          |
| 505       | HTTP Version Not Supported 不支持的 HTTP 版本 |                                                          |

​	



