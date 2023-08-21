## AVD Issue

issue :  mount: '/system' not in /proc/mounts

go to `emulator folder AVD` maybe : \AppData\Local\Android\Sdk\emulator
> emulator -avd `Pixel_5_API_31` -writable-system

restart phone to take effect, and go remount /system
replace Pixel_5_API_31 by anything using commnad  : emulator -list-avds
command to mount rw /  
```
adb root
adb disable-verity
adb reboot
adb remount
adb shell
mount -o rw,remount /
or
mount -o rw,remount /system
```
issue : AVD no internet after create

set internet IPv4 on your computer to DNS google 8888-8844
