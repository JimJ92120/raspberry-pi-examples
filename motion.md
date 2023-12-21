# motion

See `motion` for further details and more devices:

- https://motion-project.github.io/motion_guide.html
- https://motion-project.github.io/motion_config.html

Also the `camera` documentation section for **Raspberry Pi's**: https://www.raspberrypi.com/documentation/computers/camera_software.html#getting-started

---

|                       |                                      |
| --------------------- | ------------------------------------ |
| raspberry pi zero 2 w | rasberry pi os - bookworm - 64 bits  |
|                       | #rasberry pi os - bullseye - 64 bits |

---

---

# setup

Before getting started:

```sh
sudo apt-get update && sudo apt-get upgrade
```

## prepare

### camera test and setup

After plugged in your camera.  
By default `libcamera` is enabled in `bookworm`.

Note that based on the **OS** and the camera model (v1, v2, ..., by Raspberry, by third-party, etc) may require additional steps.  
e.g:

- additional `/boot/config.txt`
- enable **Legacy Camera Support** (`bullseye`)
- etc...

Test if the **camera** works out of the box with:

```sh
rpicam-hello

# test photo
rpicam-jpeg -o test.jpg

# test video
rpicam-vid -t 10000 -o test.h264

```

### additional requirements

`bookworm` based OS will required the following added:

```sh
# install `libcamerify` needed to run motion
sudo apt install libcamera-tools

# `v4l2` has been removed from main package and must be added epxlictely
sudo apt install libcamera-v4l2
```

---

## install motion

### install

Thanks to [`pimpmylifeup.com`](https://pimylifeup.com/raspberry-pi-webcam-server/) for the example.

```sh
# install dependencies
sudo apt install autoconf automake build-essential pkgconf libtool git libzip-dev libjpeg-dev gettext libmicrohttpd-dev libavformat-dev libavcodec-dev libavutil-dev libswscale-dev libavdevice-dev default-libmysqlclient-dev libpq-dev libsqlite3-dev libwebp-dev

# install motion
sudo wget https://github.com/Motion-Project/motion/releases/download/release-4.5.1/$(lsb_release -cs)_motion_4.5.1-1_$(dpkg --print-architecture
).deb

sudo dpkg -i $(lsb_release -cs)_motion_4.5.1-1_$(dpkg --print-architecture).deb
```

### configure

Create a `/motion` directory, where logs and output files will be store.

```sh
sudo mkdir /motion

# required as motion will create a `motion` user to run
sudo chgrp motion /motion
sudo chmod g+rwx /motion

# log file
touch /motion/motion.log
```

Then set `motion` config file:

```sh
sudo nano /etc/motion/motion.conf

### add or replace the following

daemon on
mmalcam_name vc.ril.camera

# set log file location
logfile /motion/motion.log

# set default "save" directory
target_dir /motion

# set port for in-browser access
stream_port 3101
webcontrol_port 3100

# for remote access (not on same device)
stream_localhost off
webcontrol_localhost off
```

---

---

# commands

```sh
# start in background
sudo libcamerify motion -b

# stop
sudo systemctl stop motion

# watch logs
tail -f -n 25 /motion/motion.log
```

---

---
