# 前端问答
## 「 CSS篇 」
1. CSS 盒子模型，绝对定位和相对定位

盒子模型：
标准盒子模型的宽度 = 左右margin + 左右border + 左右padding + width；
IE盒子模型的宽度 = 左右margin + width(IE中的content包含border和padiing)；
相对定位和绝对定位：

2. 清除浮动，什么时候需要清除浮动，清除浮动都有哪些方法

给子元素添加float属性，同时希望父元素的高度不会塌陷，此时我们需要清除浮动；
清除浮动的几种方法：
+ 给父元素设置高度

这种方式简单粗暴，但是，在子元素的高度出现变化时我们需要手动修改父元素高度，比较麻烦

+ 在父元素的最后一个子元素上添加属性`clear: both`

```html
<div class="parent">
     <div class="child"></div>
     <div class="child"></div>
     <div class="child"></div>
     <div style="clear: left;"></div> <!--页面中添加大量的没有意义的空标签，增加代码冗余-->
</div>
```
+ 为在父元素增加伪元素，并为其设置相关属性

```css
.fix::after { 
     content:"."; 
     display:block; 
     height:0; 
     visibility:hidden; 
     clear:both;
}
```
改进后的样式为

```css
.fix::after { 
     content:""; 
     display:table; 
     clear:both;
}
```
+ 给父元素添加`overflow: hidden`

这里有必要了解一下BFC块级格式化上下文，只说结论：
> 创建了 BFC的元素就是一个独立的盒子，不过只有Block-level box可以参与创建BFC， 它规定了内部的Block-level Box如何布局，并且与这个独立盒子里的布局不受外部影响，当然它也不会影响到外面的元素。它具有以下特征：
1) 内部的Box会在垂直方向，从顶部开始一个接一个地放置。
2) Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生叠加。
3) 每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。
4) BFC的区域不会与float box叠加。
5) BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，反之亦然。
6) 计算BFC的高度时，浮动元素也参与计算。
  
> 看到第六条，如获至宝。只需给父元素创建块级格式化上下文，就可以让浮动的元素参与高度计算，这样一来，父元素的高度就有了。
不单单只有给父元素添加overflow:hidden才可以创建块级格式化上下文，下列方法都可以：
1) 浮动 (元素的  float不为 none）
2) 绝对定位元素 (元素的 position为 absolute 或 fixed)
3) 行内块 inline-blocks (元素的 display: inline-block)
4) 表格单元格 (元素的 display: table-cell，HTML表格单元格默认属性)
5) 表格标题 (元素的 display: table-caption，HTML表格标题默认属性)
6) overflow的值不为 visible的元素
7) 弹性盒子 flex boxes (元素的 display: flex 或 inline-flex)
8) 用overflow:hidden较多的原因是不会带来其它的布局问题。

