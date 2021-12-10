---
title: 创建VUE项目并且git管理代码
categories: 基础配置
tags: [vue, git, 前端]
abbrlink: 'vueGitManage'
---


****

# 前端创建vue项目并使用git进行代码管理

## 预先下载软件

 [Visual Studio Code](https://code.visualstudio.com/)

[NodeJs](https://nodejs.org/en/)

[Git](https://git-scm.com/)

## 配置操作

### 使用淘宝镜像

国内使用npm可能对下载安装速率有影响，所以设置一个淘宝镜像会快很多

打开vscode，打开terminal 输入下面的命令：

npm config set registry https://registry.npm.taobao.org

### 使用npm下载安装的软件

前端所需要的组件大多都可以通过npm进行下载

以实习的项目为例子，我用到了vue ，vue-cli-service ，vue-router， vuex，element-ui， echarts，file-saver，axios，lodash-es

安装vue：**npm install vue**

安装vue-clil-service：**npm install -g @vue/cli**

安装vue-router：**npm install vue-router**

安装vuex：**npm install vuex --save**

安装element-ui：**npm i element-ui -S**

安装echart：**npm install echarts --save**

安装file-saver：**npm install file-saver --save**

安装axios：**npm install --save axios vue-axios**

安装lodash-es：**npm i -g npm**    /     **npm i --save lodash**

### 配置SSH密钥

在桌面 or 自己项目想存放的地方右键打开git bash，输入命令：

**git config --global user.name "username"**

**git config --global user.email "email"**

上面两个命令的目的：这是以后提交代码的时候显示的用户名以及邮箱



输入下面的命令查看电脑中是否存在SSH key：

**cd ~/.ssh**



如果没有SSH key ，那么输入下面的命令生成一个SSH Key：

**ssh-keygen -t rsa -C "自己的github登录邮箱"**



输入下面的命令找到SSH密钥：

**cat id_rsa.pub**

或者根据目录去找到id_rsa.pub文件，打开后复制下来里面的SSH密钥



回到github，进入setting页面，找到SSH密钥，然后新建一个SSH，设置一个title，然后下面就把复制的SSH密钥粘贴进去即可



### 创建项目并设置git管理

#### 先创建项目文件，再创建仓库

在你的电脑中找到一个你想存放项目的目录，右键打开git bash，然后输入命令：

**vue create filename**              (这个filename是自己文件的名字，按照需求填写)



接下来会进入到一个项目的需求配置选项页面，选择最下面的 **Manually select features** 按照自己的需求进行配置

配置好了之后等待安装完毕



然后输入下面的命令：

**git init**     (项目初始化)

**git add .**  （文件暂存）

**git commit -m ""**   （文件提交）



然后进入github 创建一个仓库，并把仓库的SSH克隆地址复制下来

回到命令窗口输入命令：

**git remote add origin ssh地址**

**git push -u origin master --force**

#### 先创建仓库，再创建项目文件

先在github上创建一个仓库，然后复制ssh地址

找到想存放自己项目的一个文件目录，右键点击git bash：

**git clone ssh地址**     

(如果需要密码，就是之前自己配置SSH密钥的时候设置的密码)

然后在这个文件目录  进行命令

**vue create filename**

创建自己的项目

# 2020年12月更新
## git更新代码操作

自己的master版本是1.0，且自己在1.0的版本下新建了一个名为dev的分支且在这个分支上改动较大。但是同事更新了master版本 现在远端的master版本是1.1，自己要怎么操作才能既保留同事的最新的master又将dev的改动合并到最新的master中（也就是dev分支push到github上的时候处于 0|n的地步）

首先要将自己本地的master同步远端仓库的master：

git checkout master 

git pull 

git checkout dev

git merge master

解决冲突

git add -A

git commit -m "备注"

git push origin dev --force



出现0|n的情况且n ≠1怎么处理呢？如下：

git rebase -i HEAD~n  保留一个pick 其余的pick都用squash替换

将最新的注释保留 删除其余的注释

然后:wq保存

重新push一次即可





















