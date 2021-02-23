### Threading.local 多线程
```python
 import threading
 from threading import local
 local_obj = local() # 实例化local对象
 def task(i):
  local_obj.xxx = i # 为local对象设置一个参数
  print(local_obj.xxx,i) # 获取local对象中的参数
  # threading.get_ident() 获取线程的唯一标示
  print(threading.get_ident(),i)
 for i in range(10):
  t = threading.Thread(target=task,args(i,)) # 创建线程
  t.start() # 开始执行线程


```
### 根据字典自定义类似Threading.localz
```python
 import threading
 import greenlet # 获取协程信息
 DIC = {}
 def task(i):
  # 获取协成的唯一标记
  # indent = greenlet.getcurrent()
  # treading.get_ident() 获取线程的唯一标记
  indent = treading.get_ident()
  if indent in DIC:
   DIC[indent]['xxx'] = i
  else:
   DIC[indent] = {'xxx':i}
  print(DIC[index][xxx],i) # 打印字典中的参数
 for i in range(10):
  t = threading.Thread(target=task,args=(i,)) # 创建线程
  t.start() # 开始执行线程

```
### 自定义升级版Threading.localz
```
此版本中协程与线程可以完全的兼容，完成对请求的数据隔离。
为什么需要给线程和协程数据进行隔离？
 在多个请求的时候，由于每个请求的信息不相同，所以需要对不同的请求信息进行分类管理（数据隔离），从而防止数据混乱；
 这样在需要使用视图函数取得用户请求信息的时候，才能根据不同的信息进行取值和修改。

```
```python
 import threading
 import time
 
 try:
  import greenlet # 获取协程信息
  get_indent = greenlet.getcurrent
 except Exception as e:
  get_indent = threading.get_ident
 class local(object):
  # DIC={}
  def __init__(self):
   # pass
   object.__setattr__(self,'DIC',{}) # 通过父类的方法设置类属性
  def __getattr__(self,item):
   indent = get_indent()
   if indent in self.DIC:
    return self.DIC[indent].get(item)
   else:
   	return None
    
  def __setattr__(self,key,value):
   indent = get_indent()
   if indent in self.DIC:
    self.DIC[indent][key] = value
   else:
    self.DIC[indent]= {key:value}
    
  obj = local() # 类在实例化的时候运行__init__()方法
  '''
   obj.xx # 对象.方法的时候运行__getattr__()方法，并且把xx当参数传入
   obj.xx = 123 # 对象.方法赋值的时候运行__setattr__()方法，并且把xx和123当参数传入
  '''
  def task(i):
   obj.xxx = i
   time.sleep(2)
   print(obj.xxx,i)
  for i in range(10):
   t = threading.Thread(target=task,args=(i,)) # 创建线程
   t.start() # 开始执行线程

```
