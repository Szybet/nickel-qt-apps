# nickel-qt-apps
Short guide how to port apps to nickel, mainly qt ones

For how to compile, how to get the toolchains, how to compile qt etc. look here: https://github.com/Szybet/niAudio/tree/main/apps-on-kobo mainly here: https://github.com/Szybet/niAudio/blob/main/apps-on-kobo/qt-setup.md

In this example we will use InkBox OS precompiled binaries, but I will show how to get them by yourself if needed. The example app will be sanki

I used koreader ssh server to connect to the device. The serial port is also a good option. To copy files, as an example use `scp`:
```
scp -P 2222 -r libmtdev.* root@10.42.0.28:/mnt/onboard/.adds/qt-linux-5.15.2-kobo/lib/
```

## Manually
Copy the qt directories ( plugins, lib, qml ) to the ereader at `/mnt/onboard/.adds/` from https://github.com/Kobo-InkBox/gui-bundle/tree/main/content/qt. Then compile the qt platform plugin for kobos. I will use the InkBox for of it ( https://github.com/Kobo-InkBox/qt5-kobo-platform-plugin ) because I'm the most familiar with it. But propably a better option would be the fork from Aryetis at https://github.com/Aryetis/qt5-kobo-platform-plugin because it's more nickel focused.

Note: You need some fonts & some files from the toolchain - You can either look at what gui-bundle includes or just the app at launch will say what's missing

Now launching the app with a bit of debugging info:
```
QT_DEBUG_PLUGINS=1 QT_QPA_PLATFORM=kobo LD_LIBRARY_PATH="/mnt/onboard/.adds/qt-linux-5.15.2-kobo/lib/:/lib" ./sanki
```
shows the error:
```
(libmtdev.so.1: cannot open shared object file: No such file or directory)
```
That's because the InkBox fork of platform plugin, and qt compilation has a more complicated input management. 

How to compile it manually: https://github.com/Szybet/niAudio/blob/main/apps-on-kobo/libinput.md

Or just copy it from here: https://github.com/Kobo-InkBox/compiled-binaries or here: https://github.com/Kobo-InkBox/gui-rootfs/tree/main/lib at the path: `/arm-kobo-linux-gnueabihf/arm-kobo-linux-gnueabihf/sysroot/lib/` to the `lib` directory of qt

Do this for every missing library, or if you choose to compile qt on your own, you can disable libinput & friends to not have such problems.

After running, you can pack the app as a one click - extract zip file to make it easy to install look here for an example: https://github.com/Szybet/sanki/blob/master/nickel_app/create.sh

Scripts, code examples can be found in sanki repo

You can also use this library https://github.com/Kobo-InkBox/ereaderdev-lib/tree/main to handle kobo hardware :)
