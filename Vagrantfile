# -*- mode: ruby -*-
# vi: set ft=ruby :

require_relative "credential"
include Credential

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

$dependency = <<DEPENDENCY
apt-get update
apt-get install -qq python-software-properties
apt-add-repository -y ppa:miurahr/qt5
apt-get update
apt-get install -y dpkg-dev cdbs debhelper dh-buildinfo
apt-get install -y devscripts quilt
apt-get install -y autotools-dev doxygen
apt-get install -y fdupes libdbus-1-dev libglib2.0-dev libgstreamer0.10-dev libgstreamer-plugins-base0.10-dev libpulse-dev libsqlite3-dev libudev-dev libxml2-dev libxslt1-dev python3-all-dbg python3-all-dev python3-dbus python3-dbus-dbg python-all-dbg python-all-dev python-dbus python-dbus-dbg python3-sphinx python-dbus-dev 
apt-get install -y python2.7-dev python-setuptools txt2man python-sphinx python-pil python-lxml python-mechanize python-beautifulsoup python-dateutil python-pkg-resources python-cssutils python-cssselect python-cherrypy3 python-markdown python-pyparsing python-routes python-chardet python-netifaces libmagickwand-dev libsqlite3-dev python-sip-dev libmtp-dev poppler-utils libpodofo-dev libboost-dev libudev-dev libmtdev-dev libchm-dev
apt-get install -y libqt5opengl5-dev libqt5webkit5-dev qtmultimedia5-dev qtdeclarative5-dev qttools5-dev libqt5svg5-dev libqt5xmlpatterns5-dev
apt-get install -y qtchooser libicu-dev
DEPENDENCY

$buildsip = <<BUILDSIP
cd /home/ubuntu
wget -c http://archive.ubuntu.com/ubuntu/pool/main/s/sip4/sip4_4.15.5-1build1.dsc
wget -c http://archive.ubuntu.com/ubuntu/pool/main/s/sip4/sip4_4.15.5.orig.tar.gz
wget -c http://archive.ubuntu.com/ubuntu/pool/main/s/sip4/sip4_4.15.5-1build1.debian.tar.gz
dpkg-source -x sip4_4.15.5-1build1.dsc
cd sip4-4.15.5
patch -i /vagrant/sip4.patch -p1
patch -i /vagrant/sip4-2.patch -p1
debuild -us -uc -b -i
cd ..
sudo dpkg -i python-sip*deb python3-sip*deb sip-dev*deb
BUILDSIP

$installsip = <<INSTALLSIP
wget -c https://s3-us-west-2.amazonaws.com/calibre2/python-sip-dbg_4.15.5-1build1_amd64.deb
wget -c https://s3-us-west-2.amazonaws.com/calibre2/python-sip-dev_4.15.5-1build1_amd64.deb
wget -c https://s3-us-west-2.amazonaws.com/calibre2/python-sip-doc_4.15.5-1build1_all.deb
wget -c https://s3-us-west-2.amazonaws.com/calibre2/python-sip_4.15.5-1build1_amd64.deb
wget -c https://s3-us-west-2.amazonaws.com/calibre2/python3-sip-dbg_4.15.5-1build1_amd64.deb
wget -c https://s3-us-west-2.amazonaws.com/calibre2/python3-sip-dev_4.15.5-1build1_amd64.deb
wget -c https://s3-us-west-2.amazonaws.com/calibre2/python3-sip_4.15.5-1build1_amd64.deb
wget -c https://s3-us-west-2.amazonaws.com/calibre2/sip-dbg_4.15.5-1build1_amd64.deb
wget -c https://s3-us-west-2.amazonaws.com/calibre2/sip-dev_4.15.5-1build1_amd64.deb
sudo dpkg -i python-sip*deb python3-sip*deb sip-dev*deb
INSTALLSIP

$buildpyqt = <<BUILDPYQT
cd /home/ubuntu
wget -c http://archive.ubuntu.com/ubuntu/pool/main/p/pyqt5/pyqt5_5.2.1+dfsg-1ubuntu1.dsc
wget -c http://archive.ubuntu.com/ubuntu/pool/main/p/pyqt5/pyqt5_5.2.1+dfsg.orig.tar.gz
wget -c http://archive.ubuntu.com/ubuntu/pool/main/p/pyqt5/pyqt5_5.2.1+dfsg-1ubuntu1.debian.tar.xz
dpkg-source -x pyqt5_5.2.1+dfsg-1ubuntu1.dsc
cd pyqt5-5.2.1+dfsg
patch -i /vagrant/pyqt5.patch -p1
debuild -us -uc -b -i
cd ..
sudo dpkg -i -y pyqt5-dev*.deb python-pyqt5*deb pyqt5-dev-tools*deb
BUILDPYQT

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "dummy"

  config.vm.synced_folder ".", "/vagrant", type: "rsync", rsync__exclude: ".git/"

  config.vm.provider :aws do |aws, override|
    # private credentials
    aws.access_key_id = Access_key_id
    aws.secret_access_key = Secret_access_key
    aws.keypair_name = SSH_Key_pair_name
    override.ssh.private_key_path = SSH_Private_key_path

    # EC2 configuration
    aws.instance_type = "m3.medium"
    aws.region = "us-west-2"
    aws.security_groups = "quicklaunch-1"
    aws.tags = {'Name' => 'calibre2 compile'}

    # Ubuntu 12.04.01(64bit) LTS
    aws.ami = "ami-23f78e13"

    # To build pyqt5 require 4GB, for calibre2 request ?GB
    #aws.block_device_mapping = [{ 'DeviceName' => '/dev/sda1', 'Ebs.VolumeSize' => 8 }]

    override.ssh.username = "ubuntu"
  end

  config.vm.provision "shell", inline: $dependency, privileged: true
  #config.vm.provision "shell", inline: $buildsip, privileged: false
  config.vm.provision "shell", inline: $installsip, privileged: false
  config.vm.provision "shell", inline: $buildpyqt, privileged: false

end
