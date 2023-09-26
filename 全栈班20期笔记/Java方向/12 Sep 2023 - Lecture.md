# 12 Sep 2023 - Advanced JS and ES6

- Prototype & Prototype chain
```javascript
function Person(name, age) {
    this.name = name
    this.age = age
}

Person.prototype.type = 'human';

Person.prototype.sayName = function () {
    console.log(this.name);
}

var p1 = new Person("Lily", 30);
var p1 = new Person("Mike", 40);

console.log(p1.sayName === p2.sayName); // output: true
```

```javascript
function Gog(name) {
    this.name = name;
    this.species = '犬科'；
}

var dogA = new Dog('大毛');
var dogB = new Dog('二毛');

dogA.species = '猫科';
alert（dogB.species);//output:犬科，不受dogA的影响
```
> 每次使用`new`的时候都会生成一个obj的实例

prototype
```javascript
function Dog(name){
    this.name = name;
}

God.prototype = {
    species: '犬科'
};

var dogA = new Dog('大毛');
var dogB = new Dog('二毛');

alert(dogA.species);//犬科
alert(dogB.species);//犬科

Dog.prototype.species = '猫科';
```

prototype模式
```javascript
function Cat(name, color){
    this.name = name;
    this.color = color;
}

//原型
Cat.prototype.type = "猫科动物"；
Cat.prototype.eat = function() {
    alert("吃老鼠");
};

var cat1 = new Cat("大毛", "黄色");
var cat2 = new Cat("二毛", "黑色");

alert(cat1.type); // 猫科动物
cat1.eat(); // 吃老鼠
alert(cat1.eat == cat2.eat);

//JS辅助判断方法
alert(Cat.prototype.isPrototypeOf(cat1)); // true
alert(Cat.prototype.isPrototypeOf(cat2)); // true

alert(cat1.hasOwnProperty("name")); // true
alert(cat1.hasOwnProperty("type")); // false

alert("name" in cat1); // true
alert("type" in cat1); // true

for(var prop in cat1) {
    alert("cat1[" + prop + "]=" + cat1[prop]);
}
```

- Protoyal inheritance构造函数继承
```javascript
function Animal() {
    this.species - "动物";
}

function Cat(name, color) {
    this.name = name;
    this.color = color;
}
// 继承方法
//1. 构造函数绑定
function Cat(name, color) {
    //添加, 用于继承Animal对象
    Animal.apply(this, arguments);
    this.name = name;
    this.color = color;
}

var cat1 = new Cat("大毛", "黄色");
alert(cat1.species); // 动物

//2. prototype模式
Cat.prototype = new Animal();
Cat.prototype.constructor = Cat;
alert(Cat.prototype.constructor == Animal);
alert(cat1.constructor == Cat.prototype.constructor);
alert(cat1.constructor == Animal);

var cat1 = new Cat("大毛", "黄色");
alert(cat1.species);动物

//3. 直接使用prototype, 是上面prototype模式的改进
function Animal() {}
Animal.prototype.species = "动物"；
Cat.prototype = Animal.prototype;
Cat.prototype.constructor = Cat; // Animal.prototype浮想的constructor属性也改掉了
alert(Animal.prototype.constructor); //cat

var cat1 = new Cat("大毛", "黄色");
alert(cat1.species); // 动物


//4. 利用控对象作中介
var F = function(){};
F.prototype = Animal.prototype;
cat.prototype = new F();
cat.prototype.constructor = Cat;

alert(Animal.prototype.constructor); //Animal

//将上面的方法封装成一个函数，便于使用。
function extend(Child, Parent) {
  var F = function() {};
  F.prototype = Parent.prototype;
  Child.prototype = new F();
  Child.prototype.constructor = Child;
  Child.uber = Parent.prototype;
}

extend(Cat, Animal);

var cat1 = new Cat("大毛", "黄色");

alert(cat1.species); // 动物

// 5. 拷贝继承(不推荐)
// 6. 非构造函数继承
// 7. 浅拷贝
function extendCopy(p) {
    var c = {};
    
    for(vat i in p) {
        c[i] = p[i];
    }
    
    c.uber = p;
    
    return c;
}
var Doctor = extendCopy(Chinese);
Doctor.career = '医生';
alert(Doctor.nation); //中国

Chinese.birthPlaces = ['北京', '上海', '香港'];
var Doctor = extendCopy(Chinese);
Doctor.birthPlaces.push('厦门');
alert(Doctor.birthPlaces);//北京 上海 香港 厦门

//8. 深拷贝
function deepCopy(p, c) {
  var c = c || {}; // 初始化目标对象c，如果c未定义， 则将其初始化为空对象

  for(var i in p){ // 遍历对象p的所有属性
    if(typeof p[i] === 'object') { // 如果当前属性是对象（包括数组和null）
      c[i] = (p[i].constructor === Array) ? [] : {};// 判断当前属性是数组还是对象
      deepCopy(p[i], c[i]);// 递归调用deepCopy函数，继续复制属性
    } else {
      c[i] = p[i]; // 如果当前属性不是对象，直接复制属性值到目标对象c
    }
  }

  return c; // 返回目标对象c
}

//使用的时候这样写
var Doctor = deepCopy(Chinese);
Chinese.birthPlaces = ['北京', '上海', '香港'];
Doctor.birthPlaces.push('厦门');

alert(Doctor.birthPlaces);//北京 上海 香港 厦门
alert(Doctor.birthPlaces);//北京 上海 香港
```
- Classes(ES6)
```javascript
function Student(name) {
  this.name = name;
}

Student.prototype.hello = function() {
  alert('hello,' + this.name + '!');
}

//如果用新的class关键字来编写Student，可以这样写：
class Student {
  constructor(name) {
    this.name = name;
  }

  hello() {
    alert('hello,' + this.anme + '!');
  }
}

var xiaoming = new Student('xx');
xiaoming.hello();
```

