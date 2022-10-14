# Python 和 RESTful API 

- 更新时间：2022-05-05
- 版本号：1.0.0

- 版本更新说明：
  - 1.0.0：学习和总结RESTful API接口的功能以及特性，还有写REST接口的规范和注意事项。
### REST架构
REST代表表示性状态传输，是一种软件架构风格，它定义了客户端和服务器通过网络进行通信的模式。REST 为软件架构提供了一组约束，以提高系统的性能、可伸缩性、简单性和可靠性。

REST 定义了以下架构约束：

- **无状态**：服务器不会在来自客户端的请求之间维护任何状态。
- **客户端-服务器**：客户端和服务器必须相互解耦，允许各自独立开发。
- **可缓存**：从服务器检索到的数据应该可以被客户端或服务器缓存。
- **统一接口**：服务器将提供一个统一的接口来访问资源而不定义它们的表示。
- **分层系统**：客户端可以通过代理或负载均衡器等其他层间接访问服务器上的资源。
- **按需代码（可选）**：服务器可以将代码传输到它可以运行的客户端，例如用于单页应用程序的JavaScript 。
>> 请注意，REST不是规范，而是一组关于如何构建网络连接软件系统的指南。

### HTTP方法
REST API 侦听HTTP 方法，例如GET、POST，并DELETE了解要对 Web 服务的资源执行哪些操作。资源是 Web 服务中可用的任何数据，可以通过对 REST API 的 HTTP 请求进行访问和操作。HTTP 方法告诉 API 对资源执行哪个操作。

虽然有许多 HTTP 方法，但下面列出的五种方法是最常用于 REST API 的：

| HTTP方法 | 描述               |
| -------- | ------------------ |
| GET      | 检索现有资源。     |
| POST     | 创建新资源。       |
| PUT      | 更新现有资源。     |
| PATCH    | 部分更新现有资源。 |
| DELETE   | 删除资源。         |

### 状态码
一旦 REST API 接收并处理一个 HTTP 请求，它将返回一个HTTP 响应。此响应中包含一个HTTP 状态代码。此代码提供有关请求结果的信息。向 API 发送请求的应用程序可以检查状态代码并根据结果执行操作。这些操作可能包括处理错误或向用户显示成功消息。

以下是 REST API 返回的最常见状态代码的列表：

| 代码 | 意义             | 描述                                     |
| ---- | ---------------- | ---------------------------------------- |
| 200  | 好的             | 请求的操作成功。                         |
| 201  | 已创建           | 创建了一个新资源。                       |
| 202  | 公认             | 请求已收到，但尚未进行任何修改。         |
| 204  | 无内容           | 请求成功，但响应没有内容。               |
| 400  | 错误的请求       | 请求格式不正确。                         |
| 401  | 未经授权         | 客户端无权执行请求的操作。               |
| 404  | 未找到           | 未找到请求的资源。                       |
| 415  | 不支持的媒体类型 | 服务器不支持请求数据格式。               |
| 422  | 无法处理的实体   | 请求数据格式正确，但包含无效或缺失数据。 |
| 500  | 内部服务器错误   | 服务器在处理请求时抛出错误。             |

状态码范围：

| 代码范围 | 类别       |
| -------- | ---------- |
| 2xx      | 成功运行   |
| 3xx      | 重定向     |
| 4xx      | 客户端错误 |
| 5xx      | 服务器错误 |

### Python的Flask框架开发RESTful API
安装flask
```
pip install flask
```

[helloworld](helloworld.py)
```python
from flask import Flask
from flask import request

app = Flask(__name__)

@app.route('/', methods=['GET', 'POST'])
def home():
    return '<h1>hello world</h1>'



if __name__ == '__main__':
    app.run()
```
运行```python app.py```，Flask自带的Server在端口5000上监听：

打开浏览器，输入首页地址http://localhost:5000/：

会出现hello world

