```python
#代码示例，仅仅是为了测试request的属性值
@app.route('/login', methods = ['GET','POST'])
def login():
    if request.method == 'POST':
        if request.form['username'] == request.form['password']:
            return 'TRUE'
        else:
#当form中的两个字段内容不一致时，返回我们所需要的测试信息
            return str(request.headers)        #需要替换的部分
    else:
        return render_template('login.html')
```
1. method：请求的方法
>return request.method        #POST
2. form：返回form的内容
>return json.dumps(request.form)        #{"username": "123", "password": "1234"}
3. args和values：args返回请求中的参数，values返回请求中的参数和form
```
return json.dumps(request.args)       
#url：http://192.168.1.183:5000/login?a=1&b=2、返回值：{"a": "1", "b": "2"}
print(request.args['a'])
#输出：1
return str(request.values)        
#CombinedMultiDict([ImmutableMultiDict([('a', '1'), ('b', '2')]), ImmutableMultiDict([('username', '123'), ('password', '1234')])])
```
4、cookies：cookies信息
>return json.dumps(request.cookies)        #cookies信息
5、headers：请求headers信息，返回的结果是个list
```
return str(request.headers)        #headers信息
request.headers.get('User-Agent')        #获取User-Agent信息
```
6、url、path、script_root、base_url、url_root：看结果比较直观
```
return 'url: %s , script_root: %s , path: %s , base_url: %s , url_root : %s' % (request.url,request.script_root, request.path,request.base_url,request.url_root)
'''
url: http://192.168.1.183:5000/testrequest?a&b , 
script_root: , 
path: /testrequest , 
base_url: http://192.168.1.183:5000/testrequest , 
url_root : http://192.168.1.183:5000/
'''


```
7、date、files：date是请求的数据，files随请求上传的文件
```
@app.route('/upload',methods=['GET','POST'])
def upload():
    if request.method == 'POST':
        f = request.files['file']
        filename = secure_filename(f.filename)
        #f.save(os.path.join('app/static',filename))
        f.save('app/static/'+str(filename))
        return 'ok'
    else:
        return render_template('upload.html')
 
#html
<!DOCTYPE html>
<html>
    <body>
        <form action="upload" method="post" enctype="multipart/form-data">
            <input type="file" name="file" /><br />
            <input type="submit" value="Upload" />
        </form>
    </body>
</html>
```

