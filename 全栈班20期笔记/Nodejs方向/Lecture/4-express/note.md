# 知识点

1.  开启项目文件夹，先用 npm init
2.  Npm i 安装包命令， 安完之后， 会产生 package-lock.json, 它用于记录安装的的包的的版本号，以便之后的人使用。 还会产生 node_modules 文件加， 文件夹里包含了程序运行所需的包。
3.  Package.JSON 文件当中的 dependencies 包含了安装的包
4.  Script 里面可以添加快捷启动方式，‘启动名字’：“命令行”， 用 NPM run 启动名字
5.  返回一般用 res.json 返回数据，返回状态码和 json， res.status(200).json(数据)
6.  Req.params(取 url 中的变量) 用于取特定的一个对象/users/:id; 一般用的命令有 GET PUT DELETE PATCH
7.  Req.query (query param) /users?age=xxx&pageSize=xx 配合 GET 使用
8.  Req.body 配合 POST PUT PATCH 使用，用于更新与添加数据，一定要配合 app.use(express.json)
9.  Middleware , app.use()可匹配某个特定需求开头所有的请求，比如app.use('/users', middleware), 会匹配所有users开头的请求:/users/123, /users/student。 app.get 只能匹配特定的开头，比如app.get('/users', middleware),只会匹配get /users
10. Middleware 有 next（），会启动下一个 middleware。没有 next 会 left hanging。Next 里有参数就会进入 error middleware
11. // root level

        -- package.json
        -- package-lock.json
        -- node_modules
        -- src
          |-- index.js 入口文件 （app.js, server.js）
          |-- routes （资源路径）
             |-- user(s).js (users.router.js)
             |-- task(s).js
             |-- index.js (注册所有路径) v1Router
          |-- controllers (负责逻辑处理，做关联) （data validation）
            |-- task(s).js (task.controller.js)
            |-- ...
          |-- models (数据模型) （db interactions）crud， ORM （object relational mapping）
            |-- Task.js (Capital case) (Task.model.js)
            |-- ...
          |-- middleware
            |-- cors.js
            |-- errorMiddleware
          |-- utils (utilities)
            |-- helper function
            |-- db connection

12. 项目建立实例。 第一步先在 src 文件夹建立入口文件 index.js, 第二，可在 middleware 里面建立需要的 middleware js 文件。 第三步 数据方面的放在 models 文件夹, 第四在 controller 文件夹建立一个.js 文件, 处理逻辑 做关联。 第五步 针对每个管理资源的 router，要建立一个对应的 router。在 routes 文件夹下建立资源管理的 router，用 express.Router(), 第六步将建立好的 router 关联到 routes 文件下的 index.js 文件，在 index.js 建立一个 router， 并用 use 来调用之建立好的 router。第七步 在 src 入口文件 index.js 调用 routes 文件下的 index.js 文件。
13. Helmet 是最基本的一个安全保护包
14. Doteven 处理环境变量，在项目根目录下创建.env, 项目不应该依赖于一个固定的变量。 比如 port value（端口 3000）. 端口信息是动态的，要用环境变量的方式进行传递。 Dotenv 是模拟环境变量。在.env 文件里定义环境变量. .
15. .env 语法， key=value， value 是 string.。 在.env 文件中定义 PORT =8000， 如何使用 dotenv，=》 require(‘dotenv’).config(); const PORT = process(当前程序的进程).env.PORT
16. 可以建立不同的.env, 比如.env.dev .env.pro 如何使用？=》const dotenv = require(‘dotenv’);
    Process.env.NODE_ENV 查看当前环境变量是什么。
17. Morgan 包 记录有哪些请求访问了 server。
18. Winston 包 做日志记录， 替换 console.log(); 相比 console.log, Winston 有等级比如有 debug, error, info,warn. 对信息进行区分。 用于错误排查，debug 会显示很详细的信息， 一般不会用。 1.可以灵活选择输出等级。 2. 单独处理某一种错误 3. 将日志写进一个文件，或传输到第三方平台，方便读取。
19. 用 winston， 基本用法：在一个 js 文件导入 winston, const logger = require(‘winston’); 输出信息 logger.info(‘’)
20. 建立一个 logger.js. ， 在里面 const Winston = require(‘winston’); const logger = Winston.createlogger({level: ‘info’, transports:[new Winston.transports.Console()//最基本的]})。 可以在 Console（{format: winston.format.colorize()}）里面改格式
21. Morgan 与 winston 结合使用，在 logger.js 中写入， logger.stream ={write:(message)=>{
    Logger.info(message)}}, 然后 morgan(process.env.NODE_ENV…… {stream: logger})
22. 在 transports 里面可用 new Winston.transports.file({filename:xxxx, level:XXX}),将 log 写入文件