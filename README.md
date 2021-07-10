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

`ffmpeg -ss 5 -t 3 -i video.mp4 -vf "fps=10,scale=320:-2" -loop -1 video.gif`  # trim from 5th second of the video + 3 seconds, 10 FPS, scale to 320 width, keep height even, infinite loop

`convert -loop 0 video.gif video_inf.gif`  # after ffmpeg some gif files stop in web-browsers after the first loop, "convert" solves this issue ([source](https://superuser.com/questions/159212/how-do-i-make-an-existing-animated-gif-loop-repeatedly))

## Apply an ffmpeg command to all .mov files in a directory ([source](https://stackoverflow.com/questions/5784661/how-do-you-convert-an-entire-directory-with-ffmpeg))
`for i in *.mov; do ffmpeg -i "$i" -vf  "setpts=0.5*PTS" "${i%.*}.mp4"; done`

`for i in *.mov; do ffmpeg -i "$i" -r 1 -vf "scale=1280:-2" -ac 1 "${i%.*}.mp4"; done`  1 frame per second & mono audio outputs

## Merge video.mp4 and audio.mp3 ([source](https://superuser.com/questions/277642/how-to-merge-audio-and-video-file-in-ffmpeg))
Replace the original audio stream in _video.mp4_ with the _audio.mp3_ audio file, shift the video steam forward by 1 second and cut the _output.mp4_ at the shortest _video.mp4_ or _audio.mp3_

`ffmpeg -ss -00:00:01 -i 'video.mp4' -i 'audio.mp3' -c:v copy -c:a aac -map 0:v:0 -map 1:a:0 -shortest output.mp4`

## Discard the segment from 20 seconds to 25 seconds and keep everything else ([source](https://superuser.com/questions/681885/how-can-i-remove-multiple-segments-from-a-video-using-ffmpeg))

`ffmpeg -i input.avi -vf "select='1-between(t,20,25)', setpts=N/FRAME_RATE/TB" -af "aselect='1-between(t,20,25)', asetpts=N/SR/TB" output.avi`

## Fast Way to Cut / Trim Without Re-encoding (using Copy and Input Seeking) ([source](https://ottverse.com/trim-cut-video-using-start-endtime-reencoding-ffmpeg))

`ffmpeg -ss 00:00:03 -i inputVideo.mp4 -to 00:00:08 -c:v copy -c:a copy trim_ipseek_copy.mp4`

## Compress a video for android (Telegram) using ffmpeg ([source](https://android.stackexchange.com/questions/231014/compress-a-video-for-android-using-ffmpeg))

`ffmpeg -i input.mp4 -vcodec libx265 -crf 28 output.mp4`


