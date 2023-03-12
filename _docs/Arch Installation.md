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

Assuming the bridged interface to the physical LAN provides DHCP, you should obtain a DHCP lease on that interface.  Take note of the interface name. In these documents we assume enp0s3 is bridged to the physical LAN and enp0s8 services the internal LAN.

_Need documentation here to manually configuring an IP address and default route for the situation where DHCP is not provided on the local LAN_

## Check clock
<pre>
# timedatectl status
</pre>

## Configure volumes
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
/mnt       /dev/sda3   Linux x86-64 root (/) (fdisk type "83") Remainder of device
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
pacstrap -K /mnt base base-devel linux linux-firmware nano networkmanager 
</pre>

## Set hostname
<pre>
nano /etc/hostname
schit-lab-deploy
</pre>


_CONSIDER DOING THIS AS AN OPTIMISATION_
_Set device MTU and queue length_
_You can change the device MTU and queue length by defining manually with an udev-rule. For example:_

_/etc/udev/rules.d/10-network.rules_
_ACTION=="add", SUBSYSTEM=="net", KERNEL=="wl*", ATTR{mtu}="1500", ATTR{tx_queue_len}="2000"_

## Create hosts file
<pre>
nano /etc/hosts
127.0.0.1        localhost
::1              localhost
127.0.1.1        schit-lab-deploy
</pre>

## Generate fstab
<pre>
genfstab -U /mnt >> /mnt/etc/fstab
</pre>

## Switch to chroot
<pre>
arch-chroot /mnt
</pre>

## Set timezone
<pre>
ln -sf /usr/share/zoneinfo/Australia/Perth /etc/localtime
</pre>

## Set the correct time on the hardware clock (probably meaningless in a VM)
<pre>
hwclock --systohc
</pre>

## Set the locales
<pre>
nano /etc/locale.gen 
(uncomment en_US.UTF-8 UTF-8 and other needed locales)
</pre>

<pre>
en_US.UTF-8 UTF-8
en_AU.UTF-8 UTF-8
</pre>

## Generate the locales
<pre>
locale-gen
</pre>

<pre>
nano /etc/locale.conf
</pre>
LANG=en_AU.UTF-8

##Set root password
<pre>
passwd  
(old hp password from research room)
</pre>

## Update Arch
<pre>
pacman -Syu
</pre>

## Install GRUB
<pre>
pacman -S grub efibootmgr
mkdir /boot/efi
mount /dev/sda1 /boot/efi
grub-install --target=x86_64-efi --bootloader-id=GRUB --efi-directory=/boot/efi
grub-mkconfig -o /boot/grub/grub.cfg
</pre>



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
