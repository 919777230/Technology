## 安装docker

```base
# 移除旧版本的 Docker
sudo yum remove docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate docker-engine

# 安装依赖软件包
sudo yum install -y yum-utils

# 设置 Docker 仓库
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

# 更新 Yum 包索引
sudo yum makecache fast

# 安装 Docker CE
sudo yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin

# 启动 Docker
sudo systemctl start docker

# 验证 Docker 是否成功安装
sudo docker --version

#配置镜像
{
	"registry-mirrors": ["https://77aokf3o.mirror.aliyuncs.com"]
}

```

## 安装 portainer

```base
docker search portainer/portainer
docker volume create portainer_data
docker run -d -p 8000:8000 -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer
挂载目录：/var/lib/docker/volumes
页面：http:IP:9000
```

## 安装jenkins

```base
docker search jenkinsci/blueocean
docker volume create jenkins_home
docker run -d --name jenkins -u root -p 8080:8080 -p 50000:50000 --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v jenkins_home:/var/jenkins_home jenkinsci/blueocean
页面：http://IP:8080
查看密码
docker exec -it jenkins bash
cat 目录

```

