# video-streaming

## Links

[Main reference](https://www.codeinsideout.com/blog/pi/stream-ffmpeg-hls-dash/#create-video-chunks)

## How to use

0. Build and run docker nginx docker container streaming the video content:

```
# Build 
sudo docker build -t nginx-video-streaming:stable .
# Run
# HLS on Unix systems
sudo docker run -v /dev/shm/hls:/usr/share/nginx/html/hls \
-v $(pwd)/assets:/usr/share/nginx/html \
-p 8080:80 -d nginx-video-streaming:stable
```

1. Required for live straeming (e.g. in step 2. when visiting `localhost:8080/hls-video-stream.html`). Start your stream in 1 terminal process. Therefore: 

```

# HLS with h264 (video codec) and aac (audio codec)
sudo mkdir -vp /dev/shm/hls

sudo ffmpeg -y -f v4l2 -framerate 30 -i /dev/video0 -f alsa -i hw:0 -c:v libx264 -preset veryfast -tune zerolatency -pix_fmt yuv420p -b:v 1500k -c:a aac -b:a 64k -ar 44100 -f hls -hls_time 1 -hls_list_size 30 -hls_flags delete_segments /dev/shm/hls/live.m3u8
```
2. Visit `localhost:8080/hls-live-stream.html` or `localhost:8080/hls-video-stream.html`