# MaaXBoard OSM93 (A0) Development

The purpose of this document is to provide development details for the MaaXBoard OSM93 (A0) boards, which contain the MSC-OSM-93 solder down SOM. 

![MaaXBoard OSM93](https://github.com/Avnet/MaaXBoard-OSM93-HUB/blob/main/Development/MaaXBoard_OSM93_A0/Bring-Up/BoardPictures/IMG_4034.jpg?raw=true)


# Tasks (01/09/2024)

## Hardware Setup

![MaaXBoard OSM93 H/W Setup](https://github.com/Avnet/MaaXBoard-OSM93-HUB/blob/main/Development/MaaXBoard_OSM93_A0/Bring-Up/BoardPictures/MaaXBoard-OSM93-Details.png?raw=true)

### Power
Power the MaaXBoard OSM93 with a USB-C cable, plugging it into the Power/USB_A port as shown on the silkscreen of the board. 

### Debug 
MaaXBoard OSM93 provides two debug console interfaces, one for the A55 core and the other for M33. The majority of development tasks will utilize the A-core. 

Using a [FTDI USB to TTL adapater](https://www.amazon.com/Serial-Adapter-Signal-FT232RL-Windows/dp/B08BLHGWHS/ref=asc_df_B08BLHGWHS&mcid=0c4beab03b953bfdbd8736c4d7f31481?tag=bngsmtphsnus-20&linkCode=df0&hvadid=80401905752532&hvnetw=s&hvqmt=e&hvbmt=be&hvdev=c&hvlocint=&hvlocphy=&hvtargid=pla-4584001431837592&psc=1), plug the USB end into a host PC, and the other 3 female connector pins (GND/RX/TX) into the bottom 3 pins of the debug console header, labeled A33 (*Note: this should be A55).

Open PuTTY or other terminal program and configure baud to 115200, data bits to 8, and stop bits to 1 to initiate a connection to the debug interface. 

### Ethernet
MaaXBoard OSM93 provides two ethernet ports, A & B, which can serve to make a direct connection to a PC or to a router.

To connect to the board over network, connect an ethernet from a router to the MaaXBoard OSM93. Using the debug console, use ifconfig to fetch the IP address, or use your router tools to identify this. 

Using VS code or linux terminal. ssh into the board using the following command:
ssh root@<IP_addr>

Once connected, you will have access to the Linux kernal.

### USB
MaaXBoard OSM93 has 4 USB ports. 
- Power/USB_A
- USB_B
- Stacked USB 2.0 Host adapter ports 

A thumb drive was plugged into USB_B and the two stacked USB 2.0 ports to check if the board could identify the storage medium, and did so successfully. 

A USB camera was also connected to one of the ports and a short test script was implemented to see if the board could read camera data & display over a web server. There appears to be a package missing from the board's image to allow utilization of CV2 (openCV-Python). 
 - Issue:
```python
Traceback (most recent call last):
  File "/home/root/test.py", line 1, in <module>
    import cv2
  File "/usr/lib/python3.11/site-packages/cv2/__init__.py", line 181, in <module>
    bootstrap()
  File "/usr/lib/python3.11/site-packages/cv2/__init__.py", line 153, in bootstrap
    native_module = importlib.import_module("cv2")
                    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.11/importlib/__init__.py", line 126, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
ImportError: libGL.so.1: cannot open shared object file: No such file or directory
```

### Wireless Chipset Test (IW612 M.2 card from imx93evk)
The IW612 card from imx93evk was swapped onto the MaaXBoard OSM93 for wireless (bluetooth & wifi) testing. 

Per 6.1.22 Linux user guide, the user must enter ```modprobe moal mod_para=nxp/wifi_mod_para.conf``` to being initialization of the module. Upon running this on the MaaXBoard OSM93, the following error occurs:

```
root@imx93-11x11-lpddr4x-evk:/# modprobe moal mod_para=nxp/wifi_mod_para.conf

modprobe: FATAL: Module moal not found in directory /lib/modules/6.1.22-gff4fc9ed043f-dirty
```

This is most likely due to the yocto image not being built as a "full" distribution. 

## Software
```
Linux kernal version = 6.1.22

Op. System = NXP i.MX RElease Distro 6.1-mickledore
```


### Python Packages 
```python
pip3 
pycairo==1.23.0
pyGObject==3.42.2
``````



## Issues

1. After receiving MaaXBoard OSM93 and performing initial boot, ETH_B green indicator LED was stuck ON. Plugging an ethernet cable into ETH_B port, I was unable to receive any notification from the debug console that connection was received, nor was I able to identify the port using ifconfig. While this occurred, I was able to switch the cable from port B to port A and observed a successful ethernet connection, however port B appeared to be hung. 

![ETH_B Hang](https://github.com/Avnet/MaaXBoard-OSM93-HUB/blob/main/Development/MaaXBoard_OSM93_A0/Bring-Up/BoardPictures/IMG_4033.jpg?raw=true)

2. Silkscreen of the debug console shows M33 and A33. This is an error, A33 should be A55. 

3. Observed reduced data rate mode on Eth_A port while connected. No scripts/commands running that caused entry into this mode. 

4. Yocto image build doesn't appear to be a "full" distribution built image
 - Missing key libraries for CV2 support (opencv)
 - Missing modules to support wireless initialization







