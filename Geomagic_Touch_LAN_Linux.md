# Geomagic Touch LAN for Linux

Device: Geomagic Touch, LAN port

Computer: Linux Ubuntu 22.04 Realtime

Connection: LAN to USB

OpenHaptics and Driver installation: download files and follow instructions from [3D System official website](https://support.3dsystems.com/s/article/OpenHaptics-for-Linux-Developer-Edition-v34?language=en_US).

Additional notes:

* After connecting the device using LAN to USB converter, the device will show up in `Settings-Network-USB Ethernet`. Open the setting for this network,  select  `IPv4-IPv4Method-Link-Local Only`
* In `Touch Setup-Hardware-Interface`, select `LAN Port`
* Everytime the device is disconnected, need to run Touch_Setup to pair again
* To build and run OpenHaptics examples:

  ```bash
  sudo apt install gcc build-essential freeglut3-dev lib32z1-dev lib32ncurses5-dev libncurses5
  ```

## TODO

* Touch_Diagnostic is not running. Error message: `./Touch_Diagnostic: symbol lookup error: ./Touch_Diagnostic: undefined symbol: _ZdlPvm, version Qt_5 `
* Read OpenHaptics APIguide and programming guide
