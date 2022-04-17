# See with the eyes wide open.
So the time has come; you want to purify your system? Well, unfortunately, it's not that easy. Running TempleOS on bare metal is somewhat of a challenge as TempleOS was only made for select hardware. Fortunately, running TempleOS is possible under a Virtual Machine.

## Running TempleOS on a VM
To start, you'll need to establish which virtual machine you'll be using, generally, there are two options:
* [VMware Workstation](https://www.vmware.com/au/products/workstation-player.html)
* [Oracle VirtualBox](https://www.virtualbox.org/)
* [QEMU](https://www.qemu.org/)

All essentially do the same job of virtualizing an OS, however, there is a major difference when it comes specifically TempleOS, that is that VirtualBox does not support PC speakers; it might not be that big of an issue however it is more immersive when you can use your speakers, in which case you should be using VMWare Workstation (or even QEMU). The downside to using VMWare is that it requires a license (not to worry however, you can easily find cracked keys on GitHub).

This will require you to have a basic understanding of how to create a virtual machine using your desired VM software, as the process is universally the same.

You can download TempleOS from [here](https://www.templeos.org/Downloads/TempleOS.ISO).

### Using VirtualBox / VMWare Workstation
This will be detailed specifically for VirtualBox since I don't run VMWare, however, you should be able to replicate this using VMWare; as long as you have half a brain cell, and two functional arms.

Upon opening, VirtualBox selects the "New" button.

1. Enter a name for your Virtual Machine, you can name it anything. For Type select "Other" and the version, select "Other/Unknown (64-bit)".

2. Next, you'll be required to enter the amount memory to use; generally TempleOS only needs 512MB to boot, however, be safe and give it around 2 to 4 Gigabytes of RAM.

3. After memory, you'll be prompted for a hard disk, select "Create a virtual hard disk now". On the next screen select "VDI", next "Dynamically allocated". Finally for the HD size, just enter in 2GBs and create.

4. You should now be able to launch, open doing so select the TempleOS ISO that you downloaded.

5. On the left panel, you should be prompted to install TempleOS, all you'll need to do is press `y` 3 times (Easier than installing Linux). At this point, you're all good to go.

6. Remove the live CD by selecting the Virtual Machine, going to "Settings", "Storage" and removing the ISO. This will allow you to boot into the fresh install.

I've seen orangutans cut a tree in half, so if you can't do this, I'm honestly ashamed.
### Using QEMU
I usually recommend using QEMU as it's completely free and cross-platform. Additionally, QEMU supports audio output for TempleOS. Note that this guide for using QEMU is intended for Linux specifically.

1. Once you have the ISO saved somewhere on your disk, we can create a Virtual Disk Image:
    ```sh
    qemu-img create -f qcow2 temple 2G
    ```
   qemu-img is the program for managing disks, create will create a disk, -f is a file flag and we chose a qcow2 file, temple will be the name of the file, and 2G means 2 gigabytes in size.

2. Set up the host audio. This tells QEMU how your host projects the audio. Use this command if your Linux host uses pulse audio:
    ```sh
    export QEMU_AUDIO_DRV=pa
    ```
   If instead, you use alsa replace "pa" with "alsa" in the above command.

3. Now time to load the live CD: (It's also worth mentioning if you ever fuck up your install of TempleOS, just re-run this to get the base version).
    ```sh
    qemu-system-x86_64 -soundhw pcspk -m 512M -enable-kvm -drive file=temple -cdrom TempleOS.ISO -boot order=d
    ```
   Feel free to change around certain settings, for example the ram.

4. Installing onto the Virtual Disk.
   upon starting up TempleOS, follow the prompts and install to the CD, after doing so, boot into the CD run:
    ```sh
    qemu-system-x86_64 -soundhw pcspk -m 512M -enable-kvm -drive file=temple
    ```
#### QEMU Side Note:
* The sound is incredibly loud sometimes; turn down your output for the love of God. I found this out the hard way; screaming "Fuck my ears", as I had an error and subsequently the screeching of the error sound at 100%, my ears are still recovering to this day.
* Also in case you can't exit TempleOS, just press CONTROL + A + G to bring your mouse back (this won't work in fullscreen; so probably don't use fullscreen). I mean I was honestly stuck twice, I couldn't kill the program using my Linux shortcut, nor could I switch windows on my WM; ended up having to just turn my PC's power off. 😅
* I'd like to thank [this reddit post](https://www.reddit.com/r/TempleOS_Official/comments/ewtp0n/templeos_with_sound_in_qemulinux_host/) for the info on QEMU.