> 题外话：其实用块级上下文解释清除浮动个人感觉牵强，但也解释的过去哈。开拓下思维，看下知乎的问题[CSS中为什么overflow:hidden能清除浮动(float)的影响？原理是什么？](https://www.zhihu.com/question/30938856)，或许能找到你想要的。

3. 如何保持浮层水平垂直居中
+ 利用绝对定位与`transform`
```html
<div class="parent">
 <div class="child"></div>
</div>
```
```css
 .parent {
    position: relative;
 }
 
 .child {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 300px;
    height: 200px;
    transform: translate(-50%, -50%);
 }
```
+ 利用绝对定位与`margin`
```html
<div class="parent">
 <div class="child"></div>
</div>
```
```css
 .parent {
    position: relative;
 }
 
 .child {
    position: absolute;
    top: 50%;
    left: 50%;
    margin-top: -100px;
    margin-left: -150px;
    width: 300px;
    height: 200px;
    background: #333;
 }
```
+ 利用`flex`
```html
<div class="parent">
 <div class="child"></div>
</div>
```
```css
.parent {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 100%;
}
```
+ 利用`table`
```html
<table>
    <tr>
        <td></td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td class="child"></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td></td>
        <td></td>
    </tr>
</table>
```
```css
table {
    width: 100%;
    height: 100%;
    background: #aaa;
}

td.child {
    width: 300px;
    height: 200px;
    background: #333;
}
```
+ 利用`margin: auto`
```html
<div class="parent">
 <div class="child"></div>
</div>
```
```css
.parent {
    position: relative;
    width: 100%;
    height: 100%;
    background: #aaa;
}

.child {
    position: absolute;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
    margin: auto;
    width: 300px;
    height: 200px;
    background: #333;
}
```

4. position 和 display 的取值和各自的意思和用法

position 属性规定元素的定位类型。其属性分别为 static(默认)、absolute、fixed、relative和inherit。
* static 默认值。可以利用这个值取消定位，使元素快速回归文档流，设置的top，left等值也会随之失效。
* absolute 生成绝对定位的元素，相对于static定位以外的第一个父元素进行定位。元素脱离文档流，其位置可通过top、bottom、left和right属性进行规定。
* relative 生成相对定位的元素，相对于其正常位置进行定位。元素未脱离文档流，可通过top、bottom、left和right属性进行规定。
* fixed 生成绝对定位的元素，相对于浏览器窗口进行定位。元素脱离文档流。其位置可通过top、bottom、left和right属性进行规定。
* inherit 规定应该从父元素继承 position 属性的值。

display 属性规定元素应该生成的框的类型。
* none 隐藏对象。与visibility属性的hidden值不同，其不为被隐藏的对象保留其文档空间。
* inline 指定对象为行内元素。行内元素不会独占一行，相邻的行内元素会排列在同一行里，直到一行排不下，才会换行，不可主动设置其width和height，两者由内容撑开。同时也不可设置margin和padding。
* block 指定对象为块级元素。块级元素会独占一行，可主动设置其width和height。同时也可以通过设置margin和padding控制元素间的布局和元素内的布局。
* inline-block 指定对象为行内块元素。介于inline和block，设置了该属性的元素表现为不独占一行，同时可以设置width、height、margin和padding。
* flex 弹性盒布局模型。如果一个容器被指定为flex布局，其子元素的vertical-align、float和clear等属性将会失效。整个flex布局结构中，父元素称为flex容器(flex container)，子元素称为flex项目(flex item)。
  关于弹性盒布局我推荐一篇阮一峰的文章，图文并茂，非常容易理解 [Flex 布局教程：语法篇](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html?utm_source=tuicool)

5. 样式的层级关系，选择器优先级，样式冲突，以及抽离样式模块怎么写，说出思路，有无实践经验

6. css3动画效果属性，canvas、svg的区别，CSS3中新增伪类举例

7. px、em和rem的区别

    px像素(Pixel)。通常指对应设备的物理像素。
    em是相对长度单位。相对于当前元素内文本的字体尺寸。如果当前元素内文本的字体尺寸未被设置，则相对于浏览器的默认字体尺寸。
    rem是CSS3新增的一个相对单位。其相对于HTML根元素文本的字体尺寸。

8. CSS中link 和@import的区别是？



9. 了解过flex吗？
[Flex 布局教程：语法篇](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html?utm_source=tuicool)

## 「 JavaScript 篇 」

### JavaScript 基础 
1. 什么是虚拟DOM？
```
React为啥这么大？因为它实现了一个虚拟DOM（Virtual DOM）。虚拟DOM是干什么的？这就要从浏览器本身讲起

如我们所知，在浏览器渲染网页的过程中，加载到HTML文档后，会将文档解析并构建DOM树，然后将其与解析CSS生成的CSSOM树一起结合产生爱的结晶——RenderObject树，然后将RenderObject树渲染成页面（当然中间可能会有一些优化，比如RenderLayer树）。这些过程都存在与渲染引擎之中，渲染引擎在浏览器中是于JavaScript引擎（JavaScriptCore也好V8也好）分离开的，但为了方便JS操作DOM结构，渲染引擎会暴露一些接口供JavaScript调用。由于这两块相互分离，通信是需要付出代价的，因此JavaScript调用DOM提供的接口性能不咋地。各种性能优化的最佳实践也都在尽可能的减少DOM操作次数。

而虚拟DOM干了什么？它直接用JavaScript实现了DOM树（大致上）。组件的HTML结构并不会直接生成DOM，而是映射生成虚拟的JavaScript DOM结构，React又通过在这个虚拟DOM上实现了一个 diff 算法找出最小变更，再把这些变更写入实际的DOM中。这个虚拟DOM以JS结构的形式存在，计算性能会比较好，而且由于减少了实际DOM操作次数，性能会有较大提升
```
2. 创建对象的几种模式
> 2.1 工厂模式

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

> 2.2 构造函数模式

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

> 2.3 原型模式

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

> 2.4 组合使用构造函数模式和原型模式

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

> 2.5 动态原型模式

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

> 2.6 寄生构造函数模式

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

> 2.7 稳妥构造函数模式

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

3. JavaScript 里有哪些数据类型，解释清楚 null 和 undefined，解释清楚原始数据类型和引用数据类型。比如讲一下 1 和 Number(1)的区别

4. 将一下 prototype 是什么东西，原型链的理解，什么时候用 prototype

5. 函数里的this什么含义，什么情况下，怎么用。

6. apply和 call  什么含义，什么区别？什么时候用。（我有篇文章 重点分析过）

7. 数组和对象有哪些原生方法，列举一下，分别是什么含义，比如连接两个数组用哪个方法，删除数组的指定项和重新组装数组（操作数据的重点）。

8. 怎样避免全局变量污染？ES5严格模式的作用，ES6箭头函数和ES5普通函数一样吗？

### JavaScript 的面向对象

1. JS 模块包装格式都用过哪些，CommonJS、AMD、CMD。定义一个JS 模块代码，最精简的格式是怎样。

2. JS 怎么实现一个类。怎么实例化这个类。

3. 理解闭包吗？请讲一讲闭包在实际开发中的作用；闭包建议频繁使用吗？

4. 说一下了解的js 设计模式，解释一下单例、工厂、观察者。

5. ajax 跨域有哪些方法，jsonp 的原理是什么，如果页面编码和被请求的资源编码不一致如何处理？

## 「 开源工具 」

1. 是否了解开源的架构工具 bower、npm、yeoman、gulp、webpack，有无用过，有无写过，一个 npm 的包里的 package.json 具备的必要的字段都有哪些（名称、版本号，依赖）

2. github常用不常用，关注过哪些项目

3. 会不会用 ps 扣图，png、jpg、gif 这些图片格式解释一下，分别什么时候用。如何优化图像、图像格式的区别

4. 说一下你常用的命令行工具

5. 会不会用git，说上来几个命令，说一下git和svn的区别，有没有用git解决过冲突

## 「 计算机网络基础 」

1. 说一下HTTP 协议头字段说上来几个，是否尽可能详细的掌握HTTP协议。一次完整的HTTP事务是怎样的一个过程？

2. cookies 是干嘛的，服务器和浏览器之间的 cookies 是怎么传的，httponly的cookies和可读写的cookie有什么区别，有无长度限制,请描述一下cookies，sessionStorage和localStorage的区别

3. 从敲入 URL 到渲染完成的整个过程，包括 DOM 构建的过程，说的约详细越好。

4. 是否了解Web注入攻击，说下原理，最常见的两种攻击（XSS 和 CSRF）了解到什么程度。

5. 是否了解公钥加密和私钥加密。如何确保表单提交里的密码字段不被泄露。验证码是干嘛的，是为了解决什么安全问题。

6. 编码常识：文件编码、URL 编码、Unicode编码 什么含义。一个gbk编码的页面如何正确引

## 「 前端框架  」

1. 对 MVC、MVVM的理解

2. vue、angularjs等 相对于 jQuery在开发上有什么优点？

3. 前后分离的思想了解吗？

4. 你上一个项目都用到了那些方法优化js的性能？

5. angular的生命周期？

6. 说一下你对vue和vuex的使用方法，vue的组件复用机制

## 考察学习能力和方法

1. 你每天必须登录的网站（前端技术相关）是什么？

2. 前端技术方面看过哪些书，有无笔记，都有哪些收获。

3. 收藏了哪些代码片段？有想过开源自己的代码嘛？

4. 怎么理解前端技术的大趋势？自己再做哪方面的知识储备？

5. 是否了解或精通其他（后端）的编程语言？

6. 做项目有没有遇到哪些印象深刻的技术攻关，具体遇到什么问题，怎么找答案的，最后怎么解的。

7. 对以后自己的前端职业路线，咋么规划

## 开放性问题（重要）

这些问题往往决定你是否最终被录用或者等到终轮面试，技术点回答错了不要紧，人脑不是机器，是可以恶补的。

但如果你没有思想和独到的思路，基础挖的再深，可能也打动不了面试官，因为比你基础好的一大堆，但每个人的个性思想却是不同的

1. 如果需要你加班，你会加吗，抵触吗？
其实你肯定抵触，但你肯定要回答如果项目需要肯定会加

2. 一个小项目让你自己负责搭建底层一些架构，你能胜任吗？
回答例如：我肯定愿意尝试，并做到最优的选择方案出来

3. 如果项目拖太久，你情绪低落或者厌烦了怎么调节？
回答就是， 你结合自身挑着好听的说就行，就像聊天

4. 你建议自己造轮子，还是利用开源的轮子？
回答：根据实际情况而定，如果开源完全满足 可以自己二次开发就好，大大缩短开发周期
如果实在没有契合度很高的，可以花费几个工作日尝试造轮