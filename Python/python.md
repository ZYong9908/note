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
返回一个含有read方法的对象
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

## 安装requirement

```shell
pip install -r requirements.txt
```

## 指定python版本

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



