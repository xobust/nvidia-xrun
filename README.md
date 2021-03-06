# nvidia-xrun
These utility scripts aim to make the life easier for nvidia cards users.
It started with a revelation that bumblebee in current state offers very poor performance. This solution offers a bit more complicated procedure but offers a full GPU utilization(in terms of linux drivers)

## The problem with Thinkpad t440p
The later versions of the t440p bios have a bug that makes turning off the dGPU with bbswitch impossible.
Luckily another power management technology has been implemented in the nouveau driver.
This fork of Nvidia xrun loads the nouveau driver on exit which powers down the GPU. 

## Usage: 
  1. switch to free tty
  1. login
  1. run `nvidia-xrun [app]`
  1. enjoy

Currently sudo is required as the script needs to wake up GPU, modprobe the nvidia driver and perform cleanup afterwards. For this we use bbswitch.

## Structure
* **nvidia-xrun** - uses following dir structure:
* **/usr/bin/nvidia-xrun** - the executable script
* **/etc/X11/nvidia-xorg.conf** - the main X confing file
* **/etc/X11/xinit/nvidia-xinitrc** - xinitrc config file. Contains the setting of provider output source
* **/etc/X11/xinit/nvidia-xinitrc.d** - custom xinitrc scripts directory
* **/etc/X11/nvidia-xorg.conf.d** - custom X config directory
* **[OPTIONAL] ~/.nvidia-xinitrc** - user-level custom xinit script file. You can put here your favourite window manager for example
 

## Setting the right bus id
I found that the bus is 2:0:0 was correct on my 440p. If this is not your case(you can find out through lspci or bbswitch output mesages) you can create
a conf script for example `nano /etc/X11/nvidia-xorg.conf.d/30-nvidia.conf` to set the proper bus id:

    Section "Device"
        Identifier "nvidia"
        Driver "nvidia"
        BusID "PCI:1:0:0"
    EndSection
    
Also this way you can adjust some nvidia settings if you encounter issues:

    Section "Screen"
        Identifier "nvidia"
        Device "nvidia"
        #  Option "AllowEmptyInitialConfiguration" "Yes"
        #  Option "UseDisplayDevice" "none"
    EndSection
    
## Automatically run window manager
For convenience you can create `nano ~/.nvidia-xinitrc` and put there your favourite window manager:

    openbox-session
    
With this you do not need to specify the app and you can simply run:

    nvidia-xrun
    
## Troubleshooting
### Steam issues
Yes unfortunately running Steam directly with nvidia-xrun does not work well - I recommend to use some window manager like openbox.
# 
