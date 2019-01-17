# Speedgoat Vagrant Configuration

This set of configurations files let you create a Vagrant based VM using Virtualbox with the necessary resources pre-installed to build our fork of the Mathworks Buildroot system.

## Getting Started

These instructions will get you a running system on your Virtualbox hosting machine.

### Prerequisites

Install Virtualbox and Vagrant on a Linux based System. Vagrant is a powerful provisioning tool for automating VM creation:
https://www.vagrantup.com/

After installing Virtualbox and Vagrant on your host machine, be sure to add the Vagrant addons that are necessary for the system defined by the Vagrantfile. These include:

1. vagrant-disksize (lets you quickly and easily change the size of the Virtualbox "box" disk image during provisioning.)
2. vagrant-hostmaster (Lets you change the hostname so you can give your machine a unique ID for the network.)
3. vagrant-vbguest (Installs the Virtualbox Guest OS extensions, and automatically keeps these up to date.)

### Installing

Before running "vagrant up" to initialize the system based on the Vagrantfile, make sure that the relative positions of the files are established and the required installers for the Xilinx tools are available.

Also, be sure to update the user specific parameters at the top of the Vagrantfile. Be sure to run the Vagrantfile in a location that has enough disk space available.

