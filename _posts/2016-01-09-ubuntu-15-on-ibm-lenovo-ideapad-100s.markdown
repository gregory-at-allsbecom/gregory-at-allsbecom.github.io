---
layout: post
title:  "Ubuntu 15.10 on IBM (lenovo) Ideapad 100s"
date:   2016-01-09 00:30:37 -0500
categories: allsbe update
---
Over the holidays I had an interesting situation.  My sister's MacBook Air blew up and she asked for a new laptop.  I searched around and thought, for what she uses her laptop for, an IBM ideapad 100s might be a good choice.  So, I got the ideapad 14 inch version and, by chance, ended up being able to fix her ailing MacBook.  Given the choice, she chose the obvious - to keep her MacBook.  That left me with the ideapad to play around with.  I thought it might be nice to try the pre-installed Windows 10, and see *what's up with* it.  Long story short, as you may know, the ideapad is a bare bare bones version of a real laptop.  There's not much to it, but what's there, is good general purpose computing.  Anyway, I logged into Windows, and the veritable nightmare began.  I won't bore you with the endless hours of waiting for the Windows 10 updates, realizing the office version that came pre-installed was basically unusable, and add on to that: all the extra software I needed to hunt down and install to get a usable system, the privacy concerns about the OS embedded in the *all* new *"system settings"*, and finally, but most amusing of all:  I purchased a 128gb MicroSD card to use as an extra filesystem on the laptop, and *Windows 10 couldn't read from or write to it, and disk manager was crippled for anything beyond the point of the most basic functionality*.  Enough was enough.

I proceeded to point my browser to the [Ubuntu 64bit Desktop Image version 15.10](http://mirror.pnl.gov/releases/15.10/ubuntu-15.10-desktop-amd64.iso) and downloaded the iso.

*Standard disclaimer:  I hold no responsibility for the outcome you may experience following or using this process.*

###High Level Process
* Download the Ubuntu ISO
* Obtain USB drive and use dd to write the ISO.
* Configure Ideapad BIOS settings, UFEI enable, USB boot enable, Secure boot enable, Intel trusted platform disable, Optimize OS Defaults disable.
* Set USB drive as first drive in boot priority.
* Boot Ubuntu live CD.
* Prepare 64GB MMC partition for EFI boot and OS install - and MicroSD, if applicable.
* Enable Wireless connection.
* Run installation.
* Reboot. 

###Process Details
I grabbed a USB drive I had handy, which happened to be a 4gb PNY USB 2.0, plugged it in, and, wrote the ISO image from another workstation.

Opened a terminal window:

I had pre-formatted the drive as fat32.
My USB drive was /dev/sdb.  I ran gparted and ensured the "boot" flag was set on the drive.  Then I wrote the ISO image to the USB drive.

**cd /home/username/Downloads**  
**sudo dd if=ubuntu-15.10-desktop-amd64.iso of=/dev/sdb**  

Next, I inserted the USB drive in the ideapad, powered on, hit the "fn" + F2 key to enter bios setup.  I enabled "UEFI", "USB Boot", and "Secure Boot", also, set the USB drive as boot drive.  I disabled "Optimize OS Defaults".

I saved the settings, rebooted, and blammo!  I was presented with a graphical Ubuntu desktop boot screen.  I booted the live CD, and chose to install after prepping my drives, and connecting the Intel Wireless to my wireless network.

I went with mostly defaults, except for drive partitioning.  I selected custom partitioning, and configured the following GPT (gdisk helped create the GPT scheme) disk layout:


**mmc0:**  
  **Fat32 EFI  512MB - Important, set this partition with the boot flag.**  
  **ext4 /     Delete all existing partitions and used remaining space on 64gb MMC**  
  
**mmc1:**  
  **SWAP SWAP  4068MB**  
  **ext4 /home remaining space on 128GB MMC**  


That was that.  The installer did it's thing, I rebooted and had a nicely usable system with everything I needed, already working, and already installed, from office, to development, sound, video, trackpad, and on and on - and, oh yeah, by the way, *my 128gb MicroSD card which didn't work at all in Windows 10, worked perfectly in Ubuntu as my SWAP and /home partition, go figure!!!*...honestly, I don't, and probably never will understand, why people deal with Windows and having to individually install every little thing they need to do basic computing work.

I hope this helps you down the road to being productive with Linux on an ideapad 100s, it was a bit of a journey for me to get here.  If not, I hope you found this information useful.  Take care!
