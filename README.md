# RTL88x2BU RaspberryPi-4B(arm64) Driver Patch
Patch ciylnx/rtl88x2bu driver to compile and run on RaspberryPi-4B(arm64)

The current ciylnx/rtl88x2bu driver doesn't support compilation on a Raspberry Pi 4 running a 64-bit kernel. The workaround here patches the Makefile. Tested in a RPi4B (4GB) running DietPi (Linux DietPi 5.4.72-v8+ #1356 SMP PREEMPT Thu Oct 22 13:58:52 BST 2020 aarch64 GNU/Linux)

```bash
sudo apt install bc build-essential dkms git raspberrypi-kernel-headers rsync wget
git clone https://github.com/cilynx/rtl88x2bu.git
cd rtl88x2bu
wget https://raw.githubusercontent.com/PieGuy314/RTL88x2BU-RPi4-arm64-Driver-Patch/main/Makefile.patch
patch Makefile Makefile.patch
VER=$(sed -n 's/\PACKAGE_VERSION="\(.*\)"/\1/p' dkms.conf)
sudo rsync -rvhP ./ /usr/src/rtl88x2bu-${VER}
sudo dkms add -m rtl88x2bu -v ${VER}
sudo dkms build -m rtl88x2bu -v ${VER}
sudo dkms install -m rtl88x2bu -v ${VER}
sudo modprobe 88x2bu
```

Shutdown, plug in your adaptor and check it's their using 'iw list' and 'ip addr'.

Driver will be automatically recomplied each time a new kernel is installed.