![](https://s2.loli.net/2022/04/12/o6PtBF49ZeyAaRT.png)

简单的RESTful实现

[resrfultest](restfultest.py)
```python
#!/usr/bin/python
# -*- coding: utf-8 -*-
from flask import Flask, abort, request, jsonify

app = Flask(__name__)

# 测试数据暂时存放
tasks = []

@app.route('/add_task/', methods=['POST'])
def add_task():
    if not request.json or 'id' not in request.json or 'info' not in request.json:
        abort(400)
    task = {
        'id': request.json['id'],
        'info': request.json['info']
    }
    tasks.append(task)
    return jsonify({'result': 'success'})


@app.route('/get_task/', methods=['GET'])
def get_task():
    if not request.args or 'id' not in request.args:
        # 没有指定id则返回全部
        return jsonify(tasks)
    else:
        task_id = request.args['id']
        task = filter(lambda t: t['id'] == int(task_id), tasks)
        return jsonify(task) if task else jsonify({'result': 'not found'})


if __name__ == "__main__":
    # 将host设置为0.0.0.0，则外网用户也可以访问到这个服务
    app.run(host="0.0.0.0", port=8383, debug=True)
```

运行```./restfultest.py```

验证测试：使用apifox软件对接口的功能进行测试

使用POST方法在接口创建新的资源：
```
127.0.0.1 - - [12/Apr/2022 11:39:31] "POST /add_task/ HTTP/1.1" 200 -
```

![](https://s2.loli.net/2022/04/13/ogWR7QlGF62NEc8.png)

再使用GET方法检索接口的资源：
```
127.0.0.1 - - [12/Apr/2022 11:41:11] "GET /get_task/ HTTP/1.1" 200 -
```

![](https://s2.loli.net/2022/04/13/FXVHMnZOi9lmURy.png)

### 在flask实现token验证
[flasktoken](flasktoken.py)
```python
#!/usr/bin/python
from flask import Flask
from flask.globals import request
from flask.json import jsonify
from itsdangerous import TimedJSONWebSignatureSerializer as Serializer

app=Flask(__name__)
#私钥
app.config['SECRET_KEY']='LEMUTRESTFULTOKEN'
#忽略的检查的url
NOT_CHECK_URL=['/login']
#生产token
def create_token(api_user):
    '''
    生成token
    :param api_user:用户id
    :return: access_token
    '''
    
    #第一个参数是内部的私钥，这里写在共用的配置信息里了，如果只是测试可以写死
    #第二个参数是有效期(秒)
    s = Serializer(app.config["SECRET_KEY"],expires_in=3600)
    #接收用户id转换与编码
    token = s.dumps({"id":api_user}).decode("ascii")
    return token # 返回token

@app.before_request
def login_required():
    if request.path not in NOT_CHECK_URL:
        try:
            token = request.headers["token"]
        except:
            return jsonify(code = '33',mssage='不合法的access_token')
    

@app.route('/login',methods=["POST"])
def info():
    '''
    POST携带userid来换取access_token
    代码省略其他逻辑
    '''
    return jsonify({'token':create_token(api_user=request.args.getlist('user_id'))})

@app.route('/msg')
def index():
    '''
    携带access_token登陆
    '''
    return jsonify(code = '0',mssage='access_token验证通过！！！')

if __name__ == "__main__":
    # 将host设置为0.0.0.0，则外网用户也可以访问到这个服务
    app.run(host="0.0.0.0", port=8383, debug=True)
```
运行```./flasktoken.py```

使用apifox去```POST http://127.0.0.1:8383/login```,会返回access_token回来，复制token备用。

![](https://s2.loli.net/2022/04/13/fwukxvT4jaKzWrn.png)

再使用```GET http://127.0.0.1:8383/msg```，把复制好的token粘贴到请求头token参数中：

![](https://s2.loli.net/2022/04/14/Vo5TCO61YU2pvNZ.png)

不使用正确的token再GET一次

![](https://s2.loli.net/2022/04/14/43f2KFmwQHv7Odl.png)

### 查看项目文章：
Github 项目：
- [flask](https://github.com/pallets/flask)
- [flask-restful-example](https://github.com/qzq1111/flask-restful-example)
- [flask-restplus-server-example](https://github.com/frol/flask-restplus-server-example)
- [safrs](https://github.com/thomaxxl/safrs)
- [flask_rest_api](https://github.com/ericmbernier/ericbernier-blog-posts/tree/master/flask_rest_api)
- [Flask-Scaffold](https://github.com/Leo-G/Flask-Scaffold)

技术文章：
- [How To Write REST API With Python and Flask](https://medium.com/bb-tutorials-and-thoughts/how-to-write-rest-api-with-python-and-flask-71ab42d253c5)
- [How to Deploy a REST API with Flask, Fauna, and Authentication on Koyeb](https://www.koyeb.com/tutorials/how-to-deploy-a-rest-api-with-flask-fauna-and-authentication-on-koyeb)

### Flask项目的特点：
主代码模板：
```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello, World!"

if __name__ == "__main__":
    app.run() # 默认端口是5000
```
接口功能模板:
```python
import xxx

class API(xxx,xxx):

    def func1(x):
        return "code='0',msg='ok'"
    def func2(x):
        return "code='xx',msg='xx'"
    def func3(x):
        return "code='xx',msg='xx'"
    def func3(x):
        return "code='xx',msg='xx'"
```
配置文件使用yaml类型的:
```yaml
COMMON: &common #标识
  DEBUG: False
  SECRET_KEY: insecure

  # 数据库连接
  SQLALCHEMY_DATABASE_URI: 'mysql+pymysql://root:mad123@localhost:3306/test?charset=utf8mb4'
  SQLALCHEMY_TRACK_MODIFICATIONS: False
  # 日志配置文件路径
  LOGGING_CONFIG_PATH: ./config/logging.yaml
  # 日志文件存放位置
  LOGGING_PATH: ./logs

  # 响应消息
  RESPONSE_MESSAGE: ./config/msg.yaml
```

### pytest拓展
1. 参考了[Tests for Requests](https://github.com/psf/requests/blob/main/tests/test_requests.py)例程的测试类的写法，稍微修改了钉钉通信机器人的测试脚本[lemumsg_test](https://gitlab.com/lemu-tech/devops/-/commit/53e62f59843ad0a50184d5e0cb59e083f05ff9b8)。
2. 在Github开源的项目中[Awesome pytest](https://github.com/augustogoulart/awesome-pytest),有很多pytest优秀的文章、测试范例和pytest测试框架的插件。
- [pytest-repeat](https://github.com/pytest-dev/pytest-repeat): pytest 插件，可以轻松地重复单个测试或多个测试，特定次数。
- [pytest-randomly](https://github.com/pytest-dev/pytest-randomly):用于随机排序测试和控制的 Pytest 插件random.seed。通过随机排序测试，降低了令人惊讶的测试间依赖关系的风险。
- [pytest-clarity](https://github.com/darrenburns/pytest-clarity):一个提高 pytest 输出可读性的插件
- [pytest-reportlog](https://github.com/pytest-dev/pytest-reportlog):在测试会话执行时将日志报告到文件中。
- [pytest-profiling](https://github.com/man-group/pytest-plugins/tree/master/pytest-profiling):pytest 的分析插件，具有表格和热图输出
- [pytest_profiles](https://github.com/stefanhoelzl/pytest_profiles):允许您为 pytest 创建配置文件。