## Docker build for a base raspbian image with mjpg-streamer libraries for Raspberry Pi3

This repo contains build files and instructions for a Docker image for Raspberry Pi3 (ARM), which will run jacksonliam's fork of mjpg-streamer. 
See PKOUT's [instructions](http://petrkout.com/electronics/low-latency-0-4-s-video-streaming-from-raspberry-pi-mjpeg-streamer-opencv/) for MJPEG Streamer intro and manual build

### Initial setup:


To pull a prebuilt image: 
```
docker pull openhorizon/mjpg-streamer-pi3:latest
```

(or) To build:

```
git clone https://github.com/open-horizon/cogwerx-mjpg-streamer-pi3.git
cd cogwerx-mjpg-streamer-pi3
docker build -t openhorizon/cogwerx-mjpg-streamer-pi3 .
```

### To run:

* You must run the container in "privileged" mode for docker to allow access to the Raspberry Pi 3 camera.
* mjpg-streamer uses standard picam options (Vertical flip: -vf / Horizontal flip: -hf)

```
docker run -it --rm --privileged -p 8080:8080 openhorizon/mjpg-streamer-pi3 ./mjpg_streamer -o "output_http.so -w ./www" -i "input_raspicam.so -x 640 -y 480 -fps 20 -ex night"

```

Using a web browser, visit your Pi3's IP address followed by 8080 (e.g. http://192.168.0.193:8080) on your LAN.
That's it! You should be able to see a streaming video image from your Pi.
  

