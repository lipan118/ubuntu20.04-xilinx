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
