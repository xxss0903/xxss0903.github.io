
--
layout: blog
title: '添加多个git账号'
date: 2017-04-28 13:11:34
categories: blog
tags: code
image: ''
--

## 工作电脑上添加Github和工作的Gitlab账号

本机已经添加过了Github账号，由于工作需要，现在需要添加Gitlab账号，两个账号使用不同的邮箱和用户名进行创建，所以需要新添加重新生成一个rsa文件并且添加到Gitlab的ssh中

```
* 创建rsa文件  
* 添加ssh
* 配置config文件
```

### 添加ssh
#### 生成rsa
```
ssh-keygen -t rsa -C "example@gg.com" # gitlab上使用的注册邮箱
cat gitlab.pub | pbcopy # gitlab.pub 文件是在上面创建的时候输入的 gitlab 文件名 pbcopy是OS X上的复制命令，其他系统复制文件内容就行
```
![image](http://oojt5x2dl.bkt.clouddn.com/%E5%88%9B%E5%BB%BArsa.jpeg)

#### 将rsa拷贝添加到Gitlab的ssh中
![image](http://oojt5x2dl.bkt.clouddn.com/Gitlab%E6%B7%BB%E5%8A%A0ssh.png)

#### 本地添加config文件
config文件相当重要，一般没有创建，需要要自己创建并配置
```
touch ~/.ssh/config # 创建config文件

# 参考 http://blog.csdn.net/a258831020/article/details/50373060
# 添加内容
# 该配置用于工作  
# Host 服务器别名  
Host 192.168.2.36  
# HostName 服务器ip地址或机器名  
HostName 192.168.2.36  
# User连接服务器的用户名  
User huanghs  
# IdentityFile 密匙文件的具体路径  
IdentityFile C:/Users/P/.ssh/id_rsa  
  
  
# 该配置用于个人 github 上  
# Host 服务器别名  
Host github.com  
# HostName 服务器ip地址或机器名  
HostName github.com  
# User连接服务器的用户名  
User hasonHuang  
# IdentityFile 密匙文件的具体路径  
IdentityFile C:/Users/P/.ssh/id_rsa_hason  

```
修改完文件和配置之后试一下
➜  .ssh ssh -T git@gitlab的地址  # 例如ssh -T git@153.235.223.26
welcome to Gitlab，***
如果出现welcome，恭喜你，成功了
`
