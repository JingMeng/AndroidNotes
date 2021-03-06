[TOC]

**参考** 

- [HTTP,HTTP2.0,SPDY,HTTPS你应该知道的一些事](http://web.jobbole.com/87695/)
- 《图解HTTP》




-------

**主要内容**


- Http 简介
- Http 状态码分类
- HTTPS、SPDY
- Web攻击技术
- Http 报文

#### 1.HTTP

##### 1.1 TCP/IP分层

- 应用层
   - 像用户提供应用服务的协议。FTP、DNS、HTTP
- 传输层
   - 提供网络连接中两台计算机之间数据的传输。TCP、UDP
- 网络层
   - 处理网络上流动的**数据包**。该层规定了通过怎样的路径到达对方计算机。。
- 数据链路层
   - 处理网络硬件部分，包括操作系统、设备驱动、光纤等。

##### 1.2 URI、URL

  参考图解HTTP Page28， URL是URI子集。
  
#### 1.3 HTTP的特点

- HTTP不保存状态（可通过Cookie保存）
- HTTP方法
   - GET ： 获取资源
   - POST ： 传输实体主体
   - PUT ： 传输文件
   - HEAD ： 获得报文首部
   - DELETE ： 删除文件
   - OPTIONS ： 询问支持的方法
   - TRACE ： 追踪路径
   - CONNECT ： 要求使用隧道协议链接代理（使用SSL Secure Sockets Layer 安全套接层、或 TLS Transport Layer Security 传输层安全）
- 持久连接节省通信量
  HTTP keep-alive或connection reuse，只要任意一方没有明确提出断开连接，则保持TCP状态
  ![http](https://github.com/sparkfengbo/AndroidNotes/blob/master/PictureRes/NET/http%E6%8C%81%E4%B9%85%E8%BF%9E%E6%8E%A51.png?raw=true)
  
  
  
缺点：

- 通信使用明文，内容可能会被窃听
- 不验证通信方的身份，因此有可能遭遇伪装
- 无法证明报文的完整性，所以有可能已遭篡改
  
 可参考《图解HTTP》第7章 Page135
 

 
#### 2.HTTP状态码

##### 2.1类别

- 1XX：信息性状态码（接收的请求正在处理）
- 2XX：成功状态码 （请求正常处理完毕）
- 3XX：重定向状态码 （需要进行附加操作以完成请求） 
- 4XX：客户端错误状态码 （服务器无法处理请求）
- 5XX：服务端错误状态码 （服务器处理请求出错）

##### 2.2 摘要

- 200 OK，请求被正常处理
- 204 Not Content 响应报文不包含注意部分
- 206 Partial Content 客户端进行了范围请求


- 301 Moved Permanently 永久重定向 
- 302 Found 临时重定向，请求的资源被分配了新的URI，希望用户能使用新的URI访问
- 303 See Other
- 304 Not Modified  与请求报文包含If-Match、If-Modified-Since等对应，返回304状态码的报文不包含主体部分
- 307 Temporary Redirect 临时重定向

- 400 Bad Request 请求报文中存在语法错误
- 401 Unauthorized 需要有通过HTTP认证的认证信息
- 403 Forbidden 对请求资源的访问被拒绝
- 404 Not Found 服务器无法找到请求的资源

- 500 Internal Server Error，服务器执行请求时出错
- 503 Service Unavailable，服务器暂时处于超负载或正在停机维护，无法处理请求


#### 3.HTTPS与安全认证

##### 3.1 HTTPS

HTTP加入加密处理和认证等机制，就是HTTPS（HTTP Secure）

![Https](https://github.com/sparkfengbo/AndroidNotes/blob/master/PictureRes/NET/https.png?raw=true)

增加了SSL或TLS协议代替，HTTP直接和TCP通信时，使用SSL先演变成先和SSL通信，再由SSL和TCP通信。

HTTPS使用对称加密和非对称加密的混合加密机制。在交换密钥使用非对称加密，简历通信交换报文阶段使用对称加密

##### 3.2 证明公钥正确性的证书

参考《图解HTTP》第7章 Page148


#### 4.HTTP SPDY

Google 2010发布，开发目标旨在解决HTTP性能瓶颈，缩短Web页面加载时间

**HTTP瓶颈**

- 一条连接上只能发送一个请求
- 请求只能从客户端开发，客户端不能接收除响应外的指令
- 请求、响应首部未经压缩就发送。首部信息越多延迟越长
- 发送冗长的首部。每次互相发送相同的首部造成的浪费较多
- 可任意选择数据压缩格式。非强制压缩发送


SPDY没有改写HTTP协议，而是在TCP/IP的应用层与会传输层之间通过新加会话层形式运作，SPDY规定通信中使用SSL。

使用SPDY，HTTP额外获得

- 多路复用。 通过单一的TCP连接，可以无限制处理多个HTTP请求。
- 赋予请求优先级
- 压缩HTTP首部
- 推送功能。支持服务器主动向客户端推送数据的功能，服务器可直接发送数据，而不必等待客户端的请求。
- 服务器提示功能

全双工的WebSocket

#### 4.Web攻击技术简介

1.中间人攻击 ![中间人攻击.png](https://github.com/sparkfengbo/AndroidNotes/blob/master/PictureRes/NET/%E4%B8%AD%E9%97%B4%E4%BA%BA%E6%94%BB%E5%87%BB.png?raw=true)

2.跨站脚本攻击 XSS（Cross-Site Scripting）

  指通过存在安全漏洞的Web网站注册用户的浏览器内运行非法的HTML标签或JavaSript进行的一种攻击。
  
  - 利用虚假表单骗取信息
  - 利用脚本窃取Cookie
  - 显示伪造的文章或图片

3.SQL注入攻击

4.OS命令注入攻击

5.HTTP首部注入攻击

攻击者通过在响应首部字段内插入换行，添加任意响应首部或主题的一种攻击。  

- 设置任何Cookie
- 重定向至任意URL
- 显示任意的主题

6.邮件首部注入攻击

7.目录遍历攻击

8.远程文件包含漏洞


9.强制浏览

10.开放重定向

11.会话劫持


12.点击劫持

Clickjacking，利用透明按钮或链接做成陷阱，诱使用户点击。

13.DoS攻击

Denial of Service attack。让运行中的服务器停止状态的攻击，有时也叫做服务停止攻击或拒绝服务攻击。Dos不限于Web网站，还包含网络设备和服务器等。

- 其中利用访问请求造成资源过载
- 通过攻击安全漏洞使服务停止
  
#### 5.HTTP 请求报文和响应报文

**报文结构** 参考《图解HTTP》第3章 Page49

**报文首部** 参考《图解HTTP》第6章 Page99

