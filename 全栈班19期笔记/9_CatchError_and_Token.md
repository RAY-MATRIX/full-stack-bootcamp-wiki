# 知识点
## 抓取错误的方式
1.  Callback 形式
    courseModel.find().exec((err, courses)=>{
        if(err){
            res.status(500).json({error: 'wrong'})
        }
        res.json();
    }); 
    这种方式只会抓取find（）错误， 这种写法已经不用了。

2. promise 因为exec() 返回的是promise，可以在exec（）后面加then（）来接受错误
3. async await
    try{
        代码
    }catch(e){
        建议用winston记录error
    }
4. 开发中用的方式： express-async-errors
    require ('express-async-errors');
    要自己写一个error middleware
    在middleware文件夹建立一个error文件夹，在error文件夹中建立一个unkownError.js 文件。
    在unkownError.js文件写入:
    module.exports = (error,req,res,next)=>{
        res.json('unknown wrong');
    }
    可建立特定的错误抓取的文件
    比如： validationError.js 可写入
    module.exports = (error,req,res,next)=>{
        if(error.name === "ValidationError"){
            res.status(400).json({error: error.message});
            return;
        }
        next(error);
    }

### 不重复写try and catch
写一个middleware
function CatchAll(routerHandler){
    return (req,res,next)=>{     // catchAll 返回的 middleware function
    try {
        routerHandler(req,res,next);
    }catch(e){

    }
    }
}

然后将写好的middleware function catchAll 运用到 每个router里面。 比如： courseRouter.get('/', catchAll(getAllCourses));

## 在本地删除了数据，但数据库还有数据 问题
需要删除对应的数据。 比如：删除学生数据之后，还要在course里面删除学生信息。
const deleteStudentById = async (req, res) => {
  const { id } = req.params;
  const student = await StudentModel.findByIdAndDelete(id).exec();
  if (!student) {
    res.status(404).json({ error: 'Student not found' });
    return;
  }
  await CourseModel.updateMany(
    {
      students: student._id,
    },
    {
      $pull: {
        students: student._id,
      },
    }
  ).exec();
  res.sendStatus(204);
};

## Authentication and Authorization
1. Authentication 判定你是谁，用户。
    authorization 判定你的权限、
2. jsonwebtoken 包用来获取token
    需要secret 和 payload 
    payload存储用户名，id。。。
    token = jwt.sign(payload,secret,{expireIn: '1d'}) // 签发token，expireIn 确保用户不会重复登录
    验证token const valid = jwt.verify(token,secret);
## 加密
哈希加密，不可逆， 在使用之前加入 random salt 再使用哈希。





