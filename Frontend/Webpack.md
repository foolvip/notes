### tree shaking原理
理解链接：https://zhuanlan.zhihu.com/p/32831172   
Tree-shaking的本质是消除无用的js代码  
DCE（dead code elimination）
#### Dead Code 一般具有以下几个特征
- 代码不会被执行，不可到达
- 代码执行的结果不会被用到
- 代码只会影响死变量（只写不读）  

#### tree-shaking的消除原理是依赖于ES6的模块特性    
ES6 module 特点：
- 只能作为模块顶层的语句出现
- import 的模块名只能是字符串常量
- import binding 是 immutable的
  
ES6模块依赖关系是确定的，和运行时的状态无关，可以进行可靠的静态分析，这就tree-shaking的基础。rollup只处理函数和顶层的import/export变量，不能把没用到的类的方法消除掉   
webpack Tree shaking不会清除IIFE(立即调用函数表达式)   





