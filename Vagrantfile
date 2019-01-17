# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

# This is a template vagrant file for setting up a system for working on the 
# buildroot system. Change the values below until the row of hashes to be
# user specific. B. von Gunten 1/10/19 brian.vongunten@speedgoat.ch

# Username is always set to vagrant, but you can change your password after 
# logging in the first time. You should give the system a nice hostname though 
# so it shows up on the network as something reasonable
$hostname="Developer-MPSoC-DEV-VM"
$RDP_PORT="50xx"

# Supply your username and password here so you can access the SVN shares.
# These will be created in the vagrant user home folder

#Add these values quickly before building, then remove them! (Security!)
$WIN_USER="username"
$WIN_PASS="password"

# Some GIT user name values that will be used to set up your git environment.
# This is only used for annotating commits.
$GIT_USER_var="username"
$GIT_EMAIL_var="mail@speedgoat.ch"

$SVN_USER_var="username"
$SVN_PASS_var="svnpassword"

################################################################################

$set_environment_variables = <<SCRIPT
  tee "/etc/profile.d/myvars.sh" > "/dev/null" <<EOF
  # Vivado Environment Variables
   export XILINXD_LICENSE_FILE=2100@michelangelo
   export SVN_USER=$1
   export SVN_PASS=$2
   export GIT_USER=$3
   export GIT_EMAIL=$4
  
  
  
EOF
SCRIPT

# 01/08/19 Install the Linaro toolchain, which is used by the Mathworks buildroot.
$install_linaro_toolchain = <<-SCRIPT
  mkdir -p /opt/linaro/aarch64-6.3.1-2017.02
  cd /opt/linaro/aarch64-6.3.1-2017.02
  wget https://releases.linaro.org/components/toolchain/binaries/6.3-2017.02/aarch64-linux-gnu/gcc-linaro-6.3.1-2017.02-x86_64_aarch64-linux-gnu.tar.xz
  tar -xvf gcc-linaro-6.3.1-2017.02-x86_64_aarch64-linux-gnu.tar.xz --strip-components=1
  rm gcc-linaro-6.3.1-2017.02-x86_64_aarch64-linux-gnu.tar.xz
  cd ..
  mkdir aarch32-6.3.1-2017.02
  cd aarch32-6.3.1-2017.02
  wget https://releases.linaro.org/components/toolchain/binaries/6.3-2017.02/arm-linux-gnueabihf/gcc-linaro-6.3.1-2017.02-i686_arm-linux-gnueabihf.tar.xz
  tar -xvf gcc-linaro-6.3.1-2017.02-i686_arm-linux-gnueabihf.tar.xz --strip-components=1
  rm gcc-linaro-6.3.1-2017.02-i686_arm-linux-gnueabihf.tar.xz
SCRIPT

$install_apt = <<-SCRIPT
  apt-get update
  apt-get install -y ubuntu-desktop
  add-apt-repository ppa:notepadqq-team/notepadqq
  apt-get update
  apt-get install -y  notepadqq terminator
  add-apt-repository -y ppa:rabbitvcs/ppa
  apt-get install -y rabbitvcs-core
  apt-get install -y rabbitvcs-nautilus
  dpkg --add-architecture i386
  apt-get update
  apt-get install -y tofrodos
  apt-get install -y iproute gawk make net-tools ncurses-dev tftp tftpd libssl-dev zlib1g-dev lib32z1 unzip xvfb chrpath socat autoconf libtool texinfo emacs24-nox
  apt-get install -y gcc-multilib libsdl1.2-dev libglib2.0-dev
  apt-get install -y flex
  apt-get install -y bison
  apt-get install -y expect
  apt-get install -y bc
  apt-get install -y libc6:i386 libncurses5:i386 libstdc++6:i386 lib32z1
  apt-get install -y zlib1g:i386
  apt-get install -y cifs-utils
  apt-get install -y dialog
SCRIPT

#sets up the TFTP server, and changes shell to bash instead of dash.
$setup_for_petalinux = <<-SCRIPT
  cp /installers/tftpd /etc/xinetd.d/tftpd
  mkdir -p /tftpboot
  chmod -R 777 /tftpboot
  chown -R nobody:nogroup /tftpboot
  ln -s /etc/init.d/xinetd /etc/rc5.d/S40xinetd
  /etc/init.d/xinetd restart
  rm /bin/sh
  ln -s /bin/bash /bin/sh
  mkdir -p /opt/pkg/petalinux
  chown vagrant /opt/pkg/petalinux
  chmod 755 /opt/pkg/petalinux
