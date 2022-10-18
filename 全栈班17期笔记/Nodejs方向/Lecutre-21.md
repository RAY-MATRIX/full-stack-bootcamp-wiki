# Lecture 21 MongoDB
## 21.1 Start mongo server
- Run the following command in separate terminals
  - mongod 在当前的命令行里启动并管理database server(把terminal和mongo进程绑在一起）
  - mongo 
- 把MongoDB当作服务启动：
  - 每次电脑启动时都会把MongoDB自动启动
> 'mongod' is not recognized as an internal command解决方法：把`mongod.exe`所在的路径贴到命令行上，或加入系统环境变量
- mongo shell：基于javascript的运行环境  
## 21.2 MongoDB Command
- 显示当前数据库的databases
  - `show dbs`
- 显示当前数据库的集合
  - `show collections` 
- 使用数据库(可以使用当前存在的或者不存在的） 
  - `use school`
  - 当数据库中有数据时，才会显示在db列表中
- 输入数据
  - 添加一个数据`db.students.insertOne({name:"mason"})`  name加不加双引号都可以，因为类似javascript，但是存起来之后是json格式，所以加了引号
    - `db` 指向了当前的数据库 
    - `students`对collection students进行操作
    - `insertOne()`添加一个文档
    - `{
        "acknowledged" : true,
        "insertedId" : ObjectId("62adbcb4fd2f44f3aa3e8111") 
      }` 如果没有指定id，MongoDB会帮我们自动生成一个id,id的生成没有规律
  - 添加多个数据 `db.students.insertMany([{},{}])` 
      
- 查找数据
  - `db.students.find()`
  - `db.students.find({name:"mason"})` 找到所有包含name:mason的数据

> { "_id" : ObjectId("629342745c178724d96c2a26"), "name" : "mason" } 与 { "_id" : "629342745c178724d96c2a26", "name" : "mason" } 是不同的数据类型
- 更新数据
  -`db.students.updateOne({name:"mason"},{$set:{name: "james"}})` 
  - `db.students.updateOne({name:"mason"},{$set:{age: 20}})` 
  - `db.students.updateOne({name:"mason"},{$set:{age: 20}},{upsert:true})` 找到就添加，没有找到就添加一个新的document包含我们想要的字段
    - `{
        "acknowledged" : true,
        "matchedCount" : 0,
        "modifiedCount" : 0,
        "upsertedId" : ObjectId("62adcd1d0a0a4e438830a79c")
       }`  
    - `{ "_id" : ObjectId("62adcd1d0a0a4e438830a79c"), "name" : "jason", "age" : 20 }`
  - update 找到第一个符合条件的document就更新了
- 删除数据
  - `db.students.deleteOne({name:"mason"})`
  - `db.students.deleteMany({name:{$exists:true}})` 

#### Quiz

- `db.students.insertOne({name:"mason",dob:new Date()})`
- `db.students.insertMany([{name:"jason",dob:new Date()},{name:"eden",dob:new Date()}])` 
- `db.students.updateOne({},{$set:{results:[80,90,100]}})`
- `db.students.updateOne({name:"jason"},{$set:{results:[80,90,100]}})`
- `db.students.updateOne({name:"eden"},{$set:{results:[90,100]}})`
- `db.students.find({results:90},{name:1,result:1,_id:0})`
- `db.students.deleteMany({})`
- `db.students.drop()`

> 如果一个db里没有数据，它是不会显示在db list里的

## 21.3 Relations
- 表和表的关系(建立在站在谁的角度上看的）
  - 1 to 1 (相对的，对于某个student来说，其和address的关系是1对1）
  - 1 to many
  - Many to many （一个A可以对应多个B，一个B可以对应多个A）
    - 学生和课程 多对多
    - 一个课程只允许一个老师，一个老师只允许上一个课程 1对1
    - 一个课程允许多个老师，一个老师只允许上一个课程 1对多 
- 表达关系的形式
  - Embedded 嵌入式
  - Reference 引用式  
 - Example：
  - 1 to 1 Embedded
    - id
    - name
    - address
      - city
      - address
      - postcode
  - 1 to 1 Reference
    - id
    - name
    - address：（ address id）
    -（in another collection）
    - id
    - city
    - address
    - postcode
    - student：（student id）
  - 1 to many Embedded
    - id
    - name
    - addresses
      - object 0
        - city
        - address
        - postcode
      - object 1
        - city
        - address
        - postcode
  - 1 to many Reference
    - id
    - name
    - address
      - 0
      - 1
    - (in another collection)
    - id: object 0
    - city
    - address
    - postcode
    - student: (student id) 
    - id: object 1
    - city
    - address
    - postcode
    - student: (student id)  
  - many to many 
    - many to many Embedded
      - S1,S2
      - C1,C2
      - students collection
 ```js
[
  {
    _id:"S1",
    courses:[
      {
        _id:"C1",
      },
      {
        _id:"C2",
      }
    ]
  },
  {
    _id:"S2",
    courses:[
      {
        _id:"C1",
      },
      {
        _id:"C2",
      }
    ]
   }
]
//如果要更改course名称，需要把所有学生里的该course改一遍，成本非常高昂
//而在reference的情况下，因为所有course名称都是引用式地被使用，只需要更新course里的document即可
```

### 21.3.1 Advanced Relations Thoughts
- Bi-directional referencing vs parent-refence or child reference 双边都做reference或者单边做reference
- Normalization（所有数据只出现一次） vs Denormalization（把数据进行复制）
  - 数据库规范化(Normalization)是数据库设计的⼀个⾮常重要的基本概念，⽬的是要去除重复的数据，增加数据的⼀致性。实际的作法是会将重复
的字段，抽出来变成另⼀个新的表
- one to millions relationship （长度超过百万级的，无法放在数据库的array里）
- Q：用NoSQL写数据库时，Nomalization和Denormalization是随着商业的需求而改变，并不会说哪个比较好哪个比较不好？
- A：默认做Normalization，不做数据重复，当遇到实际场景和实际数据支持时（如产品上线），平均花费在数据库请求上的时间是300ms，而其中，寻找关联的表并取回数据一起返回花费的时间是100ms，如果省掉这个关联可以节省100ms，如果该数据库天天在被用户访问，如果可以节省这个时间对于用户体验是一个相当大的改进，这个时候会选择做Denormalization
- Personal Advice：
  - 数量级在万以下，可以全部做reference没问题(澳洲大多数公司数据量级达不到万级，没有优化的机会）
  - Do embedded if you can(especially one to one)
  - One to couple,consider array of embedded docs
  - One to many,consider array of references 
  - One to million, consider parent-reference
  - If read query is much more than write query, go denormalize
  - 读数据的情况比写数据的情况多，做embedded能使数据读得更快

## 21.4 Advanced Database Knowledge
### 21.4.1 Indexes
- 索引是⼀种特殊的数据库结构，由数据表中的⼀列或多列组合⽽成，可以⽤来快速查询数据表中有某⼀特定值的记录
- 通过索引，查询数据时不⽤读完记录的所有信息，⽽只是查询索引列。否则，库系统将读取每条记录的所有信息进⾏匹配。
- 可以把索引⽐作新华字典的⾳序表。
```js
[
  d1,d2,d3,d4
]

mapping（开辟的一个新的空间，作映射）
[
  {name:'apple', mapping: d3},d1,d2,d4
]
//没有索引，需要把10000个数据全扫一遍
//有了索引，只需要扫索引然后取到相应的数据
```
- 创建索引和维护索引要耗费时间，这种时间随着数据量的增加⽽增加
  - 只会在必要时加上索引，因为添加新数据时，需要重新计算添加的新数据放在索引的什么地方，耗时间和空间，因此不会在所有地方加上索引

### 21.4.2 Aggregation
- Aggregation函数
  - 聚合操作处理数据记录并返回计算结果。将来自多个文档的操作组值聚合在一起，并可以对分组的数据执行各种操作以返回单个结果，这也被称为分组查询
  - 通常用于数据分析的情况
  - 如用户在前端输入firstname空格lastname，如果数据存的是firstname lastname分开的字段，那需要传给server端的数据量会非常大，这时可以使用aggregation pipeline，在数据库这边把相应的结果得到后传给server端
  - 参考资料：https://www.jianshu.com/p/a1d92a86896b
- Aggregation pipeline
```
[d1,d2,d3]
[d1,d3] first pipe
[d1,d3,d4] second pipe
[d1',d2',d3'] third pipe
```
### 21.4.3 Transactions
- 数据库事务
  - 事务是由一条或多条SQL语句组成的逻辑执行单元, 可以比喻成一个容器, 里面放的就是一堆SQL语句, 这些语句要么全部执行成功, 要么一个都无法执行
  - 常用于银行转账
  - 当资源的使用产生冲突的时候，有可能使用transaction
  - 参考资料： https://wenku.baidu.com/view/e90da7143a68011ca300a6c30c2259010202f332.html
 

> https://www.datensen.com/data-modeling/moon-modeler-for-databases.html Datensen 画ER diagram的工具
## 21.2 Mongoose
- 是MongoDB专用的ORM (object relational mapper)
- SQL 数据库使用Sequelize  https://sequelize.org/
- 基于Mongo Client
### 21.2.1 Schema vs Models vs Documents

```js
const schema = new mongoose.Schema({name:String});//数据的格式，schemaless数据易于迭代（如储存传感器数据），但是不容易管理
const Model = mongoose.model('Model',schema); //在mongoose里注册的一个模型，存到数据库中时，默认把Collection命名为model的名字复数小写，如models
const document = new Model({name;'document'})
```
- 如果mongoDB的数据里有age field ，但是mongoose没有声明age，那么去除的数据也是没有age的
- 当server发请求给数据库时，数据库会把数据以JSON的格式返回回来，mongoose把JSON转换成一个mongoose object
- 当想要存储object时，mongoose会把object转换成JSON，放在数据库里
### 21.2.2 代码实现
- student course M:N
- teacher course M:N
- jr-cms.herokuapp.com/api-docs swagger文档
- 先建立model，再建立controller
```js
const studentModel = require('../modeld/student');

const getAllStudents = async (req, res) => {
//exec() 不是所有的query都加exec，大多数时候都建议加；目的是告诉mongoose，query已经build结束，可以发出请求了
//异步请求 -> asynd await
	const students = await studentModel.find().exex();
	res.json(students);
}

const addStudent = async (req, res) => {
	const {firstName, lastName, email} = req.body;
	const student = new StudentModel({firstName, lastName, email});
	// save()异步
	// 存在就更新，不存在就添加
	await student.save();
	res.status(201).json(student);
}

```
- 一般会专门建一个文件db.js 管理server与数据库的链接，文件内容如下：
```js
const mongoose = require('mongoose');

const connectToDB = () => {
	const connectionString = process.env.CONNECTION_STRING;
	if (!connectionString){
		console.error('connection string is undefined');
		// 正常退出
		// 非正常退出
		// 人为正常退出 process.exit(0); 只有0是正常退出
		// 人为非正常退出
		process.exit(1);
	}
	const db = mongoose.connection;
	db.on('connected', () => {
	// logger.info
	// mongodb://user:password@xxxx.com
		console.log(`DB connected, ${connectionString}`);
	});
	db.on('error', (error) => {
		console.error(error);
		process.exit(2);
	});
	db.on('disconnected', () => {
		console.error('db disconnected');
	});
	return mongoose.connect(connectionString);
}

moduld.exports = connectToDB;
```
- 并且在入口文件里加入connectToDB（）
```js
require('dotenv').config();
const express = require('express');
const cors = require('cors');
const morgan = require('morgan');
const helmet = require('helmet');


const v1Router = require('./routes');
const connectToDB = require('./utils/db');

PORT = process.env.PORT || 3000;

const app = express();

app.use(morgan('dev'));
app.use(express.json());
app.use(cors());
app.use(helmet());


app.use('/v1',v1Router);

connectToDB(); //确保connect开启

app.get('/', (req, res) => {
    res.send('Hello World!');
});

app.listen(PORT, () => {
    // logger.info(`Example app listening at http://localhost:${PORT}`);
    console.log(`Example app listening at http://localhost:${PORT}`);
});
```
- 并在env中配置数据库地址
```js
PORT=4000
CONNECTION_STRING=mongodb://localhost:27017/jr-cms-16
```
