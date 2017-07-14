# raspi-hls
Stream raspberry pi camera via Http Live Stream HLS


## usage

Use ffmpeg. Avconv produces a lot of errors multiplexing the output of raspivid

* connect and configure Raspberry Pi camera board. https://www.raspberrypi.org/documentation/usage/camera/README.md
* check that raspivid can record video from the camera.
* install ffmpeg (not in raspbian repositories), binaries available from https://ffmpeg.org/. Choose *download* / *linux* / 
    *Linux Static Builds*. Download the armhf build for newer raspberry pi's. Unarchive and move the created directory somewhere: like to `/usr/local`. So you will have e.g.: `/usr/local/ffmpeg-3.3.2-armhf-32bit-static/`.
* create symlinks for /usr/local/ffmpeg-<version>/bin/ffmpeg, ffmpeg-10bit and ffserver into `/usr/local/bin`.

* put streamCamHls in `/usr/local/bin`.
* check paths and settings in top of file.
* run command streamCamHls
* files like movie\*.ts, movie.m3u8 shoud be written to OUTDIR

* now install a webserver, like nginx.
* put file in `/etc/nginx/conf.d/enable-symlinks.conf`: contents:
```
disable_symlinks off;
```
* create a symlink from OUTDIR to nginx http dir (usually `/var/www/html`.)

* fire up a video player, like vlc on a computer that can reach the raspi, enter the url:
  http://<raspi hostname or address>/hls/movie.m3u8. (*hls* should be name of symlink to OUTDIR in nginx http dir, *movie.m3u8* should be name of OUTFILE (as defined in streamCamHls).
  
  

**Please use a ramdisk for OUTDIR, like `/dev/shm`, standard on Raspbian. Writing to your flash card will
wear it out quickly.**


