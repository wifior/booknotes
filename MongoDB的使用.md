## MongoDB的使用

#### 1.安装

```shell
#https://www.mongodb.com/
dpkg -i mongodb-org-server_4.2.3_amd64.deb
#安装完成后，启动可能报错，我们把data目录权限更改一下就好
sudo chown -R 用户名 /data
```

#### 2.安装图形操作界面

```shell
#https://robomongo.org/download

```

#### 3.使用

```shell
#向数据库插入文档
db.<collection>.insert()
db.<collection>.insertOne()
db.<collection>.insertMany()
db.<collection>.update({查询条件},{新对象})    #替换
db.<collection>.update({查询条件},
    {$set:{
        字段：值    
    }}
) #更新
db.<collection>.update({查询条件},
    {$unset:{
        字段：值    
    }}
) #删除字段
db.<collection>.remove({条件})
db.<collection>.deleteOne({条件})
db.<collection>.deleteMany({条件})
#进入数据库
use my_test
#插入user一个集合
db.users.insert({name:"hello"})
show dbs
#查询user集合中的文档
db.users.find()
#统计user集合中的文档数量
db.users.find().count();
#查询hellor的文档
db.users.find({name:"hello"})
#为hello增加一个属性
db.users.update({name:"hello"},{
set:{
   val:"world"
}
})
#替换文档
db.users.replaceOne({name:"hello",{name:"hi"}})
#删除属性
db.users.update({name:"hello",{$unset:{val:"world"}}});
#查询内嵌文档
db.users.find({"name.sex":"男"})
#push/addToSet用于向数组加添加新的元素
db.users.update({name:"hello",{$push:{"name.sex":"女"}}})

```



