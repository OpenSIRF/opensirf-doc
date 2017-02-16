# OpenSIRF Documentation

Installing a development environment
===============================

Use the `install-dev.sh` script to install a development environment in your Linux system.

Requirements:
* Vagrant
* VirtualBox

`$ wget https://raw.githubusercontent.com/OpenSIRF/opensirf-server/develop/install-dev.sh`

`$ bash install-dev.sh`

The script will install three virtual machines:

* OpenSIRF Server
* A VM with OpenStack Swift and OpenStack Identity
* A VM with a volume group and two logical volumes set up

