
Since version 3.0, Gadgetron supports building the chroot image as a way for deployment on the Ubuntu system.

# Build gadgetron chroot package

suppose the gadgetron source code is in gadgetron-source folder and the building folder is gadgetron-build

cd gadgetron-build

cmake ../gadgetron-source

make -j12

make install

sudo make chroot

This command will install an ubuntu bootstrap environment and install gadgetron inside the chroot. It will tar and zip the package. Therefore, after the command completes, in the gadgetron-build/chroot/chroot-backups folder, there will be a .tar.gz and .img file. 

For example, in my system, they can be called:

gadgetron-20150129-1137-e207d6ed.img

gadgetron-20150129-1137-e207d6ed.tar.gz

The naming is gadgetron-date-time-first 8 numbers of SHA1 key for gadgetron code base.
The .img file is a hard-disk image file containing the same content with .tar.gz file.

# Run gadgetron from chroot image

In the gadgetron-build/chroot/chroot-backups folder, there are a few .sh scripts. Among them, start-gadgetron-from-image.sh can be used to start the gadgetron form the chroot .img file. Its usage is like:

cd gadgetron-build/chroot/chroot-backups

sudo ./start-gadgetron-from-image.sh ./gadgetron-20150129-1137-e207d6ed.img ~/gadgetron_chroot/mount_point

# Install gadgetron chroot package as an upstart service

We can also install the gadgetron chroot package as a upstart service on an ubuntu machine. To do this, there is an installation script in gadgetron-source/chroot/install_chroot_image.sh. For example, to install the chroot image i just built,

sudo gadgetron-source/chroot/install_chroot_image.sh ./gadgetron-20150129-1137-e207d6ed.tar.gz

This command will create a gadgetron chroot install point as /home/gadgetron_chroot and untar this chroot package. 

A symbolic link /home/gadgetron_chroot/current will be set up to point to /home/gadgetron_chroot/gadgetron-20150129-1137-e207d6ed. The upstart script gadgetron_chroot.conf will be copied into /etc/init. The service "gadgetron_chroot" will be started, which runs gadgetron on port 9002 by default. 

If another chroot package is going to be installed, just re-run this script and it will change where this symbolic link points to and restart the gadgetron_chroot service.