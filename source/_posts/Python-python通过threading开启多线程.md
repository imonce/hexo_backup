---
title: '[Python]通过threading开启多线程'
date: 2018-08-09 22:42:08
tags: [Python, Threading, 多线程, Python入门]
category: [Python]
---

# 导入包

```python
import threading
```

# 构造方法： 
Thread(group=None, target=None, name=None, args=(), kwargs={}) 

- group: 线程组，目前还没有实现，库引用中提示必须是None； 
- target: 要执行的方法； 
- name: 线程名； 
- args/kwargs: 要传入方法的参数。

# 实例方法：

- isAlive(): 返回线程是否在运行。正在运行指启动后、终止前。 
- get/setName(name): 获取/设置线程名。 
- start():  线程准备就绪，等待CPU调度
- is/setDaemon(bool): 获取/设置是后台线程（默认前台线程（False））。（在start之前设置）
    - 如果是后台线程，主线程执行过程中，后台线程也在进行，主线程执行完毕后，后台线程不论成功与否，主线程和后台线程均停止
    - 如果是前台线程，主线程执行过程中，前台线程也在进行，主线程执行完毕后，等待前台线程也执行完成后，程序停止
- start(): 启动线程。 
- join([timeout]): 阻塞当前上下文环境的线程，直到调用此方法的线程终止或到达指定的timeout（可选参数）。

# 示例代码
```python
import threading

# Split items and run function through n threads
# func形如func(arg_list[0], ..., arg_list[n], items),run_through_threads 可以把items分为num份分配给num个线程运行
def run_through_threads(func, arg_list, items, num=4):
    threads = []
    item_len = len(items)
    for i in range(num):
        threads.append(threading.Thread(target=func, args=(*arg_list, items[int(i*item_len/num):int((i+1)*item_len/num)])))
    for t in threads:
        t.setDaemon(True)
        t.start()
    for t in threads:
        t.join()
```