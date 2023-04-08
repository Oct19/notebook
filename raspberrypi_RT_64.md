# Raspberry Pi 4/400 with 64-bit, PREEMPT_RT, ROS2

As of now, Method A is suggested.

## Method A: Download pre-build img(with ROS2)

The image and instruction for pre-build img with ROS2 included may be found [here](https://github.com/ros-realtime/ros-realtime-rpi4-image).

### Additional steps

* Add user with sudo rights

  ```bash
  sudo adduser USERNAME
  sudo usermod -aG sudo USERNAME
  ```
* Connect WiFi, and set static IP. First you need to check route IP, and then edit file in netplan folder:

  ```bash
  ip r
  # On the line that start with "default via", you can find your gateway address
  ```

  Edit file in netplan folder

  ```
  sudo nano /etc/netplan/50-cloud-init.yaml
  ```

  looks like this

  ```yaml
  network:
      ethernets:
          eth0:
              dhcp4: true
              optional: true
      version: 2
      wifis:
        wlan0:
          dhcp4: no
          addresses:
            - 192.168.12.34/24
          routes:
            - to: default
              via: 192.168.12.1
          nameservers:
            addresses: [1.1.1.1, 1.0.0.1]
          optional: true
          access-points:
            "home network":
              password: "12345678"
            "iPhone":
              password: "PASSWORD"
  ```

  Here the static ip was set to "192.168.12.34", if you are connecting through ethernet, edit under `eth0`. If you do not wish to set static ip, change dhcp4 to `true` and remove addresses, routes, and nameservers field
* Find my ip address

  ```
  hostname -I
  ```
* Change hostname

  ```shell
  sudo hostname new-name
  sudo nano /etc/hostname
  # Replace old-name with new-name
  sudo nano /etc/hosts
  # Remove 127.0.1.1 old-name
  # Add 127.0.1.1 new-name
  ```
* To login in with hostname, you can add a `Host` in `~/.ssh/config`

  ```
  Host your_hostname
    HostName 192.168.12.34
    User your_username
    ServerAliveInterval 60
    ServerAliveCountMax 10
  ```

  where `ServerAliveInterval` and `ServerAliveCountMax` is to ensure a stable connection

## Method B: Building locally on Pi

1. Download the [64-bit Raspbery Pi OS image](https://downloads.raspberrypi.org/raspios_arm64/images/). (It’s under development as of this date, so this link will become obsolete soon.)
2. Install the image onto a microSD card using the Raspberry Pi Imager. In Imager, setup SSH and WIFI LAN to make life easier
3. Follow [official instructions](https://www.raspberrypi.com/documentation/computers/linux_kernel.html) to install dependencies and clone repository. However, after cloning the repo, you need to do the following:
   * cd into the repository folder `/home/your_username/linux`
   * check kernel version by running `head Makefile -n 4`, you will get something like

     ```plaintext
     VERSION = 5
     PATCHLEVEL = 15
     SUBLEVEL = 92
     ```
   * Download [PREEMPT_RT patch](https://mirrors.edge.kernel.org/pub/linux/kernel/projects/rt/5.15/), for the kernel version listed above, the patch file name should be `patch-5.15.92-rt57.patch.gz`, not `patches`. If patching process is interrupted, make sure the repo is clean before trying to patch again, by running `git restore .`
   * unzip the downloaded patch file by running `gunzip patch*`
   * patch the kernel by running `cat patch* | patch -p1`
   * Execute `make menuconfig` to open GUI menu for kernel configuration. If failed to open, installed additional packages suggested by command prompt
   * Navigate to `General setup ---> Preemption Model`, and select `Fully Preemptible Kernel (Real-Time)`. If this option is not shown, open the file in repository: `arch/Kconfig`, change the entry from

     ```plaintext
     config ARCH_SUPPORTS_RT
         bool
     ```

     to

     ```plaintext
     config ARCH_SUPPORTS_RT
         def_bool y
     ```
   * Navigate to `General setup ---> Embedded System`, hit `y` to enable. save the changes to the `.config` file and exit
4. Continue with the kernel-building instructions where you left off at #3, which includes Apply Raspberry Pi 4 64-bit Kernel configuration, Customising Kernel Version, and Building the 64-bit Kernel, Raspberry Pi takes 80min to compile.

## Method C: Building on PC

Follow [the same official instructions](https://www.raspberrypi.com/documentation/computers/linux_kernel.html) to build from PC. A low end desktop PC with 12-cores can finish compile in 10mins

## Reference

* [Raspberry Pi image with ROS 2 and the real-time kernel](https://github.com/ros-realtime/ros-realtime-rpi4-image)
* [Raspberry Pi 4 With 64-bit OS and PREEMPT_RT](https://robskelly.com/2020/10/14/raspberry-pi-4-with-64-bit-os-and-preempt_rt/)
* [Fully Preemptible Kernel option not shown](https://unix.stackexchange.com/questions/582075/trouble-selecting-fully-preemptible-kernel-real-time-when-configuring-compil)
