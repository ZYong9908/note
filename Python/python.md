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

# 网络请求

## post

```shell
pip  install request
```

```python
import request
import json
data = json.dumps({'data':'123'})
r = requests.post(AUTO_CAP_URL, data=data)
-------
r.status_code   状态码
r.text    返回内容
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

# supervisor

```shell
yum install supervisor
apt-get install supervisor
pip install supervisor
easy_install supervisor
```

##### supervisor配置文件：

`/etc/supervisord.conf`

##### 子进程配置文件路径：

`/etc/supervisord.d/` or `/etc/supervisord/conf.d/`

默认子进程配置文件为ini格式，可在supervisor主配置文件中修改

### 配置文件

#### supervisor.conf配置文件说明：

```ini
[unix_http_server]
file=/tmp/supervisor.sock   ;UNIX socket 文件，supervisorctl 会使用
;chmod=0700                 ;socket文件的mode，默认是0700
;chown=nobody:nogroup       ;socket文件的owner，格式：uid:gid
;[inet_http_server]         ;HTTP服务器，提供web管理界面
;port=127.0.0.1:9001        ;Web管理后台运行的IP和端口，如果开放到公网，需要注意安全性
;username=user              ;登录管理后台的用户名
;password=123               ;登录管理后台的密码
[supervisord]
logfile=/tmp/supervisord.log ;日志文件，默认是 $CWD/supervisord.log
logfile_maxbytes=50MB        ;日志文件大小，超出会rotate，默认 50MB，如果设成0，表示不限制大小
logfile_backups=10           ;日志文件保留备份数量默认10，设为0表示不备份
loglevel=info                ;日志级别，默认info，其它: debug,warn,trace
pidfile=/tmp/supervisord.pid ;pid 文件
nodaemon=false               ;是否在前台启动，默认是false，即以 daemon 的方式启动
minfds=1024                  ;可以打开的文件描述符的最小值，默认 1024
minprocs=200                 ;可以打开的进程数的最小值，默认 200
[supervisorctl]
serverurl=unix:///tmp/supervisor.sock ;通过UNIX socket连接supervisord，路径与unix_http_server部分的file一致
;serverurl=http://127.0.0.1:9001 ; 通过HTTP的方式连接supervisord
; [program:xx]是被管理的进程配置参数，xx是进程的名称
[program:xx]
command=/opt/apache-tomcat-8.0.35/bin/catalina.sh run  ; 程序启动命令
autostart=true       ; 在supervisord启动的时候也自动启动
startsecs=10         ; 启动10秒后没有异常退出，就表示进程正常启动了，默认为1秒
autorestart=true     ; 程序退出后自动重启,可选值：[unexpected,true,false]，默认为unexpected，表示进程意外杀死后才重启
startretries=3       ; 启动失败自动重试次数，默认是3
user=tomcat          ; 用哪个用户启动进程，默认是root
priority=999         ; 进程启动优先级，默认999，值小的优先启动
redirect_stderr=true ; 把stderr重定向到stdout，默认false
stdout_logfile_maxbytes=20MB  ; stdout 日志文件大小，默认50MB
stdout_logfile_backups = 20   ; stdout 日志文件备份数，默认是10
; stdout 日志文件，需要注意当指定目录不存在时无法正常启动，所以需要手动创建目录（supervisord 会自动创建日志文件）
stdout_logfile=/opt/apache-tomcat-8.0.35/logs/catalina.out
stopasgroup=false     ;默认为false,进程被杀死时，是否向这个进程组发送stop信号，包括子进程
killasgroup=false     ;默认为false，向进程组发送kill信号，包括子进程
;包含其它配置文件
[include]
files = relative/directory/*.ini    ;可以指定一个或多个以.ini结束的配置文件
```

#### 子进程配置文件说明：

```ini
#项目名
[program:blog]
#脚本目录
directory=/opt/bin
#脚本执行命令
command=/usr/bin/python /opt/bin/test.py

#supervisor启动的时候是否随着同时启动，默认True
autostart=true
#当程序exit的时候，这个program不会自动重启,默认unexpected，设置子进程挂掉后自动重启的情况，有三个选项，false,unexpected和true。如果为false的时候，无论什么情况下，都不会被重新启动，如果为unexpected，只有当进程的退出码不在下面的exitcodes里面定义的
autorestart=false
#这个选项是子进程启动多少秒之后，此时状态如果是running，则我们认为启动成功了。默认值为1
startsecs=1

#脚本运行的用户身份 
user = test

#日志输出 
stderr_logfile=/tmp/blog_stderr.log 
stdout_logfile=/tmp/blog_stdout.log 
#把stderr重定向到stdout，默认 false
redirect_stderr = true
#stdout日志文件大小，默认 50MB
stdout_logfile_maxbytes = 20MB
#stdout日志文件备份数
stdout_logfile_backups = 20
```

### 常用命令

```shell
supervisorctl status        //查看所有进程的状态
supervisorctl stop es       //停止es
supervisorctl start es      //启动es
supervisorctl restart       //重启es
supervisorctl update        //配置文件修改后使用该命令加载新的配置
supervisorctl reload        //重新启动配置中的所有程序
```

### 注意事项

使用supervisor进程管理命令之前先启动supervisord，否则程序报错

# config.ini配置文件

```ini
[info]
aaa = 1
bbb = https://zyong.vip

[config]
name = ZYong
```

```python
import configparser

base_path = '/path/config.ini'
config = configparser.ConfigParser()
config.read(base_path, encoding='utf-8')

def get_config(key):
    return config.get('info', key)

def set_config(key, value):
    config.set('info', key, value)
    config.write(open(base_path, 'w', encoding='utf-8'))
```

# 监听键盘

```shell
pip install pynput
```

```python
from pynput import keyboard
def on_press(key):
    print(key)
if __name__ == "__main__":
    with keyboard.Listener(on_press=on_press) as lsn:
        lsn.join()
```

```shell
linux:
sudo apt-get install python3-tk
sudo apt install tk-dev
```

# 函数超时

```python
pip install func_timeout
---------------
import func_timeout
func_timeout.func_timeout(10, function_name)
-------或---------
@func_timeout.func_set_timeout(10)
def function_name():
    pass
```

## 超时处理

```python
import func_timeout
try:
    func_timeout.func_timeout(10, function_name)
except func_timeout.exceptions.FunctionTimedOut as e:
    pass
```

