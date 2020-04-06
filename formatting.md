ARCH LINUX Formatting Tools
=============================

A quick overview of command line formatting tools on Arch Linux  

Resources:   
https://linuxize.com/post/how-to-format-usb-sd-card-linux/  
https://www.ostechnix.com/format-usb-drives-windows-format-arch-linux/  

Abstract and additional steps for exception cases shown below for various usecases.  


Start:  
---------------------


### Insure Software is up-to-date and installed  
```
	sudo pacman -S archlinux-keyring
	sudo pacman -Syu
```


#### Primary Formating Software and File Systems  
```
	sudo pacman gparted parted nilfs-utils jfsutils ntfsprogs reiserfsprogs xfsprogs
```


#### List Available Storage Devices  
```
	lsblk
```
or  
```
	fdisk -l
```


#### Securely Erase  
```
	sudo dd if-/dev/zero of /dev/#TARGET# bs=4096 status=progress
```

#### Format entire Drive MSDOS FAT32

Replace #TARGET# with the specific Storage Device (ex. /dev/sdb)  
```
	sudo parted /dev/#TARGET# --script -- mklabel msdos
	sudo parted /dev/#TARGET# --script -- mkpart primary fat32 1MiB 100%
```
Format's single partition  
```
	sudo mkfs.vfat -F32 /dev/sdb1
```
print final results  
```
	sudo parted /dev/sdb --script print
```