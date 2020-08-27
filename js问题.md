原型链，闭包，柯里化，递归，防抖，节流，
设计模式：发布订阅
面向对象和函数式编程的概念

**16. typeof 的返回值**

```javascript

​    //typeof可以返回number、boolean、string 、underfined、object、function;

​    console.log(typeof function); //会报错,这是个关键字

​    console.log(typeof null); //object,比较特殊

​    console.log(typeof isNaN); //function,是个判断函数

​    console.log(typeof []); //object,数组是对象
```

​**19. b 继承 a 的方法**

原型链继承、构造函数继承、混合继承、拷贝继承

1. 原型链继承：

```js
​    function B( ){ };

​    B.prototype = new A( );
```

​ 特点：

    1. 父类新增原型方法/原型属性，子类都能访问到

    2. 无法实现多继承

2. 构造函数继承：

```js
​    function B( ){

​        A.call(this);

​    }
```

​ 特点：

    1. 可以实现多继承，通过call多个父类对象

    2. 只能继承父类的实例属性和方法，不能继承原型属性/方法

3. 混合继承

```js
​    function B( ){

​        A.call(this);

​    }

​    B.prototype = new A( );

​    B.prototype.constructor = B;
```

​ 特点：

    1. 可以继承实例属性/方法，也可以继承原型属性/方法

    2. 调用了两次父类构造函数，生成两份实例，消耗一点点内存

4. 拷贝继承

```js
​    function B( ){

​        var a = new A( );

​        for(var key in a){

​            B.prototype[key] = a[key];

​        }

​    }
```

​ 特点：

    1. 支持多继承

    2. 效率低，占用内存高

    3. 无法获取父类不可遍历的方法

5. 实例继承

```js
​    function B(){

​        var a = new A();

​        return a;

​    }
```

​ 特点：

    1. new B()和B()返回的对象具有相同效果

    2. 不支持多继承

6. 寄生组合继承

```js
​    function B(){

​        A.call(this);

​    }

​    (function(){

​        var C = function(){};

​        C.prototype = A.prototype;

​        B.prototype = new C();

​    })();

​    B.prototype.constructor = B;
```

​ 特点：

    1. 堪称完美

    2. 实现过程比较复杂

​

**20. javascript 的本地对象、内置对象、和宿主对象**

​ 本地对象：Array、Object、RegExp、Function、String、Boolean、Error 等，可以用 new 实例化

​ 内置对象：Global、Math 等，是已经被实例化的，直接使用，不可以用 new 再实例化

​ 宿主对象：即浏览器提供的对象，所有的 BOM 和 DOM，如 window、document

​

**21. javascript 语言有什么特点？**

​ 1. 弱类型：所有的变量都用 var 来声明（ES6 中引入了 let 和 const）

​ 2. 动态类型：声明变量的时候不确定变量类型，运行时才明确

​ 3. 解析型：遇到一行解析一行

​

**22. 隐式转换应用**

```javascript
console.log(2 == true); // false

console.log([] == ![]); //true

console.log(NaN == NaN); //false

var foo = "11" + 2 - "1";

console.log(foo); //111
// 其实用得比较多的是在if结构或三元表达式的条件中，将语句或表达式的结果利用隐式转换当成布尔值来运用，还有就是处理请求结果的时候类似 res.aa&&do sth.
```
**25. caller 和 callee 的作用和区别**

caller：返回一个对函数的引用

```javascript

function a()
​    console.log(a.caller)

function b()
​    a()

b()
// a.caller返回的是b，log结果为function(){ a() }

callee：返回当前正在运行的函
function a()
​    console.log(arguments.callee)

a()
// arguments.callee返回的是a，log结果是function(){ console.log(arguments.callee)
// 常用在递归，斐波那契数列等
```

**26. undefined 元素对数组的影响**

当数组中存在 undefined 元素时，此元素不显示，后续的元素会显示索引值

```javascript
var arr = [0, 1, 2];

arr[4] = "z";

console.log(arr); // [0,1,2,4:'z']
```
**36. eval()的作用是什么？为什么不建议使用 eval()？**

​ eval() 函数计算 JavaScript 字符串，并把它作为脚本代码来执行。在 node 工具中常用，比如 webpack 中的 loader

​ eval() 是一个危险的函数， 他执行的代码拥有着执行者的权利。如果你用 eval() 运行的字符串代码被恶意方（不怀好意的人）操控修改，您最终可能会在您的网页/扩展程序的权限下，在用户计算机上运行恶意代码。更重要的是，第三方代码可以看到某一个 eval()被调用时的作用域，这也有可能导致一些不同方式的攻击。


**38. 了解 js 的模块化及 require.js 的使用吗？**

​ 1. js 会阻塞页面，require.js 实现 js 文件的异步加载，避免网页失去响应

​ 2. js 文件之间存在依赖关系时需要严格保证加载顺序，require.js 可以管理模块之间的依赖性，便于代码维护

