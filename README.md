## Docker build for a base raspbian image with mjpg-streamer libraries for Raspberry Pi3

This repo contains build files and instructions for a Docker image for Raspberry Pi3 (ARM), which will run jacksonliam's fork of mjpg-streamer. 
See PKOUT's [instructions](http://petrkout.com/electronics/low-latency-0-4-s-video-streaming-from-raspberry-pi-mjpeg-streamer-opencv/) for MJPEG Streamer intro and manual build

### Initial setup:

Manual pre-setup steps:
* [Download](https://bluehorizon.network/documentation/disclaimer) a raspbian image for your Pi (we tested this on a Pi3 using [Horizon's](https://bluehorizon.network/) raspbian image). Unzip and flash the image to your micro SD Card, (setup WiFi) and boot. Full setup instructions [here](https://bluehorizon.network/documentation/adding-your-device).
* Run raspi-config as root and set GPU memory and enable the Pi Cam

```
raspi-config
```
Set the following options:
  * Option 5 (Connections to peripherals): P1 (Camera) Enable the Pi Camera
  * Option 7 (Advanced Options): A3 (Memory Split): Set GPU memory to 256 MB
  * Reboot

&nbsp; &nbsp; &nbsp; <img src="https://user-images.githubusercontent.com/16260619/37161848-a253e6be-22a8-11e8-9e1b-73509ae8c4dd.png" width="480" />


You're done with pre-setup steps.

### Automatic Deployment on open-horizon
This mjpg streamer container runs on [open-horizon](https://github.com/open-horizon/). Follow the [guide](https://github.com/open-horizon/examples/wiki/Edge-Quick-Start-Guide) to set up an account in IBM Cloud Watson IoT Platform, and register your RPi3 on horizon to run this and other containers as Edge microservices. 

&nbsp; &nbsp; <img src="https://user-images.githubusercontent.com/16260619/37162344-dd30136a-22a9-11e8-8a7a-804ec2a9a603.png" width="400" /> &nbsp; &nbsp; &nbsp; <img src="https://user-images.githubusercontent.com/16260619/37161742-5fefa75e-22a8-11e8-9213-dace6a8dbd97.png" width="320" />



### Manual Deployment  
The following steps show how to pull or build the docker image and run it manually.  
It's a good idea to do this, just to test and ensure that your camera is set up properly.

#### To pull a prebuilt docker image:  
You might want to get started immediately from an existing docker image. Pull the container image from [Docker Hub](https://hub.docker.com/r/openhorizon/mjpg-streamer-pi3/):  
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
That's it! You should be able to see a simple web page with a static image from your Pi.  Connect to http://xxx.xxx.xxx.xxx:8080/?action=stream to see your video stream.

&nbsp; &nbsp; &nbsp; <img src="https://user-images.githubusercontent.com/16260619/37161339-3ccba3aa-22a7-11e8-8938-516ce59d5f2d.png" width="640" />
