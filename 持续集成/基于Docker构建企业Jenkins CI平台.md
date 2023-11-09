## CI/CD
   - CI：（continuous integration）：代码合并、构建、部署、测试、都在一起，不断执行这个过程，并对结果反馈。  
  - CD：（Continuous Deployment）：部署到测试环境、预发环境、生产环境。

## 拉取镜像
docker search jenkins/jenkins
docker pull jenkins/jenkins

## 启动镜像
cd /var
mkdir jenkins_mount
chmod 777 jenkins_mount
docker run -d \
 -uroot \
 -p 18080:8080 \
 -p 50000:50000 \
 -v /var/jenkins_mounts:/var/jenkins_home \
 -v /etc/localtime:/etc/localtime \
 --restart=always \
 --name=jenkins \
 jenkins/jenkins
 ## jenkins运行
 ip:18080

  
