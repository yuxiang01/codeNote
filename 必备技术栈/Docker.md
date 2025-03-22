- 前置条件
	- 虚拟机、云服务器下 Linux 环境 (建议 CentOS 7)
	- 终端远程连接云服务器 `ssh root@8.130.82.143` 回车输入密码
- 环境查看
	- 查看系统内核
		```shell
		[root@fishx ~]# uname -r
		4.18.0-305.3.1.el8.x86_64
		```
	- 系统版本
		```shell
		[root@fishx ~]# cat /etc/os-release
		NAME="CentOS Linux"
		VERSION="8"
		ID="centos"
		ID_LIKE="rhel fedora"
		VERSION_ID="8"
		PLATFORM_ID="platform:el8"
		PRETTY_NAME="CentOS Linux 8"
		ANSI_COLOR="0;31"
		CPE_NAME="cpe:/o:centos:centos:8"
		HOME_URL="https://centos.org/"
		BUG_REPORT_URL="https://bugs.centos.org/"
		CENTOS_MANTISBT_PROJECT="CentOS-8"
		CENTOS_MANTISBT_PROJECT_VERSION="8"
		```
## 一、Docker 安装和配置
[帮助文档](http:docs.docker.com/get-started/)、[安装文档](https://docs.docker.com/engine/install/centos/)
### 1. 安装 docker 启动容器
1. 卸载旧版本
	```shell
	yum remove docker \
	    docker-client \
	    docker-client-latest \
	    docker-common \
	    docker-latest \
	    docker-latest-logrotate \
	    docker-logrotate \
	    docker-engine
	```
2. 安装工具包, 提供实用程序
	```shell
	yum install -y yum-utils
	```
3. 设置存储库
	```shell
	yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
	```
4. 更新 yum 软件包索引 `yum makecache fast`
5. 安装 docker docker-ce 社区版 `yum install docker-ce docker-ce-cli containerd.io`
6. 启动 Docker 服务 `systemctl start docker`
7. 通过运行映像验证 Docker 引擎安装是否成功 `docker run hello-world` 
![[Pasted image 20230628093018.png]]
启动容器 -> 查找镜像 -> 拉取镜像 -> 启动容器 -> 运行
8. 查看 docker 镜像
	```shell
	[root@fishx ~]# docker images
	REPOSITORY    TAG       IMAGE ID       CREATED       SIZE
	hello-world   latest    9c7a54a9a43c   7 weeks ago   13.3kB
	``` 
### 2. 配置阿里云镜像加速器
```shell
mkdir -p /etc/docker

tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://y2s75p8m.mirror.aliyuncs.com"]
}
EOF

systemctl daemon-reload

systemctl restart docker
```
### 3. 回顾 hello-world 流程
![[Docker hello-world.svg]]
### 4. 底层原理
#### Docker 是怎么工作的?
Docker 是一个 Client-Server 结构的系统, Docker 的守护进程运行在主机上, 通过 Socket 从客户端访问
DockerServer 接收到 Docker-Client 的指令, 就会执行该命令
![[Docker工作流程.svg]]
#### Docker 为什么比 VM 快?
1. docker 有着比虚拟机更少的抽象层
2. docker 利用的是宿主机的内核, vm 需要 GuestOS

![[Pasted image 20230628110351.png|600]]
所以说, 新建一个容器时, docker 不需要像虚拟机一样重新加载一个操作系统内核, 避免引导. 虚拟机是加载 GuestOS (分钟级别的), 而 docker 是利用宿主机的操作系统, 省略了这个复杂的过程 (秒级)
![[Pasted image 20230628111501.png]]
![[Pasted image 20230628111511.png]]

## 二、Docker 命令
### 1. 帮助命令
```shell
docker version # 显示docker的版本信息
docker info # 显示docker的系统信息,包括镜像和容器的数量
docker [命令] --help # 帮助文档
```
[帮助文档](https://docs.docker.com/engine/reference/run)
### 2. 镜像命令
#### 查看镜像
- 查看指令:  [帮助文档](https://docs.docker.com/engine/reference/commandline/images/)、指令速查 `docker images --help`
- **docker images** 查看所有本地的主机上的镜像
	```shell
	[root@fishx ~]# docker images
	REPOSITORY    TAG       IMAGE ID       CREATED       SIZE
	hello-world   latest    9c7a54a9a43c   7 weeks ago   13.3kB
	
	# REPOSITORY   镜像的仓库源
	# TAG          镜像的标签
	# IMAGE ID     镜像的ID
	# CREATED      镜像的创建时间
	# SIZE         镜像的大小

	# 可选项
	  -a, --all         # 显示所有镜像
	  -q, --quiet       # 只显示镜像的ID
	```
#### 搜索镜像
```shell
[root@fishx ~]# docker search --help

Usage:  docker search [OPTIONS] TERM

Search Docker Hub for images

Options:
  -f, --filter filter   Filter output based on conditions provided
	  --format string   Pretty-print search using a Go template
	  --limit int       Max number of search results
	  --no-trunc        Don't truncate output
```
 ```shell
[root@fishx ~]# docker search redis
NAME                          DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
redis                         Redis is an open source key-value store that…   12176     [OK]
redislabs/redisearch          Redis With the RedisSearch module pre-loaded…   56

# 可选项,通过搜索来过滤
--filter=STARS=3000 # 搜索标星大于3000的镜像

[root@fishx ~]# docker search redis -f=STARS=3000
NAME      DESCRIPTION                                      STARS     OFFICIAL   AUTOMATED
redis     Redis is an open source key-value store that…   12176     [OK]
``` 
#### 下载镜像
- 指令速查
	```shell
	[root@fishx ~]# docker pull --help
	
	Usage:  docker pull [OPTIONS] NAME[:TAG|@DIGEST]
	
	Download an image from a registry
	
	Aliases:
	  docker image pull, docker pull
	
	Options:
	  -a, --all-tags                Download all tagged images in the repository
	      --disable-content-trust   Skip image verification (default true)
	      --platform string         Set platform if server is multi-platform capable
	  -q, --quiet                   Suppress verbose output
	```
 - **docker pull** 拉取镜像
	 ```shell
	 [root@fishx ~]# docker pull mysql
	Using default tag: latest # 如果不写 tag,默认为 latest
	latest: Pulling from library/mysql
	72a69066d2fe: Pull complete  # 分层下载, docker images的核心: 联合文件系统
	93619dbc5b36: Pull complete
	99da31dd6142: Pull complete
	626033c43d70: Pull complete
	37d5d7efb64e: Pull complete
	ac563158d721: Pull complete
	d2ba16033dad: Pull complete
	688ba7d5c01a: Pull complete
	00e060b6d11d: Pull complete
	1c04857f594f: Pull complete
	4d7cfa90e6ea: Pull complete
	e0431212d27d: Pull complete
	Digest: sha256:e9027fe4d91c0153429607251656806cc784e914937271037f7738bd5b8e7709 # 签名
	Status: Downloaded newer image for mysql:latest
	docker.io/library/mysql:latest # 真实地址 
	# docker pull mysql 等价于 docker.io/library/mysql:latest
	```
- 指定版本下载
	```shell
	[root@fishx ~]# docker pull mysql:5.7
	5.7: Pulling from library/mysql
	72a69066d2fe: Already exists # 联合文件系统的优势: 已下载的不需要重复下载
	93619dbc5b36: Already exists
	99da31dd6142: Already exists
	626033c43d70: Already exists
	37d5d7efb64e: Already exists
	ac563158d721: Already exists
	d2ba16033dad: Already exists
	0ceb82207cd7: Pull complete # 只需下载冲突的版本
	37f2405cae96: Pull complete
	e2482e017e53: Pull complete
	70deed891d42: Pull complete
	Digest: sha256:f2ad209efe9c67104167fc609cca6973c8422939491c9345270175a300419f94
	Status: Downloaded newer image for mysql:5.7
	docker.io/library/mysql:5.7
	 ```
  ![[Pasted image 20230628122031.png]]
#### 删除镜像
**docker rmi** 删除镜像
```shell
docker rmi [OPTIONS] IMAGE [IMAGE...]
docker rmi -f 容器id # 删除指定的容器
docker rmi -f 容器id 容器id 容器id # 删除多个容器
docker rmi -f $(docker images -aq) # 删除全部容器
```
```shell
[root@fishx ~]# docker rmi -f 3218b38490ce
Untagged: mysql:latest
Untagged: mysql@sha256:e9027fe4d91c0153429607251656806cc784e914937271037f7738bd5b8e7709
Deleted: sha256:3218b38490cec8d31976a40b92e09d61377359eab878db49f025e5d464367f3b
Deleted: sha256:aa81ca46575069829fe1b3c654d9e8feb43b4373932159fe2cad1ac13524a2f5
Deleted: sha256:0558823b9fbe967ea6d7174999be3cc9250b3423036370dc1a6888168cbd224d
Deleted: sha256:a46013db1d31231a0e1bac7eeda5ad4786dea0b1773927b45f92ea352a6d7ff9
Deleted: sha256:af161a47bb22852e9e3caf39f1dcd590b64bb8fae54315f9c2e7dc35b025e4e3
Deleted: sha256:feff1495e6982a7e91edc59b96ea74fd80e03674d92c7ec8a502b417268822ff
```
### 3. 容器命令
有了镜像才能创建容器, docker 下载一个 centos 镜像来进行学习 `docker pull centos`
![[Pasted image 20230628124044.png]]
```shell
[root@fishx ~]# docker run -it centos /bin/bash
[root@b8f0367a71b4 /]#
```
#### 列出所有运行的容器
```shell
docker ps  # 列出当前正在运行的容器
   -a   # 列出当前正在运行的容器,并带出历史运行过的容器
   -n=number # 显示最近创建的number个容器
   -q   # 只显示容器的编号
   -aq  # 显示所有容器的编号
```
#### 退出容器
```shell
exit # 直接退出容器
快捷键: Ctrl+P+Q # 退出容器但不停止容器
```
#### 删除容器
```shell
docker rm 容器id     # 删除指定容器(不能删除正在运行的容器)
docker rm -f $(docker ps -aq) # 删除所有容器
docker ps -a -q|xargs docker rm # 管道符删除所有的容器
```
#### 启动/停止容器
```shell
docker start   容器id    # 启动容器
docker restart 容器id    # 重启容器
docker stop    容器id    # 停止正在运行的容器
docker kill    容器id    # 强制停止当前容器
```
#### 其他常用命令
- 后台启动容器
	```shell
	[root@fishx ~]# docker run -d centos
	57f36d2aa5cc43336a5ab850bc1f23b8f29d0e98faded68b4454e70e7de12a6e
	
	[root@fishx ~]# docker ps
	CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
	
	# 发现 centos 停止了! 
	# 这是一个常见的坑,docker容器使用后台运行,就必须要有一个前台进程,docker发现没有应用,就会停止
	```
	```shell
	[root@fishx ~]# docker run --name centos-study -d -it centos bin/bash
	2ca3d5aa705369886a3e497ee18e468c125708f178b5ab73a6f3524941c8f687
	
	[root@fishx ~]# docker ps
	CONTAINER ID   IMAGE     COMMAND      CREATED          STATUS         PORTS     NAMES
	2ca3d5aa7053   centos    "bin/bash"   10 seconds ago   Up 9 seconds             centos-study
	```
 - 查看日志
	 ```shell
	 docker logs -tf --tail number 容器
	 
	 # 参数:
	 -tf           # 显示日志
	 --tail number # 要显示的日志条数
	
	 # 暂无日志? 编写一段shell脚本
	 docker run --name centos-study -d centos /bin/sh -c "while true;do echo fishx 666;sleep 5;done"
	
	 # 显示日志
	 [root@fishx ~]# docker logs -tf --tail 10 centos-study
	2023-06-28T05:35:59.374470565Z fishx 666
	2023-06-28T05:36:04.375027327Z fishx 666
	2023-06-28T05:36:09.376997992Z fishx 666
	 ```
 - 查看容器内部的进程信息
	 ```shell
	 [root@fishx ~]# docker top centos-study
	 UID     PID       PPID       C         STIME       TTY        TIME         CMD
	root    2785986   2785965    0         13:47       pts/0      00:00:00     bin/bash
	 ```
  - 查看镜像的元数据 `docker inspect 容器id/name`
>[!note]- `docker inspect centos-study` 示例
> ```shell
> [root@fishx ~]# docker inspect centos-study
> [
>     {
>         "Id": "2ef4bb3120cec178631884b156bbfc16d51d9a53296399d07772009bbe35f47f",
>         "Created": "2023-06-28T05:47:41.876873253Z",
>         "Path": "bin/bash",
>         "Args": [],
>         "State": {
>             "Status": "running",
>             "Running": true,
>             "Paused": false,
>             "Restarting": false,
>             "OOMKilled": false,
>             "Dead": false,
>             "Pid": 2785986,
>             "ExitCode": 0,
>             "Error": "",
>             "StartedAt": "2023-06-28T05:47:42.129201179Z",
>             "FinishedAt": "0001-01-01T00:00:00Z"
>         },
>         "Image": "sha256:5d0da3dc976460b72c77d94c8a1ad043720b0416bfc16c52c45d4847e53fadb6",
>         "ResolvConfPath": "/var/lib/docker/containers/2ef4bb3120cec178631884b156bbfc16d51d9a53296399d07772009bbe35f47f/resolv.conf",
>         "HostnamePath": "/var/lib/docker/containers/2ef4bb3120cec178631884b156bbfc16d51d9a53296399d07772009bbe35f47f/hostname",
>         "HostsPath": "/var/lib/docker/containers/2ef4bb3120cec178631884b156bbfc16d51d9a53296399d07772009bbe35f47f/hosts",
>         "LogPath": "/var/lib/docker/containers/2ef4bb3120cec178631884b156bbfc16d51d9a53296399d07772009bbe35f47f/2ef4bb3120cec178631884b156bbfc16d51d9a53296399d07772009bbe35f47f-json.log",
>         "Name": "/centos-study",
>         "RestartCount": 0,
>         "Driver": "overlay2",
>         "Platform": "linux",
>         "MountLabel": "",
>         "ProcessLabel": "",
>         "AppArmorProfile": "",
>         "ExecIDs": null,
>         "HostConfig": {
>             "Binds": null,
>             "ContainerIDFile": "",
>             "LogConfig": {
>                 "Type": "json-file",
>                 "Config": {}
>             },
>             "NetworkMode": "default",
>             "PortBindings": {},
>             "RestartPolicy": {
>                 "Name": "no",
>                 "MaximumRetryCount": 0
>             },
>             "AutoRemove": false,
>             "VolumeDriver": "",
>             "VolumesFrom": null,
>             "ConsoleSize": [
>                 30,
>                 136
>             ],
>             "CapAdd": null,
>             "CapDrop": null,
>             "CgroupnsMode": "host",
>             "Dns": [],
>             "DnsOptions": [],
>             "DnsSearch": [],
>             "ExtraHosts": null,
>             "GroupAdd": null,
>             "IpcMode": "private",
>             "Cgroup": "",
>             "Links": null,
>             "OomScoreAdj": 0,
>             "PidMode": "",
>             "Privileged": false,
>             "PublishAllPorts": false,
>             "ReadonlyRootfs": false,
>             "SecurityOpt": null,
>             "UTSMode": "",
>             "UsernsMode": "",
>             "ShmSize": 67108864,
>             "Runtime": "runc",
>             "Isolation": "",
>             "CpuShares": 0,
>             "Memory": 0,
>             "NanoCpus": 0,
>             "CgroupParent": "",
>             "BlkioWeight": 0,
>             "BlkioWeightDevice": [],
>             "BlkioDeviceReadBps": [],
>             "BlkioDeviceWriteBps": [],
>             "BlkioDeviceReadIOps": [],
>             "BlkioDeviceWriteIOps": [],
>             "CpuPeriod": 0,
>             "CpuQuota": 0,
>             "CpuRealtimePeriod": 0,
>             "CpuRealtimeRuntime": 0,
>             "CpusetCpus": "",
>             "CpusetMems": "",
>             "Devices": [],
>             "DeviceCgroupRules": null,
>             "DeviceRequests": null,
>             "MemoryReservation": 0,
>             "MemorySwap": 0,
>             "MemorySwappiness": null,
>             "OomKillDisable": false,
>             "PidsLimit": null,
>             "Ulimits": null,
>             "CpuCount": 0,
>             "CpuPercent": 0,
>             "IOMaximumIOps": 0,
>             "IOMaximumBandwidth": 0,
>             "MaskedPaths": [
>                 "/proc/asound",
>                 "/proc/acpi",
>                 "/proc/kcore",
>                 "/proc/keys",
>                 "/proc/latency_stats",
>                 "/proc/timer_list",
>                 "/proc/timer_stats",
>                 "/proc/sched_debug",
>                 "/proc/scsi",
>                 "/sys/firmware"
>             ],
>             "ReadonlyPaths": [
>                 "/proc/bus",
>                 "/proc/fs",
>                 "/proc/irq",
>                 "/proc/sys",
>                 "/proc/sysrq-trigger"
>             ]
>         },
>         "GraphDriver": {
>             "Data": {
>                 "LowerDir": "/var/lib/docker/overlay2/3c1147ecd01d953cd5141fd8d3a46cce4f5549cc3d8c9be1ea51c8a620165f7c-init/diff:/var/lib/docker/overlay2/f6199fc5f3470724c3357d8d4474afa0aaabe1616f31b50970308b3e588fe2fc/diff",
>                 "MergedDir": "/var/lib/docker/overlay2/3c1147ecd01d953cd5141fd8d3a46cce4f5549cc3d8c9be1ea51c8a620165f7c/merged",
>                 "UpperDir": "/var/lib/docker/overlay2/3c1147ecd01d953cd5141fd8d3a46cce4f5549cc3d8c9be1ea51c8a620165f7c/diff",
>                 "WorkDir": "/var/lib/docker/overlay2/3c1147ecd01d953cd5141fd8d3a46cce4f5549cc3d8c9be1ea51c8a620165f7c/work"
>             },
>             "Name": "overlay2"
>         },
>         "Mounts": [],
>         "Config": {
>             "Hostname": "2ef4bb3120ce",
>             "Domainname": "",
>             "User": "",
>             "AttachStdin": false,
>             "AttachStdout": false,
>             "AttachStderr": false,
>             "Tty": true,
>             "OpenStdin": true,
>             "StdinOnce": false,
>             "Env": [
>                 "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
>             ],
>             "Cmd": [
>                 "bin/bash"
>             ],
>             "Image": "centos",
>             "Volumes": null,
>             "WorkingDir": "",
>             "Entrypoint": null,
>             "OnBuild": null,
>             "Labels": {
>                 "org.label-schema.build-date": "20210915",
>                 "org.label-schema.license": "GPLv2",
>                 "org.label-schema.name": "CentOS Base Image",
>                 "org.label-schema.schema-version": "1.0",
>                 "org.label-schema.vendor": "CentOS"
>             }
>         },
>         "NetworkSettings": {
>             "Bridge": "",
>             "SandboxID": "b3a52528de2ea6540640b9704d9c0ade82d5f2955a3cb9e976c94d851eb19f61",
>             "HairpinMode": false,
>             "LinkLocalIPv6Address": "",
>             "LinkLocalIPv6PrefixLen": 0,
>             "Ports": {},
>             "SandboxKey": "/var/run/docker/netns/b3a52528de2e",
>             "SecondaryIPAddresses": null,
>             "SecondaryIPv6Addresses": null,
>             "EndpointID": "3174d64e8c4b99e42af728b8235bb12875d4028db333a4d15fc58303181b675b",
>             "Gateway": "172.17.0.1",
>             "GlobalIPv6Address": "",
>             "GlobalIPv6PrefixLen": 0,
>             "IPAddress": "172.17.0.2",
>             "IPPrefixLen": 16,
>             "IPv6Gateway": "",
>             "MacAddress": "02:42:ac:11:00:02",
>             "Networks": {
>                 "bridge": {
>                     "IPAMConfig": null,
>                     "Links": null,
>                     "Aliases": null,
>                     "NetworkID": "2f3b6003a4368d698d4dcd99d5a54df261b01e74e6745d9bfb20c523efd0edac",
>                     "EndpointID": "3174d64e8c4b99e42af728b8235bb12875d4028db333a4d15fc58303181b675b",
>                     "Gateway": "172.17.0.1",
>                     "IPAddress": "172.17.0.2",
>                     "IPPrefixLen": 16,
>                     "IPv6Gateway": "",
>                     "GlobalIPv6Address": "",
>                     "GlobalIPv6PrefixLen": 0,
>                     "MacAddress": "02:42:ac:11:00:02",
>                     "DriverOpts": null
>                 }
>             }
>         }
>     }
> ]
>   ```
- 进入容器
	```shell
	# 方式一: 
	docker exec -it 容器id/name /bin/bash
	# 测试:
	[root@fishx ~]# docker exec -it centos-study /bin/bash
	[root@2ef4bb3120ce /]# ls
	bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  ...
	
	# 方式二: 
	docker attach 容器id/name
	# 测试:
	[root@fishx ~]# docker attach centos-study
	[root@2ef4bb3120ce /]# ls
	bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  ...
    
	# 区别: 
	docker exec   # 进入容器后开启一个新的终端
    docker attch  # 进入容器正在执行的终端,不会启动新的进程
	```
 - 文件拷贝
	 ```shell
	 docker cp 宿主机目标目录/文件 容器(id/name):容器内路径
	 # 示例
	 [root@fishx /]# touch test.txt
	 [root@fishx /]# vi test.txt  
	 [root@fishx /]# docker cp test.txt centos-study:/
	 [root@fishx /]# docker exec -it centos-study /bin/bash
	 [root@2ef4bb3120ce /]# cat test.txt
	 测试一下,瞧瞧
	
	 docker cp 容器(id/name):容器内路径  宿主机目标目录/文件
	 # 示例
	 [root@fishx ~]# docker cp centos-study:/test.txt /root
	 Successfully copied 2.05kB to /root
	 ``` 
### 4. 小结
![[Pasted image 20230628152349.png]]

## 三、Docker 实例
### 部署 Nginx
1. [搜索 Nginx 镜像](https://hub.docker.com/_/nginx)
2. 下载镜像 `docker pull nginx`
3. 运行测试
	```shell
	# 启动nginx
	[root@fishx ~]# docker run -d --name nginx-test -p 8080:80 nginx
	858bbef9fdcf4ffd3b3971b9217579c3ad0f84a286e4cfd3b9b326f11414fe80

    # 同时在根路径创建conf、log、html三个文件夹
	mkdir -p data/nginx/{conf,log,html}

	# nginx.conf 复制到主机
	docker cp nginx-test:/etc/nginx/nginx.conf data/nginx/conf/nginx.conf
	# conf.d 文件夹复制
	docker cp nginx-test:/etc/nginx/conf.d data/nginx/conf/conf.d
	# html 文件夹复制
	docker cp nginx-test:/usr/share/nginx/html data/nginx/

	# 停止刚刚创建的 nginx 容器
	docker stop nginx-test
	# 删除刚刚创建的容器
	docker rm nginx-test

	# 重新创建 nginx 容器并挂载目录至宿主机
	docker run -d --name nginx -p 8880:80 --net myapp \
	-v /data/nginx/conf/nginx.conf:/etc/nginx/nginx.conf \
	-v /data/nginx/conf/conf.d:/etc/nginx/conf.d \
	-v /data/nginx/log:/var/log/nginx \
	-v /data/nginx/html:/usr/share/nginx/html \
	--privileged=true nginx

	# 阿里云服务器linux内自测
	[root@fishx ~]# curl localhost:8080
	<!DOCTYPE html>
	...
	
	-d      后台运行
	--name  为容器命名
	-p      宿主端口:容器内部端口
	```

![[Docker端口放行详解.svg]]

### 部署 Tomcat
```shell
# 1. 下载镜像并启动容器
docker run --name tomcat-test -p 8081:8080 -d tomcat:9.0

# 2. 进入容器,修改webapps.dist目录 为 webapps目录
docker exec -it tomcat-study /bin/bash
rm -rf webapps # 级联删除webapps目录
mv webapps.dist webapps # 重命名目录

docker cp tomcat:/usr/local/tomcat/{logs,webapps} /Users/fishx/data/tomcat/{logs,webapps}

docker run -d --name tomcat -p 8008:8080 -v /Users/fishx/data/tomcat/webapps:/usr/local/tomcat/webapps -v /Users/fishx/data/tomcat/logs:/usr/local/tomcat/logs --privileged=true tomcat:9.0

# 3. 本机自测端口, 外部访问
curl localhost:8081
```

### 部署 Elasticsearch
1. [搜索 Elasticsearch 镜像](https://hub.docker.com/_/elasticsearch)
2. 下载启动 Elasticsearch 镜像容器
	```shell
	# es暴露的端口很多、数据一般需要放置到/挂载安全目录
	docker run -d --name es -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:7.6.2
	# --net somenetwork 网络配置
	
	# 机器卡死、赶紧关闭、增加内存限制、修改配置文件 -e 环境配置修改
	docker run -d --name es -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" \
	-e ES_JAVA_OPTS="-Xms64m -Xmx512m" elasticsearch:7.6.2
	
	docker stats # 查看内存占用
	
	[root@fishx ~]# curl localhost:9200
	{
	  "name" : "bb2681dc0e75",
	  "cluster_name" : "docker-cluster",
	  "cluster_uuid" : "NWeGagD1SYGgK6axATP2zg",
	  "version" : {
	    "number" : "7.6.2",
	    "build_flavor" : "default",
	    "build_type" : "docker",
	    "build_hash" : "ef48eb35cf30adf4db14086e8aabd07ef6fb113f",
	    "build_date" : "2020-03-26T06:34:37.794943Z",
	    "build_snapshot" : false,
	    "lucene_version" : "8.4.0",
	    "minimum_wire_compatibility_version" : "6.8.0",
	    "minimum_index_compatibility_version" : "6.0.0-beta1"
	  },
	  "tagline" : "You Know, for Search"
	}
	```

>[!question]- 如何使用 kibana 连接 es
> ![[Docker网络入门.svg]]

### 实战: Redis 部署
```shell
docker run --net mynet --restart=always --log-opt max-size=100m --log-opt max-file=2 \
-p 6379:6379 --name redis \
-v Users/fishx/data/redis/redis.conf:/etc/redis/redis.conf \
-v Users/fishx/data/redis/data:/data \
-d redis:6.0.20 redis-server /etc/redis/redis.conf \
--appendonly yes --requirepass admin
```
|                   参数                      |          描述          |
|:-----------------------------------------:|:---------------------------------------------:|
|              —restart=always              |                          Redis启动方式，开机启动    |
|                 —log-opt                  |                                 日志配置      |
|               -p 6379:6379                |                   主机与容器映射端口       |
|                   —name                   |                        容器名称         |
|                    -v                     |   挂载卷地址,自动同步到容器中   |
|   -d redis-server /etc/redis/redis.conf   |              Redis 启动时使用指定目录配置文件   |
|              appendonly yes               |                                开启持久化     |
|               —requirepass                | 设置Redis的密码为admin     |  
## 四、Docker 镜像
- **镜像是什么?**
	- 镜像是一种轻量级、可执行的独立软件包, 用来打包软件运行环境和基于运行环境开发的软件, 它包含运行某个软件所需的所有内容
		- 包括代码、运行时、库、环境变量和配置文件
- 所有的应用, 直接打包 docker 镜像, 就可以直接 run!
	- 如何得到镜像?
		 - 从远程仓库下载
		 - 自己制作一个镜像 DockerFile
		 - ...
### 联合文件系统
- UnionFS (联合文件系统)
	- Union 文件系统 (UnionFS)是一种分层、轻量级并且高性能的文件系统
		- 它支持对文件系统的修改作为一次提交来一层层叠加
		- 同时可以将不同目录挂载到同一个虚拟文件系统下 
		  - unite several directories into a single virtual filesystem
	- Union 文件系统是 Docker 镜像的基础
	- 镜像可以通过分层来进行继承, 基于基础镜像 (没有父镜像), 可以制作各种具体的应用镜像
- 特性: 
	- 一次同时加载多个文件系统, 但从外面看起来, 只能看到一个文件系统
	- 联合加载会把各层文件系统叠加起来
	- 最终的文件系统会包含所有底层的文件和目录
### Docker 镜像加载原理
- 镜像加载原理
	- docker 的镜像实际上由一层一层的文件系统组成, 这种层级的文件系统 UnionFS 
	- bootFs (boot file system)主要包含 bootloader 和 kernel, bootloader 主要是引导加载 kernel
		- Linux 刚启动时会加载 bootFs 文件系统, 在 Docker 镜像的最底层是 bootFs
		- 这一层与我们典型的 Linux/Unix 系统是一样的, 包含 boot 加载器和内核
		- 当 boot 加载完成之后整个内核都在内存中了
			- 此时内存的使用权已由 bootFs 转交给内核
			- 同时系统也会卸载 bootFs
	- rootFs (root file system), 在 bootFs 之上
		- 包含的典型就是 Linux 系统中的 `/dev`、`/proc`、`/bin`、`/etc` 等标准目录和文件
		- rootFs 就是各种不同的操作系统发行版, 例如 `Ubuntu`、`Centos` 等等
 
 ![[Pasted image 20230707151345.png]]
### 分层理解
![[Pasted image 20230708095340.png]]
![[Pasted image 20230708095655.png]]
![[Pasted image 20230708104254.png]]
### Commit 镜像
```shell
docker commit 提交容器成为一个新的副本

# 命令和git原理类似
docker commit -m="提交的描述信息" -a="作者" 容器id 目标镜像名:[TAG]

docker commit -a="fishx" -m="add webapps app" tomcat-test tomcat02:1.0
```
如果你想要保存当前容器的状态, 就可以通过 commit 来提交, 获得一个镜像 (类似于快照)

## 五、容器数据卷
将容器内的目录挂载到 Linux 上, docker 产生的数据同步到本地硬盘 
容器的持久化和同步操作, 同时容器间可以数据共享
### 使用数据卷
```shell
docker run -it -v 主机目录:容器内目录

docker run --name centos-study -it -v /data/centos:/home centos /bin/bash

# 查看是否挂载成功
docker inspect centos-study
```
![[Pasted image 20230709161904.png]]
### MySql 同步数据卷
```shell
docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag
# -e 配置容器环境

docker run -d -p 3306:3306 -v /data/mysql/conf:/etc/mysql/conf.d \
-v /data/mysql/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=admin --name mysql_study mysql:5.7

docker run -d -p 13306:3306 -e MYSQL_ROOT_PASSWORD=admin@2024 --name mysql mysql

# 查看日志-排错
docker logs mysql_study
```
>[!note] 解决 docker mysql 时区问题
> 宿主机：`docker cp /usr/share/zoneinfo/Asia/Shanghai mysql:/etc/localtime`
### 具名和匿名挂载
```shell
# 匿名挂载 -P 随机分配端口 -v 容器内路径
docker run -d -P --name nginx_01 -v /ect/nginx nginx
# 查看所有的 volume 的情况
docker volume ls

# 具名挂载 -v 卷名:容器内路径
docker run -d -P --name nginx_02 -v juming-nginx:/ect/nginx nginx
# 查看所有的 volume 的情况
[root@fishx ~]# docker volume ls
DRIVER    VOLUME NAME
local     juming-nginx

# 查看卷详细信息
docker volume inspect juming-nginx
[root@fishx ~]# docker volume inspect juming-nginx
[
    {
        "CreatedAt": "2023-07-10T08:52:46+08:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/juming-nginx/_data",
        "Name": "juming-nginx",
        "Options": null,
        "Scope": "local"
    }
]
```
- 其实分为两类
	- 一类是指定目录挂载
	- 一类是匿名和具名, 不指定路径的

```shell
# 如何确定是具名挂载、匿名挂载,还是指定路径挂载
-v 容器内路径 # 匿名挂载
-v 卷名:容器内路径 # 具名挂载
-v /宿主机路径:容器内路径 # 指定路径挂载
```
拓展:
```shell
# 通过 -v 容器内路径:ro/rw 改变权限
ro readonly # 只读
rw readwrite # 可读可写

docker run -d -P --name nginx02 -v juming-nginx:/ect/nginx:ro nginx
docker run -d -P --name nginx02 -v juming-nginx:/ect/nginx:rw nginx
```

### 初识 Dockerfile 
Dockerfile 就是用来构建 docker 镜像的构建文件/命令脚本
通过此脚本可以生成镜像, 镜像是一层一层的, 脚本一个个的命令, 每个命令都是一层
新建 dockerfile 文件
```shell
FROM centos

VOLUME ["volume01","volume02"]

CMD echo "---volume end---"
CMD /bin/bash
```
```shell
# 查看镜像已经创建
[root@fishx docker-test-volume]# docker images
REPOSITORY     TAG       IMAGE ID       CREATED         SIZE
tomcat         9.0       b8e65a4d736d   18 months ago   680MB
mysql          5.7       c20987f18b13   18 months ago   448MB
fishx/centos   1.0       24e2ad821fbe   22 months ago   231MB

# 启动
docker run -d -it 24e2ad821fbe # dockerfile脚本中写了/bin/bash默认进入

# 查看目录,发现volume01、volume02已经被创建
[root@4aecc5949a11 /]# ls -ls
0 drwxr-xr-x  12 root root 144 Sep 15  2021 usr
0 drwxr-xr-x  20 root root 262 Sep 15  2021 var
0 drwxr-xr-x   2 root root  22 Jul 10 02:01 volume01
0 drwxr-xr-x   2 root root   6 Jul 10 01:55 volume02

[root@fishx ~]# docker ps
CONTAINER ID   IMAGE          COMMAND                   CREATED          STATUS          PORTS   NAMES
4aecc5949a11   24e2ad821fbe   "/bin/sh -c /bin/bash"    51 minutes ago   Up 48 minutes           vigilant_nash

[root@fishx ~]# docker inspect 4aecc5949a11
```
![[Pasted image 20230710100755.png]] 
![[Pasted image 20230710105839.png]]

## 六、DockerFile
dockerfile 是用来构建 docker 镜像的文件, 命令参数脚本
- 构建步骤
	1. 编写一个 dockerfile 文件
	2. docker build 构建成一个镜像
	3. docker run 运行镜像
	4. docker push 发布镜像

### DockerFile 构建过程
1. 每个保留字(指令)都必须大写
2. 至上而下顺序执行
3. `#` 表示注释
4. 每一个指令都会创建一个新的镜像层, 并提交

![[Pasted image 20230710110802.png]]
DockerFile: 构建文件, 定义了一切的步骤 (源码)
DockerImages: 通过 DockerFile 构建生成的镜像, 最终发布和运行的产品
Docker 容器: 容器就是镜像运行起来提供服务器

### DockerFile 指令
```shell
FROM        # 基础镜像,一切从此开始构建
MAINTAINER  # 镜像作者,姓名+邮箱
RUN         # 镜像构建时需要运行的命令
ADD         # 步骤,添加内容
WORKDIR     # 镜像的工作目录
VOLUME      # 挂载目录
EXPOSE      # 暴露端口配置
CMD         # 指定该容器启动时运行的命令,只有最后一条会生效
ENTRYPOINT  # 指定该容器启动时运行的命令,可以追加命令
ONBUILD     # 当构建一个被继承 DockerFile 时运行触发ONBUILD里的指令
COPY        # 类似ADD,将文件拷贝到镜像中
ENV         # 构建时设置环境变量
```
![[Pasted image 20230710111532.png]]
```shell
FROM centos:7 # centos已经停止运行,需要指定版本,才能下载
MAINTAINER fishx<2690866872@qq.com>

ENV MYPATH /usr/local
WORKDIR $MYPATH

RUN yum -y install vim
RUN yum -y install net-tools

EXPOSE 70

CMD echo $MYPATH
CMD echo "--- end ---"
CMD /bin/bash
```
### 实战: Tomcat 镜像
1. 准备镜像文件 tomcat、jdk 压缩包
	![[Pasted image 20230710213520.png]]
2. 编写 dockerfile 文件
	```shell
	FROM centos:7
	MAINTAINER fishx<2690866872@qq.com>
	
	COPY readme.txt /usr/local/readme.txt
	
	ADD jdk1.8.tar.gz /usr/local/
	ADD tomcat9.tar.gz /usr/local/
	
	RUN yum -y install vim
	
	ENV MYPATH /usr/local
	WORKDIR $MYPATH
	
	ENV JAVA_HOME /usr/local/jdk1.8.0_151
	ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
	ENV CATALINA_HOME /usr/local/apache-tomcat-9.0.85
	ENV CATALINA_BASH /usr/local/apache-tomcat-9.0.85
	ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin
	
	EXPOSE 8080
	
	CMD /usr/local/apache-tomcat-9.0.85/bin/startup.sh && tail -F /usr/local/apache-tomcat-9.0.85/logs/catalina.out
	```
3. 构建镜像 
	```shell
	docker build -t tomcat_diy .
	```
4. 镜像启动容器
	```shell
	docker run -d -p 8080:8080 --name tomcat -v /data/diy_tomcat/webapps:/usr/local/apache-tomcat-9.0.76/webapps -v /data/diy_tomcat/logs:/usr/local/apache-tomcat-9.0.76/logs tomcat_diy
	```
	需要先将 webapps 目录拷贝出来不然挂载的是空目录
5. 踩坑总结：
	1. 检查 Ubuntu 系统及位数，`uname --m` 如 `x86_64` 即 64 位
	2. 下载对应版本的 Tomcat、JDK
	3. 推荐使用 `wget https:..` 直接下载压缩包至当前目录
	4. <font color="#ff0000">压缩包解压后名称会改变</font>
### 发布镜像
#### 发布到 hub 上
1. [注册 docker.hub账号](https://hub.docker.com),确认此账号可以登录
2. 服务器上提交镜像
	```shell
	docker login --help
	
	Usage:  docker login [OPTIONS] [SERVER]
	
	Log in to a registry.
	If no server is specified, the default is defined by the daemon.
	
	Options:
	  -p, --password string   Password
	      --password-stdin    Take the password from stdin
	  -u, --username string   Username
	```
3. 登录完毕后就可以 `docker push` 提交镜像了
	```shell
	# 先要使用docekr tag 规范命名  用户名字/镜像名字:版本号 再docker push
	docker tag 镜像名 hub-user/repo-name [:tag]
	# 规范命名
	docker tag tomcat_diy fishx/tomcat_diy
	# 提交镜像
	docker push fishx/tomcat_diy
	```

#### 发布到阿里云镜像服务
1. 登录阿里云
2. 找到容器镜像服务
3. 创建命名空间
4. 创建容器镜像

![[Pasted image 20230710220749.png]]

## 七、Docker 网络
### 理解 Docker0
```shell
# 清空所有环境
docker rm -f $(docker ps -aq)
docker rmi -f $(docker images -aq)
```
查看本机网卡
![[Pasted image 20230711112945.png]]
需解决容器间通信问题
![[Docker网络入门.svg]]
```shell
docker run -d -P --name tomcat-01 tomcat:7.0

# 查看容器内部网卡
[root@fishx ~]# docker exec tomcat-01 ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
99: eth0@if100: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.17.0.2/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever

# linux 可以 ping 通tomcat容器内部网卡
[root@fishx ~]# ping 172.17.0.2
PING 172.17.0.2 (172.17.0.2) 56(84) bytes of data.
```
>[!note] 原理: 
>1. 我们每启动一个 docker 容器, docker 就会给 docker 容器分配一个 ip
>2. 只要安装了 docker, 就会有一个网卡 docker0
>3. 桥接模式, 使用的技术是 veth-pair 技术

![[Pasted image 20230711143040.png]]
再测试一个 Tomcat 容器
```shell
docker run -d -P --name tomcat-02 tomcat:7.0
```
分别查看容器内部、本机网卡
![[Pasted image 20230711143712.png]]
>[!summary] 
>发现这个容器带来的网卡, 都是一对对的
> `veth-pair` 就是一对的虚拟设备接口, 是成对出现的, 一段连接协议, 一段彼此相连
> 正因如此, `veth-pair` 充当一个桥梁, 连接各种虚拟网络设备
> OpenStack, Docker 容器之间的连接, OVS 的连接, 都是用了 `veth-pair` 技术

测试 tomcat-01 和 tomcat-02 是可以 ping 通!
```shell
[root@fishx ~]# docker exec -it tomcat-02 ping 172.17.0.2
PING 172.17.0.2 (172.17.0.2) 56(84) bytes of data.
64 bytes from 172.17.0.2: icmp_seq=1 ttl=64 time=0.056 ms
```
![[docker网络模型图.svg]]

>[!note] Docker 使用的是 Linux 桥接模式
>宿主机中是一个 Docker 容器的网桥 docker0 
> ![[Docker网路桥接模式.svg]]

Docker 中所有的网络接口都是虚拟的. 虚拟转发效率高
只要容器删除, 对应的一对网桥就没了!

### 容器互联--link
`--link` 就可以通过容器名访问
```shell
[root@fishx ~]# docker run -d -P --name tomcat-03 --link tomcat-02 tomcat:7.0
85f993305d68ba711823267d3fdb352ac73d7e9e085e750de7e6b04596e89144

[root@fishx ~]# docker exec -it tomcat-03 ping tomcat-02
PING tomcat-02 (172.17.0.3) 56(84) bytes of data.
64 bytes from tomcat-02 (172.17.0.3): icmp_seq=1 ttl=64 time=0.100 ms
# 正向OK,反向无法ping通

[root@fishx ~]# docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
2dac11aa2c03   bridge    bridge    local
e843db247f6f   host      host      local
954ec57ff2ce   none      null      local

# 查看桥接详细信息
[root@fishx ~]# docker network inspect bridge
```
![[Pasted image 20230711163631.png]]
```shell
[root@fishx ~]# docker inspect tomcat-03
"Links": [
    "/tomcat-02:/tomcat-03/tomcat-02"
],
```
`--link` 本质是在 `hosts` 配置中增加了一个 `172.18.0.3`, 就是静态域名解析
![[Pasted image 20230713093538.png]]
![[Pasted image 20230713093551.png]]

### 自定义网络
查看所有网络
![[Pasted image 20230713093757.png]]
- 网络模式
	- `bridge`: 桥接模式 (docker 默认)
	- `none`: 不配置网络
	- `host`: 和宿主机共享网络

```shell
# 直接启动的命令默认携带--net bridge,这就是docker0这个网卡
docker run -d -P --name tomcat-01 tomcat
docker run -d -P --name tomcat-01 --net bridge tomcat

# docker0特点: 默认、不能域名访问、虽然--link可以打通,但局限性大

# 自定义网络
docker network create --driver bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1 mynet
# 这个命令是用来创建一个名为“mynet”的 Docker 网络的。
	# 它使用桥接模式 (--driver bridge)
	# 子网掩码为 192.168.0.0/16 (--subnet 192.168.0.0/16)
	# 网关地址为 192.168.0.1 (--gateway 192.168.0.1)。
# 在 Docker 中，网络可以用来连接容器，让它们可以相互通信。
# 使用这个命令创建的网络会使得所有在这个网络下的容器都可以使用 IP 地址 192.168.0.x 进行通信。
```
![[Pasted image 20230713100017.png]]
![[Pasted image 20230713100247.png|500]]
```shell
[root@fishx ~]# docker run -d -P --name tomcat-net-01 --net mynet tomcat:7.0
381b2a863659746af10ca3d81adc17abb0bb339a7aee3bd05fca999f4b964746

[root@fishx ~]# docker run -d -P --name tomcat-net-02 --net mynet tomcat:7.0
05fb7665d69aaa59dd5fe5ee6ec31cebff6666e5757a9d98628bf420b926d1c4

# 查看mynet网络
[root@fishx ~]# docker network inspect mynet
"Containers": {
    "05fb7665d69aaa59dd5fe5ee6ec31cebff6666e5757a9d98628bf420b926d1c4": {
        "Name": "tomcat-net-02",
        "EndpointID": "d916eee544349f19b24d3d442e817604846effc7c51d66f5ddf5c421d499bc11",
        "MacAddress": "02:42:c0:a8:00:03",
        "IPv4Address": "192.168.0.3/16",
        "IPv6Address": ""
    },
    "381b2a863659746af10ca3d81adc17abb0bb339a7aee3bd05fca999f4b964746": {
        "Name": "tomcat-net-01",
        "EndpointID": "ce17fa910a11176451b96f9ba96e6492da66465418e52a6e275a37a432302e14",
        "MacAddress": "02:42:c0:a8:00:02",
        "IPv4Address": "192.168.0.2/16",
        "IPv6Address": ""
    }
}

# ping ip 能通
[root@fishx ~]# docker exec -it tomcat-net-01 ping 192.168.0.3
PING 192.168.0.3 (192.168.0.3) 56(84) bytes of data.
64 bytes from 192.168.0.3: icmp_seq=1 ttl=64 time=0.086 ms

# ping 容器名能通
[root@fishx ~]# docker exec -it tomcat-net-01 ping tomcat-net-02
PING tomcat-net-02 (192.168.0.3) 56(84) bytes of data.
64 bytes from tomcat-net-02.mynet (192.168.0.3): icmp_seq=1 ttl=64 time=0.070 ms
```
自定义网络 docker 都已经帮我们维护好了对应关系
好处: `redis`、`mysql` 不同集群使用不同的网络, 保证集群是安全和健康的
### 网络连通
![[网络连通.svg]]
```shell
# docker0
docker run -d -P --name tomcat-01 tomcat:7.0
docker run -d -P --name tomcat-01 tomcat:7.0
# mynet
docker run -d -P --name tomcat-net-01 --net mynet tomcat:7.0
docker run -d -P --name tomcat-net-02 --net mynet tomcat:7.0
# 自定义网络支持: 正反向容器名 ping
docker exec -it tomcat-net-01 ping tomcat-net-02
docker exec -it tomcat-net-02 ping tomcat-net-01
# docker0只支持ping ip
[root@fishx ~]# docker network inspect bridge
"Containers": {
    "3900cac79cedb1e73cd4f81b45cf517cd854ee509afbb5334062bd80037e3ef0": {
        "Name": "tomcat-02",
        "EndpointID": "5bd1e373d7033bfd721ff9c4a892bc4ffd2e6661bf8a6822ca226199a86d1100",
        "MacAddress": "02:42:ac:11:00:03",
        "IPv4Address": "172.17.0.3/16",
        "IPv6Address": ""
    },
    "ff945ac67fa1ac78538d13a2bf68b6128942be203f568330a7943a74e7fc40be": {
        "Name": "tomcat-01",
        "EndpointID": "00bcab618e9ed733476648eae83c320f779ffc0815437d0a7ea18af1c69ae787",
        "MacAddress": "02:42:ac:11:00:02",
        "IPv4Address": "172.17.0.2/16",
        "IPv6Address": ""
    }
}
[root@fishx ~]# docker exec -it tomcat-01 ping 172.17.0.3
[root@fishx ~]# docker exec -it tomcat-02 ping 172.17.0.2
```
将一个网卡连接到另一个容器
![[Pasted image 20230713153058.png]]
```shell
# 使 tomcat-01 能够连接 mynet
[root@fishx ~]# docker network connect mynet tomcat-01
# 查看网卡详细信息
[root@fishx ~]# docker network inspect mynet
"Containers": {
    "2f0aa025e0de78de353aef7f95681a99a53ac8be5581c23f202e1ce8be519a91": {
        "Name": "tomcat-net-02",
        "EndpointID": "526e019357cfee2e6df0b5592b862e6ed323560d17aa50055c143675c334d8f2",
        "MacAddress": "02:42:c0:a8:00:03",
        "IPv4Address": "192.168.0.3/16",
        "IPv6Address": ""
    },
    "98354a8d60d61e11d53b32fd2ba2e3ecb9b4c9a3ff2165463822fe3acbfd56c3": {
        "Name": "tomcat-net-01",
        "EndpointID": "61dad714ee3d5fc032da5cf49db294d75888dbd9d68e57141fb7cc1d5037d030",
        "MacAddress": "02:42:c0:a8:00:02",
        "IPv4Address": "192.168.0.2/16",
        "IPv6Address": ""
    },
    "ff945ac67fa1ac78538d13a2bf68b6128942be203f568330a7943a74e7fc40be": {
        "Name": "tomcat-01",
        "EndpointID": "88bbc0cadb6465b89a9e9bc8e5712bdd776f457af4ec3fc733696607c9531c06",
        "MacAddress": "02:42:c0:a8:00:04",
        "IPv4Address": "192.168.0.4/16",
        "IPv6Address": ""
    }
},
```
一个容器两个 ip 地址: 连通之后就是将 `tomcat-01` 容器放到了 `mynet` 网络下
```shell
# 连通之后docker0里的tomcat-01容器之后,就能直接ping通 mynet网络里的tomcat-net-01/02
[root@fishx ~]# docker exec -it tomcat-01 ping tomcat-net-01
[root@fishx ~]# docker exec -it tomcat-01 ping tomcat-net-02

# 因为tomcat-02容器没有与mynet网卡连通,因此不能访问
[root@fishx ~]# docker exec -it tomcat-02 ping tomcat-net-02
ping: tomcat-net-02: Name or service not known
```
>[!summary] 总结: 如果容器需要跨网络访问, 就需要使用 `docker network connect 网络 容器` 连通

## 八、Ubuntu 安装配置 Docker
[Ubuntu 16.04 安装配置Docker](https://zhuanlan.zhihu.com/p/109083850#:~:text=Ubuntu%2016.04%20%E5%AE%89%E8%A3%85%E9%85%8D%E7%BD%AEDocker%201%20%E6%B7%BB%E5%8A%A0%E8%BD%AF%E4%BB%B6%E4%BB%93%E5%BA%93%20%E5%AE%98%E6%96%B9%E4%BB%93%E5%BA%93%20...%202,docker-ce%20%23%20%E5%AE%89%E8%A3%85%E6%9C%80%E6%96%B0%E7%89%88%E7%9A%84docker%20...%203%20%E6%B7%BB%E5%8A%A0%E8%AE%BF%E9%97%AE%E6%9D%83%E9%99%90%20%E8%BF%99%E4%B8%AA%E6%97%B6%E5%80%99%E8%BF%90%E8%A1%8Cdocker%E7%9A%84%E8%AF%9D%EF%BC%8C%E5%A6%82%E6%9E%9C%E4%B8%8D%E6%98%AFroot%E7%94%A8%E6%88%B7%E4%BC%9A%E6%8A%A5%E9%94%99%EF%BC%9A%20)
```shell
sudo apt-get update # 先更新一下软件源库信息

sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
```
阿里云仓库
```shell
curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository \
     "deb [arch=amd64] https://mirrors.aliyun.com/docker-ce/linux/ubuntu \
     $(lsb_release -cs) \
     stable"
```
安装docker
```shell
apt-get update
apt-get install docker-ce # 安装最新版的docker
```
## 九、Ubuntu 配置 jdk
用命令 `sudo vim ~/.bashrc` 文件，在文件最后面加上下面几行：
```shell
#set oracle jdk environment
export JAVA_HOME=/home/jenkins/workspace/java  ## 这里要注意目录要换成自己解压的jdk 目录
export JRE_HOME=${JAVA_HOME}/jre  
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib  
export PATH=${JAVA_HOME}/bin:$PATH
```
`source ~/.bashrc`  更新环境变量，`java -version` 校验

```shell
docker cp nginx:/etc/nginx/nginx.conf /opt/nginx/conf/nginx.conf
# 将容器conf.d文件夹下内容复制到宿主机
docker cp nginx:/etc/nginx/conf.d /opt/nginx/conf/conf.d
# 将容器中的html文件夹复制到宿主机
docker cp nginx:/usr/share/nginx/html /opt/nginx/

docker run \
-p 80:80 \
--name nginx \
-v /opt/nginx/conf/nginx.conf:/etc/nginx/nginx.conf \
-v /opt/nginx/conf/conf.d:/etc/nginx/conf.d \
-v /opt/nginx/log:/var/log/nginx \
-v /opt/nginx/html:/usr/share/nginx/html \
-d nginx:latest
```

后台运行 Jar
```shell
nohup java -jar server.jar &
```
关闭 Jar
```shell
# 后台进程
ps -ef | grep java
# 终止进程
kill -9 xxxx
```
服务 `systemctl restart`