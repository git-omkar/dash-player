# dash-player

For converting given `.mov` file to separate audio and video files using `ffmpeg`

Audio:

    ffmpeg -y -i "tears_of_steel_720p.mov" -c:a aac -b:a 192k -vn "output_audio.m4a"
    
if you are using video with mutliple audio
    
    ffmpeg -y -i "Elephants_Dream.avi" -c:a aac -b:a 192k -vn "audio_english.m4a"
    ffmpeg -y -i "Elephants_Dream_ESp.avi" -c:a aac -b:a 192k -vn "audio_spanish.m4a"


Video:
File used 'tears of steel' : https://mango.blender.org/download/

    ffmpeg -y -i "tears_of_steel_720p.mov" -preset slow -tune film -vsync passthrough -write_tmcd 0 -an -c:v libx264 -x264opts "keyint=25:min-keyint=25:no-scenecut" "output_video.mp4"


If you are using multiple bitrates:

    ffmpeg -i tears_of_steel_720p.mov ^
    -preset slow -tune film -vsync passthrough -write_tmcd 0 -an -c:v libx264 -x264opts "keyint=25:min-keyint=25:no-scenecut" -s 1280x720 -b:v 5000k -maxrate 5000k -bufsize 2000k "output_video_1280x720.mp4" ^
    -preset slow -tune film -vsync passthrough -write_tmcd 0 -an -c:v libx264 -x264opts "keyint=25:min-keyint=25:no-scenecut" -s 800x600 -b:v 2000k -maxrate 2000k -bufsize 1000k "output_video_800x600.mp4" ^
    -preset slow -tune film -vsync passthrough -write_tmcd 0 -an -c:v libx264 -x264opts "keyint=25:min-keyint=25:no-scenecut" -s 640x360 -b:v 1500k -maxrate 2000k -bufsize 1000k "output_video_640x360.mp4" ^
    -preset slow -tune film -vsync passthrough -write_tmcd 0 -an -c:v libx264 -x264opts "keyint=25:min-keyint=25:no-scenecut" -s 320x180 -b:v 600k -maxrate 1000k -bufsize 500k "output_video_320x180.mp4"


Using MP4Box

To create subtitle: This create emplty mp4 file that can be fragmented

    MP4Box -add TOS-en.srt:FMT=VTT:lang=en subtitle_english.mp4
    
    MP4Box -add TOS-de.srt:FMT=VTT:lang=de subtitle_german.mp4


To add language tags to multi audio videos(Optional)

    MP4Box -lang en-US audio_english.m4a

    MP4Box -lang spa-ES audio_spanish.m4a

Creating manifest file

Multi rate video:

    MP4Box -dash-strict 10000 -rap -frag-rap -bs-switching no -profile "dashavc264:live" -out E:/dash/segments/TOS/TearsOfSteel.mpd output_video_320x180.mp4 output_video_640x360.mp4 output_video_800x600.mp4 output_video_1280x720.mp4 output_audio.m4a

Multi rate with closed captions:

    MP4Box -dash-strict 10000 subtitle_english.mp4:role=subtitle subtitle_german.mp4:role=subtitle -rap -frag-rap -bs-switching no -profile dashavc264:live -out E:/dash/segments/TOS/TearsOfSteel.mpd output_video_320x180.mp4 output_video_640x360.mp4 output_video_800x600.mp4 output_video_1280x720.mp4 output_audio.m4a

Multirate, Multiaudio with CC:
    
    MP4Box -lang 1=en -lang 2=spa -dash-strict 10000  subtitle_english.mp4:role=subtitle subtitle_spanish.mp4:role=subtitle -rap -frag-rap -bs-switching no -profile dashavc264:live output_video_320x180.mp4 output_video_640x360.mp4 output_video_800x600.mp4 output_video_1280x720.mp4 audio_english.m4a#trackID=1:id=aud0:role=aud0 audio_spanish.m4a#trackID=2:id=aud1:role=aud1 -out E:/dash/segments/ED/ElephantsDream.mpd
    
    
Start Server in root directory:

    python -m http.server 8000

and go to localhost:8000 on chrome the `index.html` file is picked up by server.
 
