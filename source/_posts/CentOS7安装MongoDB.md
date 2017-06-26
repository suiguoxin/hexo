---
title: CentOS7安装MongoDB
date: 2017-06-26 23:09:48
tags: [MongoDB,CentOS]
---

# MongoDB安装

## 服务器上安装yum

使用命令`vi /etc/yum.repos.d/mongodb-org.repo`插入如下代码：

```bash
[mongodb-org-3.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc
```
之后可以利用命令`yum repolist`确认上面命令已经生效。

随后就可以用`sudo yum install mongodb-org`安装mongodb。

## 运行mongodb，创建表格oceany-blog
使用命令systemctl start mongod启动mongo；
用命令`mongo`进入shell模式，用`use oceany-blog创建新表格，用`db`查看当前表格。

## Express项目配置
修改config，在production.js中mongodb的url修改为：
`mongodb: 'mongodb://localhost:27017/oceany-blog'`

更新ECS上代码，重新启动程序，访问网站。

bug: comme je n'ai rien fait...


ref: 
https://docs.mongodb.com/manual/tutorial/install-mongodb-on-red-hat/#configure-the-package-management-system-yum
https://www.digitalocean.com/community/tutorials/how-to-install-mongodb-on-centos-7
https://maninboat.gitbooks.io/n-blog/book/4.3%20%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6.html
https://www.tutorialspoint.com/mongodb/mongodb_create_database.htm
http://www.jianshu.com/p/65c220653afd
