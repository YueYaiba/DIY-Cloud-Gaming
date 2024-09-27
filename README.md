# Table of contents

[Hardware](#hardware)  
[Setting up LAN Cloud Gaming](#setting-up-lan-cloud-gaming)  
[Setting up Wake on Lan](#setting-up-wol)  
[Setting up your Raspberry Pi](#setting-up-your-raspberry-pi)  
[Turn on/wake your computer from your device](#wol-from-your-device)  

# Hardware

### You basically need 4 things to make this work.

• Your computer  
• Your android device  
• A Raspberry Pi (Refer to the relevant section for which models work)  
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

Install [Moonlight](https://play.google.com/store/apps/details?id=com.limelight) from the Google Play Store.  
Install [Termux](https://github.com/termux/termux-app/releases), just download and install the universal apk for the latest stable release.  
Install [Termux-Widget](https://github.com/termux/termux-widget/releases) do NOT download any termux stuff from the Google Play Store, those apps are not updated anymore.  

# Setting Up WoL

First enter your bios and enable "Resume by PCIE/Networking Device", it should be in the "Advanced" Tab, but you can easely google how to do this on your specific motherboard.  

Then go to Windows > Win+X > Device Manager > Network Adapters > Right click on your Wireless/Ethernet adapter and click on properties.  
• Go to Advanced and make sure "Wake on Magic Packet" is Enabled.  
• Go to Power Management and either untick "Allow the computer to turn off the device to save power" or make sure "Allow the device to wake the computer" is ticked.

For future reference if you are having trouble making Wake on Lan actually do anything, you can try some of these options.

• Disable all power saving options from bios and from the "Advanced" tab of your network adapter's settings.
• Setting your network to private instead of public.  
• Disable hibernation.  

# Setting Up Your Raspberry Pi

# WoL From Your Device 