## 插件和 cli 服务

> vue cli 使用了一套基于插件的架构，所有依赖关系在 文件 `package.json` 中。


### cli 服务
在 cli 项目中，安装了一个 `vue-cli-service` 命令，可以使用一下命令调用
* `npm run serve` npm调用
* `yarn serve`  Yarn调用


#### `vue-cli-service serve [options] [entry]`
```
选项：
  --open    在服务器启动时打开浏览器
  --copy    在服务器启动时将 URL 复制到剪切版
  --mode    指定环境模式 (默认值：development)
  --host    指定 host (默认值：0.0.0.0)
  --port    指定 port (默认值：8080)
  --https   使用 https (默认值：false)
```
