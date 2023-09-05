# 05 Sep 2023 - Lecture
## Advanced JavaScript 2

### Intermediate JavaScript
- Functions
    - Arguments
        "免费赠送"的关键字，只在函数内部起作用。(传递给一个参数的参数集合)
        ```javascript
        function foo(x) {
            console.log('x= ' + x); //10
            for(var i = 0; i< arguments.length; i++){
                console.log('arg' + i + ' = ' + arguments[i]); // 10, 20 ,30
            }
        }
        ```
        
        ```javascript
        function foo(a, b, ...rest) {
            console.log(a);
            console.log(b);
            console.log(rest);
        }
        ```
        > `...`可能是剩余参数，但是也可能是展开运算符！
        
        - 默认参数：
        默认情况下参数是没有附带值的，但是`ES6`开始可以给argument设置一个默认参数
        ```javascript
        function greet(name = "world") {
            console.log("Hello" + name); // "Hello world"
        }
        ```
        
    - Arrow Functions(ES6)
    ```javascript
    x => {
        if(x > 0){
            console.log(x);
        } else {
            console.log(-x);
        }
    }
    ```
    
    ```javascript
    //两个参数
    (x, y) => x * x + y * y;
    //无参数
    （）=> 3.14;
    ```
    箭头函数的特点
    - 没有arguments（可以用rest）
    ```javascript
    const getArg = (...args) => console.log(args);
    ```
    
    - Callback function
    编程模式，条件满足时，一个函数会调用传入的参数。（也就是将函数作为参数传给另一个函数）
    ```javascript
    function greeting(name) {
        console.log(`Hello ${name}`)
    }
    
    function procesUserInput(callback) {
        const name = prompt("Please enter your name: ");
        callback(name);
    }
    procesUserInput(greeting);
    ```
    
    ```javascript
    const fs = require('fs');
    
    function readCallback(err, data) {
        if(err) {
            console.error('读取文件出错： ', err);
        } else {
            console.log('文件内容: ', data.toString());
        }
    }
    
    fs.readFile('example.txt', readCallback);
    
    console.log("This is a test");
    ```
    - Constructor
    构造函数中this指向的是new的对象
    ```javascript
    function MyConstructor() {
        this.prop = 42;
    }
    
    const myInstance = new MyConstructor();
    console.log(myInstance.prop); //42
    ```
    
    ```javascript
    function Person(name, age) {
        this.name = name;
        this.age = age;
    }
    
    var person1 = new Person('Alice', 25);
    console.log(person1.name);// Alice
    console.log(person1.age); // 25
    ```
    - this
    ```javascript
    function getAge() {
        var y = new Date().getFullYear();
        return y - this.birth; //undefined - 直接指向undefined而不是指向window
    )
    
    var xiaoming = {
        name: '小明',
        birth: 1990,
        age: getAge
    };
    
    console.log(xiaoming.age()); //33
    console.log(getAge()); // NAN, this指向不明确
    ```
    
    ```javascript
    var xiaoming = {
        name: '小明',
        birth: 1990,
        age: function () {
            var that = this; //在方法内部一开始就捕获this
            function getAgeFromBirth() {
                var y = new Date().getFullYear();
                return y - that.birth; // 用that而不是用this
            }
            return getAgeFromBirth();
        }
    };
    
    xiaoming.age(); //33
    ```
    
    ```javascript
    //在全局上下文中，this指向全局对象。在浏览器中，全局对象是windows
    console.log(this === window);
    //在方法内部使用this时，它指向拥有该方法的对象
    const obj = {
        value: "xxx",
        printValue: function() {
            console.log(this.value);
        }
    };
    
    obj.printValue();
    ```
    灵活使用`this`
        - apply
        - call
        - bind
- Higher-order function
    可以作为接收其他函数作为参数的函数称为高阶函数/可以返回一个参数也称为高阶函数
    - map/reduce
    - filter
    - Array
    - every
    - find
    - findIndex
    - forEach

