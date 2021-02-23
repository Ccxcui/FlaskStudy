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
