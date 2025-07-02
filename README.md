Gentoo KVM-Proxmox Image
========================

**You would like to show your appreciation for our help 8-o. Gladly. We thank you for your donation! ;)**

<a href="https://www.paypal.com/donate/?hosted_button_id=JTFYJYVH37MNE">
  <img src="https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif">
</a>

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/L3L813B3CV)


Here you can download a Gentoo VM image for KVM (Proxmox). The image is permanently maintained and has an update cycle of about 2 weeks.

### System information

+ Hostname: gtemplate.local
+ EFI + Systemd
+ 40GB drive + 8G swap drive
+ 16GB Memory
+ 20 Cores, 1 Socket
+ CPUtype: x86-64-v2-AES
+ Serial terminal activated
+ Autofilesystem repair activated
+ CPU, Memory, Disk, Network, USB Hotplug activated
+ Guestagent activated
+ Genkernel
+ Filesystem: EXT4
+ Nano Syntax highlighting

### Make.conf Options

~~~
COMMON_FLAGS="-march=x86-64 -O2 -pipe"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
FCFLAGS="${COMMON_FLAGS}"
FFLAGS="${COMMON_FLAGS}"

LC_MESSAGES=C.utf8

USE="-X threads -libav -cups -java vaapi sudo dist-kernel"

BOOTLOADER="grub2"

CCACHE_SIZE="2G"
MAKEOPTS="-j21"

LINGUAS="de"
ACCEPT_LICENSE="*"

PORTAGE_ELOG_CLASSES="log warn error info"
PORTAGE_ELOG_SYSTEM="echo save"
PORTAGE_NICENESS="10"

#PORTDIR_OVERLAY="${PORTDIR_OVERLAY} /usr/local/portage/"
#source /var/lib/layman/make.conf

GENTOO_MIRRORS="rsync://mirror.dkm.cz/gentoo/ \
    https://ftp.agdsn.de/gentoo"
GRUB_PLATFORMS="efi-64 qemu"
~~~

### Additionally installed software

+ elogv
+ bind-tools
+ portage-utils
+ eix
+ avahi-daemon and tools
+ ZSH (powerlevel10k + Zplug)
+ cifs-utils
+ nfs-utils
+ git
+ pam_mount
+ mtr
+ preload
+ speedtest-cli
+ usbutils
+ diaglog
+ htop
+ portpeek

## Import and start on Proxmox

First you have to download the image/template from my site here: https://sourceforge.net/projects/gentoo-kvm-proxmox-image/files/

Save the file on your preferred storage and import it into your Proxmox with the webinterface or the cli. Click here for the Proxmox documentation. Topic Backup and Restore.
https://pve.proxmox.com/pve-docs/chapter-vzdump.html

After start the VM get an IP over DHCP. You are able to login with root and password "gentoo".

## Extract the image and import it on another KMV hypervisor
If you are running a different virtualisation like KVM, there is also the possibility to unpack the archive and access the RAW files.
First you have to unpack the gzip file. After that you have an VMA. With the VMA extractor you can unpack that archive and list the RAW files.

~~~
vma list <filename>
vma config <filename> [-c config]
vma create <filename> [-c config] pathname ...
vma extract <filename> [-r <fifo>] <targetdir>
vma verify <filename> [-v]
~~~

## Kernelupgrade of the Image
~~~
eselect kernel list
eselect kernel <n>
genkernel --kernel-config=/root/kernel-config --makeopts=-j21 --virtio all
grub-mkconfig -o /boot/grub/grub.cfg
~~~
