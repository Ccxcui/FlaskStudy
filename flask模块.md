**werkzurg**
```python

 # 引用werkzurg的功能模块
from werkzeug.wrappers import Request,Response
from werkzeug.serving import run_simple

 # 底层的用法
def run(environ,start_response):
 return [b'abcdefg'] 
 
if __name__ == '__main__':
 run_simple('localhost',4000,run) # 监听端口并执行run函数	
 
 # 另一种用法
@Response.application
def hello(request):
 return Response('Hello World!')
 
 '''
 	return到底返回到客户端什么内容？
 	1、状态码：status code 200,301,404，返回一个响应的客户端的状态码；
 	2、content-type：告诉客户端用什么方式解析返回的值；
 	3、其他的返回头信息。
 '''
 
if __name__ == __main__:
 run_simple('localhost',4000,hello)

vv oioi
