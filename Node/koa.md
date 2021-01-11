## koa与express的区别
- 异步流程控制: express采用callback, Koa1采用generator, koa2采用async/await。
- 错误处理：express使用callback捕获异常，对于深层次的异常捕获不了。Koa使用try catch,能更好地解决异常捕获。
- 框架：express主要基于connect中间件，并且自身封装了路由、视图等功能。koa移除了router,view等功能，框架本身很轻。
- 中间件：在Express中，响应返回时代码执行并不会回到原来的中间件，此时我们需要通过监听response.writeHead获得响应返回的时机。Koa的中间件采用了洋葱圈模型，所有的请求在经过中间件的时候都会执行两次，能够非常方便的执行一些后置处理逻辑。


### 路由是什么？
决定了不同URL是如何被不同地执行的；在Koa中，是一个中间件；
#### http options方法的作用
检测服务器所支持的请求方法；CORS中的预检请求；   
#### allowedMethods的作用(koa-router)
响应optinos方法，告诉它所支持的请求方法；相应的返回405(不允许)和501(没实现,即koa-router没实现不支持，如link)

### 控制器
#### 什么是控制器
拿到路由分配的任务，并执行；     
#### 控制器的作用
获取HTTP请求参数；处理业务逻辑；发送http响应；  
#### 获取HTTP请求参数
参数类型：    
- Query String，如 ?q=keyword
- Router Params，如/users/:id
- Body, 如{name: '李雷}
- Header, 如Accept(客户端可以接受的媒体格式)、Cookie

#### 发送响应参数
- 发送Status，如200/400等
- 发送Body，如{name: '李雷'}
- 发送Header，如Allow(允许的http方法)、Content-Type(告诉客户端返回的格式用哪种方式解析)

#### 编写控制器最佳实践
- 每个资源的控制器放在不同的文件里
- 尽量使用类 + 类方法的形式编写控制器
- 严谨的错误处理



## 代码
```js
// 中间件 next指下一个中间件
app.use(async(ctx, next) => {
  await next();console.log(2)
})
// next是异步函数

ctx.params //获取请求头路径参数
ctx.request.body //获取请求体
ctx.set('Allow', 'GET, POST'); // 设置允许的方法

ctx.throw(412, 'id不能为空')
```