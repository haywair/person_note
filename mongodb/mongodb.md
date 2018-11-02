### mongodb

----

mongodb 对比 msyql

| sql术语概念 | mongodb术语概念 | 解释说明                               |
| ----------- | --------------- | -------------------------------------- |
| database    | database        | 数据库                                 |
| table       | collection      | 数据表/集合                            |
| row         | document        | 数据记录行 / 文档                      |
| column      | field           | 数据字段 / 域                          |
| index       | index           | 索引                                   |
| table joins |                 | 表连接  mongodb不支持                  |
| primary key | primary key     | 主键， mongodb自动将_id字段设置 为主键 |



* 适合存储数据量比较大，而且数据不是很重要的数据



---

mongodb的安装

* db.getName()    获取当前数据库名称
* use 数据库名     创建数据库
* db.stats()           查看数据库状态  
* db.help()            获取帮助
* 简单运算

----

mongodb 写入数据

bson格式，类json格式

```
插入数据 (普通数据)

db.goods.insert({
	name:'huawei01',
	price:1000,
	weight:135,
	number:35
})

插入数据 （多维数据 ）

db.goods.insert({
	name:'huawei01',
	price:1000,
	weight:135,
	number:35,
	area:{
        province:'beijing',
        city:'bejing'
	}
})

插入 数组 数据

db.goods.insert({
	name:'huawei01',
	price:1000,
	weight:135,
	number:35,
	area:{
        province:'beijing',
        city:'bejing'
	}，
	color:['blank','blue','red']
})

```



---



mongodb 查询数据

* 笼统查询

  条件Bson对象

  db.数据表.findOne(条件)

  db.数据表.find(条件)

```
返回所有的记录 

db.goods.find({name:'xiaomi'})

返回集合第一条 ，并且格式化 数据

db.goods.findOne({name:'xiaomi'})

```



* id 字段内容值唯一 （该_  id 可以自行定义，不推荐 ）

* 范围条件查询 

  $gt   $lt  $gte  $lte

```

db.goods.find({
	price:{
		'$gt':10055
    }
})
```



* 多条件查询 

  相当于mysql 里面的 and 条件操作

```
价格大于10055，重量小于140
db.goods.find({
	price:{
		'$gt':10055
    }，
    weight:{
        '$lt':140
    }
})
```

* 多维条件查询

```
db.goods.find({
    'area.province':{
        '$eq':'beijing'
    }
})
```

* 数组条件查询


```
db.goods.find({color:'red'})

满足多条记录

db.goods.find({
    color:{
        '$all':['black','red']
    }
})
```



* or 条件查询

```
示例1
db.goods.find({
    '$or':[
    	{price:2000},
    	{num:{'$lt':100}}
    ]
})

示例2
db.goods.find({
    '$or':[
    	{price:{'$gt':1500}},
    	{num:{'$lt':100}}
    ]
})

```



* 限制查询 字段

db.表.find({条件}， {字段：1/0, 字段：1/0})

1： 查询此字段 

0：排除此字段

字段必须统一，要么都为1，要么都为0，同时设置1，0会报错。但是 id 除外 

```
db.goods.find(
	{price:{'$gt':1500}},
	{name:1,price:1}
)

db.goods.find(
	{price:{'$gt':1500}},
	{area:0}
)
```





* 创建权限 
* 账号设置的顺序
  1. 必须先设置 root 管理员账号
  2. 其次再设置普通账号

```
创建账号指令：

db.createUser(  
  { user: "admin",  
    customData：{description:"superuser"},
    pwd: "admin",  
    roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]  
  }  
)  

user字段，为新用户的名字；

pwd字段，用户的密码；

cusomData字段，为任意内容，例如可以为用户全名介绍；

roles字段，指定用户的角色，可以用一个空数组给新用户设定空角色。在roles字段,可以指定内置角色和用户定义的角色。
```

