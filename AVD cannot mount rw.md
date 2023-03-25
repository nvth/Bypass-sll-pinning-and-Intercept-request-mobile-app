## REsolve Windows AVD cannot
isssue :  mount: '/system' not in /proc/mounts
go to `emulator folder AVD` maybe : \AppData\Local\Android\Sdk\emulator
> emulator -avd `Pixel_5_API_31` -writable-system

restart phone to take effect, and go remount /system
replace Pixel_5_API_31 by anything using commnad  : emulator -list-avds