**45. 常用 js 框架及适用领域**

    jquery： 简化js操作，提供很好用的API

    jqueryui, jqueryeasyui: jquery基础上提供了一些常用组件，如日期，下拉，表格

    require.js, sea.js：模块化开发时使用

    zepto： 精简版jquery，常用于移动端，提供了一些移动端实用功能如touch

    ext.js：

    angular, knockoutjs, acalon: MVC框架，适用于单页应用开发

​ **47. new 操作符究竟做了些什么？**

> ​ 1. 创建一个空对象,并且将 this 变量引向该对象,同时继承该对象的原型

> ​ 2. 属性和方法被加入到 this 引用的对象中

> ​ 3. 新创建的对象由 this 引用,并且最后隐式的返回 this

 **48. AMD 和 CMD**

> ​ AMD（Asynchronous Module Definition），异步模块定义，推崇依赖前置，它采用异步方式加载模块,模块的加载不影响它后面语句的运行。所有依赖这个模块的语句，都定义在一个回调函数中，等到加载完成之后,这个回调函数才会运行，即加载完成后立即执行。代表是国外的 Require.js。

> ​ CMD（Common Module Definition），通用模块定义，推崇依赖就近，什么时候 require 什么时候加载。代表是国内的 Sea.js

> ​ AMD 用户体验好，因为提前执行了，CMD 性能好，因为按需执行。

​  
​ **49. 对于 ES6 的了解**

> ​ ECMAScript6

> ​ 1. let, const, 块级作用域

> ​ 2. 箭头函数

> ​ 3. 函数参数默认值
> ​ let sum = (a, b=1) => a + b;

> ​ 4. `字符串`，可以解析空格和换行，可以插入变量\${变量名}

> ​ 5. Symbol 数据类型，值是唯一不可变的

> ​ 6. class

> ​ 7. 语法糖，对象属性简写{name,age,sayHi(){}}

> ​ 8. promise

> ​ 9. 三个点扩展运算符

> ​ 10. Set 和 Map 数据结构

​ **50. 为什么要有 class，class 怎么写**

> ​ ES5 中通过原型的方式来模拟类和继承，ES6 提供了类（class）和继承（extend），这里的 class 并不是一种新的对象继承模型，是原来原型链的语法糖。

```javascript
​    class Person { // 定义一个Person类
​        constructor(name){ // Person类的默认构造方法，不写返回值时，默认返回实例（this）
​            this.name = name;
​        }
​        getName(){ // Person类的自定义方法，相当于定义在原型里的方法
​            return this.name;
​        }
​    }
```

> ​ // 实现继承

```javascript
​    class Student extends Person {
​        constructor(name,age){
​            super(name); // 使用super()调用父类的同名方法
​            this.age = age;
​        }
​        showInfo() {
​            console.log(this.name, this.age)
​        }
​    }
​    var stu = new Student("jack", 18);

```

**51. 哪些操作会造成内存泄漏？**

​ 内存泄漏，就是不再需要的对象仍然存在内存中，内存泄漏不断堆积的后果就是内存溢出，即内存不够用。

​ 垃圾回收机制会定期扫描对象，如果一个对象没有被其他对象引用，或两个对象互相引用但没有被第三个对象引用，则它们的内存会被回收。

> 1. setTimeout 的第一个参数使用字符串而非函数的话,会引发内存泄露

> 2. 全局变量

> 3. 闭包

> 4. dom 清空或删除时，事件未清除导致的内存泄漏

> 5. 控制台日志

> 6. 循环


**53. 简述 jQuery 的实现原理**

​ jQuery 对象是 jQuery 原型属性的 init 方法实例出来的对象，调用 jQuery 构造函数时自动返回 init 的实例（\$("div")），达到不用 new 创建对象的目的。

​ 通过改变 init 方法的原型，让它指向 jQuery 的原型，达到继承的目的，这样，init 方法中的 this 就可以使用 jQuery 原型对象中的方法。

​ 将$变量和jQuery变量暴露到window中，用户直接通过$或\$("")调用到 jQuery 构造函数和 jQuery 对象原型中的方法。

​ jQuery 构造函数和 jQuery 对象原型中有同一个 extend 方法，可以用于扩展自身的属性和方法。

**54. 深拷贝和浅拷贝，深克隆和浅克隆**

​ 数组的浅拷贝：直接赋值，只是复制了一个地址

​ var arr2 = arr1

​ 数组的深拷贝：相当于克隆了一份一样的数组出来，地址相互独立，jq 中用 clone 方法，原生 js 有以下方法：

​ 创建新数组，遍历赋值；

​ 数组的 slice 方法，var arr2 = arr1.slice(0);

​ 数组的 concat 方法，var arr2 = arr1.concat();

​ 对象浅拷贝：

​ 直接赋值，只是复制了一个地址

​ 对象深拷贝：

​ var b = JSON.parse(JSON.stringfy(a))

​ 或者使用展开运算符：b ＝｛...a｝

**59. AJAX 的原生写法**

