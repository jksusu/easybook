### 记录一下 wsl2 使用的遇到的坑

*  wsl2 里面不可以使用docker，不能守护进程，至少我没有弄成功。



#### 记录一下命令
```bin
swl (进入wsl系统)
```

#### wsl 开机自启问题
```bin
#开机自启我是写了一个 vbs 脚本，需要用的时候打开一下就行。再一次 win 系统的生命周期内点一次就行。关机再开机点一次。如果一直不关机就不需要。

#一下代码命名 wslStart.vbs
Set ws = WScript.CreateObject("WScript.Shell")
cmd = "C:\Windows\System32\bash.exe -c ""bash /home/init.sh"""
ws.Run cmd, 0, false
Set ws = Nothing
WScript.quit

//在wsl系统里面的 /home/ 目录建一个 init.sh 的文件，写入一下代码

#!/bin/bash
sudo -S service ssh start << EOF
 这里填你设置的 wsl的密码
EOF
#启动系统软件的命令填写在此处
sudo service nginx start
sudo service php7.2-fpm start
while true
do
            sleep 600
    done
```

#### 目前 wsl 还是挺好用的，其他内容待补充