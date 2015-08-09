# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  
  config.vm.define :web do |web|
    web.vm.box = "precise64"
    web.vm.box_url = "http://files.vagrantup.com/precise64.box"
    web.vm.network :private_network, ip: "10.33.33.33"
    web.vm.network :forwarded_port, guest: 80, host: 8081

    #web.vm.hostname = "togashik"

    web.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "512"]
    end

    web.vm.provision "shell" do |s|
    ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
    s.inline = <<-SHELL
    echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
    mkdir -v /root/.ssh
    echo #{ssh_pub_key} >> /root/.ssh/authorized_keys
    SHELL
    end

    web.vm.provision :ansible do |ansible|
      ansible.playbook = "build-server.yml"
      ansible.inventory_path = "hosts-vagrant"
      ansible.verbose = "v"
      ansible.ask_sudo_pass = true

      # https://github.com/mitchellh/vagrant/issues/3096
      ansible.limit = 'all'
    end

  end


end
