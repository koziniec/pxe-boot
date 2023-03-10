---
title: Arch Installation
tags: 
 - pxe
 - arch
description: Basic Arch OS configuration
---
# Building a basic CLI Arch install as a base for the PXE server.
* Download Arch ISO
* Create VM
* Allocate a volume with enough space to hold several images
* Bridge the VM Net
* Enable EFI boot

Boot into Arch installation media

## Check EFI support on the VM
Verify the boot mode supports EFI
<pre>
ls /sys/firmware/efi/efivars
</pre>
You should see files listed if EFI is functional.

## Connect to the internet
Ensure your network interface is listed and enabled, for example with ip-link(8):
<pre>
ip link
</pre>

### Obtain an IP address
<pre>
dhcpcd
</pre>

## Check clock
<pre>
# timedatectl status
</pre>

## Configure volumens
### Determine drive ID
<pre>
fdisk -l
</pre>
Documentation from here on assumes /dev/sda (substitute as necessary)

### Create partitions
<pre>
fdisk /dev/sda
</pre>

Create the following partitions within fdisk:
<pre>
/mnt/boot  /dev/sda1   EFI System partition (fdisk type "ef")  1 GB  
[SWAP]     /dev/sda2   Linux Swap  (fdisk type "82")  16 GB
/mnt       /dev/sda3   Linux x86-64 root (/)  Remainder of device
</pre>

* Make the boot partition bootable.
* write the partition (w) and exit fdisk

### Make the file systems
<pre>
mkfs.fat -F 32 /dev/sda1
mkswap /dev/sda2
mkfs.ext4 /dev/sda3
</pre>

### Mount volumes
<pre>
mount --mkdir /dev/sda1 /mnt/boot
swapon /dev/sda2
mount /dev/sda3 /mnt
</pre>

## Install Arch
<pre>
pacstrap -K /mnt base linux linux-firmware nano networkmanager 
</pre>


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
