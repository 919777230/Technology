## 文章目录
一. 基础概念
二. Docker与虚拟机
三. 安装Docker
1. 卸载旧版本
2. repo方式安装
3. rpm方式安装
四. 镜像加速
五. 卸载docker
六. 可视化管理

## 一. 基础概念
```
1.镜像image
【分层存储】【Union FS】 构建时会一层层构建，前一层是后一层的基础。后一层上的任何改变只发生在自己这一层。

2.容器container：
容器的实质是进程，容器进程运行于属于自己的独立的命名空间。容器可以拥有自己的root文件系统、网络配置、进程空间，甚至自己的用户ID空间。

3.仓库registry
一个集中的存储和分发镜像的服务，一个Docker Registry可以包含多个仓库（Repository），每个仓库可包含多个标签，每个标签对应一个镜像。
```

## 二. Docker与虚拟机
```
共享内核：
Docker容器运行在宿主机内核上（没有自己的内核和虚拟硬件），与其他容器共享内核，因此更加轻量级。

应用隔离：
Docker Engine层上面运行各种程序，利用了Host OS里的NameSpace、ControlGroup等来做应用程序分离。
```

## 三. 安装Docker
docker有repo、rpm和自动化脚本多种安装方式:
### 1. 卸载旧版本
```
$ yum remove docker \
           docker-client \
           docker-client-latest \
           docker-common \
           docker-latest \
           docker-latest-logrotate \
           docker-logrotate \
           docker-engine
```
### 2. repo方式安装
先配置repo仓库，再执行yum在线安装命令，方便快捷。
```
# ⚠️ 添加「官方」或「阿里云」的docker仓库
$ wget https://download.docker.com/linux/centos/docker-ce.repo -O /etc/yum.repos.d/docker-ce.repo
$ wget https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo -O /etc/yum.repos.d/docker-ce.repo

# 查看可用的版本,
# 版本号格式：<year>.<month>.<N>
# 版本号说明：https://docs.docker.com/engine/install/#release-channels
$ yum list docker-ce --showduplicates
  docker-ce.x86_64    	18.06.1.ce-3.el7		docker-ce-stable
  docker-ce.x86_64    	18.06.2.ce-3.el7     	docker-ce-stable
  docker-ce.x86_64    	18.06.3.ce-3.el7     	docker-ce-stable
  docker-ce.x86_64    	3:20.10.11-3.el7     	docker-ce-stable
  docker-ce.x86_64    	3:20.10.12-3.el7     	docker-ce-stable

# ⚠️ 安装「最新版本」或「指定版本」
$ yum -y install docker-ce docker-ce-cli containerd.io
$ yum -y install docker-ce-20.10.12 docker-ce-cli-20.10.12 containerd.io

# 启动Docker服务
$ systemctl start docker
$ systemctl enable docker

# 测试服务
$ docker run hello-world
```

### 3. rpm方式安装
先下载RPM包，再离线安装，适合于集群多部署，节省网络流量和下载时间。

```
前往Docker对应的 CentOS 版本列表 , 进入x86_64/stable/Packages/ 并下载相应的RPM包，如：https://download.docker.com/linux/centos/7/x86_64/stable/Packages
# 选择Docker版本
VERSION=docker-ce-20.10.12-3.el7.x86_64.rpm

# 下载RPM离线包
wget https://download.docker.com/linux/centos/7/x86_64/stable/Packages/${VERSION}

# 本地安装
yum install -y ./${VERSION}

# 启动服务
systemctl start docker
systemctl enable docker

# 查看版本
docker version

# 测试服务
docker run hello-world
```

## 四. 镜像加速
国内仓库  
DaoCloud(🌹推荐)： https://hub.daocloud.io  
网易云(⚠️需登录)： https://c.163yun.com/hub#/m/home  
阿里云(⚠️需登录)： https://cr.console.aliyun.com/cn-beijing/instances/images  

国外仓库  
Docker Hub: https://hub.docker.com  
Quay: https://quay.io/search  

手动配置，参考说明 https://www.cnblogs.com/golinux/p/12759674.html
```
$ mkdir /etc/docker
$ cat -R << EOF > /etc/docker/daemon.json
{
  "insecure-registry": [
     "hub.xxx.cn",
     "reg.xxx.cn"
],
"registry-mirror": "https://xxx.mirror.aliyuncs.com",
}
EOF

# 重启docker服务
$ systemctl daemon-reload
$ systemctl restart docker
```


## 五. 卸载docker
卸载 Docker引擎、CLI 和 Containerd 软件包
```
yum remove docker-ce docker-ce-cli containerd.io
1
删除容器、卷或自定义配置等文件

rm -rf /var/lib/docker
rm -rf /var/lib/containerd
```

## 六. 可视化管理
Portainer是一个轻量级的管理UI，可让您轻松管理不同的Docker环境（Docker主机或Swarm集群）。  
Portainer允许您管理所有Docker资源（容器，映像，卷，网络等）！它与独立的Docker引擎和 Docker Swarm模式兼容。  
Portainer还提供了公共演示实例(用户名admin和密码tryportainer，15分钟重置一次) 和游乐场模式（docker账号登录，4小时重置一次）。  
```
# 下载镜像
$ docker pull portainer/portainer

# 单机版
$ docker run -d -p 9000:9000 \
    --restart=always \
    -v /var/run/docker.sock:/var/run/docker.sock \
    --name prtainer \
    portainer/portainer

# 集群版
$ docker run -d -p 9000:9000 \
    --restart=always \
    --name prtainer \ 
    portainer/portainer 
```
