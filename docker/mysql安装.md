## 使用portainer安装
portainer安装，见
![image](https://user-images.githubusercontent.com/83051290/202120856-c0265989-b874-47e3-b979-074b6ec7deaf.png)
![image](https://user-images.githubusercontent.com/83051290/202121032-3a857470-dc22-4ada-9767-5862faee4333.png)

```base
1.在 Name 一栏中输入容器名字；

2.在 Image 一栏输入容器镜像名，比如要安装 wordpress 则输入 wordpress 即可，系统会自动拉取 wordpress 的容器镜像；

3.设置端口，如果开启 Publish all exposed ports 开关，系统会随机开启一个端口映射到容器端口。另外也可点击 map additional port 添加自定义端口(需要注意的是，host 可以输入服务器的任意无冲突端口，container 则需要根据具体容器镜像输入对应端口才行，此处用的 MySQL 容器镜像，所以填写 3306 即可)；

4.选择 Env ，然后点击 add environment variable ，添加如所示的容器环境变量，value 可自定义输入；

MySQL_ROOT_PASSWORD 为必填，作用是为 MySQL 数据库设置 root 用户密码，否则容器将无法使用；

MYSQL_DATABASE 的作用是 MySQL 容器安装完成可以自动创建一个名为 value栏所填写的值 (此处为 wordpress ) 的数据库。

5.点击 Restart policy 选择 Always，代表容器无论在什么情况下停止总会自动重新启动；

6.点击 Deploy the container 创建容器；

7.MySQL 容器自动开启了远程功能，所以只需在使用数据库时，在数据库地址栏填入 公网IP:端口 就能连接到数据库了。

```
