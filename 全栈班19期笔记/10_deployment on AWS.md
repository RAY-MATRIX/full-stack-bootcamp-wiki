# 部署到 AWS

1. 将项目拉下来，npm i 命令安装必要的包
2. windows 电脑.env 文件中的 mongodb 要设置成 127.0.0.1
3. 准备数据库，打开 mongodb atlas, 点击 build database，改 region 最近的，点 create
4. 先把账户和密码存下来，连接数据库用
5. IP white list 来允许访问的 IP。 开发中可以设置 0.0.0.0/0, 允许所有的访问，close and finish
6. 点击 connect 选择 compass，复制链接，用第 4 步存的内容替换掉链接里面相应的内容。然后把链接复制到 mongodb compass 里面，点击 connect。
7. 再把链接复制，替换到.env 文件当中。通过改链接中的名字，来更改数据库名字，默认是 test。
8. 登录 AWS 用 rootuser 登录。
9. 搜索 elastic beanstalk 并点击它，点击 create application
10. 选择 web server environment, 起 application 名字。
11. 选择 platform
12. application code 选择 sample application 测试运行
13. presets 选择 single instance
14. create and use new service role
15. EC2 key pair 一定需要设置 但不用， network&security => create key pair. 取个名字，然后回
16. 回到 EC2 key pair 点击刷新按钮
17. 在搜索栏搜索并点击 IAM，点击左边 roles，创建新的 role，选择 EC2，next
18. 在搜索栏，elasticbeanstalkwebtier,elasticbeanstalkworkertier，elasticbeanstalkmulticontainerdocker 勾选上, 点击 next。添加一个名字，点击 create role。
19. EC2 instance 选择刚在 roles 创建的那个
20. skip to review 然后 submit，等待启动。
21. 替换自己的代码。 第一步先加环境变量， 左边栏中找到 configration，update monitor 右边有个 edit， 拉到最下面，有个 add environment property。
22. 把.evn 中的内容填入. JWT_KEY 在 google 搜 rsa key generator，复制 public key，放在 JWT_KEY.点击 apply
23. 搜索 code pipeline， create pipeline，选择 github version2, connect github. install new app, 选择想用的 repo， 点击 connect。 选择 repo name 和 branch. 然后 skip。
24. deploy 选择 AWS elastic beanstalk, application name，env 自动填充。next, create.
25. fly.io 也可以用来做部署 快 命令行部署。 render.com也可以部署 慢 简单。