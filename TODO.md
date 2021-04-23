TODO
====

Bugs
----

Later
-----

- bin/apiinst
  * high:   remove init=/usr/lib/raspi-config/init_resize.sh from cmdline.txt
            (unless -n is specified)
  * low:    enable optional fs-type for -D and -H, e.g.: -D 100G/xfs
  * low:    add option -f: force shrinking of root if necessary
  * low:    support archives in addition to directories
