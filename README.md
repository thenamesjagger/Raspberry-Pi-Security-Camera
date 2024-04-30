# Raspberry-Pi-Security-Camera
Creating a network camera with a Raspberry Pi and Raspberry Pi camera module. Tested with Raspberry Pi Zero 2w and Camera Module 3 but wull likely work with most Raspberry Pi's. Stream can be accessed via VLC on another machine or Home Assistant.
## Documentation
- [libcamera](https://www.raspberrypi.com/documentation/computers/camera_software.html)
- [vlc](https://platypus-boats.readthedocs.io/en/latest/source/rpi/video/video-streaming-vlc.html)
- [home-assistant](https://www.home-assistant.io/integrations/generic/)
## Install and Set Up
VLC
```bash
sudo apt upgrade && sudo apt update
sudo apt install vlc
```
Stream via http
```bash
libcamera-vid -t 0 --framerate 30 --inline -o - | cvlc stream:///dev/stdin --sout '#standard{access=http,mux=ts,dst=:8090}' :demux=h264
```
To run on boot
```bash
sudo nano /etc/systemd/system/stream.service
```
```bash
[Unit]
Description=Stream service
After=network.target

[Service]
User=username
ExecStart=/bin/bash -c "libcamera-vid -t 0 --framerate 15 --inline -o - | cvlc stream:///dev/stdin --sout '#standard{access=http,mux=ts,dst=:8090}' :demux=h264"
Restart=always

[Install]
WantedBy=multi-user.target
```
```bash
sudo systemctl enable stream.service
sudo systemctl start stream.service
```
To access on VLC go to file, network stream, then enter http://<ip-of-rpi<ip-of-rpi>>:8090
