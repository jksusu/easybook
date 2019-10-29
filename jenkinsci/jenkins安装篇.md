### 安装jenkins


#### docker 方式安装
> 用上面的命令或者 直接在 dockerhub 上使用其他的 jenkins源

```docker pull jenkinsci/blueocean ```

> 启动容器。这里需要映射两个端口号。如果是学习直接复制下面命令。生产需要挂在目录。具体见 dockerhub 说明

``` docker run -d -p 8080:8080 -p 50000:50000 --name jenkins jenkinsci/blueocean```

> 进入容器复制初始密码 初始密码在 /var/jenkins_home/secrets/initialAdminPassword。文件

``` docker exec -it jenkins bash```

> 打开浏览器访问 127.0.0.1:8080 端口。输入密码，开始安装 插件，安装插件更具自己需求来。初次体验建议安装推荐插件，然后泡一杯咖啡，看看官网手册静静等待
。安装好程序后，创建一个管理员用户，就可以直接登陆啦！！！！