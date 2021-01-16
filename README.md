# ffmpeg
Collection of ffmpeg commands

## Video speed up / slow down ([source](https://www.bogotobogo.com/FFMpeg/ffmpeg_video_speed_up_slow_down.php))
reduces size of the video (up to 10 times)

speed up:

`ffmpeg -i video.mp4 -vf  "setpts=0.5*PTS" video_0.5x.mp4` 

slow down: 

`ffmpeg -i video.mp4 -vf  "setpts=2*PTS" video_2x.mp4`  

