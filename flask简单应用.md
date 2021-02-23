### 简单的登录
```python
from flask import Flask as fk , request,render_template as render,redirect,session,make_response 

app = fk(__name__)

app.secret_key = 'abckd' # 设置session 的加密值

@app.route('/',methods=['GET','POST'])
def index():
 if request.method == 'POST':
  user = request.form.get('user')
  pwd = request.form['pwd']
  if user == 'admin' and pwd == '123':
   print(user,pwd)
   return '登录成功！'
 return render('index.html',h1='你好')
 # home.add_url_rule('/',view_func=index) #第二种路由的声明方式，必须已“/”开头
 
if __name__ == '__main__':
 app.run(debug=True,host='192.168.0.11',port=5000) 
 
'''
执行werkzurg中的run_simple('localhost',4000,hello)
这里只是socket进行了请求监听，当浏览器发起请求后
执行内部的__call__()方法
'''

```
### 静态文件的处理方法
```html
<!-- 使用静态文件需要删除 <!DOCTYPE html> -->
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="/static/index.css" type="text/css">
    <!-- javascript引用时必须添加type="text/javascript" -->
   <script language="javaacript" src="/statics/jquery.min.js" type="text/javascript"></script> 
    <!-- 推荐使用这样加载静态文件 -->
    <link rel="stylesheet" href="{{ url_for('static',filename='index.css') }}" type="text/css"> 
    <!-- url_for()，函数配置静态文件，一定要在Flask类中设置静态文件的路径和别名，蓝图中设置静态文件路径和别名是不能使用的 -->
</head>
<body>
<h1>index</h1>
<h1>{{ h1 }}</h1>
<form action="/" method="POST">
    <input type="text" name="user" >
    <input type="password" name="pwd">
    <input type="submit" value="提交">
</form>
</body>
</html>

```

使用 url_for 函数获取静态资源时，必须在实例化 Flask 对象的时候设置 static_url_path（别名），static_folder（静态文件路径）
```python
from flask import Flask
from .views.index import home
def shb():
    app = Flask(__name__,static_url_path='/static',static_folder='./public',template_folder='./template')
    '''
    PS：
    	1、static_folder的路径一定要设置正确。
    	2、static_url_path的值必须是’/static‘，其他测试无效。
    	3、url(’static‘,filename='静态文件路径') 第一个值必须是’static‘，其他测试无效。
    	4、template_folder 配置注意存放路径。
    	5、蓝图中就不再配置存放静态文件的路径和引用别名了；模板也不用再配置，这里是全局配置。
    '''
    app.register_blueprint(home,url_prefix="/api")
    return app
```
### 应用
```
 from flask import Flask,render_template,request,redirect,session
 app = Flask(__name__) # __name__ 可以修改为任意字符串，并可以传入多个参数
 app.secret_key = 'abckd' # 设置session加密多余字符串
 # 路由装饰器，必须已“/”开头
 @app.route('/login',methods=['GET','POST'])
 # 定义与路由装饰器匹配的执行函数
 def login():
	 print(request.method) # request 需要导入，获取请求的信息
	 session['key] = value # 设置session值
	 session.get('key') # 获取session的值
	 return render_template('login.html',**{key:value})
	#return render_template('login.html',key=value)
 if __name__ == '__main__':
  app.run(url,prot)
```

