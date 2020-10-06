1 JavaScript简介

![1599376927172](D:\Typora_Image\1599376927172.png)

* ECMAScript

  ECMA-262定义的ECMAScript和Web浏览器没有依赖关系。

* DOM（文档对象模型）

  DOM把整个页面映射为一个多层节点结构，

  通过DOM 创建的这个表示文档的树形图，开发人员获得了控制页面内容和结构的主动权。借助DOM 提供的API，开发人员可以轻松自如地删除、添加、替换或修改任何节点。

  DOM并不只是针对JS的

* BOM（浏览器对象模型）

  开发人员使用BOM 可以控制浏览器显示的页面以外的部分。

# 2 在HTML 中使用JavaScript

## <script>元素

使用<script>元素的方式有两种：直接在页面中嵌入JavaScript 代码和包含外部JavaScript文件。

无论如何包含代码，只要不存在defer 和async 属性，浏览器都会按照<script>元素在页面中出现的先后顺序对它们依次进行解析。

# 3 基本概念

## 语法

严格模式

```javascript
function doSomething() {
    "use strict";
    // body
}
```

**双逻辑非**：双重非（`!!`）运算符

可能使用双重非运算符的一个场景，是**显式地将任意值强制转换为其对应的[布尔值](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Data_structures#布尔类型)**。这种转换是基于被转换值的 "truthyness" 和 "falsyness"的（参见 [truthy](https://developer.mozilla.org/zh-CN/docs/Glossary/Truthy) 和 [falsy](https://developer.mozilla.org/zh-CN/docs/Glossary/Falsy)）。

同样的转换可以通过 [Boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean) 函数完成。

```javascript
n1 = !!true                   // !!truthy 返回 true
n2 = !!{}                     // !!truthy 返回 true: 任何 对象都是 truthy 的…
n3 = !!(new Boolean(false))   // …甚至 .valueOf() 返回 false 的布尔值对象也是！
n4 = !!false                  // !!falsy 返回 false
n5 = !!""                     // !!falsy 返回 false
n6 = !!Boolean(false)         // !!falsy 返回 false
```

## 变量

虽然我们不建议修改变量所保存值的类型，但这种操作在ECMAScript 中完全有效。

用var操作符定义的变量将成为定义该变量的作用域中的局部变量

虽然省略var 操作符可以定义全局变量，但这也不是我们推荐的做法。

```javascript
function test(){
	message = "hi"; // 全局变量
}
test();
alert(message); // "hi"
```

## 数据类型

Undefined、Null、Boolean、Number和String。

* 在使用var 声明变量但未对其加以初始化时，这个变量的值就是undefined

* null值表示一个空对象指针，undefined值是派生自null值的

```javascript
alert(null == undefined); // true
```

* 浮点数值

  永远不要测定某个特定的浮点数值，这是使用基于IEEE754 数值的浮点计算的通病

* 数值范围

  Infinity不是能够参与计算的数值

* NaN（一个特殊的**数值**）

  在ECMAScript 中，任何数值除以0会返回NaN，

  NaN 本身有两个非同寻常的特点。

  首先，任何涉及NaN 的操作（例如NaN/10）都会返回NaN，这个特点在多步计算中有可能导致问题。

  其次，NaN 与任何值都不相等，包括NaN 本身。例如，下面的代码会返回false：

  ```javascript
  alert(NaN == NaN); //false
  ```


## 语句

* with语句

  with 语句的作用是将代码的作用域设置到一个特定的对象中

  定义with 语句的目的主要是为了简化多次编写同一个对象的工作

  ```javascript
  var qs = location.search.substring(1);
  var hostName = location.hostname;
  var url = location.href;
  
  // 上面几行代码都包含location 对象。如果使用with 语句，可以把上面的代码改写成如下所示：
  with(location){
  var qs = search.substring(1);
  var hostName = hostname;
  var url = href;
  }
  ```

  ==使用with语句会使得性能下降，大型项目中不建议使用==

# 4 作用域

## 没有块级作用域

```javascript
if (true) {
	var color = "blue";
}
alert(color); //"blue"
```

这里是在一个if 语句中定义了变量color。如果是在C、C++或Java 中，color 会在if 语句执行完毕后被销毁。但在JavaScript 中，if 语句中的变量声明会将变量添加到当前的执行环境（在这里是全局环境）中。

**模仿块级作用域**

JavaScript 从来不会告诉你是否多次声明了同一个变量；遇到这种情况，它只会对后续的声明视而不见（不过，它会执行后续声明中的变量初始化）。匿名函数可以用来模仿块级作用域并避免这个问题。

用作块级作用域（通常称为私有作用域）的匿名函数的语法如下所示。

```javascript
(function(){
	//这里是块级作用域
})();
```

## 声明变量

使用var 声明的变量会自动被添加到最接近的环境中

```javascript
function add(num1, num2) {
	var sum = num1 + num2;
	return sum;
}
var result = add(10, 20); //30
alert(sum); //由于sum 不是有效的变量，因此会导致错误
```

如果省略这个例子中的var 关键字，那么当add()执行完毕后，sum 也将可以访问到：

```javascript
function add(num1, num2) {
	sum = num1 + num2;
	return sum;
}
var result = add(10, 20); //30
alert(sum); //30
```

# 5 引用类型

## Array类型

### 栈

```javascript
var colors = new Array(); // 创建一个数组
var count = colors.push("red", "green"); // 推入两项
alert(count); //2
count = colors.push("black"); // 推入另一项
alert(count); //3
var item = colors.pop(); // 取得最后一项
alert(item); //"black"
alert(colors.length); //2
```

### 队列

```javascript
var colors = new Array(); //创建一个数组
var count = colors.push("red", "green"); //推入两项
alert(count); //2
count = colors.push("black"); //推入另一项
alert(count); //3
var item = colors.shift(); //取得第一项
alert(item); //"red"
alert(colors.length); //2
```

### 重排序方法

即使数组中的每一项都是数值，sort()方法比较的也是字符串，如下所示。

```javascript
var values = [0, 1, 5, 10, 15];
values.sort();
alert(values); //0,1,10,15,5
```

```javascript
function compare(value1, value2){
return value2 - value1;
}

var values = [0, 1, 5, 10, 15];
values.sort(compare);
alert(values); // 15,10,5,1,0
```

### splice()

下面我们来介绍splice()方法，这个方法恐怕要算是最强大的数组方法了，它有很多种用法。
splice()的主要用途是向数组的中部插入项，但使用这种方法的方式则有如下3 种。

* 删除：可以删除任意数量的项，只需指定2 个参数：要删除的第一项的位置和要删除的项数。
  例如，splice(0,2)会删除数组中的前两项。
* 插入：可以向指定位置插入任意数量的项，只需提供3 个参数：起始位置、0（要删除的项数）
  和要插入的项。如果要插入多个项，可以再传入第四、第五，以至任意多个项。例如，
  splice(2,0,"red","green")会从当前数组的位置2 开始插入字符串"red"和"green"。
* 替换：可以向指定位置插入任意数量的项，且同时删除任意数量的项，只需指定3 个参数：起
  始位置、要删除的项数和要插入的任意数量的项。插入的项数不必与删除的项数相等。例如，
  splice (2,1,"red","green")会删除当前数组位置2 的项，然后再从位置2 开始插入字符串
  "red"和"green"。

### 迭代方法

传入这些方法中的函数会接收三个参数：数组项的值、该项在数组中的位置和数组对象本身

* every()：对数组中的每一项运行给定函数，如果该函数对每一项都返回true，则返回true。
* filter()：对数组中的每一项运行给定函数，返回该函数会返回true 的项组成的数组。
* forEach()：对数组中的每一项运行给定函数。这个方法没有返回值。
* map()：对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组。
* some()：对数组中的每一项运行给定函数，如果该函数对任一项返回true，则返回true。

### 归并方法

接收4 个参数：前一个值、当前值、项的索引和数组对象。这个函数返回的任何值都会作为第一个参数自动传给下一项

```javascript
var values = [1,2,3,4,5];
var sum = values.reduce(function(prev, cur, index, array){
return prev + cur;
});
alert(sum); //15
```

### 函数内部属性

```javascript
function factorial(num){
    if (num <=1) {
        return 1;
    } else {
        return num * factorial(num-1)
    }
}	
```

改为：

```javascript
function factorial(num){
    if (num <=1) {
    	return 1;
    } else {
   		return num * arguments.callee(num-1)
    }
}	
```

在这个重写后的factorial()函数的函数体内，没有再引用函数名factorial。这样，无论引用函数时使用的是什么名字，都可以保证正常完成递归调用。

==函数的名字仅仅是一个包含指针的变量而已==

# 6 面向对象的程序设计

## 创建对象

* 工厂模式

  ```javascript
  function createPerson(name, age, job){
      var o = new Object();
      o.name = name;
      o.age = age;
      o.job = job;
      o.sayName = function(){
      alert(this.name);
  };
  	return o;
  }
  var person1 = createPerson("Nicholas", 29, "Software Engineer");
  var person2 = createPerson("Greg", 27, "Doctor");
  ```

* 构造函数模式

  使用构造函数的主要问题，就是每个方法都要在每个实例上重新创建一遍。

  ```javascript
  function Person(name, age, job){
      this.name = name;
      this.age = age;
      this.job = job;
      this.sayName = function(){
      alert(this.name);
  };
  }
  var person1 = new Person("Nicholas", 29, "Software Engineer");
  var person2 = new Person("Greg", 27, "Doctor");
  ```

* 原型模式

  理解原型对象：无论什么时候，只要创建了一个新函数，就会根据一组特定的规则为该函数创建一个prototype
  属性，这个属性指向函数的原型对象。

  ```javascript
  function Person(){
  }
  Person.prototype.name = "Nicholas";
  Person.prototype.age = 29;
  Person.prototype.job = "Software Engineer";
  Person.prototype.sayName = function(){
  alert(this.name);
  };
  
  var person1 = new Person();
  var person2 = new Person();
  person1.name = "Greg";
  alert(person1.name); //"Greg"——来自实例
  alert(person2.name); //"Nicholas"——来自原型
  delete person1.name;
  alert(person1.name); //"Nicholas"——来自原型
  ```

  更简单的原型语法：

  ```javascript
  function Person(){
  }
  Person.prototype = {
      name : "Nicholas",
      age : 29,
      job: "Software Engineer",
      sayName : function () {
      	alert(this.name);
  	}
  };	
  ```

  问题：原型模式的最大问题是由其共享的本性所导致的。

* **组合使用构造函数模式和原型模式（推荐）**

  ```javascript
  function Person(name, age, job){
      this.name = name;
      this.age = age;
      this.job = job;
      this.friends = ["Shelby", "Court"];
  }
  Person.prototype = {
      constructor : Person,
      sayName : function(){
      	alert(this.name);
  	}
  }
  var person1 = new Person("Nicholas", 29, "Software Engineer");
  var person2 = new Person("Greg", 27, "Doctor");
  person1.friends.push("Van");
  alert(person1.friends); //"Shelby,Count,Van"
  alert(person2.friends); //"Shelby,Count"
  alert(person1.friends === person2.friends); //false
  alert(person1.sayName === person2.sayName); //true
  ```

* 动态原型模式

  ```javascript
  function Person(name, age, job){
      //属性
      this.name = name;
      this.age = age;
      this.job = job;	
      
      // 方法
      if (typeof this.sayName != "function") {
          Person.prototype.sayName = function() {
              alert(this.name);
          };
      }
  }
  ```

## 继承

* 组合继承

```javascript
function SuperType(name){
    this.name = name;
    this.colors = ["red", "blue", "green"];
}
SuperType.prototype.sayName = function(){
alert(this.name);};
function SubType(name, age){
    //继承属性
    SuperType.call(this, name);
    this.age = age;
}
//继承方法
SubType.prototype = new SuperType();
SubType.prototype.constructor = SubType;
SubType.prototype.sayAge = function(){
	alert(this.age);
};
var instance1 = new SubType("Nicholas", 29);
instance1.colors.push("black");
alert(instance1.colors); //"red,blue,green,black"
instance1.sayName(); //"Nicholas";
instance1.sayAge(); //29
var instance2 = new SubType("Greg", 27);
alert(instance2.colors); //"red,blue,green"
instance2.sayName(); //"Greg";
instance2.sayAge(); //27
```

* 寄生组合式继承

```javascript
function inheritPrototype(subType, superType){
    var prototype = object(superType.prototype); // 创建对象
    prototype.constructor = subType; // 增强对象
    subType.prototype = prototype; // 指定对象
}
```

# 7 函数表达式

## 递归

因此，在编写递归函数时，使用`arguments.callee`总比使用函数名更保险。

## 闭包

闭包是指有权访问另一个函数作用域中的变量的函数。

## 模块模式（单例模式）

```javascript
var application = function(){
//私有变量和函数
    var components = new Array();
    //初始化
    components.push(new BaseComponent());
    //公共
    return {
        getComponentCount : function(){
            return components.length;
        },
        registerComponent : function(component){
            if (typeof component == "object"){
                components.push(component);
            }
        }
    };
}();
```

## 增强的模块模式

```javascript
var application = function(){
//私有变量和函数
    var components = new Array();
    //初始化
    components.push(new BaseComponent());
    //创建application 的一个局部副本
    var app = new BaseComponent();
    //公共接口
    app.getComponentCount = function(){
    	return components.length;
    };
    app.registerComponent = function(component){
        if (typeof component == "object"){
        	components.push(component);
    	}
    };
    //返回这个副本
    return app;
}();
```

# 8 BOM（浏览器对象模型）

## window对象

全局变量不能通过delete 操作符删除，而直接在window 对象上的定义的属性可以。

```javascript
var age = 29;
window.color = "red";
//在IE < 9 时抛出错误，在其他所有浏览器中都返回false
delete window.age;
//在IE < 9 时抛出错误，在其他所有浏览器中都返回true
delete window.color; //returns true
alert(window.age); //29
alert(window.color); //undefined
```

* top 对象始终指向最高（最外）层的框架，也就是浏览器窗口
* parent（父）对象始终指向当前框架的直接上层框架。
* 因此每个框架都有一套自己的构造函数，这些构造函数一一对应，但并不相等。

### 间歇调用（最好不要使用）

```javascript
var num = 0;
var max = 10;
var intervalId = null;
function incrementNumber() {
    num++;
    //如果执行次数达到了max 设定的值，则取消后续尚未执行的调用
    if (num == max) {
        clearInterval(intervalId);
        alert("Done");
    }
}
intervalId = setInterval(incrementNumber, 500);
```

**使用超时调用来模拟**

```javascript
var num = 0;
var max = 10;
function incrementNumber() {
    num++;
    //如果执行次数未达到max 设定的值，则设置另一次超时调用
    if (num < max) {
        setTimeout(incrementNumber, 500);
    } else {
        alert("Done");
    }
}
setTimeout(incrementNumber, 500);
```

### 系统对话框

```javascript
if (confirm("Are you sure?")) {
	alert("I'm so glad you're sure! ");
} else {
	alert("I'm sorry to hear you're not sure. ");
}
```

```javascript
var result = prompt("What is your name? ", "");
if (result !== null) {
	alert("Welcome, " + result);
}
```

## location对象

## navigator对象

## screen对象

## history对象

# 9 客户端检测

但最重要的还是要知道，不到万不得已，就不要使用客户端检测。只要能找到更通用的方法，就应该优先采用更通用的方法。一言以蔽之，先设计最通用的方案，然后再使用特定于浏览器的技术增强该方案。

**在可能的情况下，要尽量使用typeof 进行能力检测**

## 能力检测

## 怪癖检测

检查浏览器有什么缺陷（bug）

##  用户代理检测

# 10 DOM（文档对象模型）

## 节点层次

### Node类型

### Document类型

作为HTMLDocument 的实例，document 对象还有一个body 属性，直接指向<body>元素。

因为文档类型（如果存在的话）是只读的，而且它只能有一个元素子节点

* 查找元素
  * getElementById()
  * getElementsByTagName("img")
  * getElementsByName()

### Element类型

Element 类型用于表现XML或HTML元素，提供了对元素标签名、子节点及特性的访问。

在HTML 中，标签名始终都以全部大写表示；而在XML（有时候也包括XHTML）中，标签名则始终会与源代码中的保持一致。

* 取得特性

  操作特性的DOM方法主要有三个，分别是getAttribute()、setAttribute()和removeAttribute()。

  只有在取得自定义特性值的情况下，才会使用getAttribute()方法。

### Text类型

在默认情况下，每个可以包含内容的元素最多只能有一个文本节点，而且必须确实有内容存在。来看几个例子。

```html
<!-- 没有内容，也就没有文本节点 -->
<div></div>
<!-- 有空格，因而有一个文本节点 -->
<div> </div>
<!-- 有内容，因而有一个文本节点 -->
<div>Hello World!</div>
```

如果在一个包含两个或多个文本节点的父元素上调用`normalize()`方法，则会将所有文本节点合并成一个节点，结果节点的nodeValue 等于将合并前每个文本节点的nodeValue 值拼接起来的值。

### Comment类型

## DOM操作技术

### *动态脚本

### 动态样式

### 操作表格

```html
<table border="1" width="100%">
<tbody>
	<tr>
		<td>Cell 1,1</td>
		<td>Cell 2,1</td>
	</tr>
	<tr>
		<td>Cell 1,2</td>
		<td>Cell 2,2</td>
	</tr>
</tbody>
</table>
```

使用DOM方法，代码如下：

```javascript
//创建table
var table = document.createElement("table");
table.border = 1;
table.width = "100%";
//创建tbody
var tbody = document.createElement("tbody");
table.appendChild(tbody);
//创建第一行
tbody.insertRow(0);
tbody.rows[0].insertCell(0);
tbody.rows[0].cells[0].appendChild(document.createTextNode("Cell 1,1"));
tbody.rows[0].insertCell(1);
tbody.rows[0].cells[1].appendChild(document.createTextNode("Cell 2,1"));
//创建第二行
tbody.insertRow(1);
tbody.rows[1].insertCell(0);
tbody.rows[1].cells[0].appendChild(document.createTextNode("Cell 1,2"));
tbody.rows[1].insertCell(1);
tbody.rows[1].cells[1].appendChild(document.createTextNode("Cell 2,2"));
//将表格添加到文档主体中
document.body.appendChild(table);
```

### 使用NodeList

理解NodeList 及其“近亲”NamedNodeMap 和HTMLCollection，是从整体上透彻理解DOM的关键所在。这三个集合都是“动态的”；换句话说，每当文档结构发生变化时，它们都会得到更新。

### 小结

理解DOM的关键，就是理解DOM 对性能的影响。DOM操作往往是JavaScript 程序中开销最大的部分，而因访问NodeList 导致的问题为最多。NodeList 对象都是“动态的”，这就意味着每次访问NodeList 对象，都会运行一次查询。有鉴于此，最好的办法就是尽量减少DOM操作。

# 11 DOM扩展

## 选择符API

Selectors API Level 1 的核心是两个方法：`querySelector()`和`querySelectorAll()`。

## HTML5

### 焦点管理

### HTMLDocument的变化

* readyState 属性

* 兼容模式

  ```javascript
  if (document.compatMode == "CSS1Compat"){
  alert("Standards mode");
  } else {
  alert("Quirks mode");
  }
  ```

* head 属性

### 字符集属性

### 插入标记

### scrollIntoView()方法

## 专有扩展

### 文档模式

比如，要想让文档模式像在IE7 中一样，可以使用下面这行代码：
<meta http-equiv="X-UA-Compatible" content="IE=EmulateIE7">

如果不打算考虑文档类型声明，而直接使用IE7 标准模式，那么可以使用下面这行代码：

<meta http-equiv="X-UA-Compatible" content="IE=7">

# 12 DOM2和DOM3

# 13 事件

JavaScript 与HTML 之间的交互是通过事件实现的

### 事件流

事件流描述的是从页面中接收事件的顺序

事件冒泡和事件捕捉：两种完全相反的事件流概念——**建议事件冒泡**

“DOM2级事件”规定的事件流包括三个阶段：事件捕获阶段、处于目标阶段和事件冒泡阶段

### 事件处理程序

* HTML事件处理程序

  ```html
  <script type="text/javascript">
  function showMessage(){
  	alert("Hello world!");
  }
  </script>
  <input type="button" value="Click Me" onclick="showMessage()" />
  ```

* DOM 0级事件处理程序

  ```javascript
  var btn = document.getElementById("myBtn");
  btn.onclick = function(){
  	alert(this.id); //"myBtn"
  };
  ```

* DOM 2级事件处理程序

  DOM2级事件”定义了两个方法，用于处理指定和删除事件处理程序的操作：

  * `addEventListener()`

  * `removeEventListener()`

  所有DOM节点中都包含这两个方法，并且它们都接受3 个参数：要处理的事件名、作为事件处理程序的函数和一个布尔值。**最后这个布尔值参数如果是true，表示在捕获阶段调用事件处理程序；如果是false，表示在冒泡阶段调用事件处理程序。**

  ```javascript
  var btn = document.getElementById("myBtn");
  btn.addEventListener("click", function(){
  	alert(this.id);
  }, false);
  ```

  **使用DOM2 级方法添加事件处理程序的主要好处是可以添加多个事件处理程序**

  ```javascript
  var btn = document.getElementById("myBtn");
  btn.addEventListener("click", function(){
  	alert(this.id);
  }, false);
  btn.addEventListener("click", function(){
  	alert("Hello world!");
  }, false);
  ```

  通过addEventListener()添加的事件处理程序只能使用removeEventListener()来移除**

  移除时传入的参数与添加处理程序时使用的参数相同。**这也意味着通过addEventListener()添加的匿名函数将无法移除。**

## 事件对象

### DOM中的事件对象

在需要通过一个函数处理多个事件时，可以使用type属性。	

```javascript
var btn = document.getElementById("myBtn");
var handler = function(event){
    switch(event.type){
        case "click":
        alert("Clicked");
        break;
        case "mouseover":
        event.target.style.backgroundColor = "red";
        break;
        case "mouseout":
        event.target.style.backgroundColor = "";
        break;
	}
};
btn.onclick = handler;
btn.onmouseover = handler;
btn. onmouseout = handler;	
```

stopPropagation()方法用于立即停止事件在DOM 层次中的传播，即取消进一步的事件捕获或冒泡。例如，直接添加到一个按钮的事件处理程序可以调用stopPropagation()，从而避免触发注册在document.body 上面的事件处理程序，如下面的例子所示。

```javascript
var btn = document.getElementById("myBtn");
btn.onclick = function(event){
    alert("Clicked");
    event.stopPropagation();
};
document.body.onclick = function(event){
	alert("Body clicked");
};
```

## 事件类型

### UI事件

* load事件

  ```javascript
  window.addEventListener('load', (event) => {
    console.log('page is fully loaded');
  });
  ```

  ```javascript
  window.onload = (event) => {
    console.log('page is fully loaded');
  };
  ```


### 焦点事件

### 鼠标与滚轮事件

### 复合事件

### 变动事件

### HTML5事件

### 设备事件

### 触摸与手势事件

## 内存和性能

### 事件委托

使用事件委托，只需在DOM 树中尽量最高的层次上添加一个事件处理程序

## 模拟事件

# 14 表单脚本

## 表单基础知识

* 提交表单

  提交表单时可能出现的最大问题，就是重复提交表单

  解决方案：

  1. 在第一次提交表单后就禁用提交按钮
  2. 利用onsubmit 事件处理程序取消后续的表单提交操作

## 文本框脚本

### 选择文本

### 过滤输入

* 屏蔽字符

  仅考虑到屏蔽不是数值的字符还不够，还要避免屏蔽这些极为常用和必要的键

  除此之外，还有一个问题需要处理：复制、粘贴及其他操作还要用到 Ctrl 键。在除 IE 之外的所有浏览器中，前面的代码也会屏蔽Ctrl+C、Ctrl+V，以及其他使用 Ctrl 的组合键

  ```javascript
  EventUtil.addHandler(textbox, "keypress", function(event){
      event = EventUtil.getEvent(event);
      var target = EventUtil.getTarget(event);
      var charCode = EventUtil.getCharCode(event);
      if (!/\d/.test(String.fromCharCode(charCode)) && charCode > 9 &&
      !event.ctrlKey){
      	EventUtil.preventDefault(event);
      }
  });
  ```

* 操作剪切板

### 自动切换焦点

```html
<form>
        <input type="text" name="tel1" id="txtTel1" maxlength="3">
        <input type="text" name="tel2" id="txtTel2" maxlength="3">
        <input type="text" name="tel3" id="txtTel3" maxlength="4">
    </form>
```

```javascript
(function () {
	function tabForward(event) {
		event = EventUtil.getEvent(event);
		var target = EventUtil.getTarget(event);
		if (target.value.length == target.maxLength) {
			var form = target.form;
			for (var i = 0, len = form.elements.length; i < len; i++) {
				if (form.elements[i] == target) {
					if (form.elements[i + 1]) {
						form.elements[i + 1].focus();
					}
					return;
				}
			}
		}
	}
	var textbox1 = document.getElementById("txtTel1");
	var textbox2 = document.getElementById("txtTel2");
	var textbox3 = document.getElementById("txtTel3");
	EventUtil.addHandler(textbox1, "keyup", tabForward);
	EventUtil.addHandler(textbox2, "keyup", tabForward);
	EventUtil.addHandler(textbox3, "keyup", tabForward);
})();
```

## 选择框脚本

## 表单序列化

## 富文本编辑

# 15 使用Canvas绘图

## 基本用法

# 16 HTML脚本编程

## 跨文档消息传递

# 17 错误处理与调试

常见的错误类型：

* 类型转换错误

* 数据类型错误

  最好是使用instanceof 来检测其数据类型

  ```javascript
  //安全，非数组值将被忽略
  function reverseSort(values){
      if (values instanceof Array){ //问题解决了
          values.sort();
          values.reverse();
      }
  }
  ```

  大体上来说，基本类型的值应该使用typeof 来检测，而对象的值则应该使用instanceof 来检测。

  ```javascript
  function getQueryString(url){
      if (typeof url == "string"){ //通过检查类型确保安全
          var pos = url.indexOf("?");
          if (pos > -1){
          	return url.substring(pos +1);
          }
      }
      return "";
  }
  ```

* 通信错误

# 18 JavaScript与XML

# 19 E4X

# 20 JSON

关于JSON，最重要的是要理解它是一种数据格式，不是一种编程语言。

## 语法

JSON 的语法可以表示以下三种类型的值。

* 简单值：使用与JavaScript 相同的语法，可以在JSON 中表示字符串、数值、布尔值和null。
  但JSON 不支持JavaScript 中的特殊值undefined。

* 对象：对象作为一种复杂数据类型，表示的是一组无序的键值对儿。而每个键值对儿中的值可以是简单值，也可以是复杂数据类型的值。

  **对象的属性必须加==双引号==**

  ```json
  {
  "name": "Nicholas",
  "age": 29,
  "school": {
      "name": "Merrimack College",
      "location": "North Andover, MA"
  	}
  }	
  ```

  

* 数组：数组也是一种复杂数据类型，表示一组有序的值的列表，可以通过数值索引来访问其中的值。数组的值也可以是任意类型——简单值、对象或数组。
  JSON 不支持变量、函数或对象实例，它就是一种表示结构化数据的格式,虽然与JavaScript 中表示数据的某些语法相同，但它并不局限于JavaScript 的范畴。

## JSON对象

JSON 对象有两个方法：stringify()和parse()。在最简单的情况下，这两个方法分别用于把
JavaScript 对象序列化为JSON 字符串和把JSON 字符串解析为原生JavaScript 值

## 序列化选项

toJSON()可以作为函数过滤器的补充，因此理解序列化的内部顺序十分重要。假设把一个对象传入`JSON.stringify()`，序列化该对象的顺序如下。

(1) 如果存在toJSON()方法而且能通过它取得有效的值，则调用该方法。否则，返回对象本身。

(2) 如果提供了第二个参数，应用这个函数过滤器。传入函数过滤器的值是第(1)步返回的值。

(3) 对第(2)步返回的每个值进行相应的序列化。

(4) 如果提供了第三个参数，执行相应的格式化。

无论是考虑定义toJSON()方法，还是考虑使用函数过滤器，亦或需要同时使用两者，理解这个顺序都是至关重要的。

# 21 Ajax与Comet

Ajax 技术的核心是XMLHttpRequest 对象（简称XHR）

XHR 为向服务器发送请求和解析服务器响应提供了流畅的接口。能够以异步方式从服务器取得更多信息，意味着用户单击后，可以不必刷新页面也能取得新数据。也就是说，可以使用XHR 对象取得新数据，然后再通过DOM 将新数据插入到页面中。另外，虽然名字中包含XML 的成分，但Ajax 通信与数据格式无关；这种技术就是无须刷新页面即可从服务器取得数据，但不一定是XML 数据。

# 22 高级技巧

## 数组分块

在数组分块模式中，array 变量本质上就是一个“待办事宜”列表，它包含了要处理的项目。使用shift()方法可以获取队列中下一个要处理的项目，然后将其传递给某个函数。

```javascript
function chunk(array, process, context){
    setTimeout(function(){
        var item = array.shift();
        process.call(context, item);
        if (array.length > 0){
            setTimeout(arguments.callee, 100);
        }
    }, 100);
}
```

必须当心的地方是，传递给chunk()的数组是用作一个队列的，因此当处理数据的同时，数组中的条目也在改变。如果你想保持原数组不变，则应该将该数组的克隆传递给chunk()，

## 函数节流

目的是只有在执行函数的请求停止了一段时间之后才执行

```javascript
var processor = {
    timeoutId: null,
    //实际进行处理的方法
    performProcessing: function(){
   		//实际执行的代码
    },
    //初始处理调用的方法
    process: function(){
        clearTimeout(this.timeoutId);
        var that = this;
        this.timeoutId = setTimeout(function(){
            that.performProcessing();
        }, 100);
    }
};
```

## 自定义事件

观察者模式

# 23 离线应用与客户端储存

# 24 最佳实践

## 可维护性

* 避免全局量

* 避免与`null`比较

  ```javascript
  function sortArray(values){
      if (values instanceof Array){ //推荐
      	values.sort(comparator);
      }
  }
  ```

* 使用常量

  ```javascript
  var Constants = {
      INVALID_VALUE_MSG: "Invalid value!",
      INVALID_VALUE_URL: "/errors/invalid.php"
  };
  function validate(value){
      if (!value){
          alert(Constants.INVALID_VALUE_MSG);
          location.href = Constants.INVALID_VALUE_URL;
      }
  }
  ```

## 性能

* 避免全局查找

  ```javascript
  // wrong!
  function updateUI(){
      var imgs = document.getElementsByTagName("img");
      for (var i=0, len=imgs.length; i < len; i++){
      	imgs[i].title = document.title + " image " + i;
      }
      var msg = document.getElementById("msg");
      msg.innerHTML = "Update complete.";
  }
  ```

  这里，首先将document 对象存在本地的doc 变量中；然后在余下的代码中替换原来的document。与原来的的版本相比，现在的函数只有一次全局查找，肯定更快。

  ```javascript
  function updateUI(){
      var doc = document;
      var imgs = doc.getElementsByTagName("img");
      for (var i=0, len=imgs.length; i < len; i++){
      	imgs[i].title = doc.title + " image " + i;
      }
      var msg = doc.getElementById("msg");
      msg.innerHTML = "Update complete.";
  }
  ```

* 避免with语句

* 避免不必要的属性查找

  一旦多次用到对象属性，应该将其存储在局部变量中。第一次访问该值会是O(n)，然而后续的访问都会是O(1)，就会节省很多。例如，之前的代码可以如下重写：

  ```javascript
  var url = window.location.href;
  var query = url.substring(url.indexOf("?"));
  ```

* 展开循环

  ```javascript
  //credit: Jeff Greenberg for JS implementation of Duff’s Device
  //假设 values.length > 0
  var iterations = Math.ceil(values.length / 8);
  var startAt = values.length % 8;
  var i = 0;
  do {
      switch(startAt){
          case 0: process(values[i++]);
          case 7: process(values[i++]);
          case 6: process(values[i++]);
          case 5: process(values[i++]);
          case 4: process(values[i++]);
          case 3: process(values[i++]);
          case 2: process(values[i++]);
          case 1: process(values[i++]);
      }
  startAt = 0;
  } while (--iterations > 0);
  ```

  # 25 新兴的API

  * requestAnimationFrame()：是一个着眼于优化JavaScript 动画的API，能够在动画运行期间发出信号。通过这种机制，浏览器就能够自动优化屏幕重绘操作。
  * Page Visibility API：让开发人员知道用户什么时候正在看着页面，而什么时候页面是隐藏的。
  * Geolocation API：在得到许可的情况下，可以确定用户所在的位置。在移动Web 应用中，这个API 非常重要而且常用。
  * File API：可以读取文件内容，用于显示、处理和上传。与HTML5 的拖放功能结合，很容易就
    能创造出拖放上传功能。
  * Web Timing：给出了页面加载和渲染过程的很多信息，对性能优化非常有价值。
  * Web Workers：可以运行异步JavaScript 代码，避免阻塞用户界面。在执行复杂计算和数据处理的时候，这个API 非常有用；要不然，这些任务轻则会占用很长时间，重则会导致用户无法与页面交互。