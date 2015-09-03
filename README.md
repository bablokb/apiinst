The Automatic Pi Installer
==========================

Overview
--------

The *Automatic Pi Installer* is a tool (script) for those of us who need
to install a Pi over and over again and are tired of going through the
normal process of configuration with `raspi-config` each time.

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
