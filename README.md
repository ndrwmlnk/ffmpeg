# ffmpeg
Collection of ffmpeg commands

##Video speed up / slow down ([source](https://www.bogotobogo.com/FFMpeg/ffmpeg_video_speed_up_slow_down.php))

`ffmpeg -i video.mp4 -vf  "setpts=2*PTS" video_2x.mp4`

`ffmpeg -i video.mp4 -vf  "setpts=0.5*PTS" video_0.5x.mp4`