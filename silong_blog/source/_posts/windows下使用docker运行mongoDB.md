---
title: windows下使用docker运行mongoDB
abbrlink: 'mongoDBInWindows'
categories: 配置
tags: [mongoDB, 配置]
---
# 在windows中使用Docker运行mongoDB

确保自己的windows系统中安装了docker



首先使用docker下载mongoDB数据库：

进入docker官网的mongo下载地址https://hub.docker.com/_/mongo 

复制docker命令：*docker pull mongo* 

他会默认下载最新的版本

完成下载后 打开powershell 或者cmd

输入docker images 查看mongo是否存在于docker中了

确认存在后 运行容器，命令：`docker run -itd --name mongo -p 27017:27017 mongo --auth` (这个命令是让将mongo分配给一个docker容器，如果报 这个名字已经分配给了容器xxxx那么要么换个名字，要么讲这个容器移除后再运行此命令)

> 参数说明：
>
> - -p 27017:27017: 映射容器服务的27017端口到宿主机的27017端口。外部可以直接通过 宿主机ip:27017访问到mongo的服务
> - --auth: 需要密码才能访问容器服务



接着通过`*docker ps*`命令查看容器的运行信息



然后使用以下命令添加用户和设置密码，并且尝试连接：

docker exec -it mongo mongo admin （这个命令需要先运行容器再使用）

#创建一个名为admin，密码为111111的用户

db.createUser({ user:'admin',pwd:'111111',roles:[ { role:'userAdminAnyDatabase', db: 'admin'}]});

#尝试使用上面创建的用户信息进行连接

db.auth('admin','111111')



然后我们给用户添加读写命令：

db.grantRolesToUser("admin",[{role: "readWrite", db:"admin"}])



然后运行db.stats() 或者show users 进行查看users的信息

> 附：添加用户时各个角色的对应权限
>
> 1.数据库用户角色：read、readWrite;
>
> 2.数据库管理角色：dbAdmin、dbOwner、userAdmin；
>
> 3.集群管理角色：clusterAdmin、clusterManager、clusterMonitor、hostManager；
>
> 4.备份恢复角色：backup、restore
>
> 5.所有数据库角色：readAnyDatabase、readWriteAnyDatabase、userAdminAnyDatabase、dbAdminAnyDatabase
>
> 6.超级用户角色：root





### 使用compass GUI客户端进行mongo可视化管理：

下载地址：https://www.mongodb.com/products/compass

安装成功后， 解压文件，打开MongoDBCompass.exe文件

选择 new Connection 然后选择 右侧的 fill in connection fields individually

输入自己的配置，如果客户端设置了密码登录，Authentication选择 Username/Password并输入用户名密码，然后点击connect

进入数据库后选择admin的database(因为我们之前设置用户admin只在admin的databse下给了权限)

然后新建collections

剩下的操作去官网学习即可



