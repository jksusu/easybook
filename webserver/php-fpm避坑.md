## php-fpm 常见错误

### php-fpm 重启命令
> 安装的sbin目录  sudo /usr/sbin/php-fpm7.2
### `unable to bind listening socket for address .........sock no ...`
> 这个错原因是没有这个目录，建一个 报错目录就行了，然后重启 php-fpm

### php-fpm 监听端口的坑，有时候安装 fpm后不会监听端口，使用的是 sock套接字通信
> 这里也是个坑，如果使用 端口监听必须要自己配置，否则默认使用套接字通信
