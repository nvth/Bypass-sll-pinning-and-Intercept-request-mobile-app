## install cert portswigger

###  Requirement
-   Windows/Linux/macOS
-   Genymotion
-   Burpsuite
-   platform-tools
-   frida
-   objection
-   openssl

### Install requirenmet
1.  Genymotion 
-   (I suggest download Light version )Download at : https://www.genymotion.com/download/
-   Download lasted Virtualbox and VirtualBox Extension Pack at : https://www.virtualbox.org/wiki/Downloads
```
Install Virtualbox, for me (windows user) : double click run binaries file and next until install succesfully
Install VirtualBox Extension Pack, for me (windows user) : open Oracle VM Virtualbox after install and double click extension pack and waith for it.
``` 
-   Install genymotion like Install Virtualbox (need a genymotion account - we can create free and choose `personal use`)
-   Lasted, make a phone android using Genymotion
2.  Burp suite
-   Download and install at : https://portswigger.net/burp/releases/professional-community-2023-2-4?requestededition=community&requestedplatform=
3.  frida 
-   Download and unzip at : https://github.com/frida/frida/releases must be `frida-server` not `frida-qml` or anythings. Ctr + F will help.
4.  objection
-   Download at : https://github.com/sensepost/objection or `pip3 install objection` with linux or `pip install objection` with cmd on windows.
5.  openssl 
-   ubuntu/kali already have this, windows not have this just download and installed at https://slproweb.com/products/Win32OpenSSL.html

### Install system trusted cacert CA
1.  install `adb` at : https://developer.android.com/studio/releases/platform-tools ==> unzip 
![image](https://user-images.githubusercontent.com/63294758/227362955-126b395f-8f70-4d07-aa4f-20876335740f.png)
2.  make a hash cert 
-   download ca cert at : http://burp through proxy burp suite
![image](https://user-images.githubusercontent.com/63294758/227363910-06a8710b-343a-4bcd-bced-3a97bf8e4d3e.png)

-   Use openssl to convert DER to PEM, for me :

> openssl x509 -inform DER -in cacert.der -out cacert.pem

> openssl x509 -inform PEM -subject_hash_old -in cacert.pem |head -1

`9a5ba575`

>mv cacert.pem 9a5ba575.0 

3.  install certificate to the device
-   you can `drop/drag` to genymotion android or use `adb`
>   adb root

>   adb remount

>   adb push 9a5ba575.0 /sdcard/

>   adb shell

>   mv /sdcard/9a5ba575.0 /system/etc/security/cacerts/

>   chmod 777 /system/etc/security/cacerts/9a5ba575.0

>   reboot

or I just suggest press `X` to turn of your genymotion, cause when i `reboot` on `adb shell` my android will be `not responding`

After reboot, Burp CA installed. We can check on `Setting` >> `Security & location` >> `Encryption & credentials` >> `Trusted Credentials`
![image](https://user-images.githubusercontent.com/63294758/227363855-241750b2-a47b-4ab0-8d7b-4a3433d566be.png)


### Intercept request (APP without sslpining)
1.  Intercepting request
-   Open genymotion, go to `Network % Internet` go to `Wifi` (must be the same with network of computer if you using phone not vitual machine), choose `Proxy` >> `Manual`. Fill the IP is computer ip and Proxy port is a random number (random number should be > 1000)
![image](https://user-images.githubusercontent.com/63294758/227363039-f55c4358-7443-4634-ae5b-c67633a4e1e5.png)

-   Open Burp-Suite go to `Proxy` tab go to `Options` sub tab and edit `Proxy Listener` Port same `wifi port number` already edit
![image](https://user-images.githubusercontent.com/63294758/227362791-df319dde-459e-41e9-86cf-de5300dc0aca.png)

-   Turn on App and easy to capture the app (without sslpinning)

### Bypass SSL pinning
1.  Check root using adb
>   adb shell

>   su

If `su`, command promt will be like that
![image](https://user-images.githubusercontent.com/63294758/227363205-c57f7fe4-05d0-4e6a-ab90-c1bcf1cbeb91.png)

2.  Check architecture (based on 86 bit or 64 bit)

>   adb shell

>   getprop ro.product.cpu.abi

`x86` and we just download version of frida-server
![image](https://user-images.githubusercontent.com/63294758/227363291-c9d06bb1-3fd0-441a-abf7-4fcb9afbea9e.png)

3.  run frida-server 
-   After unzip `frida` clone from github. just rename binaries file to `frida-server` coppy `frida-server` to abd folder and on adb commandline screen: 
>   adb push frida-server /data/local/tmp 

>   adb shell

>   chmod 777 ./data/local/tmp/frida-server

>   ./data/local/tmp/frida-server

and minimized this windows.
4.  Check `name of package` we need to bypass
-   Open app we need find `package name`
-   name of package like : `com.android.emergency` 
>   adb shell

>   top

and finding app you need, example `twitter` have package like `com.android.twitter`
4.  run objection to sslpinning bypass
-   After install objection `objection -g <name of package> explore`
>   objection -g com.android.twitter explore

After using this command, app will be run on genymotion or your phone, Dont login or do anything just leave it!
continous using command

>   android sslpinning disable

![image](https://user-images.githubusercontent.com/63294758/227364119-68d75f0c-d14c-4103-af31-40599fd91286.png)

minimized this windows

And now we can login, do anything and intercept request without SSL pinning of mobile app.


-K-


