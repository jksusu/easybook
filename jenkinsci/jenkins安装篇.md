### 安装jenkins


#### docker 方式安装
> 用上面的命令或者 直接在 dockerhub 上使用其他的 jenkins源

```docker pull jenkinsci/blueocean ```

> 启动容器。这里需要映射两个端口号。如果是学习直接复制下面命令。生产需要挂在目录。具体见 dockerhub 说明

``` docker run -d --name jenkins -p 80:8080 -v jenkins:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock  jenkinsci/blueocean```

> 进入容器复制初始密码 初始密码在 /var/jenkins_home/secrets/initialAdminPassword。文件

``` docker exec -it jenkins bash```

> 打开浏览器访问 127.0.0.1:8080 端口。输入密码，开始安装 插件，安装插件更具自己需求来。初次体验建议安装推荐插件，然后泡一杯咖啡，看看官网手册静静等待
。安装好程序后，创建一个管理员用户，就可以直接登陆啦！！！！

### 关于 devOps 和 持续交付 的理解
> devOps 是一种协作研发的文化，持续交付和 devOps 都是为了更好的。更快的向用户交付高质量的软件
devOps 包含 持续交付，但是持续交付 不等于 devOps。持续交付更关注工具的本身，是 devOps 的一种技术实现。
总知  devOps 更多的是一种思想， 持续交付是这种思想的技术实现
