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


	数据关系
	Embedded   1 to 1
	Reference  1 to many
	           many to many
	1:1
	student : address -> 1:1
	address : student -> 1:1    
	
	1:M
	student : address -> 1:M
	address : student -> 1:1
	
	M:N
	student : address -> 1:M
	address : student -> 1:N

	student collection
	Denormalization
	[
		{
			_id: 1,
			name: "Mason",
			addresses: [
				{
					_id: "A1",
					city:"Brisbane",
					postcode: 4000
				},
				{
					_id: "A2",
					city: "Melbouren",
					postcode: 3000
				}
			]
		},
		{
			_id: 2,
			name: "Json",
			addresses: [
				{
					_id: "A1",
					city:"Brisbane",
					postcode: 4000
				},
				{
					_id: "A2",
					city: "Melbouren",
					postcode: 3000
				}
			]
		}
	]


	bi-directional referencing(双边关联) vs parent-refence or child reference（单边关联）
	Normalization vs Denormalization 
	one to million relationship


	sensors, logs
	logs在无限增长

	MongoDb, every document -> 16MB

	Do embedded if you can 

	不适合embedded情况
	A:B -> 1:1
	B:C -> 1:1

	One to couple, consider array of embedded docs

	One to many, consider arraty of references

	One to million, consider parent-reference

	If read query is much more than write query, go denormalize