---
title: 阿里云ECS部署NodeJs项目
date: 2017-06-24 15:33:38
tags: [ECS,NodeJs]
---

买了很久的阿里云服务器终于开始工作了，喜大普奔！

## 购买实例
学信网认证过期，用了黄黄的帐号

## 连接实例
初始密码不生效，因为复杂度不够，所以需要重置密码

## 运行第一个Hello World

参考[阿里云文档-部署Node.js项目](https://help.aliyun.com/document_detail/50775.html?spm=5176.product25365.6.715.uCJqsM)，按照步骤三使用NVM安装多版本Node。目前只安装了v6.10.1,每次登陆都需要需要nvm use v6.10.1，否则node命令不存在。

问题一：hostname不能使用公网ip，需要使用私网ip。后来发现listen后面不用写hostname也可以运行。外部访问时需要使用公网ip。

问题二：要添加安全组规则，开放特定端口，本次使用express默认端口3000。

## 部署oceany项目

参考[N-blog教程在UCloud的部署](https://maninboat.gitbooks.io/n-blog/content/book/4.15%20%E9%83%A8%E7%BD%B2.html)，涉及命令行：
```bash
ssh root@公网ip
yum install git #安装git
yum install npm #安装 npm
npm i npm -g #升级 npm
npm i pm2 -g #全局安装 pm2

git clone https://github.....
cd oceany-blog
npm install
npm start
pm2 logs
```
运行npm install，安装bcrypt没有成功，报错：node-gyp rebuild
发现[Github Issue](https://github.com/kelektiv/node.bcrypt.js/issues/476#issuecomment-274148353)并没有解决，将bcrypt模块换程bcryptjs，完全一样的API，草率解决。因为bcryptjs使用js写成，慢但是没有依赖模块。参考：[Github Issue](https://github.com/TekkenChicken/chicken-server/pull/8)

也可以配置python环境等解决，没有尝试，可以参考[CSDN博客](http://blog.csdn.net/allgis/article/details/46574493)。

数据库依然放在mlab上，图床依然是七牛云。

## 绑定域名

程序监听端口3000，将oceany.tech解析到服务器公网ip，新添加的DNS解析即刻生效，使用ping命令可以解析到指定ip，此时已经可以通过wwww.oceany.tech:3000；
但是，由于默认端口是80，为了直接使用域名访问，需要将80端口转到3000，还有两件事情要做：
1，安全组规则中开放80端口，此时输入wwww.oceany.tech显示phpinfo界面；
2，修改/etc/httpd/conf/httpd.conf文件，添加代码如下

#redirection 80 to 3000
<VirtualHost *:80>
  ProxyPreserveHost On
  ProxyRequests Off
  ServerName www.oceany.tech
  ServerAlias oceany.tech
  ProxyPass / http://localhost:3000/
  ProxyPassReverse / http://localhost:3000/
</VirtualHost>
之后使用 apachectl restart 重启apache。
此时wwww.oceany.tech就可以访问我们的网站啦。

参考: 
https://stackoverflow.com/questions/8541182/apache-redirect-to-another-port
https://www.centos.org/docs/5/html/Deployment_Guide-en-US/s1-apache-startstop.html

---

作者 [@Guoxin][1]     
2017 年 06月 17日    

[1]: https://github.com/suiguoxin

