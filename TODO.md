TODO
====

Data-Partition
--------------

- test
  * -H, -D, -S at the same time (-> error)
  * creation of home+swap
  * creation of data+swap


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
