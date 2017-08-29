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

## 细说插入、更新、删除和查询
  一、插入

    使用insert或save方法想目标集合插入一个文档：

    db.person.insert({"name":"ryan","age":30});

    使用batchInsert方法实现批量插入，它与insert方法非常类似，只是它接受的是一个文档数组作为参数。一次发送数十，数百乃至数千个文档会明显提高插入的速度。

    db.person.batchInsert([{"name":"ryan","age":30},{"name":"pitaya","age":2}]);

    如果在批量插入的过程中有一个文档插入失败，那么在这个文档之前的所有文档都会成功插入到集合中，而这个文档以及之后的所有文档全部插入失败。如果希望batchInsert忽略错误并且继续执行后续插入，可以使用continueOnError选项。shell并不支持这个选项，但所有的驱动程序都支持。 

  二、更新

    使用update方法来更新集合中的数据。update有四个参数，前两个参数是必须的。

    db.person.update({"name":"ryan"},{"$set":{"age":35}},true,true);

    第一个参数：查询文档，用于定位需要更新的目标文档。

    第二个参数：修改器文档，用于说明要对找到的文档进行哪些修改。

    第三个参数：true表示要使用upsert，即如果没有找到符合更新条件的文档，就会以这个条件和更新文档为基础创建一个新的文档。如果找到了匹配的文档，则正常更新。

    第四个参数：true表示符合条件的所有文档，都要执行更新。

  修改器：

    $set：用来指定一个字段的值。如果这个字段不存在，则创建它。对于更新而言，对符合更新条件的文档，修改执行的字段，不需要全部覆盖。

    db.person.update({"name":"ryan"},{"$set":{"age":35}},true,true);

    $inc：用来增加已有键的值，或者该键不存在就创建一个。对于投票等有变化数值的场景，这个会非常方便。

    db.person.update({"name":"ryan"},{"$inc":{"age":2}},true,true);//对符合name等于ryan的文档，age字段加2。

    $push：向已有数组末尾加入一个元素。

    db.person.update({"name":"ryan"},{"$set":{"language":["chinese"]}},true,true);//对符合name等于ryan的文档，添加一个language的数组

    db.person.update({"name":"ryan"},{"$push":{"language":"english"}},true,true);//给数组的末尾添加一个值。

    $addToSet：避免向数组插入重复的值。

    db.person.update({"name":"ryan"},{"$addToSet":{"language":"english"}},true,true);

    $each：与$push和$addToSet结合，一次给数组添加多个值。

    db.person.update({"name":"ryan"},{"$push":{"language":{"$each":["Japanese","Portuguese"]}}},true,true);

    db.person.update({"name":"ryan"},{"$addToSet":{"language":{"$each":["Japanese","Portuguese"]}}},true,true);

    $pop：可以从数组的任何一端删除元素。

    db.person.update({"name":"ryan"},{"$pop":{"language":1}},true,true);//从数组的末尾删除一个元素

    db.person.update({"name":"ryan"},{"$pop":{"language":-1}},true,true);//从数组的头部删除一个元素

    $pull：删除数组对应的值。全部删除。

    db.person.update({"name":"ryan"},{"$pull":{"language":"english"}},true,true);

  三、删除

    使用remove方法删除集合中的数据。它可以接受一个查询文档作为可选参数。给定这个参数以后，只有符合条件的文档才能被删除。（删除数据是永久性的，不能撤销，也不能恢复）。

    db.person.remove({"name":"ryan"});//删除person集合中name字段的值等于ryan的所有文档。

    db.person.remove();//删除person集合中所有的文档。

    使用drop方法代替remove方法，可以大幅度提高删除数据的速度。但是这个方法不能指定任何限定条件。而且整个集合都会被删除，包括索引等信息，甚用！！

    db.person.drop();

  四、查询

    MongoDB中使用find方法来进行查询。查询就是返回一个集合中文档的子集，子集的范围从0个文档到整个集合。find方法接受两个参数。

    第一个参数决定了要返回哪些文档，参数的内容是查询的条件。

    第二个参数来指定想要的键（字段）。第二个参数存在的情况：键的值为1代表要显示，为0代表不显示。“_id”默认显示，其他默认不显示。第二个参数不存在的情况：所有字段默认显示。

    db.person.find({"name":"ryan"},{"name":1});

  查询条件：

    $in、$nin，用来查询一个键的多个值。

    db.person.find({"age":{"$in":[1,3]}});//查询age等于1或3的文档。

    db.person.find({"age":{"$nin":[1,3]}});//查询age不等于1或3的文档。

    $or，用来查询多个键的多个值。可以和$in等配合使用。

    db.person.find({"$or":[{"name":"ryan2"},{"age":3}]});//查询name等于ryan2   或者   age等于3的文档。

    $exists，查询的键对应是值是null的，默认会返回null和键不存在的文档。可以通过$exists来判断该键是否存在。

    db.person.find({"age":{"$in":[null],"$exists":true}});//查询age等于null，并且键是存在的文档。

    $where，用它可以在查询中执行任意的javascript，这样就能在查询中做（几乎）任何事情。为了安全起见，应该严格限制或者消除"$where"语句的使用。

    db.person.find({"$where":function(){

        ...;//这里可以是任意的javascript语句。

    }})

  常用的shell:

    limit：只返回前面多少个结果。

    db.person.find().limit(2);//查询符合条件的文档，显示前两个文档。

    skip：跳过多少个结果后显示剩余的。

    db.person.find().skip(2);//查询符合条件的文档，显示跳过2个文档后剩余的所有文档。 

    sort：用于排序。接受一个对象（一组键值对）作为参数，键对应文档的键名，值代表排序的方向。排序的方向可以是1（升序）或者-1（降序）。如果指定了多个键，则按照这些键被指定的顺序逐个排序。

    db.person.find().sort({"name":1,"age":-1});//查询的结果，按照name升序，age降序来排序显示。