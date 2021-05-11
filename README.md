## Very light ubuntu machine with qtcreator, cern root, geant4 and Virtualbox guest addition installed

### Link to ova file of the VM machine:

Based on Lubuntu 16.04 LTS, RAM memory usage at startup: 200 MB



### Copy data from host computer

Setup Bridged Adapter on VirtualBox

ifconfig for host and vm machine!

### For qtcreator

sudo apt install libgstreamer0.10-0 libgstreamer0.10-dev libgstreamer-plugins-base0.10-0

sudo apt install git

git clone https://github.com/vihophong/qtcmakeroot.git

### For geant4

sudo apt-get install libxerces-c-dev qt5-default freeglut3-dev libmotif-dev tk-dev cmake cmake-curses-gui libxpm-dev libxmu-dev libxi-dev

sudo apt purge --auto-remove cmake

wget -O - https://apt.kitware.com/keys/kitware-archive-latest.asc 2>/dev/null | gpg --dearmor - | sudo tee /etc/apt/trusted.gpg.d/kitware.gpg >/dev/null

sudo apt-add-repository 'deb https://apt.kitware.com/ubuntu/ xenial main'

sudo apt update

sudo apt install cmake

sudo apt install cmake-curses-gui

#### Install Coin3D for Inventor 


(1) git clone --recurse-submodules https://github.com/coin3d/coin coin
 
(2) cmake -Hcoin -Bcoin_build -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX=/home/lubuntu/coin3d -DCMAKE_BUILD_TYPE=Release -DCOIN_BUILD_DOCUMENTATION=OFF

(3) cd coin_build

sudo apt install libboost-dev

make

(4) make install

(5) cd cpack.d

cpack --config debian.cmake

cpack --config fedora.cmake

specify Coin_DIR as /data/geant4/coin3d/lib/cmake/Coin-4.0.1

git clone --recurse-submodules https://github.com/coin3d/soqt.git

cmake -Hsoqt -Bsoqt_build -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX=/home/lubuntu/coin3d -DCMAKE_BUILD_TYPE=Release -DSOQT_BUILD_DOCUMENTATION=OFF

cd soqt_build

make

make install


git clone --recurse-submodules https://github.com/coin3d/soxt.git

cmake -Hsoxt -Bsoxt_build -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX=/home/lubuntu/coin3d -DCMAKE_BUILD_TYPE=Release -DSOXT_BUILD_DOCUMENTATION=OFF

cd soxt_build

make

make install


cmake ../geant4.10.07.p01

ccmake 

specify SOQT_DIR and SOXT_DIR as /data/geant4/coin3d/lib/cmake/SoQt-1.6.0 and /data/geant4/coin3d/lib/cmake/SoXt-1.4.0

### For cern root
sudo apt-get install dpkg-dev cmake g++ gcc binutils libx11-dev libxpm-dev \
libxft-dev libxext-dev python libssl-dev

sudo apt-get install gfortran libpcre3-dev \
xlibmesa-glu-dev libglew1.5-dev libftgl-dev \
libmysqlclient-dev libfftw3-dev libcfitsio-dev \
graphviz-dev libavahi-compat-libdnssd-dev \
libldap2-dev python-dev libxml2-dev libkrb5-dev \
libgsl0-dev libqt4-dev

### Install virtual box guest addition

sudo apt update

sudo apt-get install -y dkms linux-headers-generic linux-headers-$(uname -r)

sudo sh /media/lubuntu/VBox_GAs_6.1.4/VBoxLinuxAdditions.run

restart

### Shared folder

Open shared folder setting in Virtual Box

Specify shared Folder paht in host machine

Specify mount point in VM: /home/lubuntu/mnt
 
(mkdir /home/lubuntu/mnt)

Check options: Auto mount and make permanent

after that, add user to group vboxsf:

sudo adduser $USER vboxsf

restart or logout

### Resize virtual box hard disk (from to 6 gb  to 8 gb= 8192 mb, additional 2 gb allocated for new partition)

#### In Mac

cd /Applications/VirtualBox.app/Contents/Resources/VirtualBoxVM.app/Contents/MacOS

VBoxManage modifyhd --resize 8192 /Users/phongvi/VirtualBox\ VMs/lubuntu/lubuntu.vdi

#### In lubuntu

Verify the partitions available on the server: fdisk -l

Choose which device you wish to use (such as /dev/sda)

Run fdisk /dev/sdX (where X is the device you would like to add the partition to)

Type ‘n’ to create a new partition.

Specify where you would like the partition to end and start: the start block is the last block (end) in listed /dev/sda* partition, the end block can be default value

Run the command ‘partprobe’ to have the OS detect the new partition table.  If it still does not detect the partition table, you might need a reboot.

Format the partition by doing:  ‘mke2fs -j /dev/sdaX’ – where X is the number of the partition you have created.

Create a directory where you wish to mount the new drive, for example: /newpartition.  ‘mkdir -p /newpartition’

To mount, you can use the following command: ‘mount /dev/sdaX /newpartition’

If you would like the drive to be mounted automatically each time you boot, add the following to /etc/fstab: ‘/dev/sdaX /newpartition ext3 defaults 1 2’

sudo mkdir /data

add the to /etc/fstab: ‘/dev/sda3 /data ext3 defaults 1 2’

sudo chown -R lubuntu:lubuntu /data

