在 ES6 之前，社区制定了一些模块加载方案，最主要的有 CommonJS 和 AMD 两种。前者用于服务器，后者用于浏览器。   
ES6 在语言标准的层面上，实现了模块功能，而且实现得相当简单，完全可以取代 CommonJS 和 AMD 规范，成为浏览器和服务器通用的模块解决方案。   
ES6 模块的设计思想是尽量的静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量。     
CommonJS 和 AMD 模块，都只能在运行时确定这些东西。比如，CommonJS 模块就是对象，输入时必须查找对象属性。    

ES6 模块不是对象，而是通过export命令显式指定输出的代码，再通过import命令输入。    
ES6 可以在编译时就完成模块加载，效率要比 CommonJS 模块的加载方式高。当然，这也导致了没法引用 ES6 模块本身，因为它不是对象。    

### ES6 模块有以下好处：
- 静态加载：高效、可类型检测。
- 不再需要UMD模块格式了，将来服务器和浏览器都会支持 ES6 模块格式。目前，通过各种工具库，其实已经做到了这一点。  
将来浏览器的新 API 就能用模块格式提供，不再必须做成全局变量或者navigator对象的属性。  
不再需要对象作为命名空间（比如Math对象），未来这些功能可以通过模块提供。     

ES6 的模块自动采用严格模式，不管你有没有在模块头部加上"use strict";。
### 严格模式主要有以下限制。
- 变量必须声明后再使用
- 函数的参数不能有同名属性，否则报错
- 不能使用with语句
- 不能对只读属性赋值，否则报错
- 不能使用前缀 0 表示八进制数，否则报错
- 不能删除不可删除的属性，否则报错
- 不能删除变量delete prop，会报错，只能删除属性delete global[prop]
- eval不会在它的外层作用域引入变量
- eval和arguments不能被重新赋值
- arguments不会自动反映函数参数的变化
- 不能使用arguments.callee
- 不能使用arguments.caller
- 禁止this指向全局对象
- 不能使用fn.caller和fn.arguments获取函数调用的堆栈
- 增加了保留字（比如protected、static和interface）
- 上面这些限制，模块都必须遵守。由于严格模式是 ES5 引入的，不属于 ES6，所以请参阅相关 ES5 书籍。
- 其中，尤其需要注意this的限制。ES6 模块之中，顶层的this指向undefined，即不应该在顶层代码使用this。

 
### 模块功能主要由两个命令构成：export和import。
export命令用于规定模块的对外接口，import命令用于输入其他模块提供的功能。   
import命令是编译阶段执行的，在代码运行之前。     
import命令输入的变量都是只读的，因为它的本质是输入接口，如果是一个对象，改写对象属性是允许的。   
export语句输出的接口，与其对应的值是动态绑定关系，即通过该接口，可以取到模块内部实时的值。  
```js
export var foo = 'bar';   
setTimeout(() => foo = 'baz', 500);  
``` 
上面代码输出变量foo，值为bar，500 毫秒之后变成baz。这一点与 CommonJS 规范完全不同，CommonJS 模块输出的是值的缓存，不存在动态更新   

import语句会执行所加载的模块，因此可以有下面的写法。 
```js
import 'lodash';
```
上面代码仅仅执行lodash模块，但是不输入任何值。
```js
import 'lodash';
import 'lodash';
```
如果多次重复执行同一句import语句，那么只会执行一次，而不会执行多次。

### export命令与export default命令
一个模块只能有一个默认输出，因此export default命令只能使用一次。import命令后面才不用加大括号；  
#### export 
```js
// 报错
export 1;

// 报错
var m = 1;
export m;

// 写法一
export var m = 1;

// 写法二
var m = 1;
export {m};

// 写法三
var n = 1;
export {n as m};
// 上面三种写法都是正确的，规定了对外的接口m。其他脚本可以通过这个接口，取到值1。它们的实质是，在接口名与模块内部变量之间，建立了一一对应的关系。

// 报错
function f() {}
export f;

// 正确
export function f() {};

// 正确
function f() {}
export {f};

```

#### export default
```js
// 正确
export var a = 1;

// 正确
var a = 1;
export default a;

// 错误
export default var a = 1; // export default a的含义是将变量a的值赋给变量default。所以，写法会报错。

// 正确
export default 42;

// 报错
export 42;
``` 
```js
``` 
```js
// modules.js
function add(x, y) {
  return x * y;
}
export {add as default};
// 等同于
// export default add;

// app.js
import { default as foo } from 'modules';
// 等同于
// import foo from 'modules';
```
