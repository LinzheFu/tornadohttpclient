TornadoHTTPClient 是一个基于Tornado的高效的异步HTTP客户端库, 支持Cookie和代理

## 安装
```bash
python setup.py install
```

## 教程
### GET
TornadoHTTPClient的get方法可以发起一个get请求
```python
from tornadohttpclient import TornadoHTTPClient

# 实例化
http = TornadoHTTPClient()

# 发出get请求
http.get("http://www.linuxzen.com")

# 开始主事件循环
http.start()
```

### POST
TornadoHTTPClient的post方法可以发起一个post请求

### 读取响应
上面仅仅发出了请求, 但是我们无法读取GET请求回来的数据, 我们可以使用一个回调来读取响应
```python
from tornadohttpclient import TornadoHTTPClient

http = TornadoHTTPClient()

def callback(response):
    print response.read()
    http.stop()

http.get("http://www.linuxzen.com", callback = callback)
http.start()
```

通过`callback`关键字参数我们可以传进一个回调函数, 当请求成功时会调用此函数, 并给此函数传递一个与`urllib2.urlopen`返回一样的reponse实例

### 给请求传递参数
TornadoHTTPClient 的 `get`/`post`方法的第二个参数`params`可以定义请求时传递的参数`params`的类型为字典 后者`((key, value), )`类型的元组或列表,例如使用百度搜索`TornadoHTTPClient`
```python
from tornadohttpclient import TornadoHTTPClient

http = TornadoHTTPClient()

def callback(response):
    print response.read()
    http.stop()

http.get("http://www.baidu.com/s", (("wd", "tornado"),), callback = callback)
http.start()
```

以上也使用与POST方法, 比如登录网站
```python
from tornadohttpclient import TornadoHTTPClient

http = TornadoHTTPClient()

def callback(response):
    print response.read()
    http.stop()

http.post("http://ip.or.domain/login", (("username", "cold"), ("password", "pwd")), callback = callback)

http.start()
```


### 指定HTTP头
TornadoHTTPClient 的`get`/`post`方法的 `headers`关键字参数可以自定额外的HTTP头信息, 参数类型为一个字典

指定User-Agent头

```python
from tornadohttpclient import TornadoHTTPClient

http = TornadoHTTPClient()

def callback(response):
    print response.read()
    http.stop()

headers = dict((("User-Agent",
                "Mozilla/5.0 (X11; Linux x86_64)"\
                " AppleWebKit/537.11 (KHTML, like Gecko)"\
                " Chrome/23.0.1271.97 Safari/537.11"), ))

http.get("http://www.linuxzen.com", headers=headers, callback = callback)
```

### 使用代理
TornadoHTTPClient 的`set_proxy`方法可以设置代理, 其接受两个参数, 分别是代理的 主机名/ip 代理的端口, `unset_proxy`可以取消代理
```python
from tornadohttpclient import TornadoHTTPClient

http = TornadoHTTPClient()

def callback(response):
    print response.read()
    http.unset_proxy()
    http.stop()

http.set_proxy("127.0.0.1", 8087)
http.get("http://shell.appspot.com", callback = callback)
http.start()
```