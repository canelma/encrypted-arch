# `Encrypted Arch`: *Encrypted Arch Linux Installer*
**Encrypted Arch** is a semi-interactive script to install Arch Linux over SSH to a fully encrypted disk created using LVM on LUKS. 

## Highlights
- Full encryption
- Install via SSH
- Format/create disks and partitions with a pre-generated layout file or commands automatically
- GRUB and mkinitcpio configurations:
    - Set hooks and modules dynamically
    - Set GRUB_CMDLINE_LINUX_DEFAULT dynamically
- Create ssh keys and keep them safe
- Desktop: KDE Plasma
- Display Manager: sddm
- UFW firewall rules
- FAI: Except a few questions (passwords, passphrases), fully automatic installer
- la la la la la

## Usage
1. Clone it to your host machine.
2. Set permissions for the files `run.sh`, `./installer/install.sh` and `./installer/chroot.sh` to make them executable: `chmod +x run.sh ./installer/install.sh ./installer/chroot.sh`.
3. Run the `./run.sh` file. It will try to connect to your target machine and run the `./installer/install.sh` script which then runs the `./installer/chroot.sh` when needed.

## Configuration
Set the variables in the `files/install.config` according to your preferences. Example content of the `files/install.config` file is as below:

```bash
#!/bin/bash

# HOST and PORT of the machine the installation will run in
# via SSH
HOST="root@localhost"
PORT="3099"

# Different settings for different computers
# values: <desktop|laptop>
COMPUTER="desktop"

# Use the LAYOUT_FILE (Y) or
# format_partitions() function placed in the "./helpers.sh"
# If you will not use layouts, then you should update the format_partitions()
# according to your preference.
USE_LAYOUT=Y

# Layout file of the disks
# To create a layout of your current disks:
# sfdisk -d /dev/sdX > my-disks.layout
# sdX is the name of your device. E.g: /dev/sda
# Note: you may need to use sudo.
#
# You can get rid of some lines.
# Required ones are these specified in the example file.
#
# Example file:
# /dev/sda1: 500Mb > EFI
# /dev/sda2: all remaining space > LUKS: <swap>, /root, /home,
LAYOUT_FILE=desktop-disks.layout

# The whole disk (device) we will install the new system in
DISK_NAME=/dev/sda

# Sizes of logical volumes
# will be used with the `lvcreate` command
# If you want to define the HOME_VOLUME's value with "G" unit like others,
# change the -l argument to the -L in /installer/install.sh:83
SWAP_VOLUME="8G"        # 8 GiB
ROOT_VOLUME="50G"       # 50 GiB
HOME_VOLUME="100%FREE"  # 100% of the remaining space

# Disk partitions
# EFI, e.g: /dev/sda1
# LVM (swap, root, home), e.g: /dev/sda2
EFI_PART="${DISK_NAME}1"
LVM_PART="${DISK_NAME}2"

# USERNAME@MACHINE_NAME
USERNAME="archie"
MACHINE_NAME="world"

# Home directory path
HOME_DIR="/home/${USERNAME}"

# MODULES and HOOKS for initramfs
MODULES_LIST="amdgpu"
HOOKS_LIST="base udev autodetect keyboard keymap consolefont modconf block encrypt lvm2 resume filesystems fsck quiet"
```
