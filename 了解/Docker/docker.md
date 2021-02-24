 ### 学习博客
 知乎简易教程：https://zhuanlan.zhihu.com/p/83309276    
### 笔记
Docker 项目的目标是实现轻量级的操作系统虚拟化解决方案
### 镜像与容器
#### 容器（Container）
容器特别像一个虚拟机，容器中运行着一个完整的操作系统。可以在容器中装 Nodejs，可以执行npm install，可以做一切你当前操作系统能做的事情
#### 镜像（Image）
镜像是一个文件，它是用来创建容器的。如果你有装过 Windows 操作系统，那么 Docker 镜像特别像“Win7纯净版.rar”文件
#### 创建关系为
```js
Dockerfile: 类似于“package.json”
 |
 V
Image: 类似于“Win7纯净版.rar”
 |
 V
Container: 一个完整操作系统
```
#### 创建文件
我们创建一个目录hello-docker，在目录中创建一个index.html文件，内容为：
```html
<h1>Hello docker</h1>
```
然后再在目录中创建一个Dockerfile文件，内容为：

```js
FROM nginx

COPY ./index.html /usr/share/nginx/html/index.html

EXPOSE 80
```
此时，你的文件结构应该是：
```js
hello-docker
  |____index.html
  |____Dockerfile
```
#### 打包镜像
文件创建好了，现在我们就可以根据Dockerfile创建镜像了！在命令行中（Windows优先使用PowerShell）键入：
```js
cd hello-docker/ # 进入刚刚的目录
docker image build ./ -t hello-docker:1.0.0 // 基于路径./（当前路径）打包一个镜像，镜像的名字是hello-docker，版本号是1.0.0。该命令会自动寻找Dockerfile来打包出一个镜像
// tips: 使用docker images来查看本机已有的镜像   

// 不出意外，你应该能得到如下输出：
Sending build context to Docker daemon  3.072kB
Step 1/3 : FROM nginx
 ---> 5a3221f0137b
Step 2/3 : COPY ./index.html /usr/share/nginx/html/index.html
 ---> 1c433edd5891
Step 3/3 : EXPOSE 80
 ---> Running in c2ff9ec2e945
Removing intermediate container c2ff9ec2e945
 ---> f6a472c1b0a0
Successfully built f6a472c1b0a0
Successfully tagged hello-docker:1.0.0
```
可以看到其运行了 Dockerfile 中的内容，现在我们简单拆解下：   
- FROM nginx：基于哪个镜像   
- COPY ./index.html /usr/share/nginx/html/index.html：将宿主机中的./index.html文件复制进容器里的/usr/share/nginx/html/index.html 
- EXPOSE 80：容器对外暴露80端口  
  
#### 运行容器
```js
docker container create -p 2333:80 hello-docker:1.0.0
docker container start xxx  // xxx 为上一条命令运行得到的结果
```
然后在浏览器打开127.0.0.1:2333，你应该能看到刚刚自己写的index.html内容   
在上边第一个命令中，我们使用docker container create来创建基于hello-docker:1.0.0镜像的一个容器，使用-p来指定端口绑定——将容器中的80端口绑定在宿主机的2333端口。执行完该命令，会返回一个容器ID    
而第二个命令，则是启动这个容器,启动后，就能通过访问本机的2333端口来达到访问容器内80端口的效果了  

当容器运行后，可以通过如下命令进入容器内部：
```js
docker container exec -it xxx /bin/bash // xxx 为容器ID, 退出容器内容通过control + D
```
Tips: 你可以使用docker container ls来查看当前运行的容器     
原理实际上是启动了容器内的/bin/bash，此时你就可以通过bash shell与容器内交互了。就像远程连接了SSH一样

#### 总结：
- 写一个 Dockerfile
- 使用docker image build来将Dockerfile打包成镜像
- 使用docker container create来根据镜像创建一个容器
- 使用docker container start来启动一个创建好的容器