- Symbols(ES6)
    ```javascript
    let s = Symbol();
    
    typrof s; // "symbol"
    
    //接收一个字符串作为参数
    let s1 = Symbol('foo');
    let s2 = Symbol('bar');
    
    s1 // "Symbol(foo)"
    s2 // "Symbol(bar)"
    
    //对象作为参数
    const obj = {
        toString() {
            return 'abc';
        }
    };
    const sym = Symbol(obj);
    sym // Symbol(abc)
    ```
    > 不能用new来实例化Symbol因为Symbol是一个原始数据类型
    > 参数可以是`string`，也可以是`object`
    > Symbol无论是创建多少个Symbol都是互不相等的
- WeakMap & WeakSet(ES6)
    - WeakMap 是类似于 Map 的集合，它仅允许对象作为键，并且一旦通过其他方式无法访问这些对象，垃圾回收便会将这些对象与其关联值一同删除。 
    - WeakSet 是类似于 Set 的集合，它仅存储对象，并且一旦通过其他方式无法访问这些对象，垃圾回收便会将这些对象删除
```javascript
// WeakMap 相当于缩小版的Map
const weakMap = new WeakMap();
const obj = {};

weakMap.set(obj, "ok"); // works fine
weakMap.set("test", "Whoops");// Error, as "test" is not an object

// If there are no other reference to that object key, it will be removed from memory (and from the map) automatically.
const john = {name: "John"};
const weakMap = new WeakMap();

weakMap.set(john, "...");

john = null; overwrite the reference, john is removed from memory
```

```javascript
// Map
const myMap = new Map();
myMap.set('key1', 'value1');
myMap.set({}, 'value2');
console.log(myMap.size); // 2

//WeakMap
const myWeakMap = new WeakMap();
const obj = {};
myweakMap.set(obj, 'value1');
//myWeakMap.set('key1', ''value1); // 这将抛出错误，因为键必须是对象

```

- Json
JSON is a format for storing and transporting data.
JSON is often used when data is sent from a server to a web page.
```javascript
    "firstName":"John"
```
```javascript
//Create a JS string containing JSON syntax
let text = '{ "employees" : [' +
    '{ "firstName":"John" , "lastName":"Doe" },' +
    '{ "firstName":"Anna" , "lastName":"Smith" },' +
    '{ "firstName":"Peter" , "lastName":"Jones" } ]}';
// Operate this JS obj by JS built-in function
const obj = JSON.parse(text);

```

- Prototype & Prototype chain
```javascript
<script>

...let obj = {};
console.log(obj._proto__);//输出 Object.prototype
console.log(obj.__proto__=== Object.prototype); //true
</script>

<script>
function Person(name){
    this.name = name;
}

Person.prototype.greet = function (){
console.log('Hello, my name is '+ this.name);};
let person = new Person('Alice');
person.greet();//输出:"Hello，my name is Alice' console.log(person.__proto__ === Person.prototype);
</script>
```

- `__proto__` 对象返回原型的内部属性，可以通过设置属性来更改对象原型
`__proto__ `(also called the Dunder Proto or Double Underscore Prototype) is a property of Object.prototype (more on that in a minute) that exposes the hidden [[Prototype]] property of an object and allows you to access or modify it. You should not use it as it is deprecated, although you may come across it in older code.

- `prototype`基于原型的继承
`Prototype` is a hidden private property that all objects have in Javascript, it holds a reference to the object’s prototype.
    An object’s prototype is the object that an object inherits or descends from. In the following diagram the object son descends from father , so father is the prototype of son. That means the hidden [[Prototype]] property of son points to father.
```javascript

// 原型对象:
var Student = {
    name: 'Robot', 
    height:1.2,
    run: function (){
        console.log(this.name + ' is running...');
    }
};

function createStudent(name){
    // 基于student原型创建一个新对象
    var s = Object.create(Student);
    // 初始化新对象: 
    s.name =name; 
    return s;

var xiaoming = createStudent('小明'); 
xiaoming.run(); // 小明 is running... 
xiaoming._proto__ === Student; // true
```
