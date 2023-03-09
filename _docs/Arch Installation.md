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


# pacstrap -K /mnt base linux linux-firmware nano

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
