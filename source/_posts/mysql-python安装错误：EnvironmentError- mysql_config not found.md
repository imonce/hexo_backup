---
title: mysql-python安装错误：EnvironmentError:mysql_config not found
date: 2017-04-24 02:45:19
tags: [mysql, python, mysql-python安装错误, trick]
comments: true
---

## 问题描述：

安装mysql-python时报错：

```
Collecting mysql-python
  Using cached MySQL-python-1.2.5.zip
    Complete output from command python setup.py egg_info:
    sh: 1: mysql_config: not found
    Traceback (most recent call last):
      File "<string>", line 1, in <module>
      File "/tmp/pip-build-_itbcX/mysql-python/setup.py", line 17, in <module>
        metadata, options = get_config()
      File "setup_posix.py", line 43, in get_config
        libs = mysql_config("libs_r")
      File "setup_posix.py", line 25, in mysql_config
        raise EnvironmentError("%s not found" % (mysql_config.path,))
    EnvironmentError: mysql_config not found

    ----------------------------------------
Command "python setup.py egg_info" failed with error code 1 in /tmp/pip-build-_itbcX/mysql-python/
```

## 问题原因：

没有安装libmysqlclient-dev。

## 解决方案：

执行：

`sudo apt-get install libmysqlclient-dev`

安装成功后，再运行`pip install mysql-python`即可。