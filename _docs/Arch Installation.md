---
title: Arch Installation
tags: 
 - pxe
 - github
description: Basic Arch OS configuration
---
# PXE Booting lab OS image deployment system
An early work in progress and essentially a collection of progress notes
Bridge the VM Net


## Install Arch
Verify the boot mode
To verify the boot mode, list the efivars directory:

# ls /sys/firmware/efi/efivars




Connect to the internet
To set up a network connection in the live environment, go through the following steps:

Ensure your network interface is listed and enabled, for example with ip-link(8):
# ip link




get address
**dhcpcd


Use timedatectl(1) to ensure the system clock is accurate:
# timedatectl status

what is the drive ID?   
fdisk -l  
Documentation assumes /dev/sda


# fdisk /dev/the_disk_to_be_partitioned
# fdisk /dev/sda   <--- assumption


Create
/mnt/boot  /dev/sda1   EFI System partition (fdisk type "ef")  1 GB  
[SWAP]     /dev/sda2   Linux Swap  (fdisk type "82")  16 GB
/mnt       /dev/sda3   Linux x86-64 root (/)  Remainder of device

Make the boot partition bootable.

write the partition (w)

Make File Systems

mkfs.fat -F 32 /dev/sda1
mkswap /dev/sda2
mkfs.ext4 /dev/sda3

mount volumes

# mount --mkdir /dev/sda1 /mnt/boot
swapon /dev/sda2
# mount /dev/sda3 /mnt


# pacstrap -K /mnt base linux linux-firmware nano networkmanager 



nano /etc/hostname
schit-lab-deploy

**Revert to traditional interface names
If you would prefer to retain traditional interface names such as eth0, Predictable Network Interface Names can be disabled by masking the udev rule:**

# ln -s /dev/null /etc/udev/rules.d/80-net-setup-link.rules
Alternatively, add net.ifnames=0 to the kernel parameters.

CONSIDER DOING THIS AS AN OPTIMISATION
Set device MTU and queue length
You can change the device MTU and queue length by defining manually with an udev-rule. For example:

/etc/udev/rules.d/10-network.rules
ACTION=="add", SUBSYSTEM=="net", KERNEL=="wl*", ATTR{mtu}="1500", ATTR{tx_queue_len}="2000"


nano /etc/hosts
127.0.0.1        localhost
::1              localhost
127.0.1.1        schit-lab-deploy


# genfstab -U /mnt >> /mnt/etc/fstab

arch-chroot /mnt



ln -sf /usr/share/zoneinfo/Australia/Perth /etc/localtime


hwclock --systohc


nano /etc/locale.gen and uncomment en_US.UTF-8 UTF-8 and other needed locales. 
en_US.UTF-8 UTF-8
en_AU.UTF-8 UTF-8

Generate the locales by running:
locale-gen


nano /etc/locale.conf
LANG=en_AU.UTF-8


set passwd
passwd  (old hp password from research room)

pacman -Syu

GRUB
pacman -S grub efibootmgr
mkdir /boot/efi
mount /dev/sda1 /boot/efi
grub-install --target=x86_64-efi --bootloader-id=GRUB --efi-directory=/boot/efi
grub-mkconfig -o /boot/grub/grub.cfg









- DOCUMENT whatever this is and how to rebuild and configure it.
    - Seriously without documentation, in time any functionality will turn to rust.
- A Virtual Box, Arch Linux server VM to support lab deployment.
    - Why Arch?
        - The privililege or starting the project is power and I like Arch!
        - Packages very close to the authors DOCUMENTATION - Less confusion resulting from distro tweaks.
        - Stronger community skill set and high quality DOCUMENTATION with less noob noise.
    - Features, architecture and philosophy
        - It needs to be low stress
        - reliability and peace of mind over functionality
        - Keep everything in one VM - Monolythic
            - Standalone operation that is easily and quickly replicated for testing.
                - Encourages learning in a safe sandbox.
                - Develops familiarity with the restoration from a clone.
                - Accessible,Dev server can run on a local machine such as a laptop.
            - Confident backup and portability between servers
            - Components
                - DHCP Server
                - PXE server
                - NFS? repository of images

## Overview
A few paragraphs setting the scene and describing the purpose and activities of the lab.

<a name="objectives"></a>
## Learning Objectives
At the end of this lab you should be able to:
- LO 1
- LO 2

## Topology
![{{ site.baseurl }}/assets/img/topology.png]({{ site.baseurl }}/assets/img/topology.png)



## Steps
### Step 1
Instructions go here

### Step 2
<pre>
Router><b>enable</b>
Router#
</pre>
