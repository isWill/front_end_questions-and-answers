# 前端问答
## 1. 什么是虚拟DOM？
```
React为啥这么大？因为它实现了一个虚拟DOM（Virtual DOM）。虚拟DOM是干什么的？这就要从浏览器本身讲起

如我们所知，在浏览器渲染网页的过程中，加载到HTML文档后，会将文档解析并构建DOM树，然后将其与解析CSS生成的CSSOM树一起结合产生爱的结晶——RenderObject树，然后将RenderObject树渲染成页面（当然中间可能会有一些优化，比如RenderLayer树）。这些过程都存在与渲染引擎之中，渲染引擎在浏览器中是于JavaScript引擎（JavaScriptCore也好V8也好）分离开的，但为了方便JS操作DOM结构，渲染引擎会暴露一些接口供JavaScript调用。由于这两块相互分离，通信是需要付出代价的，因此JavaScript调用DOM提供的接口性能不咋地。各种性能优化的最佳实践也都在尽可能的减少DOM操作次数。

而虚拟DOM干了什么？它直接用JavaScript实现了DOM树（大致上）。组件的HTML结构并不会直接生成DOM，而是映射生成虚拟的JavaScript DOM结构，React又通过在这个虚拟DOM上实现了一个 diff 算法找出最小变更，再把这些变更写入实际的DOM中。这个虚拟DOM以JS结构的形式存在，计算性能会比较好，而且由于减少了实际DOM操作次数，性能会有较大提升
```
## 2. 创建对象的几种模式
### 2.1 工厂模式
```javascript
function createPerson( name, age, job){ 
  var o = new Object();
  o.name = name;
  o.age = age; 
  o.job = job; 
  o.sayName = function(){ 
    alert( this.name); 
  }; 
  return o;
}  
var person1 = createPerson(" Nicholas", 29, "Software Engineer"); 
var person2 = createPerson(" Greg", 27, "Doctor");
```
工厂模式没有解决对象识别的问题（你不知道一个对象的类型）

### 2.2 构造函数模式
```javascript
function Person( name, age, job){ 
  this. name = name;
  this. age = age; 
  this. job = job; 
  this. sayName = function(){ 
    alert( this. name); 
  }; 
}  
var person1 = new Person(" Nicholas", 29, "Software Engineer"); 
var person2 = new Person(" Greg", 27, "Doctor");
```
构造函数模式每实例化一个对象相同的方法都要再创建一遍（创建两个完成同样任务的Function实例是没有必要的）

### 2.3 原型模式
```javascript
function Person(){}
Person. prototype. name = "Nicholas";
Person. prototype. age = 29;
Person. prototype. job = "Software Engineer";
Person. prototype. sayName = function(){ 
  alert( this. name); 
};
var person1 = new Person();
person1. sayName(); //"Nicholas"
var person2 = new Person();
person2. sayName(); //"Nicholas"
alert(person1. sayName == person2. sayName); //true
```
原型中所有属性是被很多实例共享的，对于包含引用类型值的属性存在较大的问题。

### 2.4 组合使用构造函数模式和原型模式
```javascript
function Person (name, age, job) {
  this.name = name;
  this.age = age;
  this.job = job;
  this.friends = ["Shelby" , "Court"];
}

Person.prototype = {
  constructor : Person,
  sayName : function () {
    alert(this.name);
  }
};

var person1 = new Person("Nicholas", 29, "Software Engineer");
var person2 = new Person("Greg", 27, "Doctor");

person1.friends.push("Van");
alert(person1.friends);  // "Shelby,Count,Van"
alert(person2.friends);  // "Shelby,Count"
alert(person1.friends === person2.friends);  // false
alert(person1.sayName === person2.sayName);  // true
```
这种构造函数与原型混成的模式，是目前在ECMAScript中使用最广泛、认同度最高的一种创建自定义类型的方法。可以说，这是用来定义引用类型的一种默认模式。

### 2.5 动态原型模式
```javascript
function Person (name, age, job) {
  // 属性
  this.name = name;
  this.age = age;
  this.job = job;

  // 方法
  if (typeof this.sayName != "function") {
    Person.prototype.sayName = function () {
      alert(this.name);
    }
  }
}

var friend = new Person("Nicholas", 29, "Software Engineer");
friend.sayName();

```
使用动态原型模式时，不能使用对象字面量重写原型。前面已经解释过了，如果在已经创建了实例的情况下重写原型，那么就会切断现有实例与新原型之间的联系。

### 2.6 寄生构造函数模式
```javascript
function Person (name, age, job) {
  var o = new Object();
  o.name = name;
  o.age = age;
  o.job = job;
  o.sayName = function () {
    alert(this.name);
  }
  return o;
}
var friend = new Person("Nicholas", 29, "Software Engineer");
friend.sayName();

```
不能依赖instanceof操作符来确定对象类型

### 2.7 稳妥构造函数模式
```javascript
function Person (name, age, job) {
  // 创建要返回的对象
  var 0 = new Object();

  // 可以在这里定义私有变量和函数

  // 添加方法
  o.sayName = function () {
    alert(name);
  };

  // 返回对象
  return o;
}

```
不能依赖instanceof操作符来确定对象类型

















