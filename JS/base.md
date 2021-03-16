#### ECMA基础知识
欧洲计算机制造商协会（European Computer Manufacturers Association），Ecma国际（Ecma International）是一家国际性会员制度的信息和电信标准组织。         
国际标准化组织（International Organization for Standarization，简称ISO）  
ECMAScript：浏览器脚本语言的标准，ECMAScript是JavaScript的规格，JavaScript是ECMAScript的一种实现（另外的 ECMAScript 方言还有 JScript 和 ActionScript）。
#### ECMAScript历史
- 1997年发布的ECMAScript 1.0，是ECMA 发布 262 号标准文件（ECMA-262）的第一版，，规定了浏览器脚本语言的标准。
- 1998年6月ECMAScript 2.0发布
- 1999年12月发布了ECMAScript 3.0。
- 2000 年，ECMAScript 4.0 开始酝酿。这个版本最后没有通过，但是它的大部分内容被 ES6 继承了。
- 2007 年 10 月，ECMAScript 4.0 版草案发布，以 Yahoo、Microsoft、Google 为首的大公司，反对 JavaScript 的大幅升级，主张小幅改动；以 JavaScript 创造者 Brendan Eich 为首的 Mozilla 公司，则坚持当前的草案。
- 2008 年 7 月，由于对于下一个版本应该包括哪些功能，各方分歧太大，中止 ECMAScript 4.0 的开发，将其中涉及现有功能改善的一小部分，发布为 ECMAScript 3.1（而将其他激进的设想扩大范围，放入以后的版本，由于会议的气氛，该版本的项目代号起名为 Harmony（和谐）。会后不久，ECMAScript 3.1 就改名为 ECMAScript 5）。
- 2009 年 12 月，ECMAScript 5.0 版正式发布。Harmony 项目则一分为二，一些较为可行的设想定名为 JavaScript.next 继续开发，后来演变成 ECMAScript 6；一些不是很成熟的设想，则被视为 JavaScript.next.next，在更远的将来再考虑推出。TC39 委员会的总体考虑是，ES5 与 ES3 基本保持兼容，较大的语法修正和新功能加入，将由 JavaScript.next 完成。当时，JavaScript.next 指的是 ES6，第六版发布以后，就指 ES7。TC39 的判断是，ES5 会在 2013 年的年中成为 JavaScript 开发的主流标准，并在此后五年中一直保持这个位置。
- 2011 年 6 月，ECMAScript 5.1 版发布，并且成为 ISO 国际标准（ISO/IEC 16262:2011）。
- 2013 年 3 月，ECMAScript 6 草案冻结，不再添加新功能。新的功能设想将被放到 ECMAScript 7。
- 2013 年 12 月，ECMAScript 6 草案发布。然后是 12 个月的讨论期，听取各方反馈。
- 2015 年 6 月，ECMAScript 6 正式通过，成为国际标准。从 2000 年算起，这时已经过去了 15 年。
- 标准委员会最终决定，标准在每年的 6 月份正式发布一次，作为当年的正式版本。接下来的时间，就在这个版本的基础上做改动，直到下一年的 6 月份，草案就自然变成了新一年的版本。这样一来，就不需要以前的版本号了，只要用年份标记就可以了。
ES6 的第一个版本，就这样在 2015 年 6 月发布了，正式名称就是《ECMAScript2015标准》（简称 ES2015）。2016 年 6 月，小幅修订的《ECMAScript 2016 标准》（简称 ES2016）如期发布，这个版本可以看作是 ES6.1 版，因为两者的差异非常小（只新增了数组实例的includes方法和指数运算符），基本上是同一个标准。根据计划，2017年6月发布ES2017标准。

#### 严格模式的目的
https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Strict_mode  
ECMAScript 5标准    
- 消除Javascript语法的一些不合理、不严谨之处，减少一些怪异行为;
- 消除代码运行的一些不安全之处，保证代码运行的安全；
- 提高编译器效率，增加运行速度；
- 为未来新版本的Javascript做好铺垫。
#### 严格模式的规范
##### 变量
1. 不允许意外创建全局变量
2. 不能使用 delete 操作符删除声明变量
3. 不用使用保留字（例如 ：implements、interface、let、package、 private、protected、public、static 和 yield 标识符）作为变量名
##### 对象
1. 为只读属性赋值会抛出TypeError
2. 对不可配置的（nonconfigurable）的属性使用 delete 操作符会抛出TypeError
3. 为不可扩展的（nonextensible）的对象添加属性会抛出TypeError
4. 使用对象字面量时, 属性名必须唯一

##### 函数
要求命名函数的参数必须唯一
```js
function sum(a, a, c) { // !!! 语法错误
  return a + a + c; // 代码运行到这里会出错
}

```
##### eval与arguments
- eval不再为上下文中创建变量或函数
```js
function doSomething(){
  eval("var x=10");
  alert(x); // 抛出TypeError错误
}
```
- eval 和 arguments 不能通过程序语法被绑定(be bound)或赋值
  ```js
    // 所有尝试将引起语法错误:
    eval = 17;
    arguments++;
    ++eval;
    var obj = { set p(arguments) { } };
    var eval;
    try { } catch (arguments) { }
    function x(eval) { }
    function arguments() { }
    var y = function eval() { };
    var f = new Function("arguments", "'use strict'; return 17;");
  ```
- 参数的值不会随arguments对象的值的改变而变化
  ```js
    function f(a) {
        console.log(arguments[0]) // 17
        a = 42; // 参数值改变，arguments值改变
        console.log(arguments[0]) // 42
        return [a, arguments[0]];
    }
    var pair = f(17);
    console.log(pair); // [42, 42]
    // --------- 分割线 -------
    "use strict" 
    function f(a) {
        console.log(arguments[0]) // 17
        a = 42; // 参数值改变，arguments值不改变
        return [a, arguments[0]];
    }
    var pair = f(17);
    console.log(pair); // [42, 17]
  ```
- 禁止使用arguments.callee
  ```js
    var f = function() { 
        return arguments.callee; 
    };
    f(); // 抛出类型错误
  ```   
##### 禁止在函数内部遍历调用栈
```js
function restricted() {
  restricted.caller;    // 抛出类型错误
  restricted.arguments; // 抛出类型错误
}
```
##### 静态绑定
- 禁止使用with语句
- eval()声明变量和函数只能当前eval内部的作用域中有效

##### this指向
- 全局作用域的函数中的this不再指向全局而是undefined。
- 如果使用构造函数时，如果忘了加new，this不再指向全局对象，而是undefined报错。

