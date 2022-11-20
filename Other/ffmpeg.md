# ffmpeg

## 常用参数

- -r 帧数
- -i 输入
- -c  -vcodec -c:v 编码格式(copy原格式、h264、libx264 ...)

## 合并视频

 ```shell
ffmpeg -f concat -safe 0 -i /path/merge.txt -c copy -r 30 /path/out.mp4
==== merge.txt ====
file 'input1.mp4'
file 'input2.mp4'
file 'input3.mp4'
 ```

## 裁剪视频

```shell
fmpeg -ss 10 -i input.mp4 -r 30 -t 20 -vcodec copy -r 30 out.mp4
================
-ss 开始秒数
-t 结束秒数
```

## 调用摄像头

```shell
ffmpeg -f v4l2 -s 1920*1080 -r 30 -i /dev/video0 -r 30 -c:v libx264 -t 30 out.mp4
==============
-s 分辨率
```



## 添加时间水印

```shell
ffmpeg -f v4l2 -use_wallclock_as_timestamps true -s 1920*1080 -r 30 -i /dev/video0 -vf "drawtext=expansion=strftime:text='%Y-%m-%d %H\\:%M\\:%S':x=w-tw:fontsize=50:fontcolor=white'" -r 30 -c:v libx264 -t 30 out.mp4
```

## 多通道

```shell
ffmpeg -f v4l2 -s 1920*1080 -r 30 -i /dev/video0 -r 30 -c:v:0 libx264 out0.mp4 -c:v:1 libx264 out1.mp4 
```

## 按时长录制视频

```shell
ffmpeg -f v4l2 -s 1920*1080 -r 30 -i /dev/video0 -r 30 -c:v:0 libx264 -t 30 out.mp4
================
-t 录制时长
```

## 调用摄像头格式

```shell
ffmpeg -f v4l2 -input_format mjpeg -s 1920*1080 -r 30 -i /dev/video0 -r 30 -c:v libx264 -t 30 out.mp4
=============
-input_format 调用格式（mjpeg、YUYV）
```

## 推送直播

```shell
ffmpeg -v error -y -f rawvideo -vcodec rawvideo -pix_fmt bgr24 -s 1920*1080 -r 30 -i input.mp4 -c:v h264 -preset:v ultrafast -tune:v zerolatency -vf scale=1280:720 -f flv -r 25 rtmp://127.0.0.1:5002/live
```



