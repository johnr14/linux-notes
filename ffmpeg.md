Convert a video to mkv and attatch a specific thumbnail :
-attach cover.jpg -metadata:s:t mimetype=image/jpeg

ffmpeg -i 0052-12188.mp4 -pass 1 -c:v libvpx-vp9 \ 
-preset slow -b:v 1400K -crf 33 -threads 16 -coder 1 \ 
-tile-columns 6 -frame-parallel 1\ 
-pix_fmt yuv420p -movflags +faststart -g 30 -bf 2 \
-an -f webm /dev/null

ffmpeg -i 0052-12188.mp4 -c:v libvpx-vp9 -pass 1 -b:v 1400K -threads 16 -speed 4 \
  -tile-columns 0 -frame-parallel 0 -auto-alt-ref 1 -lag-in-frames 25 \
  -g 9999 -aq-mode 0 -an -f webm /dev/null

#PASS 2
ffmpeg -i 0052-12188.mp4 -attach Screenshot_20190315_180200.png -metadata:s:t mimetype=image/jpeg -c:v libvpx-vp9 -pass 2 -preset slow -b:v 1400K -crf 33 -threads 16 -coder 1 -pix_fmt yuv420p -movflags +faststart -g 30 -bf 2 -c:a libopus -b:a 64k BushGarden-2018.webm   
ffmpeg -i 0052-12188.mp4 -attach Screenshot_20190315_180200.png -metadata:s:t mimetype=image/jpeg -c:v libvpx-vp9 -pass 2 -b:v 1400K -threads 16 -speed 0 \
  -tile-columns 0 -frame-parallel 0 -auto-alt-ref 1 -lag-in-frames 25 -g 9999 -aq-mode 0 -c:a libopus -b:a 64k -f webm out.webm
  
  
  ffmpeg -i 0052-12188.mp4 -attach Screenshot_20190315_180200.png -metadata:s:t mimetype=image/jpeg -c:v libvpx-vp9 -pass 2 -b:v 1400K -r 24 -threads 16 -speed 3 -tile-columns 6 -frame-parallel 1 -vf setsar=1:1 -auto-alt-ref 1 -lag-in-frames 25 -crf 30 -aq-mode 0 -c:a libopus -b:a 64k -movflags +faststart -f webm out.webm
  
 
Quality
Example flag: -crf 20.

0 is lossless, 23 is default, and 51 is worst possible. 18-28 is a sane range.

All device compatibility
Flag: -profile:v baseline -level 3.0.

Fast start
Flag: -movflags +faststart.

Moves some data to the beginning of the file, allowing the video to be played before it is completely downloaded.

Flag: -pix_fmt yuv420p.

Note: Requires dimensions to be divisible by 2.


#######################################################################################################
#######################################################################################################



First pass

ffmpeg -i 0052-12188.mp4 -c:v libvpx-vp9 -pass 1 -b:v 1800k -minrate 900k -maxrate 2610k -threads 8 -speed 4 -quality good -crf 31 -tile-columns 2 -frame-parallel 1 -auto-alt-ref 1 -lag-in-frames 16 -framerate 24 -g 240 -aq-mode 0 -an -y -f webm /dev/null

Second pass

ffmpeg -i 0052-12188.mp4 -attach Screenshot_20190315_180200.png -metadata:s:t mimetype=image/jpeg -c:v libvpx-vp9 -pass 2 -b:v 1800k -minrate 900k -maxrate 2610k -threads 8 -speed 2 -quality good -crf 31 -pix_fmt yuv420p -movflags +faststart -tile-columns 2 -frame-parallel 1 -auto-alt-ref 1 -lag-in-frames 16 -framerate 24 -g 240 -aq-mode 0 -an -f webm out3.webmproject


ffmpeg -i 0052-12188.mp4 -attach Screenshot_20190315_180200.png -metadata:s:t mimetype=image/jpeg -c:v libvpx-vp9 -pass 2 -preset slow -b:v 1400K -crf 33 -threads 16 -coder 1 -pix_fmt yuv420p -movflags +faststart -g 30 -bf 2 -c:a libopus -b:a 64k BushGarden-2018.webm   

ffmpeg -i 0052-12188.mp4 -attach Screenshot_20190315_180200.png -metadata:s:t mimetype=image/jpeg -c:v libvpx-vp9 -pass 2 -b:v 1800K -threads 16 -speed 0 \
  -tile-columns 0 -frame-parallel 0 -auto-alt-ref 1 -lag-in-frames 25 -g 9999 -aq-mode 0 -c:a libopus -b:a 64k -f webm out.webm

-frame-parallel \ #Enable frame parallel decodability features.
-row-mt 1 \ #Enable row based multi-threading.
-tune-content 2 \ #Film content


VP9 bitrate : https://developers.google.com/media/vp9/settings/vod/

#24-30fps
-framerate 24 
#2160p24-30
-b:v 12000k -minrate 6000k -maxrate 17400k
#1440p24-30
-b:v 6000k -minrate 3000k -maxrate 8700k
#1080p24-30
-b:v 1800k -minrate 900k -maxrate 2610k
#1080p24-30
-b:v 1800k -minrate 900k -maxrate 2610k
#720p24-30
-b:v 1024k -minrate 512k -maxrate 1485k
#240p24-30
-b:v 150k -minrate 75k -maxrate	218k

#60fps
-framerate 60
#2160p60
-b:v 18000k -minrate 900k -maxrate 26100k
#1440p60
-b:v 9000k -minrate 4500k -maxrate 13050k
#1080p60
-b:v 3000k -minrate 1500k -maxrate 4350k
#720p60
-b:v 1800k -minrate 900k -maxrate 2610k


###########
#Quality

#2160 
-crf 15
#1440
-crf 24
#1080	
-crf 31
#720	
-crf 32
#240
-crf 37

############
#Multipass
# -y for overwrite /dev/null
#First 
-speed 4 -quality good -y
#Second 
-speed 2 -quality good

############
# Keyframes
-g 240

############
# Multithreading
-tile-columns 4 -threads 24
#1440
-tile-columns 3 -threads 16
#1080 and 720
-tile-columns 2 -threads 8

############
# Attach image
-attach Screenshot_20190315_180200.png -metadata:s:t mimetype=image/jpeg

# Alt frame https://www.webmproject.org/docs/encoder-parameters/#5-the-alternate-or-constructed-reference-frame
--auto-alt-ref 1 --lag-in-frames=16



