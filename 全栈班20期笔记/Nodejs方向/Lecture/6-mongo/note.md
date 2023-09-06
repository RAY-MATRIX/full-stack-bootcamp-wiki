###MongoDB -> no-SQL
	
	
	SQL(Stracutred query language)
	relational database：
		MySQL
		PostgreSQL
		SQLite(移动端)
		RDS
	
	
	no-SQL
	nonrelational database
	类型：
		Document-oriented -> mongodb
		Key-value -> redis
		graphe-oriented -> neo4j (图)
		column-family -> cassandra （大数据）
		
	
	store data in a JSON-like documents(key-value)
	Flexibility or schemaless
	
	
	show databases
	use <db> (切换)
	show collections
	
	对数据进行操作
	db.collection.method.filters.operators
	
	create (先闭合括号再输入)
	db.student.instertOne({xxx:"yyy"})
	
	find (返回所有符合条件)
	db.student.find({name: "xxx"})
	
	update
	db.student.updateOne({grade: 10},{$set:{name: "jack"}})
	db.student.updateMany()
	
	delete
	db.student.deleteMany({})  !!!!!会删掉collecting中的所有数据