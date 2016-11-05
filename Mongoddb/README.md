# 这里是记录了Mongodb的使用记录

## Mongodb的启动和加入Windows服务

> mongod --dbpath E:\mongodb\data

> mongod --logpath "E:\mongodb\log\logs.txt" --logappend --dbpath "E:\mongodb\data" --directoryperdb --serviceName "MongoDB" --serviceDisplayName "MongoDB" --install

## Mongodb操作命令

### 常用命令

  1、Help查看命令提示
  > db.help();

  > db.yourColl.help();

  2、切换和创建数据库
  > use yourDB;

  3、查询所有数据库
  > show dbs;

  4、删除当前使用数据库
  > db.dropDatabase();

  5、查看当前使用的数据库
  > db.getName();

  db和getName方法是一样的效果，都可以查询当前使用的数据库

  6、显示当前db状态
  > db.stats();

  7、当前db版本
  > db.version();

### Collection聚集集合

  1、创建一个聚集集合
  > db.createCollection("collName");

  2、得到指定名称的聚集集合
  > db.getCollection("account");

  3、得到当前db的所有聚集集合
  > db.getCollectionNames();

  4、显示当前db所有聚集索引的状态
  > db.printCollectionStats();

### 聚集集合查询

  1、查询所有记录
  > db.userInfo.find();

  2、查询聚集集合某列的数据
  > db.userInfo.distinct("name");

  3、查询age = 22的记录
  > db.userInfo.find({"age":22});

  4、查询age > 22的记录
  > db.userInfo.find({"age":{$gt:22}});

  5、查询age < 22的记录
  > db.userInfo.find({"age":{$lt:22}});

  6、查询age >= 25
  > db.userInfo.find({"age":{$gte:25}});

  7、查询age <= 22的记录
  > db.userInfo.find({"age":{$lte:22}});

  8、查询age >= 23 并且 age <= 26
  > db.userInfo.find({age: {$gte: 23, $lte: 26}});

  9、查询name中包含 mongo的数据
  > db.userInfo.find({"name": /mongo/});

  10、查询name中以mongo开头的
  > db.userInfo.find({"name": /^mongo/});

  11、查询指定列name、age数据
  > db.userInfo.find({}, {name: true, age: true});

  name值：true是显示name列信息，false就是排除name，显示name以外的列信息

  12、查询指定列name、age数据, age > 25
  > db.userInfo.find({age: {$gt: 25}}, {name: 1, age: 1});

  13、按照年龄排序
  > 升序：db.userInfo.find().sort({age: 1});

  >降序：db.userInfo.find().sort({age: -1});

  14、查询前5条数据
  > db.userInfo.find().limit(5);

  15、查询10条以后的数据
  > db.userInfo.find().skip(10);

  16、查询在5-10之间的数据
  > db.userInfo.find().limit(10).skip(5);

  可用于分页，limit是pageSize，skip是第几页*pageSize

  17、or与 查询
  > db.userInfo.find({$or: [{age: 22}, {age: 25}]});

  18、查询第一条数据
  > db.userInfo.findOne();

  > db.userInfo.find().limit(1);

  19、查询某个结果集的记录条数
  > db.userInfo.find({age: {$gte: 25}}).count();

  20、按照某列进行排序
  > db.userInfo.find({sex: {$exists: true}}).count();

### 修改、添加、删除集合数据

  1、添加
  > db.users.save({name: ‘zhangsan', age: 25, sex: true});

  2、修改
  > db.users.update({age: 25}, {$set: {name: 'changeName'}}, false, true);

  > db.users.update({name: 'Lisi'}, {$inc: {age: 50}}, false, true);

  > db.users.update({name: 'Lisi'}, {$inc: {age: 50}, $set: {name: 'hoho'}}, false, true);

  > db.users.update({},{$unset: {'name': ''}},false,true);

  3、删除
  > db.users.remove({age: 132});