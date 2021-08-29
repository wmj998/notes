# MongoDB



| SQL术语/概念 | MongoDB术语/概念 | 解释/说明                     |
| ------------ | ---------------- | ----------------------------- |
| database     | database         | 数据库                        |
| table        | collection       | 数据库表/集合                 |
| row          | document         | 数据记录行/文档               |
| column       | field            | 数据字段/域                   |
| index        | index            | 索引                          |
| table joins  |                  | 表连接/mongodb不支持          |
| primary key  | primary key      | 主键/mongodb自动将_id设置主键 |



## 数据库操作

+ 查看当前数据库名字

  ```
  db
  ```

+ 查看当前数据状态

  ``` 
  db.stats()
  ```

+ 显示所有数据库

  ``` 
  show dbs
  ```

+ 切换数据库

  ``` 
  use db_name
  
  数据库存在则直接切换到指定的数据库，如果数据库不存在，则创建数据库，那么必须插入一条数据，才能保证数据库创建成功。数据库中不能直接插入数据，只能往集合(collections)中插入数据。
  ```

+ 删除数据库

  ``` 
  db.dropDatabase()
  ```



## 集合操作（document）

+ 手动创建集合

  ``` 
  db.createCollection(name, options)
  
  举例：
  a.db.createCollection("comments", {capped:true, size:10, max:2})
  b.db.createCollection("comments")（capped默认false，即无上限；size指大小，单位字节；max指数量上限）只有在用了size的情况下才能用max
  ```

+ 查看当前所有集合

  ```
  show collections
  ```

+ 删除集合

  ```
  db.col.drop()
  ```

集合的命名规则：

1. 集合名不能是空串。
2. 不能含有空字符\0。
3. 不能以“system.”开头，这是系统集合保留的前缀。
4. 集合名不能含保留字符$。



## 数据操作

+ 增

  ```
  db.col.insert({field: value})
  ```

+ 删

  ```
  db.col.remove({field: value}) （全部）
  db.col.remove({field: value}, {justOne:true}) （一个）
  ```

  以上都不能对创建集合时设置了capped:true（有上限）的集合删除；对于capped:true的集合，只能删除文档

+ 改

  ``` 
  db.col.update({field: value}, {$set: {field: value}}, {multi:true})
  
  默认multi值为false，只更新一条；如果true，表示更新匹配到的整个文档
  ```

+ 查

  ```
  db.col.find()
  db.col.find().limit()
  db.col.find().pretty()
  
  查询 age >= 25 并且 age <= 30
  db.user.find({age: {$gte: 25, $lte: 30}})
  
  使用OR查询年龄是18岁或者年龄是22岁的
  db.user.find({$or: [{age: 18}, {age: 22}]})
  ```

  

  

  

