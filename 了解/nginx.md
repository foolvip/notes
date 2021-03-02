
nginx官网：https://www.nginx.cn/doc/index.html
### nginx基础
正向代理服务器代理客户端，反向代理服务器代理服务器端 
#### nginx服务器  
Nginx是一款轻量级的Web服务器、反向代理服务器，由于它的内存占用少，启动极快，高并发能力强，在互联网项目中广泛应用。
#### nginx进程(master、worker)
Master进程的作用是：读取并验证配置文件nginx.conf；管理worker进程；   
Worker进程的作用是：每一个Worker进程都维护一个线程（避免线程切换），处理连接和请求；注意Worker进程的个数由配置文件决定，一般和CPU个数相关（有利于进程切换），配置几个就有几个Worker进程。
#### 三个作用
反向代理、负载均衡、动静(资源)分离
##### 正向代理
在客户端（浏览器）配置代理服务器，通过代理服务器进行互联网访问   
##### 反向代理
我们只需要将请求发送到反向代理服务器，由反向代理服务器去选择目标服务器获取数据后，再返回客户端，此时反向代理服务器和目标服务器对外就是一个服务器，暴露的是代理服务器地址，隐藏了真实服务器IP地址。   
Nginx提供的负载均衡策略有2种：内置策略和扩展策略。   
- 内置策略为轮询、加权轮询、Ip hash。
- 扩展策略，就天马行空，只有你想不到的没有他做不到的啦，你可以参照所有的负载均衡算法，给他一一找出来做下实现。
##### 负载均衡
单个服务器解决不了，我们增加服务器的数量，然后将请求分发到各个服务器上，将原先请求集中到单个服务器上的情况改为将请求分发到多个服务器上，将负载分发到不同的服务器上，也就是我们所说的负载均衡。    
### nginx的安装
1. yum install -y gcc-c++pcre pcre-develzlib zlib-developenssl openssl-devel（安装依赖）
2. wget http://nginx.org/download/nginx-1.18.0.tar.gz（安装）
3. tar zxvf nginx-1.18.0.tar.gz（解压）
4. cd nginx-1.18.0
5. ./configure --prefix=/usr/local/nginx
6. make&&make install
7. cd /usr/local/sbin/nginx 
8. ./nginx（启动）

### 查看nginx启动状态
- ps -ef | grep nginx（通过进程查询是否启动）
- lsof -i:80（查询80端口占用进程）
- curl 127.0.0.1（访问网页查询）

### nginx启动成功，在浏览器中通过IP无法访问
- 第一步，对80端口进行防火墙配置：firewall-cmd --zone=public --add-port=80/tcp --permanent
- 第二步，重启防火墙服务：systemctl restart firewalld.service
  
### nginx的常用命令
使用命令的前提条件：进入到nginx的目录中（usr/local/nginx/sbin）    
1. ./nginx -v：查看nginx的版本号
2. nginx -V：显示 nginx 的版本，编译器版本和配置参数
3. ./nginx：启动nginx
4. ./nginx -s stop：关闭nginx
5. ./nginx -s reload：重新加载nginx
6. nginx -s stop ：快速关闭Nginx，可能不保存相关信息，并迅速终止web服务
7. nginx -s quit ：平稳关闭Nginx，保存相关信息，有安排的结束web服务
8. nginx -s reopen ：重新打开日志文件
9. nginx -c filename ：为 Nginx 指定一个配置文件，来代替缺省的

### nginx的配置文件
#### nginx配置文件位置
usr/local/nginx/conf/nginx.conf
#### nginx配置文件组成
```python
...              #全局块

events {         #events块
   ...
}

http      #http块
{
    ...   #http全局块
    server        #server块
    { 
        ...       #server全局块
        location [PATTERN]   #location块 location[ = | ~ | ~* | ^~] url {}
        {
            ...
        }
        location [PATTERN] 
        {
            ...
        }
    }
    server
    {
      ...
    }
    ...     #http全局块
}
```
- =:用于不含正则表达式的url前，要求字符串与url严格匹配，匹配成功就停止向下搜索并处理请求
- ~：用于表示url包含正则表达式，并且区分大小写。
- ~*：用于表示url包含正则表达式，并且不区分大瞎写
- ^~：用于不含正则表达式的url前，要求ngin服务器找到表示url和字符串匹配度最高的location后，立即使用此location处理请求，而不再匹配
- 如果有url包含正则表达式，不需要有~开头标识

