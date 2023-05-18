# 知识点
1.	开发API的时候 会有一个API文档。 Swagger yaml 文件 来写API文档。 它是通过缩进来代表结构。前面有个给横杠可以理解为是一个object。
2.	Debug 创建launch.json 改路径为入口文件。 成功启动后，颜色会变。可以加断点
3.	Mongodb， Sql relational database。Server包含database， database 包含collection， collection包含documents， documents包含fields。
4.	Mongodb， show dbs, 可以当前有哪些database， 用use +名字，切换数据库或添加新的database。 
5.	给数据库添加数据， db.数据库名字.insertOne({document})， db.数据库名字.insertMany([ ])添加多个要用array。
6.	查找数据 db.数据库名字.findOne({查找条件 }) 找第一个，  db.数据库名字.findMany({查找条件 }) 找到多个
7.	更新数据 db.数据库名字.updateOne({查找条件 }，{ $set: {更新数据}}) 所有的operator前面加$. 可以更新现有字段，也可以更新没有的字段。 可添加第三个参数，upsert，找不到的数据自动添加。 .updateOne({查找条件 }，{ $set: {更新数据}}，{upsert: true})
8.	db.数据库名字.updateMany({ },{$set:{ 添加的数据}})
9.	删除一个或多个 db.数据库名字.deleteOne/deleteMany({查找条件})。 如果遇到nested 数据查找，就用引号将nested代码括起来。 比如 想删除数据库中，地址城市为BJ的数据，’address.city’:’BJ’ db.数据库名字.deleteMany({’address.city’:’BJ’ })
10.	看数据库是否为空， db.数据库名字.countDocuments()。 清空所有数据db.数据库名字.drop()
11.	Promise.all(promise 的数组)，可以让promise并发, Promise.all([promise1,promise2,promise3]).then
12.	关联性数据库，一般用reference。 Embedded 将信息直接存在关联的数据库。 
13.	数据库设计Normalization和Denormalization, normalization 数据不会重复。 Denormalization 会将需要的数据复制到关联的数据库。
14.	Async 和 await关键字 使代码可读性更高。 在函数前面添加async， 返回的就是promise类型的函数。Await不可以单独存在，需要和async结合使用。 Await 是等待promise resolve之后的返回值。  
15.	进程退出， 人为正常退出：Process.exit(0)，人为非正常退出： Process.exit(1 or 2)
16.	Mongoose.conect(connectionString); 与数据库进行连接
17.	可使用mongoose.connection 进行监听 例子：const db = mongoose.connection; db.on(‘error’,(error)=>{})//对连接错误进行监听
18.	可使用alias来定义_id的别名。
_id:{
Type: string，
alias: ‘code’
}
19.	Update, findByIdAndUpdate(id,{update 内容}，{new:true}).exec(); 如果更新数据缺失, 那么数据库会保持原来的数据
20.	Delete, findByIdAndDelete(id).exec();
21.	关联两个数据用reference， 要看关联的数据属性类型。 例子， 学生与课程， 学生schema添加 course:[
{type: String, //因为course的类型是String
Ref: “course”
}
]
课程schema添加 student：[{type:Schema.Types.ObjectId, 
Ref: ‘Student’}
] //用array，因为一个课程包含很多学生。
22.	用addToSet添加确保没有重复的
23.	用pull去删除
24.	查找时Populate（’courses’,”name description”）可以显示course 的name和description。如果只是Populate（’courses’），会显示course的所有信息。 例子: StudentModel.findById(id).populate(‘course’).exec();
25.	数据验证： Joi包 验证email例子： Joi.string().email().validate(email).error === undefined 
26.	在接收数据之前验证：

Const schema = Joi.object({
Name: Joi.string().min().max().required(),
Description: Joi.string().optional(),
})
Const validBody = await schema.validateAsync(req.body,{
allowUnknown:true, // 允许不知道属性
stripUnknown: true // 去除不知道的属性
})

