## Bypass SSL pinning
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
-   Download and install frida-server at : https://github.com/frida/frida/releases
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

![image](https://user-images.githubusercontent.com/63294758/227427844-6cad9db1-9385-4c1c-adbf-d8a0544f0309.png)


and finding app you need, example `twitter` have package like `com.android.twitter`
5.  run objection to sslpinning bypass
-   Install objection on your computer : pip install objection  
-   After install objection `objection -g <name of package> explore`
>   objection -g com.twitter.android explore

After using this command, app will be run on genymotion or your phone, Dont login or do anything just leave it!
continous using command

>   android sslpinning disable

![image](https://user-images.githubusercontent.com/63294758/227427808-e3a90c81-5f88-4d3b-ac3b-88bab5865c26.png)


minimized this windows

And now we can login, do anything and intercept request without SSL pinning of mobile app.


-K-


