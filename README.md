# dash-player

For converting given `.mov` file to separate audio and video files using `ffmpeg`

Audio:

    ffmpeg -y -i "tears_of_steel_720p.mov" -c:a aac -b:a 192k -vn "output_audio.m4a"

Video:

    ffmpeg -y -i "tears_of_steel_720p.mov" -preset slow -tune film -vsync passthrough -write_tmcd 0 -an -c:v libx264 -x264opts "keyint=25:min-keyint=25:no-scenecut" "output_video.mp4"

To create MPD using MP4Box:

    MP4Box -dash-strict 2000 -rap -frag-rap -bs-switching no -profile "dashavc264:live" "output_video.mp4" "output_audio.m4a" -out "output.mpd"

Start Server in root directory:

    python -m http.server 8000

and go to localhost:8000 on chrome the `index.html` file is picked up by server.

Inside index.html

I have added few events like:

PLAYBACK_PAUSED, PLAYBACK_STARTED, PLAYBACK_ENDED, PLAYBACK_TIME_UPDATED

BUFFER_EMPTY, BUFFER_LOADED

FRAGMENT_LOADING_COMPLETED (just giving request information from json)

For PLAYBACK_PAUSED and PLAYBACK_STARTED on selecting the option, each pause and each start of video will trigger the event and display in text area

Events like "PLAYBACK_TIME_UPDATED" and "FRAGMENT_LOADING_COMPLETED" keep running till the end of video so I have added button to stop the events from continuing 
