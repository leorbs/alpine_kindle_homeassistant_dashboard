# Alpine Linux on Kindle
Here you find a set of utilities to get [Alpine Linux](https://alpinelinux.org/) running on Kindles 
with the target to run the Midori Browser to display the AppDeamon
[dashboard](https://appdaemon.readthedocs.io/en/latest/DASHBOARD_CREATION.html) from HomeAssistant. 
I use this on Paperwhite 2, but it should work on any Kindle (not Kindle Fire though) that has a touchscreen
and enough Flash/RAM.

This differs from [the version it was forked from](https://github.com/schuhumi/alpine_kindle) in that it works 
with older Kindles. It uses Midori instead of chrome and has some other tweaks to save resources.
In addition to that I wil describe the approach I took to Jailbreak my Kindle. This will presumably lead to more 
chaos about the whole "how to Jailbreak my Kindle" in the internet, but nevertheless maybe you just need the right
entrypoints for your research. I had difficulties to find up to date information and techniques, so here you are,
let's begin.


## Overview
Kindles run a Linux operating system with X11 and everything on board already. 
To make better use of that one can utilize a full blown Linux distro including a proper desktop 
environment through chroot. Your Kindle stays fully functional to buy & read books.
With the Linux distro we can run another browser Midori.
With that it is possible to display and Interact with the AppDeamon Dashboard from HomeAssistant.

## Get started
### Road to glory (outline):
1. Backup Kindle
1. Downgrade Kindle
1. K5 Jailbreak
1. Upgrade
1. Install Hotfix from K5 Jailbreak
1. install KUAL
1. install Helper and KTERM
1. create alpine and run it
1. open the browser and enjoy

### Main Resources
- [Original Post that got this AppDeamon Project started](https://thomaspreece.com/2019/12/10/home-assistant-kindle-dashboard/) Here you can also
  find some information on how to tweak App Deamon with some CSS for better Kindle support. With the new AppDeamon integration a docker container
  isn't necessary anymore to run AppDeamon. Yous still need to check if the CSS is applicable in that case.  
- [Current maintained packages post from mobileread.com](https://www.mobileread.com/forums/showthread.php?t=225030) 
  Here you will find all current information about jailbreaking and installing KUAL. Use this as your primary information source
  if you are stuck.
- [Some page I found with a good tutorial on jailbreaking and downgrading inclusive binaries](https://www.epubor.com/how-to-jailbreak-kindle-paperwhite.html)  


### Backup Kindle
Connect you Kindle to your Computer and copy the `documents` folder somwhere safe. Reset your Kindle afterwards to 
factory settings.

### Downgrade Kindle
I had my Firmware version somewhere at 5.8.x. Luckily I didn't update my Kindle in the last couple of years. That meant, the
downgrade -> Jailbreak -> Update Methode could be done.

Simply put the ``update_PW2_5.4.3.2_initial.bin`` in the rood folder of the Kindle and press "update your Kindle"
in the settings menu. You can find the binary I used in the releases of this repository.

[Here](https://www.mobileread.com/forums/showthread.php?t=338268) you can find a very up to date post of how to Jailbreak 
your Kindle if downgrading isn't working for you. This methode is also linked in the current maintained package post listed above
in the resources and is called Kindlebreak.

### K5 Jailbreak
I used [this](https://www.mobileread.com/forums/showthread.php?t=186645) to Jailbreak my Kindle. I will not put the jailbreak
file into the releases, because there could be updates coming out and I won't maintain this README.
It is as simple as the downgrade process. Just follow th post and extract the contents of a certain zip file into the root
kindle folder and tap on "update your Kindle". A "JAILBREAK" sign should appear on the bottom of your screen.

### Upgrade your Kindle
Now you can use an update file of a Kindle version you like to upgrade your Kindle. You can get the Upgrade files directly from Amazon.

First look [here](https://www.amazon.com/gp/help/customer/display.html?ascsubtag=8c72bbe45f8e4a138a8567b621876940a12c95aa&nodeId=200203720) 
and choose a version of Kindle you like. For the PW2 you have to look into the ``Kindle Paperwhite (6th Generation)`` section. Note that it is
not a complete list.

Use following Url schema to retrieve the .bin file for the paperwhite 2 (methode taken from [here](https://www.mobileread.com/forums/showthread.php?t=338268)): 
[https://s3.amazonaws.com/firmwaredownloads/update_kindle_paperwhite_v2_5.XX.X.bin](https://s3.amazonaws.com/firmwaredownloads/update_kindle_paperwhite_v2_5.XX.X.bin)

I used version 5.10.3. because it is the lowest version that supports the newest jailbreak method: Kindlebreak. Also I ONLY want 
the Alpine to work and also the Kindle to show book pages lol why would I need the newest firmware? With the newest firmware
there is always the possibility that Amazon prevents Jailbreaking or finds a way to silently update the Kindle.
After downloading I placed the .bin file into the root directory and clicked "update your Kindle".

### Install Hotfix from K5 Jailbreak
I found it necessary to update the jailbreak. For that place the "hotfix" file from the K5 jailbreak into the root folder and
once again press "update your Kindle".

### Install KUAL
The KUAL post and information can be found [here](https://www.mobileread.com/forums/showthread.php?t=203326). Because we have
a software version >5.9 we use the "Booklet" version of KUAL. The other versions are old approaches to the KUAL system,
so don't bother with them. 

To install KUAL first we need to "install" MRPI. For that head to this post [here](https://www.mobileread.com/forums/showthread.php?t=251143)
and retrive the MRPI files. Those need to be extracted into the root folder of the Kindle. MRPI is technically an extention
of KUAL but for installing KUAL we need to first "install" MRPI.

Then retrieve the KUAL files and place the install file into the ``mrpackages`` folder that should be in the root directory 
of your Kindle after the "install" of MRPI. After that type ``;log mrpi`` into
the search bar of your Kindle. Some magic should happen now and KUAL installs itself.

you should now have KUAL as a Book on your Kindle
### Install Helper and KTERM

The Helper wil activate a Button to disable the OTA Updates and the Screensaver. Kterm is needed to Configure and Start 
Alpine.

Retrieve the Helper from the package post that is listed in the resources. Extract the contents and put the folder that is in the 
`extensions` folder into the `extentions` folder that on in the Kindle. All KUAL extensions go in the `extensions` folder of your Kindle.
It should look like `/extensions/helper/...`.

After that get KTERM from [here](https://www.fabiszewski.net/kindle-terminal/) and extract it into the Kindle ``extensions foler``.
It should look like `/extensions/kterm/...`.

Confirm the installation by spotting the new button `kterm` and `HELPER+` in KUAL.

### Create Alpine and run it

*********************
#### !!WARNING!!
WHILE ALPINE IS RUNNING / THE IMAGE IS MOUNTED, DO NOT CONNECT YOUR KINDLE TO THE COMPUTER! 
The image resides in /mnt/us, and that is your usb mass storage location. When Alpine and
the computer write on the userstore partition (partition 4) at the same time, it will be destroyed, 
and you need to fix that partition to get your Kindle working again. It might even be possible to brick 
the Kindle!!!
*********************
The default password of the user "alpine" is "alpine".


#### Create the alpine image

Creating your own doesn't take long, and if you have a linux computer it's pretty easy. 
All you need to install is qemu-user-static to execute arm software, but that should be in the repositories of your 
distro in most cases. Then have a look at the create_kindle_alpine_image.sh script especially at the configuration part (top). 
Finally you can execute it, and after a while you should be dropped into a shell inside Alpine. I've used
a Linux VM and got heaps of "qemu: Unsupported syscall: 397" (also with different numbers) log output. Although I didn't feel good
ignoring it, the Image works in the end.
You can have a look around and tweak whatever you want, and with "exit" the script unmounts your fresh image and terminates.
After that you should end up with a "alpine.ext3" file.

#### Transfer image to Kindle
Open Kterm and make sure you have more than 1G available on your Kindle. The userspace is mounted in ``/mnt/us``.
```
[kterm]# df -h /mnt/us
Filesystem                Size      Used Available Use% Mounted on
fsp                       1.3G       20M      1.2G   1%    /mnt/us
```

You now need to copy the ``Alpine.conf, Alpine.ext3, Alpine.sh`` on to the Kindle into the root folder.

To save on RAM the Kindle GUI can be stopped before Alpine gets started. That must be done through upstart
though, because when you stop the Kindle GUI, so does Kterm and Alpine itself when you start it from there.
"alpine.conf" is a script to allow for that, it must be copied to the appropriate place though. Upstart is already on the
kindle and does not need to be installed.
e.g. assuming you have put it into ``/mnt/us``:
```
mntroot rw
cp /mnt/us/alpine.conf /etc/upstart/
mntroot r
```
#### Run it

For running Alpine you have two options:
1. **good for debugging** In Kterm, first run ``sh alpine.sh`` in the ``/mnt/us`` folder. That drops you
   into an Alpine shell, where you can do text based stuff, or run ``sh startgui.sh`` to start the desktop
2. **recommended** You can use upstart to save on ram, for that run ``start alpine`` from Kterm. It will bring you to the desktop immediately.


The default password of the user "alpine" is "alpine".


### Open the browser and enjoy
Now we can open the browser with a tap on Apps -> Web Browser. You may need to choose Midori as standard browser. 
Then you can connect to your AppDeamon dashbord. 

By pressing F11 on the keyboard you will drop into fullscreen. But beware, you can't exit that and need to restart the Kindle
by pressing the power button for 10 - 15 seconds.

If you log out of Alpine you will restart into Kindle mode. 
