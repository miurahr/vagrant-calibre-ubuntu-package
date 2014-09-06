# -*- mode: ruby -*-
# vi: set ft=ruby :

require_relative "credential"
include Credential

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

# apt-fast and PPA
$aptprepare = <<APTPREPARE
apt-get update
apt-get install -qq python-software-properties
apt-get install -y git aria2
cd /tmp
git clone https://github.com/ilikenwf/apt-fast.git
cp apt-fast/apt-fast /usr/bin/
chmod +x /usr/bin/apt-fast
chown root.root /usr/bin/apt-fast
cp apt-fast/apt-fast.conf /etc/
cp apt-fast/completions/bash/apt-fast /etc/bash_completion.d/
chown root.root /etc/bash_completion.d/apt-fast
APTPREPARE

$dependency = <<DEPENDENCY
apt-add-repository -y ppa:miurahr/qt5
apt-add-repository -y ppa:miurahr/calibre
apt-add-repository -y ppa:miurahr/calibre2
apt-fast update
apt-fast install -y dpkg-dev cdbs debhelper dh-buildinfo devscripts quilt autotools-dev doxygen
apt-fast install -y fdupes libdbus-1-dev libglib2.0-dev libgstreamer0.10-dev libgstreamer-plugins-base0.10-dev libpulse-dev libsqlite3-dev libudev-dev libxml2-dev libxslt1-dev python3-all-dbg python3-all-dev python3-dbus python3-dbus-dbg python-all-dbg python-all-dev python-dbus python-dbus-dbg python3-sphinx python-dbus-dev 
apt-fast install -y python2.7-dev python-setuptools txt2man python-sphinx python-imaging python-lxml python-mechanize python-beautifulsoup python-dateutil python-pkg-resources python-cssutils python-cssselect python-cherrypy3 python-markdown python-pyparsing python-routes python-chardet python-netifaces libmagickwand-dev libsqlite3-dev python-sip-dev libmtp-dev poppler-utils libpodofo-dev libboost-dev libudev-dev libmtdev-dev libchm-dev
apt-fast install -y libqt5opengl5-dev libqt5webkit5-dev qtmultimedia5-dev qtdeclarative5-dev qttools5-dev libqt5svg5-dev libqt5xmlpatterns5-dev
apt-fast install -y qtchooser libicu-dev
apt-fast install -y  python-cssselect python-cherrypy3 python-markdown python-pyparsing python-routes python-netifaces libmagickwand-dev qtbase5-private-dev libmtp-dev poppler-utils libpodofo-dev libboost-dev libmtdev-dev libchm-dev txt2man qt5-default python-imaging python-lxml python-mechanize  python-beautifulsoup python-dateutil python-cssutils
DEPENDENCY

$buildsip = <<BUILDSIP
cd /mnt/calibre
wget -c http://archive.ubuntu.com/ubuntu/pool/main/s/sip4/sip4_4.15.5-1build1.dsc
wget -c http://archive.ubuntu.com/ubuntu/pool/main/s/sip4/sip4_4.15.5.orig.tar.gz
wget -c http://archive.ubuntu.com/ubuntu/pool/main/s/sip4/sip4_4.15.5-1build1.debian.tar.gz
dpkg-source -x sip4_4.15.5-1build1.dsc
cd sip4-4.15.5
patch -i /vagrant/sip4.patch -p1
patch -i /vagrant/sip4-2.patch -p1
debuild -us -uc -b -i |tee ../sip4_build.log
cd ..
sudo dpkg -i python-sip*deb python3-sip*deb sip-dev*deb
BUILDSIP

$installsip = <<INSTALLSIP
cd /mnt/calibre
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
cd /mnt/calibre
wget -c http://archive.ubuntu.com/ubuntu/pool/main/p/pyqt5/pyqt5_5.2.1+dfsg-1ubuntu1.dsc
wget -c http://archive.ubuntu.com/ubuntu/pool/main/p/pyqt5/pyqt5_5.2.1+dfsg.orig.tar.gz
wget -c http://archive.ubuntu.com/ubuntu/pool/main/p/pyqt5/pyqt5_5.2.1+dfsg-1ubuntu1.debian.tar.xz
dpkg-source -x pyqt5_5.2.1+dfsg-1ubuntu1.dsc
cd pyqt5-5.2.1+dfsg
patch -i /vagrant/pyqt5.patch -p1
debuild -us -uc -b -i | tee ../pyqt5_build.log
cd ..
sudo dpkg -i pyqt5-dev_*.deb python-pyqt5*deb pyqt5-dev-tools*deb
BUILDPYQT

