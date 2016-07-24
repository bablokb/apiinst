QEmu-Support
============

Apiinst now offers support for creating images which are ready to boot
within QEmu.

Note that for running this image you still need a special kernel,
extracting the kernel from the Raspbian-image does not work (this
will change with newer versions of QEmu starting from 2.6).


Create an Out-of-the-Box Image for QEmu
---------------------------------------

To create a simple image ready for QEmu, you can run the following command:

    sudo apiinst -i 2016-05-27-jessie-lite.zip -t jessie-qemu.img -Q

This command just unpacks the image from the zip-file and then changes the
files `/boot/cmdline.txt`, `/etc/ld.so.preload` and `/etc/fstab`.


Create a 8GB image for QEmu
---------------------------

If you need more space, use the following commands:

    sudo dd if=/dev/zero of=jessie-8G.img bs=32M count=280 conv=sparse
    sudo apiinst -i 2016-05-27-jessie-lite.zip -t jessie-8G.img -Q
    sudo qemu-img convert -O qcow2 jessie-8G.img jessie-8G.qcow2
    sudo rm jessie-8G.img

The third command converts the image to a special QEmu-format with
additional functionality (it even  takes up less space than the sparse
image itself).

Limitations
-----------

Be sure your special qemu-kernel supports your version of Raspbian. Most
recipies available on the net are from pre-jessie times and the recommended
kernel-configurations do not fully support systemd, which is an integral
part of Debian-Jessie.

