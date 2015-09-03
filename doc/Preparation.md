Initial Preparation
===================

During the preparation phase you

1. install a Pi using the normal process
   - copy the image to a SD-card
   - boot the system
   - configure the system using *raspi-config*
2. Collect all (relevant) files changed during the initial boot and configuration
3. Copy these files to a template directory
4. Prepare a boot-script for one-time execution during first boot

The following sections explain all the details.

Create the SD-Card
------------------

To create the SD-card, you should use the following command:

    sudo apiinst -i 2015-05-05-wheezy.img -t /dev/sdb -P

This command assumes that your sd-card reader is /dev/sdb. Please change
this according to your hardware, since a wrong device can delete
all your data.

The *-i* option defines the input-source. This is the raspbian-image
of your choice. Note that you don't even have to unzip the downloaded
archive, since *apiinst* will happily do that on the fly.

The *-t* option defines the target device the command will write to. As
noted above, it is important to get this right.

Finally, the *-P* option turns on *preparation-mode*. This will copy the
image to the sd-card using dd and do some additional manipulation of
the fake hardware clock time (file */etc/fake-hwclock.data*).

Prepare a helper USB-Stick
--------------------------

Fetch an USB-stick and copy the file *colfiles* from the bin-directory of
this project to the stick. The stick does not have to be empty, but it
should at least have one MB of free disk space. Put this stick aside for
later use.


Booting the System
------------------

Now remove all network cables or WLAN-dongles from your Pi, attach a monitor
and a keyboard and boot the system. It is very important that the Pi does
not have access to the internet, since we don't want the Pi to update it's
time.

Once bootet, the system starts the configuration dialog *raspi-config*.
Configure the system to your needs, but don't expand the root partition and
don't reboot the system after configuration.


Collect Files
-------------

When your finished with *raspi-config*, exit the utility and mount the
USB-stick you prepared a few minutes ago. Mount the stick and execute
the following commands:

    sudo mount /dev/sda /mnt
    sudo /mnt/colfiles
    sudo umount /mnt

You might have to replace */dev/sda* with */dev/sda1*. This depends on your stick.
The stick now contains the file *apiinst_files.tgz*.

The script *colfiles* consists mainly of two commands: a find command which
searches all files changed in the last hour, and a tar command which packs
them into an archive. The find command only works if the time of the 
Pi does not change, this is the reason why it should not be connected to
the network.


Create the template directory
-----------------------------

Mount the stick on your PC, create a directory and unpack the archive:

    sudo mkdir -p /var/lib/apiinst/template
    sudo tar -xvzpf /mnt/apiinst_files.tgz -C /var/lib/apiinst/template

Of course you can change the location of the template-directory to your
personnel needs. The second command assumes that the stick was mounted to
/mnt. Note that unpacking with sudo is necessary to keept the correct
ownership and access-rights for the files.

Add other files of your choice to the template directory. I for example
always add my network configuration file and my ssh host keyfiles. I
also add the home-directories for my users with various config files
like editor settings and so on. 

At this point, you are done with the one-time preparation work. If you
need to reinstall your Pi, you can now use a command like

    sudo apiinst -i -i 2015-05-05-wheezy.img -t /dev/sdb /var/lib/apiinst/template

Again, make sure that */dev/sdb* is the correct device. The installer
supports a number of options, please read the doc (apiinst.md) for details.

One (optional) thing is still missing. If you like to update your system
automatically, add some packages and so on, you also need a one-time
boot-script. It has to be in the template-directory and must be named 
*.../usr/local/sbin/apiinst2*. For details, read the document FirstBoot.md.


A note on SSH-keys
------------------

During initial configuration, the system creates a set of unique SSH host keys.
With the procedure described above, these keys are stored in your
template directory and everytime you reinstall your Pi, it will run with
the same host keys.

The advantage of this setup is, that connections using ssh won't complain
about illegal keys after a fresh installation. The drawback is that if
you clone multiple systems using apiinst, all systems will have the same
set of ssh keys which is not what you want. So you in the latter case
it is better to remove the ssh host keys from the template directory.