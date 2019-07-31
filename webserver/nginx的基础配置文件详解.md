# ngx_http_fastcgi_module 模块指令解释

### 这是一个常见的 nginx.conf 配置文件
```c++
server {
    listen 80; #监听一个端口
    listen 443 ssl http2;
    
    #设置虚拟服务器的名称
    server_name .smartcheckin.test;
    
    #root 指令位于 server 的上下文中，将所有请求映射到本地文件系统的目录下
    root "/home/vagrant/code/test/";
    
    #默认访问页面
    index index.html index.htm index.php;
    
    #设置所需的响应字符集
    charset utf-8;
    
    #对url进行匹配，这里所有的url都可以匹配到这个指令块
    location / {
    
    #以指定顺序检查文件是否存在，并使用第一个找到的文件进行请求处理
        try_files $uri $uri/ /index.php?$query_string;
        
    }
    
    #location使用 = 修正符可以使 URI 和 location 的匹配精确,这里是精确匹配 favicon.ico 和 robots.txt，这两个文件
    #access_log off 特殊值 off 取消当前级别的所有 access_log 指令
    #log_not_found (on启用|off禁用),启用或禁用将文件未找到错误记录到 error_log 中。
    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }
    
    #特殊值 off 取消当前级别的所有 access_log 指令
    access_log off;
    
    #设置 error_log 路径
    error_log  /var/log/nginx/smartcheckin.test-error.log error;
    
    #sendfile （on|off）,是否开启预加载
    sendfile off;
    
    #在 Content-Length 请求头域中指定客户端请求体的最大允许大小。
    #如果请求的大小超过配置值，则将 413 （请求实体过大）错误返回给客户端。
    #请注意，浏览器无法正确显示此错误。将 size 设置为 0 将禁用检查客户端请求正文大小。
    client_max_body_size 100m;
    
    #匹配 后缀 为 .php的文件
    #location ~ 有 ~ 修正符，在匹配的时候会区分大小写。
    location ~ \.php$ {
    
        #定义一个捕获 $fastcgi_path_info 变量值的正则表达式
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        
        #设置 FastCGI 服务器的地址
        #fastcgi_pass localhost:9000; 指定域名和端口
        #fastcgi_pass unix:/tmp/fastcgi.socket; 或者作为 UNIX 域套接字路径：
        fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
        
        #在 $fastcgi_script_name 变量的值中设置一个文件名，该文件名追加到 URL 后面并以一个斜杠结尾。
        #这里请求 SCRIPT_FILENAME 参数 = /home/vagrant/code/test/index.php;意思就是默认访问 index.php文件
        fastcgi_index index.php;
        include fastcgi_params;
        
        #设置应传递给 FastCGI 服务器的 parameter (参数)。
        #该值可以包含文本、变量及其组合。当且仅当在当前级别上没有定义 fastcgi_param 指令时，这些指令才从前一级继承。
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        
        #是否开启 FastCGI 服务器响应码大于或等于 300 时是否应传递给客户端，或者拦截并重定向到 nginx 以便使用 error_page 指令进行处理。
        fastcgi_intercept_errors off;
        
        #设置读取 FastCGI 服务器收到的响应的第一部分的缓冲区的 size（大小）
        fastcgi_buffer_size 16k;
        
        #设置单个连接从 FastCGI 服务器读取响应的缓冲区的 number （数量）和 size （大小）
        #默认情况下，缓冲区大小等于一个内存页。为 4K 或 8K，因平台而异。
        fastcgi_buffers 4 16k;
        
        #设置与 FastCGI 服务器建立连接的超时时间。需要注意的是，这个超时通常不能超过 75 秒。
        fastcgi_connect_timeout 300;
        #设置向 FastCGI 服务器发送请求的超时时间。超时设置在两次连续写入操作之间，而不是传输整个请求的过程。
        
        #FastCGI 服务器在此时间内没有收到任何内容，则连接将关闭。
        fastcgi_send_timeout 300;
        
        #定义从 FastCGI 服务器读取响应的超时时间
        fastcgi_read_timeout 300;
    }

    location ~ /\.ht {
        deny all;
    }
    #ssl 指令属于 ngx_http_ssl_module 模块
    #指定一个有给定虚拟服务器的 PEM 证书文件（file）
    ssl_certificate     /etc/nginx/ssl/smartcheckin.test.crt;
    
    #指定有给定虚拟服务器的 PEM 密钥文件。
    ssl_certificate_key /etc/nginx/ssl/smartcheckin.test.key;
}
```
### location
>`location`以把网站分为不同的模块，定位到不同的处理方式上。location一套匹配的规则<br/>

- `~*` 修正符（用于不区分大小写的匹配）
- `~` 修正符（用于区分大小写的匹配）
- `=/` 修正符 会加快请求的速度
- 。。。。。。。。等等

>了解全部请看官方手册


### `fastcgi_split_path_info` 指令的作用
定义一个捕获 `$fastcgi_path_info` 变量值的正则表达式

```
location ~ ^(.+\.php)(.*)$ {

    #此处是定义的 捕获 `$fastcgi_path_info` 变量的正则表达式。他的指令是 `fastcgi_split_path_info`
    fastcgi_split_path_info       ^(.+\.php)(.*)$;
    
    #这里是 上面正则表达式捕获的第一个值 $fastcgi_script_name = SCRIPT_FILENAME
    fastcgi_param SCRIPT_FILENAME /path/to/php$fastcgi_script_name;
    
    #这里是 上面正则表达式捕获的第二个值 $fastcgi_path_info = PATH_INFO
    fastcgi_param PATH_INFO       $fastcgi_path_info;
}
```
> 如果原始请求是 `/show.php/article/0001`
通过分割
* SCRIPT_FILENAME: /path/to/php/show.php
* PATH_INFO: /article/0001


### 传参到 FastCGI 服务器
HTTP 请求头字段作为参数传递给 FastCGI 服务器，在作为 FastCGI 服务器运行的应用程序和脚本中，这些参数通常作为环境变量提供。<br/>
例如，User-Agent 头字段作为 HTTP_USER_AGENT 参数传递。除 HTTP 请求头字段外，还可以使用 `fastcgi_param` 指令传递任意参数。<br/>

* `ngx_http_fastcgi_module` 模块支持在 `fastcgi_param` 指令设置参数时使用内嵌变量：
> `$fastcgi_script_name` 变量
请求 url,或者 url 以斜杠 / 结尾，则请求 url索引文件，索引文件是由 `fastcgi_index` 指令设置。<br/>
以上 `fastcgi_index = index.php`<br/>
所以说这里 的变量 `$fastcgi_script_name = index.php`。<br/>
`SCRIPT_FILENAME = /path/to/php/index.php`。<br/>



