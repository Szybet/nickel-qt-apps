# nickel-qt-apps
Short guide how to port apps to nickel, mainly qt ones

For how to compile, how to get the toolchains, how to compile qt etc. look here: https://github.com/Szybet/niAudio/tree/main/apps-on-kobo mainly here: https://github.com/Szybet/niAudio/blob/main/apps-on-kobo/qt-setup.md

In this example we will use InkBox OS precompiled binaries, but I will show how to get them by yourself if needed. The example app will be sanki

## Manually
Copy the qt directories ( plugins, lib, qml ) to the ereader at `/mnt/onboard/.adds/` from https://github.com/Kobo-InkBox/gui-bundle/tree/main/content/qt. Then compile the qt platform plugin for kobos. I will use the InkBox for of it ( https://github.com/Kobo-InkBox/qt5-kobo-platform-plugin ) because I'm the most familiar with it. But propably a better option would be the fork from Aryetis at https://github.com/Aryetis/qt5-kobo-platform-plugin because it's more nickel focused.

Now launching the app with a bit of debugging info:
```
QT_DEBUG_PLUGINS=1 QT_QPA_PLATFORM=kobo LD_LIBRARY_PATH="/mnt/onboard/.adds/qt-linux-5.15.2-kobo/lib/:/lib" ./sanki
```
shows the error:
```
(libmtdev.so.1: cannot open shared object file: No such file or directory)
```
That's because the InkBox fork of platform plugin, and qt compilation has a more complicated input management. 
