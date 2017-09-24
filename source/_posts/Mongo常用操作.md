---
title: Mongo常用操作
date: 2017-07-09 16:09:58
tags: [MongoDB]
categories: Web
---

记录Seasim网站部署时Mongo的常用操作。
<!-- more -->

## mongodb shell 命令

因为GUI并不完善，所以很多操作要通过shell。记录一下用到的操作。

进入shell：`mongo`
显示dbs：`show dbs`
选择或者创建一个db：`mongo use oceany-blog`
显示所有collections：`show collections`， 不需要特意创建collection，插入新元素直接创建。
显示一个collection的内容：`db.users.find()｀
插入元素：
```shell
db.menus.insert({
    "section": "hos",
    "items": [
        {
            "index": "1",
            "title": "基础教程"
        },
        {
            "index": "2",
            "title": "代码解析"
        },
        {
            "index": "3",
            "title": "算法理论"
        },
        {
            "index": "4",
            "title": "演示算例"
        },
        {
            "index": "5",
            "title": "海洋工程应用"
        }
    ]
}
)
```
更新元素：（注意这样写会完全替换，假如该元素还包含"telephoe"属性，并不会被保留“）
```bash
db.users.update(
   {_id : ObjectId("59579c5d7ccf2a3a7a925ada")},
   {
     "email" : "admin@oceany.tech",
      "name" : "admin",
      role: "admin"
   }
)
```
删除元素：
```bash
db.users.remove(
   {_id : ObjectId("5957a035b1c7755b40f3e909")}
)
```

参考：[MongoDB Doc](https://docs.mongodb.com/)
---

作者 [@Guoxin][1]     
2017 年 07月 09日    

[1]: https://github.com/suiguoxin
