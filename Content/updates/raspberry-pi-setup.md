+++
title = "Setting up raspberry pi"
date = "2020-06-10T11:17:14+05:30"
+++

1. On raspberry pi, improve chromium perfomance by increasing swap size

``` shell
# /etc/dphys-swapfile
CONF_SWAPSIZE=1024
```

<!--more-->

2. Enable simultaneous access point (AP) and managed mode WiFi 

Source - [blog.thewalr.us](https://blog.thewalr.us/2017/09/26/raspberry-pi-zero-w-simultaneous-ap-and-managed-mode-wifi/)

This uses raspberry pi Zero-W, raspbian "stretch" in headless mode and OTG mode enabled, which lets the pi communicate as a virtual serial, ethernet or USB mass storage device. 

3. Securing a raspberry pi

Source - [makezine 2017/09](https://makezine.com/2017/09/07/secure-your-raspberry-pi-against-attackers/)

- create a new user account
- disable default 'Pi' account
- secure ssh server
- setup a firewall
- configure fail2ban and logwatch

