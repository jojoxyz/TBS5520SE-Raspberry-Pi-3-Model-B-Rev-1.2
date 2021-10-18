# TBS5520SE-Raspberry-Pi-3-Model-B-Rev-1.2

The latest version of raspbian 5.10.63-v7+ not work.
---------------------------------------------------------------------------------
------------------2019-04-08-raspbian-stretch-lite-------------------------------
--------------4.14.98-v7+  / upgraded to /  4.19.66-v7+
---------------------------------------------------------------------------------
Upgrade required for mismatch headers when installing raspberrypi-kernel-headers
#
sudo apt update
sudo apt full-upgrade
sudo reboot

sudo apt install raspberrypi-kernel-headers
or
sudo apt install rbp2-headers-$(uname -r)         // osmc

sudo reboot

sudo apt install libproc-processtable-perl
sudo apt install build-essential
sudo apt install patchutils
sudo apt install git -y
sudo apt install gcc-4.9 g++-4.9 -y
sudo apt install mc -y
sudo apt install libncurses5-dev -y
sudo reboot

git clone https://github.com/tbsdtv/media_build.git
git clone --depth=1 https://github.com/tbsdtv/linux_media.git -b latest ./media

cd media_build
make dir DIR=../media

------------ Enable some staging drivers------------
make stagingconfig

------------    Disable RC/IR support --------------

sed -i -r 's/(^CONFIG.*_RC.*=)./\1n/g' v4l/.config
sed -i -r 's/(^CONFIG.*_IR.*=)./\1n/g' v4l/.config

make    //1 core = 60 min  or make -j4 

sudo rm -r -f /lib/modules/$(uname -r)/updates/extra
sudo make install

wget http://www.tbsdtv.com/download/document/linux/tbs-tuner-firmwares_v1.0.tar.bz2
sudo tar jxvf tbs-tuner-firmwares_v1.0.tar.bz2 -C /lib/firmware/

sudo reboot

---------------- Show installed DVB´s --------------

dmesg | grep frontend
dmesg | grep -i dvb
dmesg | grep TBS
lsusb -vvv | grep 5521
