# Custom video background incrustation

Heavily based on https://elder.dev/posts/open-source-virtual-background/ from @BenTheElder

Thanks for your post !

## Setup the loopback webcam

Install kernel module

    sudo apt install v4l2loopback-dkms

Unload it (automatically loaded during install)

    sudo modprobe -r v4l2loopback

Load it with relevant options

    sudo modprobe v4l2loopback devices=1 video_nr=20 card_label="v4l2loopback" exclusive_caps=1

## Usage

If you have a Nvidia GPU, don't bother using my code.
You'll have better results using example from blog post above.

If you only have CPU like me (better have a quite recent and decent one, by the way), you're on the right place.

### Start

    docker-compose up --build

### Stop

    docker-compose down

### Change webcam resolution

To change resolution, edit `fakecam/fake.py`, change line 79, save, and run:

    docker-compose up fakecam --build

Please note you first should close the software reading the fake webcam.

### Read a different webcam

Edit `docker-compose.yaml`, change line 22, save and run:

     docker-compose up --detach --build fakecam

Please note the second part of the string "/dev/videoX:/dev/video0" should **always** be `/dev/video0`.  

It's the device the Python script will read the video stream from.

## Why

- I don't have any `NVidia GPU`, so couldn't use the exact blog post mentioned above
- To manage all of this through a `docker-compose` interface
- To get some fun despite COVID ! ;-)

## TODO

- build Tensorflow with AVX2 and FMA CPU's instructions
- use docker params to pass background image name and resolution
- expose an basic API to:
  - change background image "on the fly"
  - change webcam 
  - change webcam resolution
