## phpstrom 2019.3版本配合 xdebug 调试php

### 必备工具
* phpstrom 2019.3版本(这个版本要求主要是统一环境，一般来说各个版本都差不多)
* php xdebug 扩展(我用的php7.3nts版本 为了调试方便我直接下载的 phpstudy v8.0)

### 具体操作步骤
* 执行一下 php --ini 确定下当前使用的 php版本以及php.ini 文件的路径（如果提示 没有php命令，说明你没安装php或者没有加入 win 环境变量）
* 更具 php --ini 命令找到 php.ini 文件位置，打开 php.ini文件加入以下一段配置。一下 zend_extension 配置需要你本地具体安装扩展的路径
```ini
zend_extension=D:/phpstudy/Extensions/php/php7.3.4nts/ext/php_xdebug.dll
xdebug.remote_enable=1
xdebug.remote_host=127.0.0.1
xdebug.remote_port=9000
```
* 打开 phpstrom 左上角 File->Settings->Languages & Frameworks->PHP->Debug。把 Xdebug Xdebug port 那个参数 改成 你在 php.ini文件中配置的 remote_port 端口
* 到这里 在 cli 模式下调试配置完毕。来到需要调试的页面，比如 index.php 单击右键 选择 Debug 那个功能。然后会出现一个 index.php (PHP Script)。点击一下就可以开心的调试了。

### 常见问题
* 为什么执行没有效果？
* 首先确认 cli 模式下 php能否找到，以及 phpstrom 中是否配置的 cli 下 php的版本(配置方式:左上角 File->Settings->Languages & Frameworks->PHP 这里选择 CLI Interpreter 然后自己配置一下 php cli 执行的版本以及路径)
* 配置了php cli 还是没有效果？
* 确认一下是否正确的安装了 xdebug  php --ri xdebug (如果什么都没有说明你没安装扩展,出现了配置项，确认下是否配置正确，只需要配置我在上文提到的参数。其他参数不管)
* 如果还没有效果，请多多百度熟悉下，多配置几遍。

### xdebug 大致原理
* 第一步: phpstrom 自带的 xdebug 插件开启一个 自定义监听的端口通常是 9000(可自定义),开始监听 9000端口
* 第二部: 你用 cli 模式启动 一个 php脚本，他就会在 你当前php.ini中得知 xdebug插件开启了，需要想 remote_host:remote_port 发请求，
* 第三步: phpstrom  xdebug 插件 监听到了这个请求，就去找你打的那个断点(编辑器左边的红色小点点)他会在第一个断点哪里停，并输出，等待你的下一次操作。大概流程是这样的


### 写在后面
> 以上流程仅自己学习感悟，如先了解详细流程原理，请移步官方文档 http://xdebug.com/ ，如果我理解的有误，请指正，感谢！！！！
> xdebug 调试 php对新手不太友好，需要配置一些东西，导致很多新手懵逼，(包括我)。下一篇更新 xdebug 配合 谷歌浏览器调试 php