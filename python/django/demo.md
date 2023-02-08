## 创建项目
命令：python manage.py startapp sales  
![image](https://user-images.githubusercontent.com/83051290/217459305-9925656d-eb1c-4f41-b42f-3d30d7b97a95.png)
## 启动
python manage.py runserver 0.0.0.0:80

## url路由
更多路由配置：https://docs.djangoproject.com/en/2.1/topics/http/urls/  
* 项目下创建urls.py 
![image](https://user-images.githubusercontent.com/83051290/217498101-42320d0d-a3bf-4736-996b-a35fd108ee16.png)
* 修改根目录urls.py
![image](https://user-images.githubusercontent.com/83051290/217498212-e5a294a9-1e86-4bd2-a572-76eb5107079a.png)

## 数据库
项目中数据库配置再settings.py
```base
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'agent_visen1',
        'USER': 'sd_test',
        'PASSWORD': 'oIRzvTQi',
        'HOST': '10.2.0.15',
        'PORT': '3306',
        'OPTIONS': {'charset': 'utf8mb4'}
    }
}
```


## 其他说明
### 数据库
```base
当前文件，的目录的上级目录：BASE_DIR = Path(__file__).resolve().parent.parent
![image](https://user-images.githubusercontent.com/83051290/217513153-0dd04886-de07-4e23-a61a-32e27543217d.png)
如果丢失执行：python manage.py migrate
可以下载可视化工具查看db.sqlite3
https://github.com/pawelsalawa/sqlitestudio/releases
```
