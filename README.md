# ffmpeg
Collection of ffmpeg commands

## Video speed up / slow down ([source](https://www.bogotobogo.com/FFMpeg/ffmpeg_video_speed_up_slow_down.php))
also reduces size of the video (up to 10 times)

`ffmpeg -i video.mp4 -vf  "setpts=0.5*PTS" video_speed_up.mp4`

`ffmpeg -i video.mp4 -vf  "setpts=2*PTS" video_slow_down.mp4`  

## Resize a video ([source](https://superuser.com/questions/624563/how-to-resize-a-video-to-make-it-smaller-with-ffmpeg))
`ffmpeg -i input.avi -filter:v scale=720:-1 -c:a copy output.mkv`

`ffmpeg -i input.avi -vf scale=1280:-2 output.mp4`  width will be divisible by 2 ([source](https://stackoverflow.com/questions/20847674/ffmpeg-libx264-height-not-divisible-by-2))

## Convert a video file into gif e.g., for GitHub ([source](https://superuser.com/questions/556029/how-do-i-convert-a-video-to-gif-using-ffmpeg-with-reasonable-quality))
`ffmpeg -i video.mp4 video.gif`

`ffmpeg -ss 5 -t 3 -i video.mp4 -vf "fps=10,scale=320:-2" -loop -1 video.gif` # trim from 5th second of the video + 3 seconds, 10 FPS, scale to 320 width, keep height even, infinite loop


## Apply an ffmpeg command to all .mov files in a directory ([source](https://stackoverflow.com/questions/5784661/how-do-you-convert-an-entire-directory-with-ffmpeg))
`for i in *.mov; do ffmpeg -i "$i" -vf  "setpts=0.5*PTS" "${i%.*}.mp4"; done`