```js
var Ajax = {
  get: function(url, fn) {
    // XMLHttpRequest对象用于在后台与服务器交换数据
    var xhr = new XMLHttpRequest();
    xhr.open("GET", url, false);
    xhr.onreadystatechange = function() {
      // readyState == 4说明请求已完成
      if (xhr.readyState == 4) {
        if (xhr.status == 200 || xhr.status == 304) {
          console.log(xhr.responseText);
          fn.call(xhr.responseText);
        }
      }
    };
    xhr.send();
  },

  // data应为'a=a1&b=b1'这种字符串格式，在jq里如果data为对象会自动将对象转成这种字符串格式
  post: function(url, data, fn) {
    var xhr = new XMLHttpRequest();
    xhr.open("POST", url, false);
    // 添加http头，发送信息至服务器时内容编码类型
    xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
        if (xhr.status == 200 || xhr.status == 304) {
          // console.log(xhr.responseText);
          fn.call(xhr.responseText);
        }
      }
    };
    xhr.send(data);
  }
};
```
**69. 写一个通用的事件侦听器函数**

>

**68. jQuery 中的 extend 的实现原理是什么？**

>

**69. null 和 undefined 的区别？**

> ​ null 意思是空，表示此处不应该有值，undefined 是未定义，表示此处缺少一个值。

> ​ null 的类型是 Null，但 typeof 输出结果是 Object（js 最初实现的一个错误），undefined 的类型是 undefined。

> ​
**73. js 中创建命名空间的方法**

> ​ 命名空间为了解决多人开发时函数重名问题，类似于不同文件夹下可以有同名的文件，命名空间就是创建出一个虚拟的“文件夹”。

> ​ ES6 中提供了模块 module，不再需要对象创建命名空间。

> ​ 不使用 ES6 的方法：

> ​ \- 创建一个对象字面量来打包所有的函数名和变量名。

```js

var NAMESPACE = {
  person: function(){
    this.getName = ()=>{}
  }
}

// 使用时，就可以声明多个 person 对象供多人使用：

var per = new NAMESPACE.person();

per.getName();

```

**74. javascript 中的 map()和 forEach()有什么区别？**

​ 相同点：

    都是循环遍历数组中的每一项

    forEach 和 map 方法里每次执行匿名函数都支持 3 个参数，参数分别是 item（当前每一项）、index（索引值）、arr（原数组）

    匿名函数中的 this 都是指向 window

    只能遍历数组

​ 不同点：

    map()会创建一个新的数组，每个元素是由原数组每个元素经过回调函数得来的

    forEach()会修改原来的数组，没有返回值

​ 用法举例：
  ``` js
  
  ​ let arr = [1,2,3,4]
  
  ​ arr.forEach((num, index)=>{
  ​   return arr[index] = num\*2
  ​ })
  
  ​ let arr2 = arr.map(num=>{
  ​   return num\*2
  ​ })
  ```


**75. jQuery 中 animate()方法的实现**

**76. jQuery 中 addClass()方法的实现**

> ​
**81.jQ 的 on、bind、live、delegate 各有什么异同？**

**112. 手写 js 原型继承**

\1. 原型式继承：

![img](http://note.youdao.com/yws/public/resource/493dfe1ab5c8c391fb978cf6701337e0/xmlnote/7AE737B9D9DD481A938E7B646EFED9C7/2345)

\2. ES6 的 class 继承：

class 语法糖用于让定义类更简单，比如上面的第一步定义 Student 类，可以写为：

![img](http://note.youdao.com/yws/public/resource/493dfe1ab5c8c391fb978cf6701337e0/xmlnote/17CED069500A4A128320345AD40FD693/2381)

class 继承的写法：

![img](http://note.youdao.com/yws/public/resource/493dfe1ab5c8c391fb978cf6701337e0/xmlnote/C38ECD47C62D4375A493B777063312F5/2386)

**121. ES5 的严格模式**
使用严格模式下 全局函数调用的 this 是 undefined，而非严格模式下是 window

**126. 常见的几种设计模式**

工厂模式：定义一个用于创建对象的接口，让子类决定实例化哪一个类。工厂方法使一个类的实例化延迟到其子类。

原型模式：用原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象。

中介者模式：用一个中介对象封装一系列的对象交互，中介者使各对象不需要显示地相互作用，从而使其耦合松散，而且可以独立地改变它们之间的交互。

观察者模式：（即发布/订阅模式，MVVM 中有体现）定义对象间一种一对多的依赖关系，使得每当一个对象改变状态，则所有依赖于它的对象都会得到通知并被自动更新。

工厂模式理解：

![img](http://note.youdao.com/yws/public/resource/493dfe1ab5c8c391fb978cf6701337e0/xmlnote/84D051E80DCE4E6480955C016FF81F43/2366)



**108. 数组去重的所有方式**

方法 1：双层循环，外层循环元素，内层循环作比较

方法 2：利用对象属性不能相同的特点去重,

方法 3：数组递归去重,

方法 4：使用 ES6 的 set，一句话搞定：Array.from(new Set([1,1,2,3,3,4,2])) // [1,2,3,4]