## php 安装扩展

### linux 系统安装php扩展
#### 在 linux 下安装php扩展有两种方式，

* 通过 pecl 安装    http://php.net/manual/zh/install.pecl.php
* 源代码手工安装（有很多源代码在 pecl 上面没有，只能采取手工安装的方式）

#### pecl 安装
* pecl install 安装源|包名
###### 如果提示  phpize not found 
```php
<?php
sudo apt-get update 更新源
sudo apt install php7.2-dev 安装
whereis phpize 查询下 phpize 存在就可以pecl 安装了
pecl install swoole 安装swoole 扩展
php -m 查询是否安装
```

#### 源码编译安装
* `wget git@gitee.com:swoole/swoole.git`
*  使用 phpize生成 configure 文件
*  `make test` 测试
*  `make install`然后安装
*  `make && make install`或者两个步骤合并
*   配置ini 文件  `extension=swoole.so`
*   `php -m`检查是否存在

##### 注意
偶尔一些扩展会出现一些错误（请参考下篇文章）
https://www.zybuluo.com/DFFuture/note/387585