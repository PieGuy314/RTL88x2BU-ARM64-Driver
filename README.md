# RTL88x2BU-ARM64-Driver
Patch ciylnx/rtl88x2bu driver to compile on Raspberry Pi 4B (arm64)

The currentciylnx/rtl88x2bu driver doesn't support compilation on a Raspberry Pi 4 running a 64-bit kernel.

Here's how to fix that...

```bash
sudo apt install bc build-essential dkms git raspberrypi-kernel-headers rsync wget
cd rtl88x2bu
wget https://raw.githubusercontent.com/PieGuy314/RTL88x2BU-ARM64-Driver/main/Makefile.patch
patch Makefile Makefile.patch
VER=$(sed -n 's/\PACKAGE_VERSION="\(.*\)"/\1/p' dkms.conf)
sudo rsync -rvhP ./ /usr/src/rtl88x2bu-${VER}
sudo dkms add -m rtl88x2bu -v ${VER}
sudo dkms build -m rtl88x2bu -v ${VER}
sudo dkms install -m rtl88x2bu -v ${VER}
sudo modprobe 88x2bu
```

Shutdown, plug in your adaptor and check it's their using 'iw list' and 'ip addr'.
