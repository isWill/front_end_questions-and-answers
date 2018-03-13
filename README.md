# 前端问答
## 「 CSS篇 」
### 1. CSS 盒子模型，绝对定位和相对定位

#### 盒子模型：
* 标准盒子模型的宽度 = 左右margin + 左右border + 左右padding + width；
* IE盒子模型的宽度 = 左右margin + width(IE中的content包含border和padiing)；
#### 相对定位和绝对定位：

### 2. 清除浮动，什么时候需要清除浮动，清除浮动都有哪些方法

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
     content: "."; 
     display: block; 
     height: 0; 
     visibility: hidden; 
     clear: both;
}
```
改进后的样式为

```css
.fix::after { 
     content: ""; 
     display: table; 
     clear: both;
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

### 3. 如何保持浮层水平垂直居中
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

### 4. position 和 display 的取值和各自的意思和用法

#### position 属性规定元素的定位类型。其属性分别为 static(默认)、absolute、fixed、relative和inherit。
+ static 默认值。可以利用这个值取消定位，使元素快速回归文档流，设置的top，left等值也会随之失效。
+ absolute 生成绝对定位的元素，相对于static定位以外的第一个父元素进行定位。元素脱离文档流，其位置可通过top、bottom、left和right属性进行规定。
+ relative 生成相对定位的元素，相对于其正常位置进行定位。元素未脱离文档流，可通过top、bottom、left和right属性进行规定。
+ fixed 生成绝对定位的元素，相对于浏览器窗口进行定位。元素脱离文档流。其位置可通过top、bottom、left和right属性进行规定。
+ inherit 规定应该从父元素继承 position 属性的值。

#### display 属性规定元素应该生成的框的类型。
+ none 隐藏对象。与visibility属性的hidden值不同，其不为被隐藏的对象保留其文档空间。
+ inline 指定对象为行内元素。行内元素不会独占一行，相邻的行内元素会排列在同一行里，直到一行排不下，才会换行，不可主动设置其width和height，两者由内容撑开。同时也不可设置margin和padding。
+ block 指定对象为块级元素。块级元素会独占一行，可主动设置其width和height。同时也可以通过设置margin和padding控制元素间的布局和元素内的布局。
+ inline-block 指定对象为行内块元素。介于inline和block，设置了该属性的元素表现为不独占一行，同时可以设置width、height、margin和padding。
+ flex 弹性盒布局模型。如果一个容器被指定为flex布局，其子元素的vertical-align、float和clear等属性将会失效。整个flex布局结构中，父元素称为flex容器(flex container)，子元素称为flex项目(flex item)。
  关于弹性盒布局我推荐一篇阮一峰的文章，图文并茂，非常容易理解 [Flex 布局教程：语法篇](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html?utm_source=tuicool)

### 5. 样式的层级关系，选择器优先级，样式冲突，以及抽离样式模块怎么写，说出思路，有无实践经验

样式的层级关系要从CSS在HTML页面的的作用机制说起。
    
首先，当一个web页面使用多个样式表，多个样式表的样式可层叠为一个。在多个样式表之间所定义的样式没有冲突的时候，浏览器会显示所有的样式。如果发生样式定义冲突时，浏览器首先会按照不同样式规则的优先级来应用样式。CSS样式的优先级如下（其中数字3拥有最高的优先权）：
+ 1 浏览器缺省设置；
+ 2 外部样式表（.CSS文件）或者内部样式表（位于<head>元素内部）；
+ 3 内联样式（作为某个元素的style属性的值）。
+ 同等优先级下，以最后定义的样式为准，important比内联高。
    
其次，CSS在使用选择器对元素施加属性的时候会有先后顺序，包括特殊性和重要性等概念在内，这里我们就来详解CSS中的选择器优先级顺序。CSS对于多个选择器的优先性使用了一个叫做特殊性的方式。
##### CSS特殊性
选择器的特殊性分为4个等级，a b c d,从左到右，越左边的越优先，如果一个选择器规则有多个相同类型选择器，则+1.
+ 如果是HTML内样式，那么特殊性最优先，a=1；
+ id选择器的特殊性是b；
+ 类选择器、伪类选择器和属性选择器为c；
+ 元素选择器、伪元素选择器伪d；
+ 如果两个规则的特殊性相同，那么后定义的会覆盖先定义的。
##### CSS重要性
+ CSS中还有一种东西可以无视特殊性，那就是!important，使用此标记的CSS属性总是最优先的。
+ 如果两个属性都有 !important 那么由特殊性来决定优先级。
    
#### 抽离样式模块，说的通俗点就是CSS文件的分类和按需引用：
通常，一个项目我们只引用一个CSS，但是对于较大的项目，我们需要把CSS文件进行分类。
我们按照CSS的性质和用途，将CSS文件分成“公共型样式”、“特殊型样式”、“皮肤型样式”，并以此顺序引用（按需求决定是否添加版本号）。
+ 公共型样式：包括了以下几个部分：“标签的重置和设置默认值”、“统一调用背景图和清除浮动或其他需统一处理的长样式”、“网站通用布局”、“通用模块和其扩展”、“元件和其扩展”、“功能类样式”、“皮肤类样式”。
+ 特殊型样式：当某个栏目或页面的样式与网站整体差异较大或者维护率较高时，可以独立引用一个样式：“特殊的布局、模块和元件及扩展”、“特殊的功能、颜色和背景”，也可以是某个大型控件或模块的独立样式。
+ 皮肤型样式：如果产品需要换肤功能，那么我们需要将颜色、背景等抽离出来放在这里。

### 6. CSS3动画效果属性，canvas、svg的区别，CSS3中新增伪类举例

#### CSS3动画相关的属性：
+ @keyframes	规定动画。
+ animation	所有动画属性的简写属性，除了 animation-play-state 属性。
+ animation-name	规定 @keyframes 动画的名称。
+ animation-duration	规定动画完成一个周期所花费的秒或毫秒。默认是 0。
+ animation-timing-function	规定动画的速度曲线。默认是 "ease"。
+ animation-delay	规定动画何时开始。默认是 0。
+ animation-iteration-count	规定动画被播放的次数。默认是 1。
+ animation-direction	规定动画是否在下一周期逆向地播放。默认是 "normal"。
+ animation-play-state	规定动画是否正在运行或暂停。默认是 "running"。
+ animation-fill-mode	规定对象动画时间之外的状态。
    
#### Canvas 与 SVG 的比较
下表列出了 canvas 与 SVG 之间的一些不同之处。
    
##### Canvas
+ 依赖分辨率
+ 不支持事件处理器
+ 弱的文本渲染能力
+ 能够以 .png 或 .jpg 格式保存结果图像
+ 最适合图像密集型的游戏，其中的许多对象会被频繁重绘
    
##### SVG
+ 不依赖分辨率
+ 支持事件处理器
+ 最适合带有大型渲染区域的应用程序（比如谷歌地图）
+ 复杂度高会减慢渲染速度（任何过度使用 DOM 的应用都不快）
+ 不适合游戏应用
    
CSS3为了区分伪类和伪元素，已经明确规定了伪类用一个冒号来表示，而伪元素则用两个冒号来表示。但因为兼容性的问题，所以现在大部分还是统一的单冒号，但是抛开兼容性的问题，我们在书写时应该尽可能养成好习惯，区分两者。
单冒号(:)用于CSS3伪类，双冒号(::)用于CSS3伪元素。伪元素由双冒号和伪元素名称组成。不过浏览器需要同时支持旧的已经存在的伪元素写法，比如:first-line、:first-letter、:before、:after等，而新的在CSS3中引入的伪元素则不允许再支持旧的单冒号的写法。
    
#### CSS3新增伪类
+ p:first-of-type 选择属于其父元素的首个\<p>元素的每个\<p>元素。
+ p:last-of-type 选择属于其父元素的最后\<p>元素的每个\<p>元素。
+ p:only-of-type 选择属于其父元素唯一的\<p>元素的每个\<p>元素。
+ p:only-child 选择属于其父元素唯一的子元素的每个\<p>元素。
+ p:nth-child(n) 选择属于其父元素的第n个子元素的每个\<p>元素。
+ p:nth-last-child(n) 选择属于其父元素的倒数第n个子元素的每个\<p>元素。
+ p:nth-of-type(n) 选择属于其父元素第n个\<p>元素的每个\<p>元素。
+ p:nth-last-of-type(n)  选择属于其父元素倒数第n个\<p>元素的每个\<p>元素。
+ p:last-child 选择属于其父元素最后一个子元素的每个\<p>元素。
+ p:empty 选择没有子元素的每个\<p>元素（包括文本节点）。
+ p:target  选择当前活动的\<p>元素。
+ :not(p) 选择非\<p>元素的每个元素。
+ :enabled 控制表单控件的可用状态。
+ :disabled  控制表单控件的禁用状态。
+ :checked  单选框或复选框被选中。
    
### 7. px、em和rem的区别

+ px像素(Pixel)。通常指对应设备的物理像素。
+ em是相对长度单位。相对于当前元素内文本的字体尺寸。如果当前元素内文本的字体尺寸未被设置，则相对于浏览器的默认字体尺寸。
+ rem是CSS3新增的一个相对单位。其相对于HTML根元素文本的字体尺寸。

### 8. CSS中link 和@import的区别是？

将样式定义在单独的.CSS的文件里。link和@import都可以在html页面引入CSS文件。有link和@import两种方式，导入方式如下

link方式：

```html
<link rel="stylesheet" type="text/css" href="style.css">
```

@import方式：

```html
<style>
     @import "style.css";
</style>
```
link和@import两种导入css文件的区别：

+ 祖先的差别。link属于HTML标签，而@import完全是css提供的一种方式。link标签除了可以加载css外，还可以做很多其他的事情，比如定义RSS，定义rel链接属性等。@import就只能加载css了；
+ 加载顺序的差别。当一个页面被加载的时候，link引用的css会同时被加载，而@import引用的css会等到页面全部被下载完再被加载。所以有时候浏览@import加载css的页面时开始会没有样式（就是闪烁），网速慢时更为明显；
+ 兼容性的差别。由于@import是css2.1提出的所以老的浏览器不支持，@import只有在IE5以上的才能识别，而link标签无此问题；
+ 使用DOM控制样式时的差别。当使用JavaScript控制DOM去改变样式的时候，只能使用link标签，因为@import不是DOM可以控制的；
+ @import可以在css中再次引入其他样式表，比如可以创建一个主样式表，在主样式表中再引入其他的样式表。



### 9. 了解过flex吗？

[Flex 布局教程：语法篇](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html?utm_source=tuicool)

## 「 JavaScript 篇 」

## JavaScript 基础 
### 1. 什么是虚拟DOM？
```
React为啥这么大？因为它实现了一个虚拟DOM（Virtual DOM）。虚拟DOM是干什么的？这就要从浏览器本身讲起

如我们所知，在浏览器渲染网页的过程中，加载到HTML文档后，会将文档解析并构建DOM树，然后将其与解析CSS生成的CSSOM树一起结合产生爱的结晶——RenderObject树，然后将RenderObject树渲染成页面（当然中间可能会有一些优化，比如RenderLayer树）。这些过程都存在与渲染引擎之中，渲染引擎在浏览器中是于JavaScript引擎（JavaScriptCore也好V8也好）分离开的，但为了方便JS操作DOM结构，渲染引擎会暴露一些接口供JavaScript调用。由于这两块相互分离，通信是需要付出代价的，因此JavaScript调用DOM提供的接口性能不咋地。各种性能优化的最佳实践也都在尽可能的减少DOM操作次数。

而虚拟DOM干了什么？它直接用JavaScript实现了DOM树（大致上）。组件的HTML结构并不会直接生成DOM，而是映射生成虚拟的JavaScript DOM结构，React又通过在这个虚拟DOM上实现了一个 diff 算法找出最小变更，再把这些变更写入实际的DOM中。这个虚拟DOM以JS结构的形式存在，计算性能会比较好，而且由于减少了实际DOM操作次数，性能会有较大提升
```
### 2. 创建对象的几种模式
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

### 3. JavaScript 里有哪些数据类型，解释清楚 null 和 undefined，解释清楚原始数据类型和引用数据类型。比如讲一下 1 和 Number(1)的区别

JavaScript的数据类型分为两种：**原始类型**和**引用类型**；
+ 原始类型包括number、string、boolean、null和undefined。
+ 引用类型包括Object、Function、Date、Number、String和Boolean。

先说相同点，在if语句中，都会被自动转为false，相等运算符甚至直接报告两者相等。`undefined == null //true`。既然undefined和null的含义与用法都差不多，为什么要同时设置两个这样的值，这是有历史原因的。详见[undefined与null的区别](http://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html)
再来说说不同点：
1）null和undefined在现代JS语义里面是有明确区别的：
        
null表示一个值被定义了，定义为“空值”；undefined表示根本不存在定义。
所以设置一个值为null是合理的，如 objA.valueA = null；但设置一个值为undefined是不合理的，如 objA.valueA = undefined；// 应该直接使用delete objA.valueA;任何一个存在引用的变量值为undefined都是一件错误的事情。这样判断一个值是否存在，就可以用 objA.valueA === undefined // 不应使用null 因为undefined == null，而null表示该值定义为空值。这个语义在JSON规范中被强化，这个标准中不存在undefined这个类型，但存在表示空值的null。在一些使用广泛的库（比如jQuery）中的深度拷贝函数会忽略undefined而不会忽略null，也是针对这个语义的理解。
2）JS中同时存在undefined和null是合理的。
首先在Java中不存在undefined是很合理的：Java是一个静态类型语言，对于Java来说不可能存在一个“不存在”的成员（不存在的话直接就编译失败了），所以只用null来表示语义上的空值。而JavaScript是一门动态类型语言，成员除了表示存在的空值外，还有可能根本就不存在（因为存不存在只在运行期才知道），所以这就要一个值来表示对某成员的getter是取不到值的。
包装类型：专门封装原始类型的值，提供对值进行操作的方法
为什么使用：原始类型的值不带任何操作方法，必须通过包装类型提供对原始类型操作的方法
何时使用：在试图对原始类型的值调用方法时，js会自动创建对应的包装类型对象，封装原始类型的值。    
    
### 4. 讲一下 prototype 是什么东西，原型链的理解，什么时候用 prototype
我们创建的每个函数都有一个prototype（原型）属性，这个属性是一个指针，指向一个对象，而这个对象的用途是包含可以由特定类型的所有实例共享的属性和方法。
如果按照字面意思来理解，那么prototype就是通过调用构造函数而创建的那个对象实例的原型对象。使用原型对象的好处是可以让所有对象实例共享它所包含的属性和方法。
换句话说，不必在构造函数中定义对象实例的信息，而是可以将这些信息直接添加到原型对象中。
ECMAScript中描述了原型链的概念，并将原型链作为实现继承的主要方法。其基本思想是利用原型让一个引用类型继承另一个引用类型的属性和方法。简单回顾一下构造函数、原型和实例的关系：每个构造函数都有一个原型对象，原型对象都包含一个指向构造函数的指针，而实例都包含一个指向原型对象的内部指针。那么，假如我们让原型对象等于另一个类型的实例，结果会怎么样呢？显然，此时的原型对象将包含一个指向另一个原型的指针，相应地，另一个原型中也包含着一个指向另一个构造函数的指针。假如另一个原型又是另一个类型的实例，那么上述关系依然成立，如此层层递进，就构成了实例与原型的链条。这就是所谓原型链的基本概念。

### 5. 函数里的this什么含义，什么情况下怎么用。

    this是JavaScript语言的一个关键字。
    它代表函数运行时，自动生成的一个内部对象，只能在函数内部使用，比如：
    
    ```javascript
    function test() {
       this.x = 1;
    }
    ```
    随着函数使用场合的不同，this的值会发生变化。但是有一个总的原则，那这时this指的是，调用函数的那个对象。
    下面分四种情况，详细讨论this的用法。
    情况一：纯粹的函数调用
    这是函数的最通常用法，属于全局性调用，因此this就代表全局对象Global。
    请看下面这段代码，它的运行结果是1。
    
    ```javascript
    function test() {
       this.x = 1;
       alert(this.x);
    }
 
    test(); // 1
    ```
    为了证明this就是全局对象，我对代码做一些改变：
    
    ```javascript
    var x = 1;
    function test() {
       alert(this.x);
    }
     
    test(); // 1
    ```
    运行结果还是1.再变一下：
    
    ```javascript
    function test() {
       this.x = 0;
    }
     
    test();
    alert(x);
    ```
    
    情况二：作为对象方法的调用
    函数还可以作为某个对象的方法调用，这时this就指这个上级对象。
    
    ```javascript
   function test() {
       alert(this.x);
   }
    
   var o = {};
   o.x = 1;
   o.m = test;
   o.m(); //1
    ```
    
    情况三：作为构造函数调用
    所谓构造函数，就是通过这个函数生成一个新对象（object）。这时，this就指这个新对象。
    
    ```javascript
    function test() {
        this.x =1;
    }
        
    var o = new test();
    alert(o.x); //1
    ```
    运行结果为1。为了表明这时this不是全局对象，我对代码做一些改变：
    
    ```javascript
    var x = 2;
    function test() {
       this.x = 1;
    }
    var o = new test();
    alert(x); // 2
    
    ```
    运行结果为2，表明全局变量x的值根本没变。
    
    情况四：apply调用
    apply()是函数对象的一个方法，它的作用是改变函数的调用对象，它的第一个参数就表示改变后的调用这个函数。
    因此，this指的就是这第一个参数。
    
    ```javascript
    var x = 0;
    function test() {
       alert(this.x);
    }
    var o = {};
    o.x = 1;
    o.m = test;
    o.m.apply(); //0
        
    ```
    apply()的参数为空时，默认调用全局对象。因此，这时的运行结果为0，证明this指的是全局对象。
    如果把最后一行代码修改为
    
    ```javascript
    o.m.apply(o); //1
    ```
    运行结果就变成了1，证明了这时this代表的是对象o。
    
    引用**阮一峰**的文章[Javascript的this用法](http://www.ruanyifeng.com/blog/2010/04/using_this_keyword_in_javascript.html)

### 6. apply和 call  什么含义，什么区别？什么时候用。（我有篇文章 重点分析过）
    
    引用公众号**前端你别闹**的文章[Java script | call or apply](https://mp.weixin.qq.com/s?__biz=MzI5ODM3MjcxNQ==&mid=2247483821&idx=1&sn=d7491f01c21b33c4690c040ff9fe8fc9&mpshare=1&scene=23&srcid=0524HXxZHRuOj7RI1vYYXkmZ#rd)

### 7. 数组对象有哪些原生方法，列举一下，分别是什么含义，比如连接两个数组用哪个方法，删除数组的指定项和重新组装数组（操作数据的重点）。

    7.1 检测数组
    
    ```javascript
    var a = [];
    var b = {};
    Array.isArray(a); // true
    Array.isArray(b); // false
    ```
    
    7.2 转换方法
    
    ```javascript
    var colors = ["red","green","blue"];
    colors.toString(); // "red,green,blue" 调用数组内每一项的toString()方法
    colors.toLocaleString(); // "red,green,blue" 调用数组内每一项的toLocalString()方法
    colors.valueOf(); // ["red","green","blue"]
    alert(colors); // "red,green,blue" 由于alert()要接收字符串参数，所以它会隐式地调用toString()
    colors.join("|"); // "red|green|blue"
    colors.join(); // "red,green,blue"  如果不给join()方法传入任何职，或者给它传入undefined，则使用逗号作为分隔符
    ```
    
    7.3 栈方法
    
    ```javascript
    var colors = [];
    var count = colors.push("red","green"); 
    console.log(count); //2 数组调用push()后返回数组的长度
    var item = colors.pop();
    console.log(item); //green 在调用pop()时，它会返回数组的最后一项
    ```
    
    7.4 队列方法
    
    ```javascript
    var colors = [];
    colors.push("red","green"); // 从数组后面添加元素
    var item = colors.shift(); // 从数组前面取出元素
    console.log(item); //red 在调用shift()时，它会返回数组的第一项
    ```
    ```javascript
    var colors = [];
    var count = colors.unshift("red","green"); // 从数组后面添加元素
    console.log(count); //2 数组调用push()后返回数组的长度
    var item = colors.pop(); // 从数组前面取出元素
    console.log(item); //green
    ```
    
    7.5 重排序方法
    
    ```javascript
    var values = [1,2,3,4,5,6];
    values.reverse(); // reverse()反转数组项的顺序
    console.log(values);// [6, 5, 4, 3, 2, 1]
    ```
    ```javascript
    var values = [0,1,5,10,15];
    values.sort(); // sort()方法默认按升序排列数组项，为了排序，sort()方法会调用每个数组项的toString（）方法转型，然后比较得到的字符串，以确定顺序。
    console.log(values);// [0,1,10,15,5]
    ```
    ```javascript
    var values = [0,1,5,10,15];
    function compare(value1, value2){ // 这个比较函数可以适用于大多数数据类型
       if(value1 < value2){
           return -1; 
       } else if (value1 > value2){
           return 1;
       } else {
           return 0; 
       }
    }
    values.sort(compare); // sort()方法可以接收一个比较函数作为参数。
    console.log(values);// [0,1,5,10,15]
    ```
    
    **reverse()和sort()方法的返回值是经过排序之后的数组。**
    
    7.6 操作方法
    ```javascript
    var colors = ["red","green","blue"];
    var colors2 = colors.concat("yellow",["black", "brown"]);
    console.log(colors); //["red","green","blue"] 数组colors不变
    console.log(colors2); //["red","green","blue","yellow","black","brown"] 数组在调用concat()方法后返回一个新数组
    ```
    ```javascript
    var colors = ["red","green","blue","yellow","purple"];
    var colors2 = colors.slice(1); // slice()方法可以接受一或两个参数，即要返回项的起始和结束位置。在只有一个参数的情况下，slice()方法返回从该参数指定位置开始到当前数组末尾的所有项。如果有两个参数，该方法返回起始和结束位置之间的项——但不包括结束位置的项。注意，slice()方法不会影响原始数组。
    var colors3 = colors.slice(1,4);
    console.log(colors2); //["green", "blue", "yellow", "purple"]
    console.log(colors3); //["green", "blue", "yellow"]
    ```
    ```javascript
    var colors = ["red","green","blue","yellow","purple"];
    var colors2 = colors.slice(1); // slice()方法可以接受一或两个参数，即要返回项的起始和结束位置。在只有一个参数的情况下，slice()方法返回从该参数指定位置开始到当前数组末尾的所有项。如果有两个参数，该方法返回起始和结束位置之间的项——但不包括结束位置的项。注意，slice()方法不会影响原始数组。
    var colors3 = colors.slice(1,4);
    console.log(colors2); //["green", "blue", "yellow", "purple"]
    console.log(colors3); //["green", "blue", "yellow"]
    ```
    splice()的主要用途是向数组的中部插入项，但使用这种方法的方式则有如下3种。
    * **删除：** 可以删除任意数量的项，只需指定2个参数：要删除的第一项的位置和要删除的项数。例如，splice(0,2)会删除数组中的前两项。
    * **插入：** 可以向指定位置插入任意数量的项，只需提供3个参数：起始位置、0（要删除的项数）和要插入的项。如果要插入多个项，可以再传入第四、第五，以至任意多个项。例如，splice(2,0,"red","green")会从当前数组的位置2开始插入字符串"red"和"green"。
    * **替换：** 可以向指定位置插入任意数量的项，且同时删除任意数量的项，只需指定3个参数：起始位置、要删除的项数和要插入的任意数量的项。插入的项数不必与删除的项数相等。例如，splice(2,1,"red","green")会删除当前数组位置2的项，然后再从位置2开始插入字符串"red"和"green"。
    
    ```javascript
    var colors = ["red","green","blue"];
    var removed = colors.splice(0,1);
    console.log(colors); // ["green","blue"]
    console.log(removed); // ["red"]
 
    removed = colors.splice(1,0,"yellow","orange");
    console.log(colors); // ["green","yellow","orange","blue"]
    console.log(removed); // 返回一个空数组
    
    removed = colors.splice(1,1,"red","purple");
    console.log(colors); // ["green","red","purple","orange","blue"]
    console.log(removed); // ["yellow"] 返回的数组中只包含一项
    ```
    
    7.7 位置方法
    
    ```javascript
    var numbers = [1,2,3,4,5,4,3,2,1];
    console.log(numbers.indexOf(4)); // 3
    console.log(numbers.lastIndexOf(4)); // 5
    console.log(numbers.indexOf(4,4)); // 5
    console.log(numbers.lastIndexOf(4,4)); // 3
    var person = {name:"Nicholas"};
    var people = [{name:"Nicholas"}];
    var morePeople = [person];
    console.log(people.indexOf(person)); // -1
    console.log(morePeople.indexOf(person)); // 0
    ```
    
    7.8 迭代方法
    * every()：对数组中的每一项运行给定函数，如果该函数对每一项都返回true，则返回true。
    * filter()：对数组中的每一项运行给定函数，返回该函数会返回true的项组成的数组。
    * forEach()：对数组中的每一项运行给定函数，这个方法没有返回值。
    * map()：对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组。
    * some()：对数组中的每一项运行给定函数，如果该函数对任一项返回true，则返回true。
    以上方法都不会修改数组中的包含的值。
    
    ```javascript
    var numbers = [1,2,3,4,5,4,3,2,1];
    var everyResult = numbers.every(function(item, index, array){
       return (item > 2);
    });
    console.log(everyResult); //false
    var someResult = numbers.some(function(item, index, array){
       return (item > 2);
    });
    console.log(someResult); //true
    ```
    ```javascript
    var numbers = [1,2,3,4,5,4,3,2,1];
    var filterResult = numbers.filter(function(item, index, array){
       return (item > 2);
    });
    console.log(filterResult); //[3,4,5,4,3]
    ```
    ```javascript
    var numbers = [1,2,3,4,5,4,3,2,1];
    var mapResult = numbers.map(function(item, index, array){
       return item * 2;
    });
    console.log(mapResult); //[2, 4, 6, 8, 10, 8, 6, 4, 2]
    ```
    ```javascript
    var numbers = [1,2,3,4,5,4,3,2,1];
    numbers.forEach(function(item, index, array){
       // 执行某些操作
    });
    ```
    
    7.9 归并方法
    ECMAScript5新增了两个归并数组的方法：reduce()和reduceRight()。这个两个方法都会迭代数组的所有项，然后构建一个最终返回的值。
    
    ```javascript
    var values = [1,2,3,4,5];
    var sum = values.reduce(function(prev, cur, index, array){
       return prev + cur;
    });
    console.log(sum); //15
    ```
    
    ```javascript
    var values = [1,2,3,4,5];
    var sum = values.reduceRIght(function(prev, cur, index, array){
       return prev + cur;
    });
    console.log(sum); //15
    ```
    
### 8. 怎样避免全局变量污染？ES5严格模式的作用，ES6箭头函数和ES5普通函数一样吗？

    引用CSDN博客“小北哥哥”的博文[防止js全局变量污染方法总结-待续](http://blog.csdn.net/xllily_11/article/details/52816699)
    
    严格模式是为JavaScript定义了一种不同的解析与执行模型。在严格模式下，ECMAScript3中的一些不确定的行为将得到处理，而且对某些不安全的操作也会抛出错误。
    启用安全模式的两种方式：
    
    ```javascript
    //全局使用
    'use strict';
    ```
    ```javascript
    //函数内使用
    function test(){
       'use strict';
    }
    ```
    引用CSDN博客“brizer的博客”的博文[初步探究ES6之箭头函数](http://blog.csdn.net/mevicky/article/details/49942559)
    
## JavaScript 的面向对象

### 1. JS 模块包装格式都用过哪些，CommonJS、AMD、CMD。定义一个JS 模块代码，最精简的格式是怎样。
   
   - [Javascript模块化编程（一）：模块的写法](http://www.ruanyifeng.com/blog/2012/10/javascript_module.html)
   - [Javascript模块化编程（二）：AMD规范](http://www.ruanyifeng.com/blog/2012/10/asynchronous_module_definition.html)
   - [Javascript模块化编程（三）：require.js的用法](http://www.ruanyifeng.com/blog/2012/11/require_js.html)

### 2. JS 怎么实现一个类。怎么实例化这个类。
   
   [Javascript定义类（class）的三种方法](http://www.ruanyifeng.com/blog/2012/07/three_ways_to_define_a_javascript_class.html)

### 3. 理解闭包吗？请讲一讲闭包在实际开发中的作用；闭包建议频繁使用吗？

    [学习Javascript闭包（Closure）](http://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html)

### 4. 说一下了解的js 设计模式，解释一下单例、工厂、观察者。

### 5. ajax 跨域有哪些方法，jsonp 的原理是什么，如果页面编码和被请求的资源编码不一致如何处理？

   JSONP是通过动态<script>元素来使用的，使用时可以为src属性指定一个跨域URL。这里的<script>元素与<img>元素类似，都有能力不受限制地从其他域加载资源。因为JSONP
   是有效的JavaScript代码，所以请求完成后，即在JSONP响应加载到页面中以后，就会立即执行。
   
   ```javascript
    function handleResponse(){
       alert("You're at IP address " + response.ip + ", which is in " + response.city + ", " + response.region_name)
    }
    var script = document.createElement("script");
    script.src = "http://freegeoip.net/json/?callback=handleResponse";
    document.body.insertBefore(script, document.body.firstChild);
   ```

## 「 开源工具 」

### 1. 是否了解开源的架构工具 bower、npm、yeoman、gulp、webpack，有无用过，有无写过，一个 npm 的包里的 package.json 具备的必要的字段都有哪些（名称、版本号，依赖）

### 2. github常用不常用，关注过哪些项目

### 3. 会不会用 ps 扣图，png、jpg、gif 这些图片格式解释一下，分别什么时候用。如何优化图像、图像格式的区别

    可参考[png、jpg、gif三种图片格式的区别](http://www.cnblogs.com/Fran-Lily/p/3792641.html)

### 4. 说一下你常用的命令行工具

    SecureCRT: 一款Windows下登录UNIX或Linux服务器主机的软件。
    Git bash: Windows下的命令行工具。主要用于git。

### 5. 会不会用git，说上来几个命令，说一下git和svn的区别，有没有用git解决过冲突

    SVN是集中式版本控制系统，git是分布式版本控制系统，有什么区别呢？
    可以阅读廖雪峰的文章[集中式vs分布式](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001374027586935cf69c53637d8458c9aec27dd546a6cd6000)

## 「 计算机网络基础 」

### 1. 说一下HTTP 协议头字段说上来几个，是否尽可能详细的掌握HTTP协议。一次完整的HTTP事务是怎样的一个过程？

    * [互联网协议入门（一）](http://www.ruanyifeng.com/blog/2012/05/internet_protocol_suite_part_i.html)
    * [互联网协议入门（二）](http://www.ruanyifeng.com/blog/2012/06/internet_protocol_suite_part_ii.html)
    * [HTTP 协议入门](http://www.ruanyifeng.com/blog/2016/08/http.html)

### 2. cookies 是干嘛的，服务器和浏览器之间的 cookies 是怎么传的，httponly的cookies和可读写的cookie有什么区别，有无长度限制,请描述一下cookies，sessionStorage和localStorage的区别

### 3. 从敲入 URL 到渲染完成的整个过程，包括 DOM 构建的过程，说的约详细越好。

### 4. 是否了解Web注入攻击，说下原理，最常见的两种攻击（XSS 和 CSRF）了解到什么程度。

### 5. 是否了解公钥加密和私钥加密。如何确保表单提交里的密码字段不被泄露。验证码是干嘛的，是为了解决什么安全问题。

### 6. 编码常识：文件编码、URL 编码、Unicode编码 什么含义。一个gbk编码的页面如何正确引

## 「 前端框架  」

### 1. 对 MVC、MVVM的理解

### 2. vue、angularjs等 相对于 jQuery在开发上有什么优点？

### 3. 前后分离的思想了解吗？

### 4. 你上一个项目都用到了那些方法优化js的性能？

### 5. angular的生命周期？

### 6. 说一下你对vue和vuex的使用方法，vue的组件复用机制

## 考察学习能力和方法

### 1. 你每天必须登录的网站（前端技术相关）是什么？

github

### 2. 前端技术方面看过哪些书，有无笔记，都有哪些收获。

### 3. 收藏了哪些代码片段？有想过开源自己的代码嘛？

### 4. 怎么理解前端技术的大趋势？自己再做哪方面的知识储备？

### 5. 是否了解或精通其他（后端）的编程语言？

### 6. 做项目有没有遇到哪些印象深刻的技术攻关，具体遇到什么问题，怎么找答案的，最后怎么解的。

### 7. 对以后自己的前端职业路线，咋么规划

## 开放性问题（重要）

这些问题往往决定你是否最终被录用或者等到终轮面试，技术点回答错了不要紧，人脑不是机器，是可以恶补的。

但如果你没有思想和独到的思路，基础挖的再深，可能也打动不了面试官，因为比你基础好的一大堆，但每个人的个性思想却是不同的

### 1. 如果需要你加班，你会加吗，抵触吗？

其实你肯定抵触，但你肯定要回答如果项目需要肯定会加

### 2. 一个小项目让你自己负责搭建底层一些架构，你能胜任吗？

回答例如：我肯定愿意尝试，并做到最优的选择方案出来

### 3. 如果项目拖太久，你情绪低落或者厌烦了怎么调节？

回答就是， 你结合自身挑着好听的说就行，就像聊天

### 4. 你建议自己造轮子，还是利用开源的轮子？

回答：根据实际情况而定，如果开源完全满足 可以自己二次开发就好，大大缩短开发周期
如果实在没有契合度很高的，可以花费几个工作日尝试造轮
