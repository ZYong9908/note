# 硬件设备
## 查看指定相机的输出图像格式及对应分辨率
- v4l2-ctl --list-formats-ext -d /dev/video0

## ffmpeg

### 合并

 ```shell
 ffmpeg -f concat -safe 0 -i merge.txt -c copy -r 30 out.mp4
 ====merge.txt
 ```





"fmpeg -ss " + str(start_s) + " -i " + env_video_path + "/merge/" + video_names + " -r 30 -t " + str(video_times) + " -vcodec copy -r 30 " + env_video_path + "/cut/" + video_names_cut

ffmpeg -f v4l2 -use_wallclock_as_timestamps true -input_format mjpeg -s 1920*1080 -r 30 -i /dev/video0 -vf "drawtext=expansion=strftime:text='%Y-%m-%d %H\\:%M\\:%S':x=w-tw:fontsize=50:fontcolor=white'" -r 30 -c:v:0 libx264 -t 30 000h264mjpeg.mp4 -c:v:1 libx264 -t 30 111h264mjpeg.mp4 

os.popen("ffprobe -v error -show_entries format=duration -of default=noprint_wrappers=1:nokey=1 " + env_video_path + "/merge/" + video_names).read()

 command = ['ffmpeg', '-v', 'error', '-y', '-f', 'rawvideo', '-vcodec', 'rawvideo', '-pix_fmt', 'bgr24', '-s', '1920*1080', '-r', '30', '-i', '-', '-c:v',
               'h264', '-preset:v', 'ultrafast', '-tune:v', 'zerolatency', '-vf', 'scale=1280:720', '-f', 'flv', '-r', '25', env_rtmp_url]
 p = sp.Popen(command, stdin=sp.PIPE)



