title: Python 学习记录 (更新中)
date: 2015-05-31 13:06:31
tags: python
categories: 学习记录
---

<blockquote class="blockquote-center">用一种方法，最好是只有一种方法来做一件事。</blockquote>

## 常见问题
### 1. [格式化字符串：`%` vs `.format`](http://stackoverflow.com/questions/5082452/python-string-formatting-vs-format)
### 2. [Python 抽象基类(Abstract Base Class)](http://zaiste.net/2013/01/abstract_classes_in_python/)
### 3. [Python 魔法方法指南](http://pyzh.readthedocs.org/en/latest/python-magic-methods-guide.html)
<!-- more -->
### 4. Python 没有真正的 Private

> Strictly speaking, private methods are accessible outside their class, just not easily accessible. Nothing in Python is truly private; internally, the names of private methods and attributes are mangled and unmangled on the fly to make them seem inaccessible by their given names. You can access the \__parse method of the MP3FileInfo class by the name _MP3FileInfo__parse. Acknowledge that this is interesting, then promise to never, ever do it in real code. Private methods are private for a reason, but like many other things in > Python, their privateness is ultimately a matter of convention, not force.

严格来说，Python 没有真正的 private 方法，任何 `_method` , `__method` 都是可以访问的，只是不建议这么做。

```python
>>> class MyClass:
        def myPublicMethod(self):
            print 'public method'
        def __myPrivateMethod(self):
            print 'this is private!!'

>>> obj = MyClass()
>>> obj.myPublicMethod()
public method

>>> obj.__myPrivateMethod()
Traceback (most recent call last):
  File "", line 1, in
AttributeError: MyClass instance has no attribute '__myPrivateMethod'

>>> dir(obj)
['_MyClass__myPrivateMethod', '__doc__', '__module__', 'myPublicMethod']

>>> obj._MyClass__myPrivateMethod()
this is private!!
```

Ref:
- [why-are-pythons-private-methods-not-actually-private](http://stackoverflow.com/questions/70528/why-are-pythons-private-methods-not-actually-private)

### 5. [`_` `__` `__xx__` 的区别](http://igorsobreira.com/2010/09/16/difference-between-one-underline-and-two-underlines-in-python.html)

### 6. [`*args` & `**kwargs`](http://stackoverflow.com/questions/36901/what-does-double-star-and-star-do-for-python-parameters)

### 7. [理解修饰器（@decorator）](http://coolshell.cn/articles/11265.html)

### 8. [`with`的用法](http://blog.kissdata.com/2014/05/23/python-with.html)

### 9. [变量作用域](http://www.saltycrane.com/blog/2008/01/python-variable-scope-notes/)

### 19. [Python 的 import 机制](https://loggerhead.me/posts/python-de-import-ji-zhi.html)

## Django

### 1. 常用命令
#### 1. `django-admin startproject mysite`
```
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        wsgi.py
```

#### 2. `python manage.py runserver`
runserver 在每次请求会自动重新加载代码，除非是新建文件，不然不需要重启

#### 3. `python manage.py startapp polls`
在 django 的项目里，project 可以有多个 apps，每个 apps 实际上就是一个模块，独立完成某个功能。同一个 app 也可以同时被多个 project 使用
```
polls/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
```

#### 4. `python manage.py makemigrations polls`

#### 5. `python manage.py sqlmigrate polls 0001`
Dry run.
The sqlmigrate command doesn’t actually run the migration on your database - it just prints it to the screen so that you can see what SQL Django thinks is required. It’s useful for checking what Django is going to do or if you have database administrators who require SQL scripts for changes.

#### 6. `python manage.py check`
this checks for any problems in your project without making migrations or touching the database.

#### 7. `python manage.py migrate`

#### 8. `python manage.py shell`
We’re using this instead of simply typing “python”, because manage.py sets the DJANGO_SETTINGS_MODULE environment variable, which gives Django the Python import path to your mysite/settings.py file.

#### 9. `python manage.py createsuperuser`
