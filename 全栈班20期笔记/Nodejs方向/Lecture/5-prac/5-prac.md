###prac讲解

	一定加上gitignor来避免加入node_modules

	命名规则要具体 taskRouter

###end point

1: 	返回用res.json(xxx)

	taskRouter.get('/tasks', )	
	const {description} = req.query;
	if (description) {
		const filteredTask = tasks.filter((task => task.description.includes(description)
		res.json(filteredTask)
		return
	})
	
	res.json(task)
	
2: id类型转换

	filter做遍历，目的是找到就返回，不需要filter
	tasks.find就可以
	// 不写if (!task.id)，因为找到undefined时js会报错
	//fail fast避免多层嵌套使用else
	if(!task){
		res.status(404).json({
			msg: `Sorry xxxx id:${id}`,
		
		}) //应返回找不到的错误状态码
		return;
	}
	res.json(task);
	
	
3: update

	task未找到，同上
	task找到
	
	const {description, done} = req.body;
	if (description !== undefined) {
		//还需要type check
		task.description = description;
	}
	
	if (done!== undefined){
		task.done=done
	}
	
	res.json(task);
	
4: POST

	const {description} = req.body
	or 
	const {description, done=false} = req.body
	// data validation 与更新保持一致
	if (description === undefined){
		res.status(400).json({
			msg:'invalidation description type'
		})
		return;
	}
	
	//never user data from the client DIRECTLY
	
	const newTask = {id: id++,description, done: false}
	//最外面id配置值
	tasks.push(newTask);
	res.status(201).json(newTask)
	
	
5: DELETE

	id类型转换
	取index
	js使用equal check使用===
	// if (index == undefined) 检查undefined, null
	
	if (index === -1) {
		res.status(404).send ({
			msg: `Sorry xxx id:${id}`,
		})
		return;
	}
	
	task.splice(index, 1);
	res.sendStatus(204); //No content
	
	
	
6: cors

	不使用cors模块
	被CORS block
	const cors = (req, res, next) => {
		res.setHeader('Accsee-Control-Allow-Origin', '*');
		res.setHeader('Accsee-Control-Allow-Headers', '*');
		res.setHeader('Accsee-Control-Allow-Methods', '*');
		next();
	}
	
	app.use(cors);
