ARCH LINUX 2019.06.01 Installation Guide
========================================

Follow Guide Shown:
https://wiki.archlinux.org/index.php/Installation_guide

Missing steps or steps I followed will be divided between the two documents. For further details what you're doing refer to the guide above or the install.txt document included with each arch linux iso. From within the live arch linux environment can access install.txt with:
```
nano install.txt
```


Start:
---------------------

### Confirm and Adjust Network Settings:  
```
	ip link  
	ping archlinux.org  
```
### Time & Date:
```
	timedatectl set-ntp true  
```
### Adjust and Partition Drives:  
```
	fdisk -l  

	parted /dev/sda  

	mklabel  msdos  
	mkpart  primary  ext4  0%  100%  
	set 1 boot on  
	print  
	quit  
```

### Format Partitions:  
```
	mkfs.ext4 /dev/sda1  
```
### Mount Partitions:  
```
	mount  /dev/sda1  /mnt  
```

### Install Base Arch System:  
```
	pacstrap /mnt base base-devel  
```
### Generate Filesystem Table:  
```
	enfstab -U /mnt >> /mnt/etc/fstab  
```
### Move work to Mount:  
```
	arch-chroot /mnt  
	exit (exit?)  
```

### Set Region:  
```
	ln  -sf  /usr/share/zoneinfo/America/New_York  /etc/localtime  
```
### Set Hardware System Clock:  
```
	hwclock --systohc  
```
### Language Preference:
```
	locale-gen #uncomment country language of preference  
```
### Network configuration:

Create the hostname file:
/etc/hostname  
	
	myhostname  

Add matching entries to hosts(5):

	nano /etc/hostname  

add your "myhostname" to first line of file  
save file  

Create the hosts file:
/etc/hosts  

	127.0.0.1	localhost  
	::1		localhost  
	127.0.1.1	myhostname.localdomain	myhostname 

copy & paste text above into -  
	
	nano /etc/hosts  

### Initramfs:  
```
	mkinitcpio -p linux  
```
### Insure Internet Services begin at startup:  
```
	systemctl enable dhcpcd.service  
```

### Bootloader:  
```
	pacman -S intel-ucode  
	pacman -S grub  
	grub-install --target=i386-pc /dev/sda  
	grub-mkconfig -o /boot/grub/grub.cfg      
```
### Root Password:  
```
	passwd  
```

### Create Additional Users:  
```
	useradd -m -g users -s /bin/bash archie  
	passwd archie  
```

### Add Temporary Permissions:  
```
	pacman -S sudo  
```


# Funzies 
## installing a desktop environment:  
```
	pacman -S net-tools pkgfile base-devel  
```

### first, install Xorg
```
	pacman -S xorg  xorg-server  xorg-apps  

	sudo pacman -S gnome  
	#optional line if you'd like to start gnome (your GUI desktop environment)  
	sudo systemctl start gdm.service  

```

## Additional Applications:  
AUR package security keys
```
	pacman -S archlinux-keyring
```



### MINER SOFTWARE AND HARDWARE DRIVERS (VARIES BASED ON COMPONENTS)  
```
	git clone https://aur.archlinux.org/ethminer.git  
	cd #INTOPACKAGE  
	makepkg -sri  
```

install correct drivers for graphics cards by following these steps:  
https://wiki.archlinux.org/index.php/Xorg#AMD  

```
example:   
	pacman -S xf86-video-amdgpu  

git clone https://aur.archlinux.org/opencl-amd.git  
	cd #INTOPACKAGE  
	makepkg -sri  
```

all done:  
	start miner with:  
	https://github.com/ethereum-mining/ethminer/blob/master/docs/POOL_EXAMPLES_ETH.md  
```
example:  
	ethminer -G -P stratum2+tcp://BTC_WALLET.WORKERNAME@daggerhashimoto.br.nicehash.com:3353  
```

### MANAGE SYSTEM WITH SSH:
https://github.com/crouther/archie/blob/master/motd    
https://securitytrails.com/blog/mitigating-ssh-based-attacks-top-15-best-security-practices  
