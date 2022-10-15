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

`ffmpeg -i video.mp4 -vf "crop=800:300:0:60" -loop -1 video.gif`  # crop video "crop=w : h : x : y" ([source](https://www.linuxuprising.com/2020/01/ffmpeg-how-to-crop-videos-with-examples.html))

`convert -loop 0 video.gif video_inf.gif`  # some gifs stop in web browsers at the end of the first loop. "convert" solves this problem and makes them run in an endless loop ([source](https://superuser.com/questions/159212/how-do-i-make-an-existing-animated-gif-loop-repeatedly))

## Create a video from images ([source](https://stackoverflow.com/questions/24961127/how-to-create-a-video-from-images-with-ffmpeg))
`ffmpeg -framerate 30 -pattern_type glob -i '*.jpg' -c:v libx264 -pix_fmt yuv420p out.mp4`

## Convert video to images ([source](https://stackoverflow.com/questions/40088222/ffmpeg-convert-video-to-images))
`ffmpeg -i input.mp4 -vf fps=1 out%d.png`

## Apply an ffmpeg command to all .mov files in a directory ([source](https://stackoverflow.com/questions/5784661/how-do-you-convert-an-entire-directory-with-ffmpeg))
`for i in *.mov; do ffmpeg -i "$i" -vf  "setpts=0.5*PTS" "${i%.*}.mp4"; done`

`for i in *.mov; do ffmpeg -i "$i" -r 1 -vf "scale=1280:-2" -ac 1 -b:a 48k "${i%.*}.mp4"; done`  1 frame per second & mono audio outputs with 48k bitrate (default 69k)

## Merge video.mp4 and audio.mp3 ([source](https://superuser.com/questions/277642/how-to-merge-audio-and-video-file-in-ffmpeg))
Replace the original audio stream in _video.mp4_ with the _audio.mp3_ audio file, shift the video steam forward by 1 second and cut the _output.mp4_ at the shortest _video.mp4_ or _audio.mp3_

`ffmpeg -ss -00:00:01 -i 'video.mp4' -i 'audio.mp3' -c:v copy -c:a aac -map 0:v:0 -map 1:a:0 -shortest output.mp4`

## Remove audio from video file with FFmpeg ([source](https://superuser.com/questions/268985/remove-audio-from-video-file-with-ffmpeg))

`ffmpeg -i video.mp4 -c copy -an output.mp4`

## Discard the segment from 20 seconds to 25 seconds and keep everything else ([source](https://superuser.com/questions/681885/how-can-i-remove-multiple-segments-from-a-video-using-ffmpeg))

`ffmpeg -i input.avi -vf "select='1-between(t,20,25)', setpts=N/FRAME_RATE/TB" -af "aselect='1-between(t,20,25)', asetpts=N/SR/TB" output.avi`

## Fast Way to Cut / Trim Without Re-encoding (using Copy and Input Seeking) ([source](https://ottverse.com/trim-cut-video-using-start-endtime-reencoding-ffmpeg))

`ffmpeg -ss 00:00:03 -i inputVideo.mp4 -to 00:00:08 -c:v copy -c:a copy trim_ipseek_copy.mp4`

## Compress a video for android (Telegram) using ffmpeg ([source 1](https://android.stackexchange.com/questions/231014/compress-a-video-for-android-using-ffmpeg), [source 2](https://android.stackexchange.com/questions/231014/compress-a-video-for-android-using-ffmpeg))

`ffmpeg -i input.mp4 -vcodec libx265 -crf 28 output.mp4`

`ffmpeg -i input.mp4 -vcodec libx265 -b 2000k -maxrate 4000k output.mp4`

## Resize all images in a folder with mogrify ([source 1](https://unix.stackexchange.com/questions/196399/how-to-batch-resize-all-images-in-a-folder-including-subfolders), [source 2](https://stackoverflow.com/questions/43435712/how-to-set-bitrate-limit-in-ffmpeg))

`find . -name '*.jpg' -execdir mogrify -resize 20% {} +`

## Merge all the videos in a folder to make a single video file ([source](https://stackoverflow.com/questions/28922352/how-can-i-merge-all-the-videos-in-a-folder-to-make-a-single-video-file-using-ffm/37756628))

`find *.mp4 | sed 's:\ :\\\ :g'| sed 's/^/file /' > fl.txt; ffmpeg -f concat -i fl.txt -c copy output.mp4; rm fl.txt`

## Video Stabilization With `ffmpeg` and `VidStab` ([source](https://www.paulirish.com/2021/video-stabilization-with-ffmpeg-and-vidstab))

`ffmpeg -i clip.mkv -vf vidstabdetect -f null -` 

`ffmpeg -i clip.mkv -vf vidstabtransform clip-stabilized.mkv`

## Video Stabilization With `ffmpeg` and `deshake` ([source](http://blog.gregzaal.com/2014/05/30/camera-stabilisation-with-ffmpeg))

`ffmpeg -i input.mov -vf deshake output.mov` 

## Batch Convert PNG to JPG from Mac Terminal ([source](http://tutorialshares.com/batch-convert-png-jpg-mac-terminal))

`mkdir jpegs; sips -s format jpeg *.* --out jpegs`