SCRIPT

$create_vivado_folder = <<-SCRIPT
  mkdir /opt/Xilinx
  chown vagrant /opt/Xilinx
  chmod u+rwX /opt/Xilinx
SCRIPT

$install_vivado = <<-SCRIPT
  /installers/Xilinx_Vivado_SDK_2017.4_1216_1/xsetup -a XilinxEULA,3rdPartyEULA,WebTalkTerms -b Install -c /installers/vivado_2017_4_install_config.txt
SCRIPT

$install_petalinux = <<-SCRIPT
  expect /installers/expect.exp
SCRIPT

# How i branched the mathworks release.
# git checkout tags/mathworks_zynq_R18.2.1 -b speedgoat_zynq_R18.2.1
# git checkout speedgoat_zynq_R18.2.1
# git push
# git push --set-upstream origin speedgoat_zynq_R18.2.1
# git add -all
# git add --all
# git commit
# git push

$update_svn = <<-SCRIPT
  sudo bash -c 'echo "192.168.21.1    enterprise.liebefeld.local" >> /etc/hosts'
  cd svn
  svn update FPGA/projects --non-interactive --trust-server-cert --username=$SVN_USER --password=$SVN_PASS
  cd ..
SCRIPT

$set_up_versioning = <<-SCRIPT
  sudo bash -c 'echo "192.168.21.1    enterprise.liebefeld.local" >> /etc/hosts'
  mkdir svn
  cd svn
  svn co --non-interactive --trust-server-cert --username=$SVN_USER --password=$SVN_PASS https://enterprise.liebefeld.local:8443/svn/FPGA/projects FPGA/projects
  cd ..
  mkdir git
  cd git
  git clone https://github.com/SpeedgoatGmbH/buildroot
  git clone https://github.com/enclustra-bsp/bsp-xilinx.git
  cd buildroot
  git checkout speedgoat_zynq_R18.2.1
  cd ../bsp-xilinx
  git checkout develop
  git config --global user.name "$GIT_USER"
  git config --global user.email "$GIT_EMAIL"
SCRIPT

$edit_fstab = <<-SCRIPT
  echo "username=$WIN_USER" | tee --append /root/cifs_creds
  echo "password=$WIN_PASS" | tee --append /root/cifs_creds
  chmod 700 /root/cifs_creds
  echo "//defiant/shares/MPSoC /MPSoC cifs uid=1000,gid=1000,credentials=/root/cifs_creds,sec=ntlm 0 0" | tee --append /etc/fstab
SCRIPT

Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/xenial64"

  config.vm.define $hostname
  
  #Resize disk using the vagrant-disksize plugin

  config.disksize.size = '250GB'
  
  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  config.vm.hostname = $hostname

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.

  config.vm.network "public_network", bridge: "eno1"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.customize "post-boot",["controlvm", :id, "vrde", "on"]
    vb.customize ["modifyvm", :id, "--vrdeport", $RDP_PORT] # change here to a free port
    #Give the VM more memory and cpus
    vb.customize ["modifyvm", :id, "--memory", 8192]
    vb.customize ["modifyvm", :id, "--cpus", 8]
    config.vm.post_up_message = "The VM is ready!"
  end 
  
  
  config.vm.provision "shell", run: "always" do |s|
    s.inline = $set_environment_variables
    s.args = [$SVN_USER_var, $SVN_PASS_var, $GIT_USER_var, $GIT_EMAIL_var]
  end
  config.vm.provision "shell", inline: $install_apt
  config.vm.provision "shell", inline: $edit_fstab
  config.vm.provision "shell", inline: $create_vivado_folder
  config.vm.provision "shell", inline: $install_linaro_toolchain
  config.vm.synced_folder "../installers", "/installers"
  #The Vivado installer should not run with SU privileges
  config.vm.provision "shell", inline: $install_vivado, privileged: false
  config.vm.provision "shell", inline: $setup_for_petalinux
  #config.vm.provision "shell", inline: $install_petalinux, privileged: false
  #SVN should not be checked out with sudo rights.
  config.vm.provision "shell", inline: $set_up_versioning, privileged: false
  config.vm.provision "shell", inline: $update_svn, run: "always"
end
