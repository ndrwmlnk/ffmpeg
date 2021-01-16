# ffmpeg
Collection of ffmpeg commands

## Video speed up / slow down ([source](https://www.bogotobogo.com/FFMpeg/ffmpeg_video_speed_up_slow_down.php))
also reduces size of the video (up to 10 times)

`ffmpeg -i video.mp4 -vf  "setpts=0.5*PTS" video_speed_up.mp4`

`ffmpeg -i video.mp4 -vf  "setpts=2*PTS" video_slow_down.mp4`  


## Convert a video file into gif (for GitHub)
`ffmpeg -i video.mp4 video.gif`

## Apply an ffmpeg command to all .mov files in a directory ([source](https://stackoverflow.com/questions/5784661/how-do-you-convert-an-entire-directory-with-ffmpeg))
`for i in *.mov; do ffmpeg -i "$i" -vf  "setpts=0.5*PTS" "${i%.*}.mp4"; done`