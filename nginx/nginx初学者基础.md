[nginx 中文手册](http://www.nginx.cn/doc/)

[nginx 官方手册](http://nginx.org/en/docs/)

## nginx 配置文件 👇

#### nginx 配置命名
```
* nginx.conf
```
#### nginx 配置文件存放路径
```
* /usr/local/nginx/conf
* /etc/nginx
* /usr/local/etc/nginx
```
---
#### nginx 会开启一个主进程(master)，和几个工作进程（worker）
```
vagrant@homestead:/etc/nginx/sites-enabled$  ps ax | grep nginx
 1340 ?        Ss     0:00 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
 1341 ?        S      0:00 nginx: worker process
 7548 pts/0    S+     0:00 grep --color=auto nginx
```
###### 执行  <font color=#FF7F50>ps ax | grep nginx</font> 命令，可以看到 开启了一个 master 进程 一个 worker 进程

master 进程读取配置，维护 worker 进程<br/>
worker 进程 处理请求 (worker 进程数量是可以配置的)   [配置手册](http://nginx.org/en/docs/ngx_core_module.html#worker_processes)<br/>

---
#### nginx 命令
##### nginx -s signal(信号)
```
  通过 nginx -s 发送信号给 master 进程
  stop (停止快速关闭)
  reload  (重新加载配置)
  reopen  (重新打开日志文件)
```
---
##### nginx 是由模块组成，这些模块由配置文件中的指令控制，指令分为简单指令和块指令
#####简单指令 以 逗号结尾<br/>
`error_log /var/log/nginx/homestead.test-error.log error;`

#####块指令是以 {} 结尾 <br/>
```
location ~ \.php$ {
    fastcgi_split_path_info ^(.+\.php)(/.+)$;
    fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
    fastcgi_index index.php;
    include fastcgi_params;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;

    fastcgi_intercept_errors off;
    fastcgi_buffer_size 16k;
    fastcgi_buffers 4 16k;
    fastcgi_connect_timeout 300;
    fastcgi_send_timeout 300;
    fastcgi_read_timeout 300;
}
```
##### 块指令中的简单指令称之为 上下文


---

#### 配置一个 FastCgi 代理
nginx可以将所有请求路由到 php所开发的程序，是由一个 叫 cgi 的东西实现的。由于 cgi协议，每次都要 fock and execute(创建，销毁)
所以出现了 fastcgi 协议，fastcgi 会在服务器启动时候开启，并且不会退出。不需要频繁的创建销毁，性能比 cgi 高很多。
```
server {
    location / {
        fastcgi_pass  localhost:9000; #fastcgi 监听的端口是9000
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name; #确定脚本名字
        fastcgi_param QUERY_STRING    $query_string;#传递参数
    }
}
```
##### 这里设置一个 server，将所有请求路由到通过 FastCGI 协议在 localhost:9000 上运行的代理服务器。