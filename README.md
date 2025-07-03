# YOCTO-on-beagleboneblack
YOCTO on beagleboneblack


You must use Ubuntu 18. It won’t compiled on higher distribution.

Dependencies

sudo apt install gawk wget git diffstat unzip texinfo gcc build-essential chrpath socat cpio python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev pylint3 xterm python3-subunit mesa-common-dev zstd liblz4-tool
Download the yocto
```
git clone git://git.yoctoproject.org/poky -b kirkstone
cd poky
source oe-init-build-env
```
Create a source directory in the poky's directory
```
mkdir ../../sources

nano ./conf/local.conf
```
Put the exact path of the sources to local.conf
Change the machine name
```
MACHINE ?= "beaglebone-yocto"
```
Add following variables too
```
SOURCES="/home/asd/Documents/beagleboneblack/yocto_tutorial/sources"
DL_DIR ?= "${SOURCES}/downloads"
SSTATE_DIR ?= "${SOURCES}/sstate-cache"
TMPDIR = "${SOURCES}/tmp"
```
and

```
bitbake core-image-full-cmdline
```
Under the sources directory that we added

This is the image file that we need to flash
```
core-image-full-cmdline-beaglebone-yocto.wic

sudo picocom -b 115200 /dev/ttyUSB0
```

![image](https://github.com/user-attachments/assets/fc5d5e93-64f9-47d4-a56a-14756272aba2)

```
bitbake -c menuconfig virtual/kernel
```

![image](https://github.com/user-attachments/assets/f98cd177-24bc-41c9-939a-df3094097d5f)

```
bitbake -c savedefconfig virtual/kernel
```

vmlinux is under the this folder

![image](https://github.com/user-attachments/assets/47bb1460-5bd8-431b-99f5-8df520357632)

Creating a new layer for a OLED LCD Driver

bitbake-layers create-layer ../../meta-lcd
![image](https://github.com/user-attachments/assets/a2df81ac-368a-4dc9-8b2d-3fbbe1987ead)


Adding the layer to bbfiles i guess
```
bitbake-layers add-layer ../../meta-lcd
```
![image](https://github.com/user-attachments/assets/64389758-b3a8-4f97-92c2-ce6640edd9ee)

```
cp the /meta-skeleton/recipes-kernel to the layer’s folder
```
change names according to this link

https://community.nxp.com/t5/i-MX-Processors-Knowledge-Base/Incorporating-Out-of-Tree-Modules-in-YOCTO/ta-p/1373825

```
#Add this line at the end of local.conf
IMAGE_INSTALL_append = " pmu-mod"
This should be 
IMAGE_INSTALL:append = ” pmu-mod”
```

in new version

Good luck
