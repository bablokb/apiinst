The Automatic Pi Installer
==========================

News
----

- **2019/03**  added support for UUIDs instead of device-names
- **2018/11**: add support for additional system-partitions
- **2016/07**: support images, optionally targeted at QEmu
               (see [qemu.md](./doc/qemu.md "qemu.md"))
- **2016/06**: added support for creating a data-partition
- **2015/09**: added support for HDD/SDD installation
- **2015/09**: initial version


Overview
--------

The *Automatic Pi Installer* is a tool (script) that automates various
tasks during installation of a Raspberry Pi. It supports direct
installation of standard Raspbian images to (micro) SDHC, to HDDs or SSDs.
It also allows direct manipulation of image-files and has an option
to convert images for QEmu.

Installing Raspbian to an USB-connected HDD/SDD-device usually speeds up the
Pi if you are doing a lot of disk I/O, since the throughput of such a device
is typically better than the throughput of the SD-card. But the installation
is a bit more complicated than the standard `dd`.

The same holds true if you want to run Raspbian within QEmu. The standard
images need some manipulation before the system actually boots within
the emulator. Apiinst takes care of all issues and hides the complexities.

Although the script has these simple, direct use cases, it's real strength
lies in the preconfiguration of Raspbian already while writing the image to
the SD-card.

Those of us who need to install a Pi over and over again and are tired of
going through the normal process of configuration with `raspi-config` each
time will profit the most.


Installing and preconfiguring Raspbian
--------------------------------------

The process consists of a one-time preparation phase followed by
(multiple) installation(s).

During the preparation phase you

1. install a Pi using the normal process
   - copy the image to a SD-card
   - boot the system
   - configure the system using `raspi-config`
2. Collect all (relevant) files changed during the initial boot and configuration
3. Copy these files to a template directory
4. Prepare a boot-script for one-time execution during first boot

The next time you install a Pi, you

1. run the script `bin/apiinst` and pass the template directory. This script
   - copies the image to the SD-card
   - expands the root-filesystem
   - then copies all files from the template-directory to the SD-card
2. now boot your Pi with the newly created SD-card
3. No more additional configuration with `raspi-config` is necessary ;-)


Project structure
-----------------

This project consists of some utility-files in the `bin`-directory,
mainly the main `apiinst` script and a utility-script called `colfiles`
which automatically collects all modified files as described above.

The `files`-directory hosts some template files you typically need
for the initial boot phase. Copy these files to your own template
directory and modify them as necessary.

In the `doc`-directory you find all the necessary documentation:

  - [Preparation.md](./doc/Preparation.md "Preparation.md"): describes the
    preparation process in detail
  - [FirstBoot.md](./doc/FirstBoot.md "FirstBoot.md"): execute a script
    during first boot
  - [apiinst.md](./doc/apiinst.md "apiinst.md"): How to use the script
    `bin/apiinst`
  - [qemu.md](./doc/qemu.md "qemu.md"): Prepare an image for QEmu
