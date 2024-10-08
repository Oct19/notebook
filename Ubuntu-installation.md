# Ubuntu Installation and Setup

## Installation

1. Download and install LTS version of Ubuntu as .iso file
2. Use `Rufus` software to install .iso file to USB
3. Install Ubuntu (parallel with existing operation system)

## Setup

* Set `update` as alias for quick update between all the steps

  ```bash
  sudo gedit ~/.bashrc
  # add following to ~/.bashrc
  alias update='sudo apt update -y && sudo apt upgrade -y && sudo apt autoremove -y'
  # save the file, and restart the terminal
  ```
* Install useful packages

  ```bash
  sudo apt install vim git ibus ibus-pinyin ubuntu-restricted-extras libfuse2 dconf-editor gh alacarte -y
  ```
* (Optional) Download [clash-verge](https://github.com/clash-verge-rev/clash-verge-rev/releases) for VPN service
* To use proxy in terminal (bash)

  ```bash
  # proxy
  proxy () {
    export http_proxy="http://127.0.0.1:7897"
    export https_proxy=$http_proxy
    export socks5_proxy="socks5://127.0.0.1:7897"
    echo "HTTP Proxy on"
  }

  # noproxy
  noproxy () {
    unset http_proxy
    unset https_proxy
    echo "HTTP Proxy off"
  }

  ```

* (Optional) Realtime patch: Activate Ubuntu pro and enable Realtime service. In this case, if you have NVIDIA GPU, you need to install the driver manually. This requires you to disable X and nouveau, and install the `.run` file with `IGNORE_PREEMPT_RT_PRESENCE=1`
* In `Settings>Search`, disable `Characters`
* (Optional)To open txt file generated by Windows with GBK encoding

  ```bash
  dconf-editor
  # org/gnome/gedit/preferences/encodings
  # Change default to 'uft8','GB18030'
  ```
* View Chinese characters in Vim

  ```bash
  sudo vim /etc/vim/vimrc
  # add lines:
  set fileencodings=utf-8,gb2312,gbk,gb18030
  set termencoding=utf-8
  set encoding=prc
  ```
* Use alacarte to customize Searchable Apps

  ```bash
  alacarte
  ```
* Fix snap store update problem

  ```bash
  sudo snap refresh snap-store
  # prompt saying "snap-store" has running apps, pids: 1234
  kill 1234
  ```
* To disable the shutdown/reboot timer

  ```bash
  gsettings set org.gnome.SessionManager logout-prompt false
  ```
* (Optional) Install weixin

  ```bash
  wget -O- https://deepin-wine.i-m.dev/setup.sh | sh
  sudo apt install com.qq.weixin.deepin
  ```
