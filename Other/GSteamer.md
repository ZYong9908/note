# GSteamer

- mpph264enc 为硬件解码，不支持需更换对应解码

## 测试

### 参数

- xxxsrc 输入
- xxxsink 输出

```shell
gst-launch-1.0 videotestsrc name=a videotestsink name=b a.src ! b.sink
```

## 摄像头录视频

### 参数

- video/x-raw ，image/jpeg 调用格式 (YUYV , MJPG)
- format 色彩格式 (仅YUYV)
- framerate 帧率
- mpph264enc 硬件解码

### YUYV

```shell
gst-launch-1.0 v4l2src device=/dev/video0 ! videoconvert ! video/x-raw,format=NV12,width=1920,height=1080,framerate=30/1 ! queue ! mpph264enc ! queue ! h264parse ! mpegtsmux ! filesink location=/path/out.mp4
```

### MJPG

```shell
gst-launch-1.0 v4l2src device=/dev/video0 ! image/jpeg,width=1920,height=1080,framerate=30/1 ! jpegdec ! videoconvert ! queue ! mpph264enc ! queue ! h264parse ! mpegtsmux ! filesink location=/path/out.mp4
```

### 解决 MJPG 颜色异常

```shell
gst-launch-1.0 v4l2src device=/dev/video0 ! image/jpeg,width=1920,height=1080,framerate=30/1 ! jpegdec ! videoconvert ! video/x-raw,format=NV12 ! queue ! mpph264enc ! queue ! h264parse ! mpegtsmux ! filesink location=/path/out.mp4
```



## 推流

```shell
gst-launch-1.0 v4l2src device=/dev/video0 ! image/jpeg,width=1920,height=1080,framerate=25/1 ! jpegdec ! videoconvert ! queue ! mpph264enc ! queue ! h264parse ! flvmux ! rtmpsink location='rtmp://127.0.0.1:5002/live'
```

## 水印

### 静态水印

```shell
gst-launch-1.0 v4l2src device=/dev/video0 ! image/jpeg,width=1920,height=1080,framerate=25/1 ! jpegdec ! textoverlay text="静态水印" ! videoconvert ! queue ! mpph264enc ! queue ! h264parse ! flvmux ! rtmpsink location='rtmp://127.0.0.1:5002/live'
================
gst-launch-1.0 v4l2src device=/dev/video0 ! image/jpeg,width=1920,height=1080,framerate=25/1 ! jpegdec ! textoverlay text="<span foreground=\"blue\" background=\"green\" size=\"x-large\"> TEST </span> is <i>cool</i>"  ! videoconvert ! queue ! mpph264enc ! queue ! h264parse ! flvmux ! rtmpsink location='rtmp://127.0.0.1:5002/live'
```

### 动态时间水印

- `font-desc` 字体大小
- `shaded-background` 背景
- valignment 位置（上下）
- halignment 位置（左右）

-------------------------

​	`font-desc`和`shaded-background`会占用cpu，低性能设备上会影响帧率

```shell
gst-launch-1.0 v4l2src device=/dev/video0 ! image/jpeg,width=1920,height=1080,framerate=30/1 ! jpegdec ! clockoverlay halignment=right valignment=top shaded-background=false font-desc='8' time-format="%Y-%m-%d %H:%M:%S" ! videoconvert ! video/x-raw,format=NV12,width=1920,height=1080,framerate=30/1 ! queue ! mpph264enc ! queue ! h264parse ! flvmux ! rtmpsink location='rtmp://127.0.0.1:5002/live'
```

## 多通道

```shell
gst-launch-1.0 -e v4l2src device=/dev/video0 ! image/jpeg,width=1280,height=960,framerate=30/1 ! jpegdec ! videoconvert ! video/x-raw,format=NV12,width=1280,height=960,framerate=30/1 ! queue ! mpph264enc ! queue ! h264parse ! tee name=t t. ! queue ! flvmux ! rtmpsink location='rtmp://127.0.0.1:5002/live' t. ! queue ! splitmuxsink location=/path/out.mp4
```

## 按时长录制视频 (动态命名)

```shell
gst-launch-1.0 -e v4l2src device=/dev/video0 ! image/jpeg,width=1280,height=960,framerate=30/1 ! jpegdec ! videoconvert ! video/x-raw,format=NV12,width=1280,height=960,framerate=30/1 ! queue ! mpph264enc ! queue ! h264parse ! queue ! splitmuxsink location=/psth/out_%d.mp4 max-size-time=11000000000 async-finalize=TRUE
================
out_0.mp4
out_1.mp4
out_2.mp4
```

### 参数

- max-size-time 秒数*1000000000

## 爱称拍-推流并按时长录制

```shell
gst-launch-1.0 -e v4l2src device=/dev/video0 ! image/jpeg,width=1920,height=1080,framerate=25/1 ! jpegdec ! clockoverlay halignment=right shaded-background=false font-desc='8' time-format="%Y-%m-%d %H:%M:%S" ! videoconvert ! video/x-raw,format=NV12 ! queue ! mpph264enc ! queue ! h264parse ! tee name=t t. ! queue ! splitmuxsink location=/media/ztl/56BF-01281/Videos/out_%d.mp4 max-size-time=11000000000 async-finalize=TRUE t. ! queue ! flvmux ! rtmpsink location='rtmp://127.0.0.1:5002/live'
```
