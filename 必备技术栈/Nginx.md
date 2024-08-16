## 一、Nginx 核心功能
「反向代理、负载均衡、动静分离」
### 1.1 正向代理
 如果我们要访问 `www.google.com` 但是直接访问不到，则需要通过代理服务器来访问，这种代理服务就称为正向代理		
 ![[Pasted image 20230623180717.png]]
 - 图解
	 - 正向代理: 
		 - `www.google.com` 访问不到, 所以使用代理服务器帮助我们 (即客户端)来上网 
			 - 正向代理<u>帮助的对象是<font color="#ff0000">客户端</font></u>
				 - 因此可以把 `客户端+正向代理服务`, 视为一个整体 
	- 正向代理同时也隐藏了客户端信息

### 1.2 反向代理
客户端将请求发送到代理服务器，由代理服务器去选择目标服务器获取数据后，返回给客户端，这种代理方式为反向代理
![[Pasted image 20230623204535.png]]
- 图解: 
	- 不希望客户端直接访问目标 Web 服务器 
		- 比如目标 Web 服务器是集群, 如果直接访问就会提供多个公网 IP, 而是希望提供一个统一的访问 IP
			- 这个是理解反向代理的前提，即为什么要反向代理.
	- 反向代理<u>帮助的对象是<font color="#ff0000">目标 Web 服务器</font></u>
	- 当客户端请求达到反向代理服务后，<u>由反向代理服务来决定如何访问目标 Web 服务器 (或者是哪个 Web 服务器)</u>, 这个过程对客户端是不可见的.
		- 可以将 `反向代理服务 + 反向代理服务代理的 Web 服务器` 视为一个整体 
	- 反向代理服务会暴露公共的 IP, 只要能上网，就可以访问，但是对于反向代理服务器管理的/代理的 Web 服务器通常是在局域网内，不能直接访问，只能通过反向代理来访问.
	- 反向代理会<font color="#ff0000">屏蔽内网服务器</font> (也就是他代理的服务)<font color="#ff0000">信息</font>, 并实现负载均衡访问

### 1.3 负载均衡
当客户端向反向代理服务器 (比如 Nginx)发出请求，
如果 Nginx 代理了多个 WEB 服务器 (集群)，
Nginx 会将请求/负载分发到不同的服务器，也就是负载均衡
![[Pasted image 20230623211225.png]]

### 1.4 动静分离
为了加快网站的解析速度，可以把<span style="background:#fff88f">动态资源和静态资源由不同的服务器来解析</span>，降低单个服务器的压力
![[Pasted image 20230625082745.png]]

