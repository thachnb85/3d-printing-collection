
# 1. OS image
### Download Ubuntu 18.04 for XU 4
https://odroid.in/ubuntu_18.04lts/XU3_XU4_MC1_HC1_HC2/ubuntu-18.04.3-4.14-minimal-odroid-xu4-20190910.img.xz
BUG: 20.04: webcam doesn't work

### Flashing image:
diskutil list
diskutil unmountDisk /dev/disk2
sudo dd bs=1m if=ubuntu-18.04.3-4.14-minimal-odroid-xu4-20190910.img of=/dev/disk2

### Wifi configuration
https://wiki.odroid.com/troubleshooting/minimal_image_wifi_setup_nmcli
```
Need to append the net.ifnames=0 to bootargs in /media/boot/boot.ini to change the interface name to wlan0.

nmcli device
nmcli radio wifi on
nmcli device wifi list
nmcli device wifi connect "SSID" password "PASSWORD"
```

# 2. Setup octoprint
https://community.octoprint.org/t/setting-up-octoprint-on-a-raspberry-pi-running-raspbian-or-raspberry-pi-os/2337

## Add user 
```
adduser pi
usermod -aG sudo pi
usermod -a -G video pi
usermod -a -G tty pi
usermod -a -G dialout pi
```
Modify not asking password for sudo
```
sudo EDITOR=nano visudo
```
Adding to the bottom:
```
pi ALL=(ALL) NOPASSWD:ALL
```

## Install OctoPrint
```
cd ~
sudo apt update
sudo apt install -y python3-pip python3-dev python3-setuptools python3-venv git libyaml-dev build-essential
mkdir OctoPrint && cd OctoPrint
python3 -m venv venv
source venv/bin/activate
pip install pip --upgrade
pip install octoprint
```

### Testing
```
~/OctoPrint/venv/bin/octoprint serve
```
First time config:
```
Restart OctoPrint: sudo service octoprint restart
Restart system: sudo shutdown -r now
Shutdown system: sudo shutdown -h now
```
Webcam config
```
Stream URL: /webcam/?action=stream
Snapshot URL: http://127.0.0.1:8080/?action=snapshot
Path to FFMPEG: /usr/bin/ffmpeg
```

### Automatic start up
```
wget https://github.com/OctoPrint/OctoPrint/raw/master/scripts/octoprint.service && sudo mv octoprint.service /etc/systemd/system/octoprint.service
sudo systemctl enable octoprint.service
sudo systemctl start octoprint.service
```

### Open port 80
```
sudo apt install haproxy
```
Adding config file
```
sudo nano /etc/haproxy/haproxy.cfg
```
Adding OctoPrint at the bottom:
```
global
        maxconn 4096
        user haproxy
        group haproxy
        daemon
        log 127.0.0.1 local0 debug

defaults
        log     global
        mode    http
        option  httplog
        option  dontlognull
        retries 3
        option redispatch
        option http-server-close
        option forwardfor
        maxconn 2000
        timeout connect 5s
        timeout client  15min
        timeout server  15min

frontend public
        bind :::80 v4v6
        use_backend webcam if { path_beg /webcam/ }
        default_backend octoprint

backend octoprint
        reqrep ^([^\ :]*)\ /(.*)     \1\ /\2
        option forwardfor
        server octoprint1 127.0.0.1:5000

backend webcam
        reqrep ^([^\ :]*)\ /webcam/(.*)     \1\ /\2
        server webcam1  127.0.0.1:8080
```

Enable HAProxy:
```
sudo nano /etc/default/haproxy 
```
Then adding 
```
ENABLED=1 
```

Then start the service:
```
sudo service haproxy start
```

## Adding Camera
```
cd ~
sudo apt install -y subversion libjpeg62-dev imagemagick ffmpeg libv4l-dev cmake
git clone https://github.com/jacksonliam/mjpg-streamer.git
cd mjpg-streamer/mjpg-streamer-experimental
export LD_LIBRARY_PATH=.
make
```

### Testing Camera
```
./mjpg_streamer -i "./input_uvc.so" -o "./output_http.so"
```
Check webcam supported formats:
```
sudo apt install v4l-utils
v4l2-ctl --list-formats

```
Check if we have JPEG, then test:
```
sudo ./mjpg_streamer -i "./input_uvc.so -n -f 10 -r 640x480" -o "./output_http.so -p 10088 -w /usr/local/www"
```
