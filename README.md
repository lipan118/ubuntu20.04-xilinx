# ubuntu20.04-xilinx ise 14.7  usb 
## 1 
```
sudo apt-get install gitk git-gui libusb-dev build-essential libc6-dev-i386 fxload libusb-dev
cd /opt/Xilinx     #or some directory to build the driver in
sudo git clone git://git.zerfleddert.de/usb-driver
cd usb-driver
sudo make --如果这里有错误，我使用的64位系统，直接sudo make libusb-driver.so 
sudo cp -a /opt/Xilinx/14.7/ISE_DS/ISE/bin/lin64/xusb*.hex /usr/share/
sed -e 's/[$]TEMPNODE/%N/' -e 's/SYSFS/ATTRS/g' -e 's/BUS="usb",/SUBSYSTEM="usb", ENV{DEVTYPE}=="usb_device",/' -e 's/MODE=/MODE:=/' /opt/Xilinx/14.7/ISE_DS/ISE/bin/lin64/xusbdfwu.rules >xusbdfwu-new.rules
sudo cp xusbdfwu-new.rules /etc/udev/rules.d/
sudo udevadm control --reload
```

## 2 
```
source /opt/Xilinx/14.7/ISE_DS/settings64.sh
export PATH=/opt/Xilinx/14.7/ISE_DS/ISE/bin/lin64:$PATH
export LD_PRELOAD=/opt/Xilinx/usb-driver/libusb-driver.so
impact
```
## 3 
如果是使用的ise的13.1 ，卸载的话，找到xsetup -uninstall
如果是使用的ise的14.7，卸载的话，需要找到隐藏文件夹.xinstall/bin/lin64/xsetup -uninstall


## 4
建立快捷方式
首先是建立一个脚本，脚本内容为：

```
#!/bin/bash
export LD_PRELOAD=/opt/Xilinx/usb-driver/libusb-driver.so
ISE_DS_DIR=/opt/Xilinx/14.7/ISE_DS
unset LD_PRELOAD
export gmake=/usr/bin/make

cd "$ISE_DS_DIR"
source "$ISE_DS_DIR"/settings64.sh

export LANG=''  # reset locale to English to fix decimal/comma seperation

"$ISE_DS_DIR"/ISE/bin/lin64/ise
```


在/usr/share/applications建立一个desktop文件，取名为ISE.desktop
用文本编辑器打开输入：

```
[Desktop Entry]
Version=1.0
Name=ISE
Exec=/opt/Xilinx/14.7/ISE_DS/ise
Terminal=false
Icon=/opt/Xilinx/14.7/ISE_DS/ISE/data/images/pn-ise.png
Type=Application
Categories=Development
```
 


注意Exec的可执行文件为我们刚才建立的脚本文件，注意这个脚本必须有执行的权限。
