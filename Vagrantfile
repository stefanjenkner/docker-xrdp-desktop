# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.hostname = "docker-xrdp-desktop.jenkner.org"
  config.vm.box = "precise64"
  #config.vm.box_url = "http://files.vagrantup.com/precise64.box"
  config.vm.network :forwarded_port, guest: 3389, host: 3389
  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", "4096", "--cpus", 2]
  end

  config.vm.provision "docker" do |d|
    d.build_image "/vagrant/desktop-precise-xrdp-xfce4", args: "-t desktop-precise-xrdp-xfce4"
    d.build_image "/vagrant/desktop-precise-xrdp-unity2d", args: "-t desktop-precise-xrdp-unity2d"
    d.build_image "/vagrant/desktop-trusty-xrdp-xfce4", args: "-t desktop-trusty-xrdp-xfce4"
    d.build_image "/vagrant/desktop-wheezy-xrdp-xfce4", args: "-t desktop-wheezy-xrdp-xfce4"
    d.build_image "/vagrant/desktop-vivid-xrdp-gnome3", args: "-t desktop-vivid-xrdp-gnome3"
  end

end
