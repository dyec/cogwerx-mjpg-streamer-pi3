## Docker build for a base raspbian image with mjpg-streamer libraries for Raspberry Pi3

This repo contains build files and instructions for a Docker image for Raspberry Pi3 (ARM), which will run jacksonliam's fork of mjpg-streamer. 
See PKOUT's [instructions](http://petrkout.com/electronics/low-latency-0-4-s-video-streaming-from-raspberry-pi-mjpeg-streamer-opencv/) for MJPEG Streamer intro and manual build

### Initial setup:

Manual pre-setup steps:
* Download a raspbian image for your Pi (we tested this on a Pi3 using [Horizon's](https://bluehorizon.network/) raspbian image [download](https://bluehorizon.network/documentation/disclaimer)). Unzip and flash the image to your micro SD Card, (setup WiFi) and boot. Full setup instructions [here](https://bluehorizon.network/documentation/adding-your-device).
* Run raspi-config as root and set GPU memory and enable the Pi Cam

```
raspi-config
```
    * Option 5 (connections to peripherals): P1 (Camera) Enable the Pi Camera
    * Option 7 (Advanced Options): A3 (Memory Split): Set GPU memory to 256 MB
    * Reboot

You're done with pre-setup steps.

#### To pull a prebuilt docker image: 
You might want to get started immediately from an existing docker image. Pull the container image from docker hub:

```
docker pull openhorizon/mjpg-streamer-pi3:latest
```

#### (or) To build a docker image yourself:
This took about 45 mins on a Pi3...


```
git clone https://github.com/open-horizon/cogwerx-mjpg-streamer-pi3.git
cd cogwerx-mjpg-streamer-pi3
docker build -t openhorizon/mjpg-streamer-pi3 .
```

### To run:

* You must run the container in "privileged" mode for docker to allow access to the Raspberry Pi 3 camera.
* mjpg-streamer uses [standard picam options](https://www.raspberrypi.org/documentation/usage/camera/) (Vertical flip: -vf / Horizontal flip: -hf)

```
docker run -it --rm --privileged -p 8080:8080 openhorizon/mjpg-streamer-pi3 ./mjpg_streamer -o "output_http.so -w ./www" -i "input_raspicam.so -x 640 -y 480 -fps 20 -ex night"

```

Using a web browser, visit your Pi3's IP address followed by 8080 (e.g. http://xxx.xxx.xxx.xxx:8080) on your LAN.
That's it! You should be able to see a streaming video image from your Pi.
  

