## nginx是怎么分发请求给php的？

### 本片文章主要描述一下几点
* nginx 怎么转发请求 给 PHPFPM?
* CGI 和 FastCGI 到底是个什么玩意？
* PHPFPM 是什么？有什么作用？

## 场景描述
在浏览器上访问一个 php+nginx+mysql 构建的商城，并且购买一件商品。
### 分析 （这里访问的有两种资源）
* 静态资源（网站的一些图片，图标等）
* 动态资源 （购买商品的价格，商品的简介等）

浏览器发起请求 --> web_server（nginx）分发处理 --> php执行代码返回结果 （这是大概的流程）

## nginx是怎么分发请求？
> 当用户发起请求的时候(浏览器默认请求 80 端口)，nginx 监听到 80端口，通过 nginx 配置正则匹配是否属于静态资源，如果是静态资源则返回文件，请求结束。如果是动态资源，通过 正则匹配到请求 php脚本，那么他会通过 nginx 的模块 ngx_http_fastcgi_module 把请求分发给 PHPFPM 处理，然后处理完毕返回结果。


* CGI 
CGI 是 Web 服务器运行外部程序的规范。意思就是通过 CGI 可以与你的程序通信，通过 CGI 标准格式。你的程序可以和浏览器交互。
（简单理解 CGI 就是一个协议，规定了一些东西该怎么传，你的程序这边怎么接受处理等规范。）<br/>

* PHP-CGI
PHP-CGI 就是 CGI 协议 php的一个实现版。PHP-CGI 会为每个请求 fork 一个进程处理，处理完成后退出。(这个模式叫做 fork-and-execute)。这样的模式不符合现在动不动大规模的流量，所以已退出历史舞台。

* FastCGI
FastCGI 是 CGI 的升级版，他会预先启动一个 master 进程读取配置文件，然后 fork 多个 work 进程等待连接。监听到请求，分配个 work 进程做具体的处理。这样大大提高了程序的性能。（FastCGi 会管理进程，处理完成后不会轻易销毁。而 CGI 会为每一个请求 创建进程，销毁进程。）

* PHPFPM
作为世界上最好的语言，当然要跟上潮流。当发现 PHP-CGI 性能不佳时，又恰好出现了 FastCGI 协议。所以 PHP 实现了一个php版本的 FastCGI，名字叫做 PHPFPM（FastCGI Process Manager）。	PHPFPM 启动时会开启 一个 master 进程和若干个 work 进程。master 进程监听请求，并转发给 work 进程处理，每一个 work 进程都有一个 php 解释器，你的代码在每一个 work 进程中都有一份，work 进程是真正执行代码的地方。

SO
> PHPFPM 监听 9000 端口，nginx 匹配到 php 文件，把请求转发给  PHPFPM。PHPFPM master 监听到请求后，分配给 work处理（每一个work 进程中都有一个 php解释器），PHPFPM 在启动的时候就已经 work进程已经加载了配置，加载了你写代码。所以说 work 进程收到请求后立马执行，然后返回结果。


### ngx_http_fastcgi_module 模块
在浏览器请求 web_server 是 http 协议 或者 https 协议，但是 PHPFPM 不懂怎么办了？这里 nginx 提供了一个 ngx_http_fastcgi_module ，ngx_http_fastcgi_module 把 http 或者 https 请求 映射成 FastCGI 请求。这样 php 程序就能和用户互动了。