具体配置项：
```py
########### 每个指令必须有分号结束。#################
#user administrator administrators;  #配置用户或者组，默认为nobody nobody。
#worker_processes 2;  #允许生成的进程数，默认为1
#pid /nginx/pid/nginx.pid;   #指定nginx进程运行文件存放地址
error_log log/error.log debug;  #制定日志路径，级别。这个设置可以放入全局块，http块，server块，级别以此为：debug|info|notice|warn|error|crit|alert|emerg
events {
    accept_mutex on;   #设置网路连接序列化，防止惊群现象发生，默认为on
    multi_accept on;  #设置一个进程是否同时接受多个网络连接，默认为off
    #use epoll;      #事件驱动模型，select|poll|kqueue|epoll|resig|/dev/poll|eventport
    worker_connections  1024;    #最大连接数，默认为512
}
http {
    include       mime.types;   #文件扩展名与文件类型映射表
    default_type  application/octet-stream; #默认文件类型，默认为text/plain
    #access_log off; #取消服务日志    
    log_format myFormat '$remote_addr–$remote_user [$time_local] $request $status $body_bytes_sent $http_referer $http_user_agent $http_x_forwarded_for'; #自定义格式
    access_log log/access.log myFormat;  #combined为日志格式的默认值
    sendfile on;   #允许sendfile方式传输文件，默认为off，可以在http块，server块，location块。
    sendfile_max_chunk 100k;  #每个进程每次调用传输数量不能大于设定的值，默认为0，即不设上限。
    keepalive_timeout 65;  #连接超时时间，默认为75s，可以在http，server，location块。
    # -------------- gzip压缩开始 -------------------
    gzip on;
    gzip_buffers 32 4K;#设置gzip申请内存的大小,其作用是按块大小的倍数申请内存空间,param2:int(k) 后面单位是k。这里设置以4k为单位,按照原始数据大小以4k为单位的32倍申请内存。建议此项不设置，使用默认值。
    gzip_comp_level 6; #选择gzip的压缩级别，级别越底压缩速度越快文件压缩比越小，反之速度越慢文件压缩比越大
    gzip_min_length 100; # 小于100字节文件不压缩
    gzip_types application/javascript text/css text/xml; # 选择要被压缩的文件类型
    gzip_disable "MSIE [1-6]\."; #配置禁用gzip条件，支持正则。此处表示ie6及以下不启用gzip（因为ie低版本不支持）
    gzip_vary on;
    # -------------- gzip压缩结束 -------------------

    upstream mysvr {   
      server 127.0.0.1:7878;
      server 192.168.10.121:3333 backup;  #热备
    }
    error_page 404 https://www.baidu.com; #错误页
    server {
        keepalive_requests 120; #单连接请求上限次数。
        listen       4545;   #监听端口
        server_name  127.0.0.1;   #监听地址       
        location  ~*^.+$ {       #请求的url过滤，正则匹配，~为区分大小写，~*为不区分大小写。
           #root path;  #根目录
           #index vv.txt;  #设置默认页
           proxy_pass  http://mysvr;  #请求转向mysvr 定义的服务器列表
           deny 127.0.0.1;  #拒绝的ip
           allow 172.18.5.54; #允许的ip           
        } 
    }
}

```
##### 全局快
从配置文件开始到events块之间的内容，主要会设置一些影响nginx服务器整体运行的配置指令;   
比如worker_processes 1;值越大，可以支持并发处理的数量也越多
##### events块
events块涉及的指令主要影响Nginx服务器与用户的网络连接；   
比如work_connections 1024;支持的最大连接数
##### http块
nginx服务器配置中最频繁的部分，http块包括http全剧块，server块
### 具体配置
- 找到nginx配置文件，进行反向代理配置
- 开放对外访问的端口号



### 防火墙基础
CentOS7 防火墙（firewall）的操作命令
#### 设置对外开放访问的端口
```js
firewall-cmd --add-port=8080/tcp --permanent   
firewall-cmd -reload // 防火墙重新加载配置
firewall-cmd --list-all // 查看已经开放的端口 
firewall-cmd --version // 查看版本
firewall-cmd --help // 查看帮助
firewall-cmd --state  // 显示状态
firewall-cmd --zone=public --list-ports // 查看所有打开的端口
firewall-cmd --completely-reload // 更新防火墙规则，重启服务
firewall-cmd --get-active-zones // 查看已激活的Zone信息
firewall-cmd --get-zone-of-interface=eth0 // 查看指定接口所属区域
firewall-cmd --panic-on
firewall-cmd --panic-off
firewall-cmd --query-panic  
```
#### 信任级别，通过Zone的值指定
```js
drop: 丢弃所有进入的包，而不给出任何响应 
block: 拒绝所有外部发起的连接，允许内部发起的连接 
public: 允许指定的进入连接 
external: 同上，对伪装的进入连接，一般用于路由转发 
dmz: 允许受限制的进入连接 
work: 允许受信任的计算机被限制的进入连接，类似 workgroup 
home: 同上，类似 homegroup 
internal: 同上，范围针对所有互联网用户 
trusted: 信任所有连接
```
#### firewall开启和关闭端口
以下都是指在public的zone下的操作，不同的Zone只要改变Zone后面的值就可以
添加：
```js
firewall-cmd --zone=public --add-port=80/tcp --permanent（--permanent永久生效，没有此参数重启后失效）
firewall-cmd --reload // 重新载入
firewall-cmd --zone=public --query-port=80/tcp  // 查看
firewall-cmd --zone=public --remove-port=80/tcp --permanent // 删除
```
