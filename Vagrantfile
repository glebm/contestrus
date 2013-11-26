# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "saucy64"

  # vm.provision cannot be overriden, so declare once per provider, every time
  # more details: https://github.com/dotcloud/docker/blob/7e580aacec10d64c2fa0811e41a35159e47fcfa4/Vagrantfile#L115-L155
  default_provision = [:shell, :path => "script/provision-development"]

  # Because Virtualbox's shared filesystem is extremely slow
  config.vm.synced_folder ".", "/vagrant"
  config.vm.network :forwarded_port, guest: 3000, host: 3005

  config.vm.provider "vmware_fusion" do |vmware, override|
    override.vm.box_url = "http://shopify-vagrant.s3.amazonaws.com/packer_ubuntu-13.10_vmware.box"
    override.vm.provision *default_provision
    vmware.vmx['memsize'] = 1024
  end

  config.vm.provider "virtualbox" do |virtualbox, override|
    override.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/saucy/current/saucy-server-cloudimg-amd64-vagrant-disk1.box"
    override.vm.provision *default_provision
    virtualbox.customize ["modifyvm", :id, "--memory", "1024", "--ioapic", "on"]
  end

  # via https://github.com/yshahin/vagrant-parallels
  config.vm.provider 'parallels' do |parallels, override|
    # this is an Ubuntu 12.04 LTS (precise), provided by vagrant-parallels:
    override.vm.box_url = 'https://s3-eu-west-1.amazonaws.com/vagrant-parallels/devbox.box'
    override.vm.provision *default_provision
  end
end