## 二、Nginx 命令行参数
[地址](https://nginx.org/en/docs/switches.html)
![[Pasted image 20230625114612.png]]
![[Pasted image 20230625114629.png]]

## 三、config 配置文件
[[nginx.config 配置文件]]
### 3.1 Nginx 的配置文件位置
1. `安装目录/conf/nginx.conf`
2. `安装目录/nginx.conf`

两个文件是一样的
使用 `/usr/local/nginx/sbin/nginx` 启动 Nginx，
默认用的是 `安装目录/nginx.conf` 配置文件

### 3.2 config 文件详解
`nginx.config` 作用：完成对 Nginx 的各种配置，包括端口，并发数，重写规则等
![[Pasted image 20230625151428.png]]

#### (1) 全局块
- 范围: 从配置文件开始到 events 块之间的内容
- 主要会设置一些影响 nginx 服务器整体运行的配置指令
	+ 主要包括配置运行 Nginx 服务器的用户 (组)
	+ 允许生成的 worker process 数
	+ 进程 PID 存放路径
	+ 日志存放路径和类型以及配置文件的引入等
- 这是 Nginx 服务器并发处理服务的关键配置
	- worker_processes 值越大，可以支持的并发处理量也越多
	- 但是会受到硬件、软件等设备的制约

#### (2) events 块
- events 块涉及的指令主要影响 Nginx 服务器与用户的网络连接
- 常用的设置包括: 
	+ 是否开启对多 work process 下的网络连接进行序列化
	+ 是否允许同时接收多个网络连接
	+ 选取哪种事件驱动模型来处理连接请求
	+ 每个 work process 可以同时支持的最大连接数等
```shell
events {
    worker_connections  1024;
}
```

#### (3) http 块
- Nginx 服务器配置中最复杂的部分，代理、缓存和日志定义等绝大多数功能和第三方模块的配置都在这里
- http 块也可以包括: 
	- http 全局块
		- 指令包括文件引入、MIME-TYPE 定义、日志自定义、连接超时时间、单连接请求数上限等
			```shell
			http {
			    # MIME-TYPE 定义
		      include       mime.types; 
		      default_type  application/json;
		      # 开启文件传输
		      sendfile        on;
		      # 连接超时时间(s)
		      keepalive_timeout  65;
			 }
			```
	- <font color="#ff0000"> server 块</font>
		- 这块和虚拟主机有密切关系
			- 虚拟主机从用户角度看，和一台独立的硬件主机是完全一样的，该技术的产生是为了节省互联网服务器硬件成本
		- 每个 http 块可以包括多个 server 块，而每个 server 块就相当于一个虚拟主机
			- 每个 server 块也分为全局 server 块，以及可以同时包含多个 location 块
		- <font color="#ff0000">全局 server 块</font>
			- 最常见的配置是本虚拟机主机的监听配置和本虚拟主机的名称或 IP 配置
			- <font color="#ff0000">location 块</font>
				- 一个 server 块可以配置多个 location 块


## 六、docker 安装 Nginx
- 启动容器、映射80端口到宿主8880端口
	```shell
	docker run --name nginx-test -p 8880:80 -d nginx
	```
- 创建挂载目录
	```shell
	# 同时在根路径创建conf、log、html三个文件夹
	mkdir -p data/nginx/{conf,log,html}
	```
- 将容器中的文件进行复制
	```shell
	# nginx.conf 复制到主机
	docker cp nginx-test:/etc/nginx/nginx.conf data/nginx/conf/nginx.conf
	
	# conf.d 文件夹复制
	docker cp nginx-test:/etc/nginx/conf.d data/nginx/conf/conf.d
	
	# html 文件夹复制
	docker cp nginx-test:/usr/share/nginx/html data/nginx/
	```
 - 停止并删除 nginx 容器
	```shell
	# 停止刚刚创建的 nginx 容器
	docker stop nginx-test
	# 删除刚刚创建的容器
	docker rm nginx-test
	```
- 重新创建 nginx 容器并挂载目录至宿主机
	```shell
	docker run -d --name nginx -p 8880:80 \
	-v data/nginx/conf/nginx.conf:/etc/nginx/nginx.conf \
	-v data/nginx/conf/conf.d:/etc/nginx/conf.d \
	-v data/nginx/log:/var/log/nginx \
	-v data/nginx/html:/usr/share/nginx/html \
	--privileged=true nginx
	```
	```shell
	docker run -d --name nginx -p 8880:80 \
	-v /Users/fishx/data/nginx/conf/nginx.conf:/etc/nginx/nginx.conf \
	-v /Users/fishx/data/nginx/conf/conf.d:/etc/nginx/conf.d \
	-v /Users/fishx/data/nginx/log:/var/log/nginx \
	-v /Users/fishx/data/nginx/html:/usr/share/nginx/html \
	--privileged=true nginx
	```

```shell
# linux 删除文件夹
rm -rf webapps
# Linux 重命名
mv webapps.dist webapps
```

```shell
        location / {
            root   /usr/share/nginx/html/hmdp;
            index  index.html index.htm;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   /usr/share/nginx/html;
        }


        location /api {  
            default_type  application/json;
            #internal;  
            keepalive_timeout   30s;  
            keepalive_requests  1000;  
            #æ¯ækeep-alive  
            proxy_http_version 1.1;  
            rewrite /api(/.*) $1 break;  
            proxy_pass_request_headers on;
            #more_clear_input_headers Accept-Encoding;  
            proxy_next_upstream error timeout;  
            proxy_pass http://host.docker.internal:8081;
            #proxy_pass http://backend;
        }
```