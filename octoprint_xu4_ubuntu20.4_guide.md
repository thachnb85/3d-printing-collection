
# 1. OS image
### Download Ubuntu 20.4 for XU 4
https://odroid.in/ubuntu_20.04lts/XU3_XU4_MC1_HC1_HC2/ubuntu-20.04.1-5.4-minimal-odroid-xu4-20200812.img.xz

### Flashing image:
diskutil list
diskutil unmountDisk /dev/disk3
sudo dd bs=1m if=<your image file>.img of=/dev/disk3

# 2. Setup octoprint
