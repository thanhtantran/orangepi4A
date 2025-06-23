# OrangePi 4A Bluetooth Mouse
## Cause
Kernel was built with `CONFIG_UHID` disabled
```
zgrep -i uhid /proc/config.gz
# CONFIG_UHID is not set
grep -i uhid /boot/config-5.15.147-sun55iw3 
# CONFIG_UHID is not set
```
## Observation
My MIcrosoft mouse as well as A4Tech mouse are not working. THey get connected / paired but zero functionality. Checking the system logs, I found the following clue:
```
[bluetoothd] input-hog profile accept failed for XX:XX:XX:XX:XX:XX
```
Where XX:XX:XX:XX:XX:XX is the bluetooth address for the mouse. After some digging, I came across the solution. `CONFIG_UHID` needs to be set to ‘y’ in the kernel config to enable userspace I/O driver support for the HID subsystem.
```
CONFIG_UHID=y
```
## Solution
The config file is located at `orangepi-build/external/config/kernel/linux-5.15-sun55iw3-current.config` You can also enable Microsoft / A4Tech HID stuff. Before running `build.sh` do this to make changes in kernel config file permanent
```
sudo nano orangepi-build/userpatches/config-default.conf
IGNORE_UPDATES="yes"
```
After making this change, recompiling the kernel and rebooting the M585 pairs and works properly as a mouse. Additionally, the MS Bluetooth Mobile Mouse 3600 now works properly as well.
Prebuilt *.debs https://github.com/defencedog/orangepi4A/tree/main/kernels
## Solutions that didn't work
- https://groups.google.com/g/linux.debian.bugs.dist/c/CBVXtXabWl8?pli=1
- https://github.com/bluez/bluez/issues/531#issuecomment-1913058753
```
echo "blacklist hidp" | sudo tee /etc/modprobe.d/blacklist-hidp.conf
echo -e  "# Fix BLE mouse issue\nuhid" | sudo tee /etc/modules-load.d/uhid.conf
sudo sed -ie "s:.*UserspaceHID=.*:UserspaceHID=true:" /etc/bluetooth/input.conf
```
## Tip
Disable _Natural Scroling_ in Ubuntu settings; I faced mouse scroll reversed!
