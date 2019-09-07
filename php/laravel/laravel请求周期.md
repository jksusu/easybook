### 生命周期
* public/index.php 加载bootstrap/app.php 加载 composer 文件
* 接下来请求到达 app/Http/Kernel.php http 内核中  在这里加载事件配置文件，错误处理等。所有请求必须经过 http中间件
* 加载服务提供者
* 服务提供者被注册，请求会被递送给路由。路由将会调度请求，交给绑定的路由或控制器，也包括路由绑定的中间件。