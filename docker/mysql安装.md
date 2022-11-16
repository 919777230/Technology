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
## 命令安装
```base
下载mysql镜像文件:
　　docker search mysql
　　docker pull mysql:5.7.32
创建mysql容器：
　　docker run -d --name myMysql -p 6666:3306 -v /data/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 mysql:5.7.32
　　　　这里第一个6666是主机端口，第二个3306是容器端口，用navicat设6666端口就能连docker中的mysql数据库
文件挂载：
　　1、先在主机创建三个目录：mkdir -p my/mysql/conf; mkdir -p my/mysql/data;  mkdir -p my/mysql/logs;
　　2、docker cp myMysql:/etc/mysql/mysql.conf.d/mysqld.cnf  my/mysql/conf/
　　3、修改mysqld.cnf文件 vim mysqld.cnf ，然后在最后加character-set-server=utf8，最后保存文件。
　　4、docker run -d --name myMysql2 -p 6666:3306  -v /my/mysql/conf:/etc/mysql/mysql.conf.d/ -e MYSQL_ROOT_PASSWORD=123456 mysql:5.7.32
　　5、最后用navicat去连接数据库，端口是6666。
　　6、这样就可以在数据库里加中文数据。
```
