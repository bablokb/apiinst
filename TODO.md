TODO
====

- install to given partition
  * unzip to temp. file
  * loop-mnt image
  * mount target partition
  * rsync partition 2 to target partition
  * rsync partition 1 to target/boot
  * umount source
  * remove temp. file
  * copy templates
  * fix cmdline.txt / fstab
  * move target/boot to target/_boot

Later
-----

- bin/apiinst
  * high:   remove init=/usr/lib/raspi-config/init_resize.sh from cmdline.txt
            (unless -n is specified)
  * high:   enable optional fs-type for -D and -H, e.g.: -D 100G/xfs 
  * medium: add option to copy root-partition to SDHC even if root is
            on second usb device
  * medium: add option for explicit root-size
  * low:    add option -f: force shrinking of root if necessary
  * low:    support archives in addition to directories
