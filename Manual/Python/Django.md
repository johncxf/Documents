# Django

文档：

- https://docs.djangoproject.com/zh-hans/4.1/
- https://www.runoob.com/django/django-tutorial.html

## 安装配置

```shell l
# pip 安装
$ pip3 install Django

# 查看版本
$ python -m django --version
```

## 开始使用

创建项目

```shell
$ django-admin startproject mysite
```

生成目录

```
mysite
|-- mysite
|   |-- __init__.py
|   |-- asgi.py
|   |-- settings.py
|   |-- urls.py
|   |-- wsgi.py
|-- manage.py
```

- 最外层 `mysite`目录：项目的容器
- `manage.py`：一个让你用各种方式管理 Django 项目的命令行工具。
- 里层 `mysite` 目录：包含你的项目，它是一个纯 Python 包。它的名字就是当你引用它内部任何东西时需要用到的 Python 包名。 (比如 `mysite.urls`).
- `mysite/__init__.py`：一个空文件，告诉 Python 这个目录应该被认为是一个 Python 包。
- `mysite/settings.py`：Django 项目的配置文件。
- `mysite/urls.py`：Django 项目的 URL 声明，就像你网站的“目录”。
- `mysite/wsgi.py`：作为你的项目的运行在 WSGI 兼容的Web服务器上的入口。

启动服务

```shell
$ python manage.py runserver
```

