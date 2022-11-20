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

