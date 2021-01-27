### HTTP简介
> HTTP 协议全称超文本传输协议,处在引用层，无状态，由请求响应构成。由 TCP 协议传输数据
监听两个端口 80和443。其中80端口是 HTTP协议监听端口，443则是HTTPS监听端口

> HTTP 有8种方法，GET POST PUT DELETE HEAD CONTENT OPTIONS TRACE

> 三个版本 1.0 1.1 2.0 目前大部分程序都是 1.1版本

#### 方法比较 GET POST

[关于post发送两个数据包的文章](https://gocardless.com/blog/in-search-of-performance-how-we-shaved-200ms-off-every-post-request/)

> GET 通过URL传输数据，数据量会受到浏览器设置的URL长度影响。会被浏览器主动缓存。

> POST 通过REQUEST BODY传输数据。不会被主动缓存，除非手动设置。

> 有种说法 GET 产生一个数据包 POST 产生 两个数据包。但事实是都是基于TCP流传输，发几次包完全是程序的事情，和HTTP协议的POST方法无关。

#### HTTP三个版本区别
[w3shool](https://www.w3.org/Protocols/rfc2616/rfc2616-sec8.html#sec8.2.3)
[HTTP详细文章](https://www.cnblogs.com/phpper/p/9127553.html)
> 1.0 短链接 一次请求创建一个 TCP 链接。结束后断开

> 1.1 默认长连接，一个TCP链接可以多次请求。支持文件断点续传

> 2.0 IO多路复用，多个请求公用一个TCP链接，服务器可主动推送


#### HTTPS
[SSL到底慢在哪里](http://www.ruanyifeng.com/blog/2014/09/ssl-latency.html)、

> HTTPS 就是 TCP+SSL。端口443


