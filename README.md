## FFmpeg cheatsheet

### Common switches
```
-codecs          # list codecs
-c:v             # video codec (-vcodec) - 'copy' to copy stream
-c:a             # audio codec (-acodec)
-fs SIZE         # limit file size (bytes)
```

### Bitrate
```
-b:v 1M          # video bitrate (1M = 1Mbit/s)
-b:a 1M          # audio bitrate
```

### Audio
```
-aq QUALITY      # audio quality (codec-specific)
-ar 44100        # audio sample rate (hz)
-ac 1            # audio channels (1=mono, 2=stereo)
-an              # no audio
-vol N           # volume (256=normal)
```

### Video
```
-aspect RATIO    # aspect ratio (4:3, 16:9, or 1.25)
-r RATE          # frame rate per sec
-s WIDTHxHEIGHT  # frame size
-vn              # no video
```

### Add a Watermark to a Video
```
ffmpeg -i input.mp4 -i image.png -filter_complex "overlay=36:36" -codec:a copy output.mp4
```

### Remove Audio from Video
```
ffmpeg -i input.mp4 -vcodec copy -an output.mp4
```

### Join Separate Audio and Video Files
```
ffmpeg -i video.mp4 -i audio.mp3 -c copy output.mp4
```

### Convert Images To a Video Sequence
```
ffmpeg -f image2 -i image%d.jpg output.mp4
```

### Resize a Video
```
ffmpeg -i input.mp4 -vf scale=320:240 output.mp4
```

### Extract Sound From a Video, And Save It in Mp3 Format
```
ffmpeg -i input.mp4 -vn -ar 44100 -ac 2 -ab 192k -f mp3 output.mp3
```

### Convert Video to Animated Gif (Uncompressed)
```
ffmpeg -i input.mp4 output.gif
```

### Add Text Subtitles to a Video
```
ffmpeg -i input.mp4 -i subtitles.srt -c copy -c:s mov_text output.mp4
ffmpeg -i input.mp4 -vf subtitles=subtitles.srt output.mp4
```

### Trimming
```
ffmpeg -ss [start] -i input.mp4 -t [duration] -c copy output.mp4
ffmpeg -ss [start] -i input.mp4 -t [duration] -c:v libx264 -c:a aac -strict experimental -b:a 128k output.mp4
```

### Concat
```
Create a file `list.txt`
file '/path/to/file.mp4'

ffmpeg -f concat -i list.txt -c copy output.mp4
```

### Create Thumbnails
```
ffmpeg -i input.mp4 -vf fps=1 -an -s 400x222 output%d.jpg
```

### Scaling
```
ffmpeg -i input.mp4 -vf "scale=800:800:force_original_aspect_ratio=decrease,pad=800:800:(ow-iw)/2:(oh-ih)/2" output.mp4
```

### Add Text to a Video
```
ffmpeg -i input.mp4 -filter_complex \
	   "[0:v]pad=iw:ih:0:(oh-ih)/2:color=yellow, \
	   drawtext=text='EXAMPLE':fontfile=/path/to/font.ttf:fontsize=48:x=(w-tw)/2:y=(h-th)/6, \
	   drawtext=text='EXAMPLE':fontfile=/path/to/font.ttf:fontsize=48:x=(w-tw)/2:y=h-(h-th)/6)" \
	   output.mp4
```
