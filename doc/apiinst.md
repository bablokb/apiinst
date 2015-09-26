apiinst
=======

This is the SD-card creation script which does all the magic. The scripts
must be run as root and has builtin help:

    apiinst: Write an (Raspberry Pi) image of a sd-card to a physical sd-card
             and copy additional files from all given directories to the sd-card
  
    usage: apiinst -i image -t target [options] [dir [...]]
      Possible options:
        -i image    source image (required)
        -t target   target device (required, e.g. /dev/sdc)
        -B boot     boot device (optional, target device is USB-HDD/SSD)
    
        -P          preparation mode
    
        -H size     create a home-partition of given size (default: no home)
                    (use size>0 or 'rest')
        -S size     create a swap--partition of given size (default: no swap)
                    (use size>0 or 'rest')
        -n          don't expand root partition
                    (note that '-H rest' or '-S rest' will also prevent expansion)
    
        -k          keep all files
        -L logfile  write log-messages additionally to given logfile
        -1 script   run the given script after  copying the image to SD
        -2 script   run the given script after  resizing/partition creation
        -3 script   run the given script after  copying template files to the SD
        -h          show this help


Prerequisites
-------------

`apiinst` is a shell script. It uses various tools to execute it's tasks.
Specfically, you need:

  - fdisk 
  - funzip 
  - gunzip 
  - bunzip2 
  - dd
  - partprobe
  - bc 
  - sfdisk 
  - resize2fs 
  - rsync

All of these tools are either part of a standard linux-installation, or they
can be installed easily from your package system.

Some tools are optional:

  - wget
  - realpath

You only need `wget` if you want to feed an image directly from the web to
the install script. With `realpath`, you can also give links to directories
(instead of the directories itself) as input to `apiinst`.


Preparation mode
----------------

With the option `-P` apiinst runs in preparation mode. See the file
[Preparation.md](./Preparation.md "Preparation.md") for details.


Sample invocations
------------------

Here are some sample invocation of the script. The samples always asume that
`/dev/sdb` is the device of your card-reader, please change this according
to your hardware environment.

The first example replicates a simple `dd` of the image to the sd-card:

    sudo apiinst -i 2015-05-05-wheezy.img -t /dev/sdb -n

Raspbian distributes their images in compressed form, and apiinst also
directly processes these images:

    sudo apiinst -i 2015-05-05-wheezy.zip -t /dev/sdb -n

If you have a stable internet connection you can even supply a http-url
as input source.

Without the option `-n` the script also expands the root-partition to it's
maximum size (replicating what `raspi-config` does).

If you want an additional home-partition, just add the option `-H size`, e.g.:

    sudo apiinst -i 2015-05-05-wheezy.zip -t /dev/sdb -H 1GB

This will create a home-partition of one gigabyte. The root-partition is
resized to fill the remaining space. If you want the home-partition to take
up all the free space, use

    sudo apiinst -i 2015-05-05-wheezy.zip -t /dev/sdb -H rest

You could also add a swap partition (using the option `-S size`), but additional
swap is seldom necessary.

The real benefit of apiinst is the possibility to copy files from a
template directory to the sd-card. You can add any number of directories
to the command, all files from all directories are copied to the sd-card
(in the given order):

    sudo apiinst -i 2015-05-05-wheezy.zip -t /dev/sdb -H 1GB \
      config-files-dir/ user-files-dir/

This example uses two directories, one for configuration files and one for
user files. If you want to use a single or multiple directories is a 
matter of taste. Multiple directories come in handy if you want to 
support multiple configurations which share some common files.


File deletion
-------------

After apiinst has created the image, it will delete some files so that
raspi-config does not run anymore automatically if you login to the system.
It also deletes some files which generate new SSH host keys (but only
if you provide your own keys). Anyhow, you can prevent such modifications
with the option `-k`.


Log file
--------

With the option `-L logfilename` apiinst will write all log messages
to that file (in addition to the console).


Add functions using you own scripts
-----------------------------------

The parameters `-1`, `-2` and `-3` allow you to plug in your own scripts
for special processing. Apiinst will provide the current mount-directory
to the third script, so you can access all files on the sd-card using
this parameter.


Installation to HDD/SDD
-----------------------

Apiinst now also supports the installation of a Raspbian image to HDD/SDD.
Technically, the Pi still needs an SD-card for the small boot-partition,
but the large root-partition can reside on an USB connected HDD/SDD or
an USB-stick. Although throuput of an USB-device is still limited, it is
usually better than the throuput of the SD-card.

To install an image to an USB-device, you have to plug in the device in
addition to the SD-card. Assume your SD-card is on `/dev/sdb` and your
USB-connected HDD is on `/dev/sdc`. Then you would run

    sudo apiinst -i 2015-05-05-wheezy.zip -B /dev/sdb -t /dev/sdc

You can combine the additional `-B device`-option with all other options
of `apiinst` as described above.

