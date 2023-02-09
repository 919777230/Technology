## 安装：django2的版本
pip install https://codeload.github.com/sshwsfc/xadmin/zip/django2  

## 注册app
```base
INSTALLED_APPS = [
    # ...
    # xadmin主体模块
    'xadmin',
    # 渲染表格模块
    'crispy_forms',
    # 为模型通过版本控制，可以回滚数据
    'reversion',
]
```
## 设置主路由替换掉admin：主urls.py
```base
urlpatterns = [
    # ...
    path(r'xadmin/', xadmin.site.urls),
]
```
## 创建超级账号
```base
python manage.py createsuperuser
# 账号密码设置：admin | Admin123
```
## 在admin.py中注册model
```base
from . import models
import xadmin
#注册
xadmin.site.register(models.Book)
```

## 异常问题
* ImportError: cannot import name 'SKIP_ADMIN_LOG'

  * 方法一、
1.打开 import_export/admin.py，搜索“SKIP_ADMIN_LOG”，发现确实没有SKIP_ADMIN_LOG 变量，只有一个方法 get_skip_admin_log(self) ，此方法返回了skip_admin_log，而这个方法是在ImportMixin 类中定义的。
所以猜测，由于版本原因，旧版本中admin.py 是有SKIP_ADMIN_LOG的，新版本中放在了类中。而git上的项目用的是旧版包，我们拉取到本地之后下载的是新包，所以无法引用。
2.此时修改报错文件的代码，引入需要的类，用类调用方法，以此获取变量：
改成： from import_export.admin import DEFAULT_FORMATS, ImportMixin, ImportExportMixinBase

  * 方法二、
![image](https://user-images.githubusercontent.com/83051290/217809864-abd82519-1e83-4fbd-b8a8-9e13f2af0325.png)

