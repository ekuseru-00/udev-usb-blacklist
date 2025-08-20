# udev-usb-blacklist
Selective Device Blacklisting with udev Rules

This is the most precise approach - it prevents specific devices from binding to drivers.

# Create udev rules directory
```
#bash
mkdir -p /boot/config/udev/rules.d
```

# Create the blacklist rule
```
#bash
nano /boot/config/udev/rules.d/99-usb-vm-blacklist.rules
```

# In Nano file, add this:

Change the vendor id "1038" and product id "161a" to your device's id's.

You can more lines,  if you want to blacklist more USB devices.
```
#Prevent driver binding
SUBSYSTEM=="usb", ATTRS{idVendor}=="1038", ATTRS{idProduct}=="161a", ENV{ID_MM_DEVICE_IGNORE}="1", ENV{MTP_NO_PROBE}="1"

#Additional rule to unbind if already bound
ACTION=="add", SUBSYSTEM=="usb", ATTRS{idVendor}=="1038", ATTRS{idProduct}=="161a", RUN+="/bin/sh -c 'echo $kernel > /sys/bus/usb/drivers/usbhid/unbind'"
```
# Reload the udev rules to make them active
```
#bash
udevadm control --reload-rules
udevadm trigger
```
# Test Rules Manually
Test if the rules work by unplugging and replugging one of your target devices

OR trigger the rules manually for existing devices
```
udevadm trigger --subsystem-match=usb
```
Sometimes rebooting your server helps trigger the udev rules
