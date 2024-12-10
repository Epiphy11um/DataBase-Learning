# Database learning
记录自己配置服务器和docker完成数据库项目的一些列傻逼问题

~~(目前只是草稿，只是表示思考流程，一些用语还不专业，后续会继续update)~~
## 1. docker无法正常pull
无法拉取主要是因为docker以及国内的镜像源被墙了，可以参考以下博客：
[无法正常拉取镜像](https://www.cnblogs.com/xietingfeng321/p/18451170)

## 2. 服务器重启后在docker中，先前进行的操作被清空
这个问题分为两个部分
### 2.1 docker内的镜像被清空
在第一次重启服务器后，再次使用docker，发现以前docker进行的配置被清空
### 2.2 又无法pull
为什么说是**又**，因为我重启（物理重启服务器）之后发现原本的配置不能用了
仍然是拉取报错，具体的报错信息为：
```
$ docker run hello-world
...
Error response from daemon: Get "https://registry-1.docker.io/v2/": net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers).
```
这时候我的解决方法是第一步的clash与docker配置，从而完全支持pull
## 3. clash以及相关配置
### 3.1 clash以及clash verge的下载
[clash verge](https://www.clashverge.dev/index.html#_2) （这是单独的，如果想要和docker配置的一套流程，下面的可能更适合你）
里面的下载以及配置教程比较完善，不需要再赘述了
### 3.2 clash配置完后链接docker
[clash和docker结合](https://slipegg.github.io/2024/06/23/ClashForDocker/)
如果你单独下载了clash verge, 要配置docker，具体的问题集中在配置docker运行时的环境变量环节（既然你选择了单独下载，需要自己去找配置的操作）

#### 3.3 clash运行正常，docker不能正常运行
上面已经提到，docker需要链接clash的端口
我遇到的问题主要是clash可以正常使用，docker也正常配置完成，docker自己也知道配置完成了环境变量
但是在实际操作的时候docker并不走环境变量
目前采取的解决办法是clash使用tun模式，强制所有操作都走环境变量，docker可以成功`pull mysql`
目前不知道会有什么后续的问题

## 4.docker重启后不正常工作
最无厘头的问题！
`2024-12-10` 下午，在重启服务器后docker无法正常工作，具体表现为：
`status docker` 显示`active`，正常
`docker ps / docker images` 无法正确运行，指令`denied`
docker配置的daemon，socket文件都在

docker daemon看日志没问题

docker client就是连不上
## 5.重装系统
我受不了了
