---
title: "Atheros AR5BXB63 on Ubuntu Hardy Heron 8.04 with madwifi"
date: 2008-05-12
---

I finally got time to upgrade my personal laptop (Acer 3050 series) to Ubuntu Hardy (was Edgy). On Edgy I was using `ndiswrapper` in conjunction with the Atheros AR5BXB63 Windows XP driver in order to get wi-fi working.

Now, on Ubuntu Hardy, I've found a way to get it running using [madwifi](http://madwifi-project.org/)! By default, Ubuntu Hardy recognizes the wi-fi card but it doesn't get it working, so we need to compile and install a modified madwifi snapshot containing AR5007 models support.

Procedures are below:

1. Go to `System` > `Administration` > `Hardware Drivers`

2. Disable the following options
- Atheros Hardware Access Layer (HAL)
- Support for Atheros 802.11 wireless LAN cards.

3. Reboot your system

4. After reboot, open a terminal and issue the following:
```shell
$ sudo apt-get install build-essential
$ wget http://snapshots.madwifi.org/special/madwifi-ng-r2756+ar5007.tar.gz
$ tar xfz madwifi-ng-r2756+ar5007.tar.gz
$ cd madwifi-ng-r2756+ar5007
$ make
$ sudo make install
$ sudo modprobe ath_pci
```

5. Done! If everything went well, your wi-fi is ready to use! In order to verify it, you can issue the following:
```
$ ifconfig wifi0
wifi0     Link encap:UNSPEC  HWaddr 00-19-7E-3F-59-55-00-00-00-00-00-00-00-00-00-00
        UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
        RX packets:12151 errors:0 dropped:0 overruns:0 frame:1786
        TX packets:2676 errors:0 dropped:0 overruns:0 carrier:0
        collisions:0 txqueuelen:199
        RX bytes:3036377 (2.8 MB)  TX bytes:407225 (397.6 KB)
        Interrupt:18
```

See you on next tutorial! Thanks to UbuntuGeek for helping me out with this!