# Lecture 23-24 Course controller & 
## 23 Complete course controller
- 跟student controller类似
## 24.1 处理error
```js
	function foo(){
		//code maybe async or may throw error
	}
```
- try and catch
```js
try{
	//code
}catch(e){
	handle error
	next(e);
	res.status(400).send();
}
```
- callback
```js
courseModel.find((error, res) => {
	if(error){
		next(e);
		res.status.(400).send();
	}
	//code
})
```
- promise
```js
courseModel.find().exec().then((res) => {
	//code
	}).catch(error => {
		next(e);
		res.status(400).send()	
});
```
- express用4的话，可以用async errors的包
## 24.2 debug mode
- 代码执行到断点（breakpoint）的时候会停住，等待用户操作
- 在vscode行号左边单击就可以设置断点
- vscode里run and bug可以进入debug mode
- 在package.json的scripts里新增一条：
```js
"debug": "nodemon --inspect src/index.js"
```
## 24.3 Complete user controller
- 通过token判断是否登录成功
- authentication: who you are
- authorization: what you can do
### 24.3.1 JWT
  - 三部分组成，前两部分明文存在；通常不把用户隐私信息放在第二部分；最后一部分是前两部分加上一个密钥通过第一部分的算法生成的
  - 第三部分的签名一旦改变就会发现token被篡改了
  - token存在local storage或者session里
### 24.3.2 password 加密
- 加密 encrypt 可逆
- 解密 decrypt 可逆
- 哈希 hash 不可逆 密码加密用哈希
   x -> hash -> Y -> no way -> x
- add salt: 把随机生成的salt和password混在一起，这样同样的密码加密后的结果一定是不一样的；同时salt会明文存在hash后的结果里
- add pepper: 只有server知道pepper是什么
- 密码加密代码实现：
	- npm i bcrypt
	- 在user的model或者在util里写compare pwd函数
