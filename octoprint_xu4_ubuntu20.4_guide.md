
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
