### 推送自己制作的 docker image 到 dockerhub

* docker login  登陆dockerhub账号
* docker tag image 给自己的镜像打一个 tag
* docker push push 上传到自己的 dockerhub上面
### 注意
docker tag [本地镜像名] [我的用户名/我定义的tag名]，这样在推送的时候不会报权限错误