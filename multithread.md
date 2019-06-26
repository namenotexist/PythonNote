- thread模块
    - 创建新的进程
    - thread.start_new_thread(function,args)
- threading模块
    - 方法
        - threading.currentThread()
        - threading.enumereate()
        - threading.activeCount()
    - 类
        - Thread
            - run() 线程活动的方法 
            - start() 开启线程
            - join() 阻塞主线程，等待子线程终止再运行主线程
            - isAlive()
            - getName()
            - setName()
```
#!/usr/bin/python
# -*- coding: UTF-8 -*-
 
import threading
import time
 
exitFlag = 0
 
class myThread (threading.Thread):   #继承父类threading.Thread
    def __init__(self, threadID, name, delay):
        threading.Thread.__init__(self)
        self.threadID = threadID
        self.name = name
        self.delay = delay
    def run(self):                   #把要执行的代码写到run函数里面 线程在创建后会直接运行run函数 
        print("Starting " + self.name)
        print_time(self.name, self.delay, 5)
        print("Exiting " + self.name)
 
def print_time(threadName, delay, counter):
    while counter:
        if exitFlag:
            (threading.Thread).exit()
        time.sleep(delay)
        print("%s: %s" % (threadName, time.ctime(time.time())))
        counter -= 1
 
# 创建新线程
thread1 = myThread(1, "Thread-1", 1)
thread2 = myThread(2, "Thread-2", 2)
 
# 开启线程
thread1.start()
thread2.start()
 
print("Exiting Main Thread")
```

- 线程同步
```
#!/usr/bin/python
# -*- coding: UTF-8 -*-
 
import threading
import time
 
class myThread (threading.Thread):
    def __init__(self, threadID, name, counter):
        threading.Thread.__init__(self)
        self.threadID = threadID
        self.name = name
        self.counter = counter
    def run(self):
        print("Starting " + self.name)
       # 获得锁，成功获得锁定后返回True
       # 可选的timeout参数不填时将一直阻塞直到获得锁定
       # 否则超时后将返回False
        threadLock.acquire()
        print_time(self.name, self.counter, 3)
        # 释放锁
        threadLock.release()
 
def print_time(threadName, delay, counter):
    while counter:
        time.sleep(delay)
        print("%s: %s" % (threadName, time.ctime(time.time())))
        counter -= 1
 
threadLock = threading.Lock()
threads = []
 
# 创建新线程
thread1 = myThread(1, "Thread-1", 1)
thread2 = myThread(2, "Thread-2", 2)
 
# 开启新线程
thread1.start()
thread2.start()
 
# 添加线程到线程列表
threads.append(thread1)
threads.append(thread2)
 
# 等待所有线程完成
for t in threads:
    t.join()
print("Exiting Main Thread")
```



