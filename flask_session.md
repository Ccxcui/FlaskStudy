**将session写入redis**
```python
import redis # 连接redis模块
from flask import Flask,request,session
from flask_session import Session # 将session数据存入数据库

app = Flask(__name__)

app.config['SESSION_TYPE'] = 'redis'
 # 连接到redis数据库
app.config['SESSION_REDIS'] = redis.Redis(host='',db='',port='',password='',encoding=''，)
Session(app)

@app.route('/')
def index():
    return '首页'
    
if __name__ == '__main__':
    app.run()
