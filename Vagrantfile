# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "ubuntu/trusty64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  config.vm.network "forwarded_port", guest: 3000, host: 3000

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
   config.vm.provision "shell", inline: <<-SHELL
     sudo apt-get update >/dev/null 2>&1
     sudo apt-get upgrade >/dev/null 2>&1
     
     sudo ufw allow 80
     sudo ufw reload
     sudo ufw status

     sudo apt-get -y install git curl g++ make
     sudo apt-get -y install zlib1g-dev libssl-dev libreadline-dev 
     sudo apt-get -y install libyaml-dev libxml2-dev libxslt-dev
     sudo apt-get -y install sqlite3 libsqlite3-dev nodejs
     sudo apt-get -y install libffi-dev
     mkdir .rbenv
     git clone git://github.com/sstephenson/rbenv.git ~/.rbenv
     echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.profile
     echo 'eval "$(rbenv init -)"' >> ~/.profile
     source ~/.profile
     mkdir -p ~/.rbenv/plugins
     cd ~/.rbenv/plugins
     git clone git://github.com/sstephenson/ruby-build.git
     rbenv install 2.5.1
     rbenv global 2.5.1
     touch ~/.gemrc
     cat <<-EOF > ~/.gemrc
     install: --no-ri --no-rdoc
     update: --no-ri --no-rdoc
     EOF
     gem install rails --version="~> 5.2"
     rbenv rehash
     cd ~
     mkdir work
     cd work
     rails new testproj --skip-bundle
     cd testproj
     bundle install
     rails g scaffold user name:string email:string
     rake db:migrate
     rails s
     
   SHELL

end