class继承
```javascript
class PrimaryStudent extends Student {
  constructor(name, grade) {
    super(name);
    this.grade = grade;
  }

  myGrade() {
    alert('I am at grade' + this.grade);
  }
}

//支持重写override/constructor/field
```
- Static method and static properties(ES6)
```javascript
class MyClass {
  static myStaticProperty = 42;
}

console.log(MyClass.myStaticProperty); // output: 42

class MyClass01 {
  static myStaticMethod() {
    console.log('Hello, static world!');
  }
}

MyClass01.myStaticMethod();
```
- DefineProperty
```javascript
Object.defineProperty(obj, prop, descriptor)
//obj: 要在其上定义属性的对象
// prop: 要定义或修改的属性的名称
//descriptor: 描述要定义或修改的属性的属性描述符
```
- Object Properties(Data Properties&Accessor Properties)
    - Data Properties
        - 数据属性包含一个简单的数据值以及四个描述该属性的特性
        - [[value]]:这是属性的实际值，它可以是任何有效的JS值
        - [[writable]]:这是一个布尔值，用于确定属性值是否可以被改变
        - [[Enumerable]]:这是一个布尔值用于确定该属性是否可以通过for...in循环或Object.keys方法枚举。
        - [[Configurable]]: 这是一个布尔值用于确定该属性的特性
    - Accessor Properties
        - 访问器属性不包含实际的值，而是定义了获取和设置属性值时调用的函数，他们也有两个描述行为的特性。
        - [[get]]
        - [[set]]
        - [[Enumerable]]
        - [[Configurable]]
        - 使用Object.defineProperty或Object.defineProperties方法也可以创建或修改访问器属性。

```javascript
//定义一个访问器属性
let obj = {
    _value: 42
};

Object.defineProperty(obj, 'property1', {
    get: function() {
        return this._value;
    },
    set: function(newValue) {
        this._value = newValue;
    },
    enumerable: true,
    configurable: true
});

console.log(obj.property1);//42
obj.property1 = 43;
console.log(obj.property2);//43
```
- Property getters and setters(ES6)
```javascript
class Person {
  constructor(firstName, lastName){
    this._firstName = firstName;
    this._lastName = lastName;
  }

  get fullName() {
    return `${this._firstName} ${this._lastName}`;
  }
}

const person = new Person('John', 'Doe');
console.log(person.fullName); // John Doe

class Person {
  constructor(firstName, lastName){
    this._firstName = firstName;
    this._lastName = lastName;
  }

  set firstName(value){
    if( typeof value === 'string'){
      this._firstName = value;
    } else {
      console.log('First name must be a string');
    }
  }

  get fullName() {
    return this._firstName;
  }
}

const person01 = new Person01('John', 'Doe');
person.firstName = 'Jane';
console.log(person.firstName);

function User(name, birthday) {
  this.name = name;
  this.birthday = birthday;

  Object.defineProperty(this, "age", {
    get() {
      return new Date().getFullYear() - this.birthday.getFullYear();
    }
  });
}

const john = new User("John", new Date(1992, 6, 1));
john.age;
```
settimeout
```javascript
function greet() {
    console.log("xxx");
}

setTimeout(greet, 2000);
console.log("Setting a timer for 2 seconds..." );

function callback() {
  console.log('Done');
}
console.log('before setTimeout()');
setTimeout(callback, 1000);
console.log('after setTimeout()');
```
- Promisses async,await(ES6)
```javascript 
function test(resolve, reject) {
  var timeOut = Math.random() * 2;
  log('set timeout to:' + time0ut + ' seconds.'); 
  setTimeout(function (){
    if (time0ut < 1){
        log('call resolve()...'); 
        resolve('200 OK');
    } else {
        log('call reject()...');
        reject('timeout in '+ time0ut + ' seconds.');
    }
  }, timeOut * 1000);
  
}

var p1 = new Promise(test);
// 如果成功，执行这个函数:
var p2 = p1.then(function (result) {
    console.log('成功:' + result);
});
// 如果失败，执行这个函数:
var p3 = p2.catch(function (reason) {
    console.log('失败:'+ reason);
});

// Promise对象可以串联起来，所以上述代码可以简化为：
new Promise(test).then(function (result) {
    console.log('成功：' + result);
}).catch(function (reason) {
    console.log('失败：' + reason);
});
```

```javascript
async function get(url) {
    let resp = await fetch(url);
    return resp.json();
}

async function get(url){
    try{
        let resp = await fetch(url);
        return resp.json();
    } catch(e) {
        //message
    }
}  

// promise写法
function get(url){
    return fetch(url)
        .then(response => response.json())
        .catch(error => {
            console.error('Error fetching the url:', error);
            throe error;
        });
}  
```
- Closure
```javascript
function lazy_sum(arr){
    var sum = function() {
        return arr.reduce(function(x, y) {
            return x+y;
        });
    }
    return sum;
}
var f = lazy_sum([1, 2, 3, 4, 5]); // function sum()

f(); //15

var f1 = lazy_sum([1, 2, 3, 4, 5]);
var f2 = lazy_sum([1, 2, 3, 4, 5]);
f1 === f2 // false
```

- 推荐书籍
    - Object-Oriented JavaScript
    - JavaScrip for Web Developers