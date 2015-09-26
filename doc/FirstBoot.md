First Boot
==========

The automatic Pi installer tries to do as much work as possible during the
creation phase of the SD-card. But a number of steps require a running
system. So these installation tasks are postponed to a script running at
first boot.

If you add a script named `/usr/local/sbin/apiinst2` to your template
directory, then `apiinst` will automatically change the file `/etc/rc.local`
to execute that script at startup.

To simplify things, this script contains a sample implementation of apiinst2
in the directory `files/usr/local/sbin/`. This script will do the following

  - prevent login
  - check if SSH host keys exist and regenerate if them if necessary
  - update the package system
  - install some packages (bc, jed, zip)
  - remove some packages (scratch squeak-vm)
  - add a new user (named tux)
  - reenable login

At the end, it removes the executable-flag from the file, so it won't be
executed on future boots.

This is really only a sample implementation, you should adapt the script
to your needs (e.g. remove the prevent login/reenable login stuff ;-).
The minimum you should do is to remove the creation of
the user tux (or at least change the default password).

