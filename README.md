# Gentoo-Surface
A guide to installing gentoo linux on Microsoft Devices.

## Compatability Check
Before installation please check with the [Linux Surface Compatability Table](https://github.com/linux-surface/linux-surface/wiki/Supported-Devices-and-Features#feature-matrix)

Some device's wifi won't work with **Gentoo's Live CD**:

Known Devices:
- Surface Laptops 1 and 2

## Install Process
To mitigate this, I installed Gentoo using an *Ubuntu Live CD* (Gentoo can be installed using any livecd) also followed this [Youtube Video](https://youtu.be/6yxJoMa05ZM).


1. https://wiki.gentoo.org/wiki/Installation_alternatives#Installation_from_non-Gentoo_LiveCDs
2. Proceed installation as normal.
3. **When you get to the kernel part**, issue this command:

`echo sys-kernel/gentoo-sources ~amd64 >> /etc/portage/package.accept_keywords` --> Tells gentoo to pull the latest version of the kernel.

- Then emerge/install gentoo-sources.

`emerge -aq gentoo-sources`

- Install git in order to clone [Linux-Surface](https://github.com/linux-surface/linux-surface) repo:

`emerge -aqn dev-vcs/git`

## Patch Kernel
- Then cd into the kernel source dir and clone the patches as well as this **Repo's config**.

`cd /usr/src & git clone https://github.com/linux-surface/linux-surface & git clone https://github.com/Parinz/Gentoo-Surface`

- Copy this repo's *kernel config* to the linux kernel and navigate to linux.

`cp Gentoo-Surface/config linux/.config & cd linux/`

- Apply the patches to your kernel:

`for i in ../linux-surface/patches/5.10/*.patch; do patch -p1 < $i; done`

- Then make whatever settings you need to the kernel.

`make nconfig`

- When you're done, compile the kernel:

`make -j8 && make -j8 modules_install && make install`

- Install Genkernel to create the Initramfs.

`emerge --ask sys-kernel/genkernel`

- Install Intel micro-code for Intel-CPU's

`emerge --ask --oneshot --noreplace sys-apps/iucode_tool
 emerge --ask --noreplace sys-firmware/intel-microcode`

- Generate Initramfs

`genkernel --install --microcode-initramfs --kernel-config=/usr/src/linux/.config initramfs`

## Fixing Network
- Add *Network Manager [USE FLAG](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/USE#Using_USE_flags)* to your *make.conf*:

`nano /etc/portage/make.conf` To exit do Ctrl+o and Ctrl+x
Flags: `NetworkManager wifi`

- Emerge NetworkManager - [Read here for more info on how to setup]

`emerge -aq net-misc/networkmanager`

- Emerge Linux-Firmware for wifi

`emerge --ask sys-kernel/linux-firmware`

- Follow the [video](https://youtu.be/6yxJoMa05ZM?t=2528) to install GRUB2.

# Contributions
Pull requests are welcomed! And help is always amazing.
