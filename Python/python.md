# 运行 shell 命令

## os.system()

```python
os.system('shell')
======
返回状态码(0,1,2)
```

## os.popen()

###  从一个命令打开一个管道

```python
os.popen('shell')
======
返回一个含有read方法的对象（异步）
```

### 获取执行后结果

```python
os.popen('shell').read()
```

# 管道

```python
import subprocess as sp
command = ['ffmpeg', '-y', '-f', 'rawvideo', '-vcodec', 'rawvideo', '-pix_fmt', 'bgr24', '-s', '1920*1080', '-r', '30', '-i',
           '-', '-c:v', 'h264', '-preset:v', 'ultrafast', '-tune:v', 'zerolatency', '-f', 'flv', '-r', '30', 'rtmp://127.0.0.1:5002/live']
p = sp.Popen(command, stdin=sp.PIPE)
p.stdin.write(frame.tobytes())
```

# 语法

## 三元表达式

```python
1.===========
erroStr = "More" if a > b else "Less"
print(erroStr) # 运行结果为：Less
2.============
print({True: "More", False: "Less"}[a > b]) # 运行结果为：Less
3.============
print(("FalseValue", "TrueValue")[a > b]) # 运行结果为：FalseValue
```

## 跳出两层循环

```python
for i in range(5):
    for j in range(10):
        if j > 7:
            break
	else:
        continue
	break
```

-  在内层中如果不是`break`，则外层循环走`else: continue`；如果内层`break`跳出后，外层循环就不走`else: continue`了，而是走的`break`，这样就跳出了两层循环。所以这个本质上还是一层一层的跳出循环。

# 文件操作

## 写入一行

```python
with open('txt.txt', 'a') as f:
    f.writelines('line_ string\n')
f.close()
```

## 新建并写入一行

```python
with open('txt.txt', 'w') as f:
    f.writelines('line_ string\n')
f.close()
```



# 环境变量

## 读取

```python
os.environ.get('BIAN_LIANG_MING', '默认值')
```

## 设置

```python
os.environ.set('BIAN_LIANG_MING', '值')
```

# 日志

## loguru

```python
from loguru import logger
```

### 打印到控制台

```python
logger.debug("debug message")
logger.info("info level message")
logger.warning("warning level message")
logger.critical("critical level message")
```

![loguru](https://img.jbzj.com/file_images/article/202202/202202160856239.png)

### 写入文件

```python
logger.add('log.log')
logger.info("info level message")
```

### 其他操作

```python
logger.add('log.log', rotation='3 day', encoding='utf-8', enqueue=True, retention='7 days', filter=lambda record: record["extra"]["name"] == "name")
logger_name = logger.bind(name="name")
logger_name.info("info level message")
===========
logger.add("runtime_{time}.log", rotation="500 MB")    # 文件大于500M,重新生成一个文件
logger.add("runtime_{time}.log", rotation="00:00")     # 0点创建新文件
logger.add("runtime_{time}.log", rotation="1 week")    # 超过一周则创建新文件
logger.add("test_4.log", retention="5 days")  # 只保留最近五天的日志文件
logger.add("test_5.log", compression="zip")    # 以zip格式对日志进行保存
```

# pip

## 安装

```shell
pip install packagename
```

## 安装 requirement

```shell
pip install -r requirements.txt
```

## 指定 python 版本

```shell
python3 -m pip install -r requirements.txt
```

# 多线程

```python
from threading import Thread
def asyncs(f):
    def wrapper(*args, **kwargs):
        thr = Thread(target=f, args=args, kwargs=kwargs)
        thr.start()

    return wrapper
@asyncs
def fun():
    print('fun')
```

# 多进程

```python
import multiprocessing
def fun(str):
    print(str)
p = multiprocessing.Process(target=fun, args=('fun'))
p.start()
```

# Flask

## 非测试环境

```python
from wsgiref.simple_server import make_server
server = make_server('0.0.0.0', 5000, app)
server.serve_forever()
```

## Flask-sockets

```python
from flask_sockets import Sockets
from gevent import pywsgi
from geventwebsocket.handler import WebSocketHandler

app = Flask(__name__)
sockets = Sockets(app)


# socket 路由，访问url是： ws://localhost:5000/echo
@sockets.route('/echo')
def echo_socket(ws):
    while not ws.closed:
        message = ws.receive()
        ws.send("come from web server: " + str(message))
        
        
if __name__ == "__main__":
    server = pywsgi.WSGIServer(('', 5000), app, handler_class=WebSocketHandler)
    print("web server start ... ")
    server.serve_forever()
```

# websocket-client

```python
pip install websocket-client
------------------
from websocket import create_connection

# 通过socket路由访问
ws = create_connection("ws://url:port/route/")
ws.send("Hello, ws")
result = ws.recv()
print(result)
ws.close()
```

## 长连接+心跳

```python
import websocket


def on_message(ws, message):
    print('socket message: {}'.format(message))


def on_data(ws, data, opcode, is_masked):
    print('socket data: {}'.format(data))


def on_error(ws, error):
    print('socket error: ' + str(error))


def on_close(ws, close_status_code, close_msg):
    print('socket close, close_status_code: ' + str(close_status_code) + ', close_msg: ' + str(close_msg))


def on_open(ws):
    print('socket connected')


if __name__ == "__main__":
    websocket.enableTrace(True)
    ws = websocket.WebSocketApp(env_websocket_url + env_device_id, on_message=on_message, on_error=on_error,							on_close=on_close, on_open=on_open, on_data=on_data)
    ws.on_open = on_open
    ws.run_forever(ping_interval=3, ping_timeout=2)
```