$installpyqt = <<INSTALLPYQT
wget -c https://s3-us-west-2.amazonaws.com/calibre2/pyqt5-dev-tools_5.2.1%2Bdfsg-1ubuntu1_amd64.deb
wget -c https://s3-us-west-2.amazonaws.com/calibre2/pyqt5-dev_5.2.1%2Bdfsg-1ubuntu1_all.deb
wget -c https://s3-us-west-2.amazonaws.com/calibre2/pyqt5-doc_5.2.1%2Bdfsg-1ubuntu1_all.deb
wget -c https://s3-us-west-2.amazonaws.com/calibre2/pyqt5-examples_5.2.1%2Bdfsg-1ubuntu1_all.deb
wget -c https://s3-us-west-2.amazonaws.com/calibre2/python3-dbus.mainloop.pyqt5-dbg_5.2.1%2Bdfsg-1ubuntu1_amd64.deb
wget -c https://s3-us-west-2.amazonaws.com/calibre2/python3-dbus.mainloop.pyqt5_5.2.1%2Bdfsg-1ubuntu1_amd64.deb
wget -c https://s3-us-west-2.amazonaws.com/calibre2/python3-pyqt5-dbg_5.2.1%2Bdfsg-1ubuntu1_amd64.deb
wget -c https://s3-us-west-2.amazonaws.com/calibre2/python3-pyqt5.qtmultimedia-dbg_5.2.1%2Bdfsg-1ubuntu1_amd64.deb
wget -c https://s3-us-west-2.amazonaws.com/calibre2/python3-pyqt5.qtmultimedia_5.2.1%2Bdfsg-1ubuntu1_amd64.deb
wget -c https://s3-us-west-2.amazonaws.com/calibre2/python3-pyqt5.qtopengl-dbg_5.2.1%2Bdfsg-1ubuntu1_amd64.deb
wget -c https://s3-us-west-2.amazonaws.com/calibre2/python3-pyqt5.qtopengl_5.2.1%2Bdfsg-1ubuntu1_amd64.deb
wget -c https://s3-us-west-2.amazonaws.com/calibre2/python3-pyqt5.qtquick-dbg_5.2.1%2Bdfsg-1ubuntu1_amd64.deb
wget -c https://s3-us-west-2.amazonaws.com/calibre2/python3-pyqt5.qtquick_5.2.1%2Bdfsg-1ubuntu1_amd64.deb
wget -c https://s3-us-west-2.amazonaws.com/calibre2/python3-pyqt5.qtsql-dbg_5.2.1%2Bdfsg-1ubuntu1_amd64.deb
wget -c https://s3-us-west-2.amazonaws.com/calibre2/python3-pyqt5.qtsql_5.2.1%2Bdfsg-1ubuntu1_amd64.deb
wget -c https://s3-us-west-2.amazonaws.com/calibre2/python3-pyqt5.qtsvg-dbg_5.2.1%2Bdfsg-1ubuntu1_amd64.deb
wget -c https://s3-us-west-2.amazonaws.com/calibre2/python3-pyqt5.qtsvg_5.2.1%2Bdfsg-1ubuntu1_amd64.deb
wget -c https://s3-us-west-2.amazonaws.com/calibre2/python3-pyqt5.qtwebkit-dbg_5.2.1%2Bdfsg-1ubuntu1_amd64.deb
wget -c https://s3-us-west-2.amazonaws.com/calibre2/python3-pyqt5.qtwebkit_5.2.1%2Bdfsg-1ubuntu1_amd64.deb
wget -c https://s3-us-west-2.amazonaws.com/calibre2/python3-pyqt5.qtxmlpatterns-dbg_5.2.1%2Bdfsg-1ubuntu1_amd64.deb
wget -c https://s3-us-west-2.amazonaws.com/calibre2/python3-pyqt5.qtxmlpatterns_5.2.1%2Bdfsg-1ubuntu1_amd64.deb
wget -c https://s3-us-west-2.amazonaws.com/calibre2/python3-pyqt5_5.2.1%2Bdfsg-1ubuntu1_amd64.deb
sudo dpkg -i pyqt5-dev_*.deb python-pyqt5*deb pyqt5-dev-tools*deb
INSTALLPYQT

$buildcalibre = <<BUILD
wget -c https://s3-us-west-2.amazonaws.com/calibre2/calibre_2.0.0%2Bdfsg.orig.tar.xz
wget -c https://s3-us-west-2.amazonaws.com/calibre2/calibre_2.0.0%2Bdfsg-1.debian.tar.xz
wget -c https://s3-us-west-2.amazonaws.com/calibre2/calibre_2.0.0%2Bdfsg-1.dsc
dpkg-source -x calibre_2.0.0+dfsg-1.dsc
patch -i /vagrant/calibre.patch -p1
debuild -us -uc -b -i
cd ..
BUILD

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
    aws.ami = "ami-0d5a283d" # instance store precise
    override.ssh.username = "ubuntu"
  end

  # make working directory on SSD
  config.vm.provision "shell", inline: "mkdir /mnt/calibre;chown ubuntu.ubuntu /mnt/calibre"

  config.vm.provision "shell", inline: $aptprepare, privileged: true
  config.vm.provision "shell", inline: $dependency, privileged: true

  #config.vm.provision "shell", inline: $buildsip, privileged: false
  config.vm.provision "shell", inline: $installsip, privileged: false

  config.vm.provision "shell", inline: $buildpyqt, privileged: false
  #config.vm.provision "shell", inline: $installpyqt, privileged: false

  config.vm.provision "shell", inline: $buildcalibre, privileged: false

end
