# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "vagrant-python-dev"

  config.vm.box_url = "https://opscode-vm-bento.s3.amazonaws.com/vagrant/opscode_debian-7.1.0_provisionerless.box"
  config.vm.network :forwarded_port, guest: 80, host: 8080
  config.vm.network :private_network, ip: "192.168.33.10"
  config.ssh.forward_agent = true

  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", "1024"]
  end

  # 時間があったらAnsibleとかChefとかPuppetでやる
  $script = <<EOF

sudo apt-get update
sudo apt-get -y upgrade
sudo apt-get -y install git-core curl vim tmux zsh make

# python
sudo aptitude install zlib1g-dev
sudo aptitude install libssl-dev
sudo aptitude install libreadline-dev
sudo aptitude install libsqlite3-dev
sudo apt-get -y install python-pip

wget https://www.python.org/ftp/python/3.4.1/Python-3.4.1.tar.xz
tar xvf Python-3.4.1.tar.xz && rm -f Python-3.4.1.tar.xz
mkdir -p /home/vagrant/local
cd Python-3.4.1/
./configure --prefix=/home/vagrant/local/python-3.4
make -j4
make install

cd /home/vagrant
sudo rm -rf Python-3.4.1

sudo pip install virtualenv
sudo virtualenv /home/vagrant/local/python3
source /home/vagrant/local/python3/bin/activate
sudo pip install ipython

echo "export PATH=/home/vagrant/local/python-3.4/bin:$PATH" >> /home/vagrant/.bashrc

# other
# TODO mysql入れると文字化けするから気が向いたら直す
sudo apt-get install sqlite3 mysql-server-5.5

EOF

        config.vm.provision :shell, :inline => $script
end
