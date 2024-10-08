# Table of contents

[Hardware](#hardware)  
[Setting up LAN Cloud Gaming](#setting-up-lan-cloud-gaming)  
[Setting up Wake on Lan](#setting-up-wol)  
[Setting up your Raspberry Pi](#setting-up-your-raspberry-pi)  
[Turn on/wake your computer from your device](#wol-from-your-device)  
[Disclaimer and alternatives](#disclaimer-and-alternatives)

# Hardware

### You basically need 4 things to make this work.

• Your computer  
• Your android device  
• A Raspberry Pi (Refer to the relevant section for which models work)  and a micro sd card.  
• A router with port forwarding  


# Setting Up Lan Cloud Gaming

## Computer Software

### Nvidia

You can download and use [Geforce Experience](https://github.com/moonlight-stream/moonlight-docs/wiki/Setup-Guide)  

### Nvidia, AMD and Intel

I recommend this option as I find it to be the simplest and fastest to setup, simply download and install [Sunshine](https://github.com/LizardByte/Sunshine)  

• Either enable UPnP on your router and on Sunshine and let it do the work, or open the ports Sunshine tells you on the "Network" tab of the settings.  

• In order to be able to use sunshine before even logging in on Windows, you must set it as a service, for that go to the installation folder of Sunshine, then "scripts" then run the "install-service.bat". This will start Sunshine before you even log in (the keyboard of your android device might not work to type in the password, you can simply click the icon in the bottom right of the Windows login screen to open and use the virtual keyboard instead).  

## Android Software

• Install [Moonlight](https://play.google.com/store/apps/details?id=com.limelight) from the Google Play Store. This is to actually stream your pc to your device.   
• Install [Termux](https://github.com/termux/termux-app/releases), just download and install the universal apk for the latest stable release. This is a command terminal for android.  
• Install [Termux-Widget](https://github.com/termux/termux-widget/releases) do NOT download any termux stuff from the Google Play Store, those apps are not updated anymore. This is a widget tool to run scripts as shortcuts on your android homescreen.  
• Install [Wireguard](https://download.wireguard.com/android-client/) (Needed for the VPN setup)  

At this point you can already setup Moonlight and Sunshine and stream your PC to your device on your local network and see how the performance is.

# Setting Up WoL

First enter your bios and enable `Resume by PCIE/Networking Device`, it should be in the "Advanced" Tab, but you can easely google how to do this on your specific motherboard.  

Then go to Windows > Win+X > Device Manager > Network Adapters > Right click on your Wireless/Ethernet adapter and click on properties.  
• Go to Advanced and make sure `Wake on Magic Packet` is Enabled.  
• Go to Power Management and either untick `Allow the computer to turn off the device to save power` or make sure `Allow the device to wake the computer` is ticked.

For future reference if you are having trouble making Wake on Lan actually do anything, you can try some of these options.

• Disable all power saving options from bios and from the "Advanced" tab of your network adapter's settings, also enable `Enable Shut Down WoL` to power on from a power off state.
• Setting your network to private instead of public.  
• Disable hibernation.  
• Open UDP ports 7 and 9  

# Setting Up Your Raspberry Pi

For simplicity this guide will assume we are working with at least a Raspberry 4 2GB or a more recent model, however even a Raspberry Pi Zero with wireless connectivity could work, we will also set a VPN to your local network and while it is not mandatory, it is highly recommended.

• If you do not set up the VPN, replace all local IPs in this guide by Public IPs, or if you set up the VPN with anything below a Raspberry Pi 4, only connect to the VPN to send the WoL packet, do not use it while actually playing games as it will slow your internet speed down. Also make sure to redirect the ssh ports.

## Creating SSH keys

First we will need to setup SSH keys for your Windows PC (most likely works the exact same on Linux). 

• Open cmd on Windows, and type `ssh-keygen`.  
You can leave the name file blank and the passphrase too, press enter.

•One the keys are generated, go to your windows drive and go to the .ssh folder, it should be in `C:\Users\'yourname'\.ssh`.  
Open the file `id_rsa.pub` with any text editor such as notepad, keep that for later.

## Setting up the image

• Download the [Raspberry Pi Imager](https://www.raspberrypi.com/software/)

• Mounth the micro sd card on your pc and launch the Imager, select your model, OS (recommended), and locate your SD Card.

• When you click "Next" it will prompt you if you want to open custom settings, open them.

• Set up your username and password, then go to services, tick "Enable SSH" and choose authentification with public key, then paste the key you got from `id_rsa.pub`.  

• Install the image on your SD card and plug it into your Raspberry Pi.

• I highly recommend setting up a VPN to your local network with [PiVPN](https://www.pivpn.io/) which will make things easier (and hey, it has other cool uses too, might aswell have it).  

- Open cmd on your computer and type `ssh your-raspberry-username@local-ip-of-the-raspberry` if you entered the correct ssh public key in the Imager, it should connect! You are now basically in the command terminal of your raspberry.  

- Enter the command `curl -L https://install.pivpn.io | bash` and follow the installation prompts, the installer even tells you what to answer when you don't know what it's talking about, it's super easy! Pick Wireguard as the VPN and forward the ports it tells you.

- Once the VPN is setup, enter the command `pivpn add` and name the client (the name of your device for example).

- Get the newly created config file over to your android device (you can get it to your pc or a usb drive first, for that you can look up how to do file transfer via ssh or use something like midnight commander to transfer it to a usb drive).

• Then install etherwake `sudo apt install etherwake`.

• Install midnight commander (not mandatory but that's what I use) `sudo apt install mc` (can be used anytime "nano" is mentioned)

# WoL From Your Device 

### Wireguard

• Get the config file from the VPN you got, open the wireguard app and set up a VPN from a config file, congrats your device can now conect to your local netword from anywhere at anytime!

### Termux

• Lets setup permissions first, open termux and type `termux-setup-storage` and allow, then type and allow 
```
am start --user 0 -a android.settings.action.MANAGE_OVERLAY_PERMISSION -d "package:com.termux"
```

• Might aswell enter these for later too (Creating a shortcuts and tasks folder for our WoL shortcut later)
```
mkdir -p /data/data/com.termux/files/home/.shortcuts
chmod 700 -R /data/data/com.termux/files/home/.shortcuts
```

```
mkdir -p /data/data/com.termux/files/home/.shortcuts/tasks
chmod 700 -R /data/data/com.termux/files/home/.shortcuts/tasks
```

Now let's get an ssh key for your device too!

```
apt install openssh
```
```
ssh-keygen -b 4096 -t rsa
```
Same thing, leave file name and passphrase blank, just press enter.

You can get your key by typing `nano ~/.ssh/id_rsa.pub` and copy pasting the key somewhere.

• Now go back to your pc with your newly acquired key, connect back to your raspberry via ssh and type `nano ./.ssh/authorized_keys` and just paste the key on the next line, Ctrl+X then "Y" to confirm the changes, then enter to leave the file name unchanged.  

You device can now connect to the raspberry, and at this point, the setup is already complete (refer to the example command in the next secton) , but we are gonna make it a bit more smooth.

### Termux-Widget

Now we will simply create a shortcut that will allow you to automatically send the Wake on Lan packet and close the terminal app by just pressing one app on your home screen.

Go to termux and type 
```
nano ~/.shortcuts/name-of-the-shortcut
```
*You can choose any name for the shortcut

Now type:
```
ssh -t your-raspberry-username@local-ip-of-the-raspberry 'sudo etherwake mac:address:of:your:computer'
exit
```
For example:
```
ssh -t guide@255.255.0.255 'sudo etherwake 10:dd:b1:b9:1b:f1'
exit
```
Ctrl+X then "Y" to confirm the changes, then enter to leave the file name unchanged.  

Now just go on the home screen of your android device, long press on an empty spot, tap "widgets" and search for termux-widget, long press on the single app icon and put it on your home screen, choose the shortcut you created and you're done!
Make sure that you are connected to the wireguard VPN when doing this if you put the local IP of your devices in the commands.

# Disclaimer and alternatives

• Your router may come with a VPN setup and a Wake on Lan functionality out of the box, rendering this entire process trivial, and this guide useless.

• If it doesn't come with that out of the box, you may look into stuff such as openWRT which may also be an easier path for you if installing custom firmware on your router is an option.

