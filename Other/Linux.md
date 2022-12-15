# 常用

## 屏蔽 shell 输出

```shell
shell cmd  > /dev/null 2>&1
```

## 下载

```shell
wget https://down.con/down.tar.gz
```

## 查看磁盘使用

```shell
df -h
```

## 重命名

```shell
mv name1.txt name2.txt
```

## 查看cpu Serial

```shell
cat /proc/cpuinfo
```



# 解压缩

## 压缩

```shell
#仅打包，不压缩
tar -cvf test.tar test
# 打包后，以gzip压缩
tar -zcvf test.tar.gz test
# 将目录里所有jpg文件打包成jpg.tar后，并且将其用bzip2压缩，生成一个bzip2压缩过的包，命名为jpg.tar.bz2
tar –cjf jpg.tar.bz2 *.jpg 
```

## 解压

```shell
#解压 tar包
tar –xvf file.tar
#解压tar.gz
tar -xzvf file.tar.gz
#解压 tar.bz2
tar -xjvf file.tar.bz2   
#把当前目录下的222.tar.gz解压到ee/下，前提要保证存在ee这个目录
tar -zxvf 222.tar.gz -C ee/
```

## 其它

```shell
#将所有.gif的文件增加到all.tar的包里面去。-r 增加文件的
tar -rf all.tar *.gif 
#更新原来tar包all.tar中logo.gif文件，-u 是表示更新文件
tar -uf all.tar logo.gif 
#列出all.tar包中所有文件，-t 是列出文件
tar -tf all.tar 
```

### bz2

- 解压1：
  
  ```shell
  bzip2 -d filename.bz2
  ```
  
- 解压2：
  
  ```shell
  bunzip2 filename.bz2
  ```
  
- 压缩：
  
  ```shell
  bzip2 -z filename
  ```

### tar.bz2

- 解压：

  ```shell
  tar jxvf filename.tar.bz2
  ```

- 压缩：

  ```shell
  tar jcvf filename.tar.bz2 dirname
  ```

### bz

- 解压1：
  
  ```shell
  bzip2 -d filename.bz
  ```
  
- 解压2：
  
  ```shell
  bunzip2 filename.bz
  ```

### tar.bz

- 解压：
  
  ```shell
  tar jxvf filename.tar.bz
  ```

### zip

- 解压：
  
  ```shell
  unzip filename.zip
  ```
- 压缩：
  
  ```shell
  zip filename.zip dirname
  ```

# 编译

## cmake 参数

```shell
cmake .. -D CMAKE_BUILD_TYPE=RELEASE -D PENCV_EXTRA_MODULES_PATH=/opencv_contrib-4.6.0/modules -D -D BUILD_opencv_python3=ON -D BUILD_opencv_python2=OFF -D PYTHON3_EXECUTABLE=/usr/bin/python3.8 -D PYTHON3_INCLUDE_DIR=/usr/include/python3.8 -D PYTHON3_LIBRARY=/usr/lib/x86_64-linux-gnu/libpython3.8m.so -D PYTHON3_NUMPY_INCLUDE_DIRS=/python3.8/site-packages/numpy/core/include -D PYTHON3_PACKAGES_PATH=/python3.8/site-packages
=============
.. 上层路径
-D 后空格是否保留
```

# v4l2 (Video for linux2)

## 查看指定相机的输出图像格式及对应分辨率

```shell
v4l2-ctl --list-formats-ext -d /dev/video0
```

# 蓝牙

## pybluze

```shell
sudo apt-get install libbluetooth-dev
```

```shell
pip install pybluze
```

# 网络

## 查看状态

### ifconfig

- 最常用的配置和查看网络接口信息的命令，服务器上执行此命令会得到类下文的内容，一下内容可看到多个设备和设备状态、信息。

### 查看无线网络状态

- iwconfig

## 热点 create_ap

- https://github.com/oblique/create_ap

```shell
create_ap -n wlan0 nova-car-wifi nova123456 -g 192.168.12.1 --no-virt
create_ap --stop nova-car-wifi
```

## 连接 wifi

```shell
sudo nmcli dev wifi connect "DESKTOP-84VRIRM 8964" password "12345678" ifname wlan0
```

## HOST

```shell
sudo vim /etc/hosts
```

# 声音

## 查看声卡

```shell
cat /proc/asound/cards
```

## 得到当前系统可用麦克风的设备名称

```shell
arecord -l
----------
arecord -l | awk -F \: '/,/{print $2}' | awk '{print $1}' | uniq
```

## 得到当前系统可用扬声器的设备名称

```shell
cat /proc/asound/cards
```



## 设置默认声卡

- /etc/asound.conf

```shell
sudo vim /etc/asound.conf
```

```
pcm.!default {
type hw
card rockchiprk809co
}

ctl.!default {
type hw
card rockchiprk809co
}
----------或------------
defaults.ctl.card 1
defaults.pcm.card 1
defaults.timer.card 1
```

