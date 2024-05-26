# Geomagic Touch LAN for Linux

Device: Geomagic Touch, LAN port

Computer: Linux Ubuntu 22.04 Realtime

Connection: LAN to USB

OpenHaptics and Driver installation: download files and follow instructions from [3D System official website](https://support.3dsystems.com/s/article/OpenHaptics-for-Linux-Developer-Edition-v34?language=en_US).

Additional notes:

* After connecting the device using LAN to USB converter, the device will show up in `Settings-Network-USB Ethernet`. Open the setting for this network,  select  `IPv4-IPv4Method-Link-Local Only`
* In `Touch Setup-Hardware-Interface`, select `LAN Port`
* QT5 need to be installed. On Ubuntu 22.04, newer version or QT might be installed, and you need to [manually install QT5 and change the Qt version](https://askubuntu.com/questions/1404263/how-do-you-install-qt-on-ubuntu22-04)
