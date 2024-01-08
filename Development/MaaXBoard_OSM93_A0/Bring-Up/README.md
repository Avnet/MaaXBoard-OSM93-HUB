# MaaXBoard OSM93 (A0) Development

The purpose of this document is to provide development details for the MaaXBoard OSM93 (A0) boards, which contain the MSC-OSM-93 solder down SOM. 

![MaaXBoard OSM93](https://github.com/Avnet/MaaXBoard-OSM93-HUB/blob/main/Development/MaaXBoard_OSM93_A0/Bring-Up/BoardPictures/IMG_4034.jpg?raw=true)


## Tasks

### Hardware Setup

![MaaXBoard OSM93 H/W Setup](https://github.com/Avnet/MaaXBoard-OSM93-HUB/blob/main/Development/MaaXBoard_OSM93_A0/Bring-Up/BoardPictures/IMG_4035.jpg?raw=true)

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


## Issues

1. After receiving MaaXBoard OSM93 and performing initial boot, ETH_B green indicator LED was stuck ON. Plugging an ethernet cable into ETH_B port, I was unable to receive any notification from the debug console that connection was received, nor was I able to identify the port using ifconfig. While this occurred, I was able to switch the cable from port B to port A and observed a successful ethernet connection, however port B appeared to be hung. 

2. Silkscreen of the debug console shows M33 and A33. This is an error, A33 should be A55. 






