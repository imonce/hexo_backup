---
title: jupyter配置远程访问
date: 2018-09-19 11:10:45
tags: [jupyter,远程访问,ipython,notebook]
---

# 安装jupyter

> pip install ipython
> pip install jupyter

# 生成jupyter配置文件

> jupyter notebook --generate-config

```
#: jupyter notebook --generate-config
Writing default config to: /home/xm/.jupyter/jupyter_notebook_config.py
```

# 自动生成密码

> jupyter notebook password

```
#: jupyter notebook password
Enter password: # 这里输入密码不会显示字符的
Verify password: 
[NotebookPasswordApp] Wrote hashed password to /home/xm/.jupyter/jupyter_notebook_config.json
# 密码已经被加密记录到这个文件中了
```

# 获取密码

> cat /home/xm/.jupyter/jupyter_notebook_config.json

```
#: cat /home/xm/.jupyter/jupyter_notebook_config.json
{
  "NotebookApp": {
    "password": "这是你的密码，一整段都复制 下来"
  }
}
```

# 修改配置文件

> vim /home/xm/.jupyter/jupyter_notebook_config.py

```
#懒得找对应配置项的朋友，直接把这四项配置写到文件开头就可以了
c.NotebookApp.ip = '*'
c.NotebookApp.password = 'sha:ce...刚才复制的那个密文'
c.NotebookApp.open_browser = False
c.NotebookApp.port = 8888 #可自行指定一个端口，访问时使用该端口
```
