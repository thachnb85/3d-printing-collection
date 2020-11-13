
# 1. OS image
### Download Ubuntu 20.4 for XU 4
https://odroid.in/ubuntu_20.04lts/XU3_XU4_MC1_HC1_HC2/ubuntu-20.04.1-5.4-minimal-odroid-xu4-20200812.img.xz

### Flashing image:
diskutil list
diskutil unmountDisk /dev/disk4
sudo dd bs=1m if=ubuntu-20.04.1-5.4-minimal-odroid-xu4-20200812.img of=/dev/disk4

### Wifi configuration
https://wiki.odroid.com/troubleshooting/minimal_image_wifi_setup_nmcli
```
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
usermod -a -G tty pi
usermod -a -G dialout pi
```

## Install OctoPrint
```
cd ~
sudo apt update
sudo apt install python3-pip python3-dev python3-setuptools python3-venv git libyaml-dev build-essential
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

### Automatic start up
```
wget https://github.com/OctoPrint/OctoPrint/raw/master/scripts/octoprint.service && sudo mv octoprint.service /etc/systemd/system/octoprint.service
```

### Open port 80
```
sudo apt install haproxy
```
Adding config file
```
nano /etc/haproxy/haproxy.cfg
```
with content
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

Then start the service:
```
sudo service haproxy start
```
