# -*- mode: ruby -*-
# vi: set ft=ruby :


Vagrant.configure(2) do |config|

  if Vagrant.has_plugin?("vagrant-proxyconf")
    config.proxy.http = ENV["http_proxy"]
    config.proxy.https = ENV["https_proxy"]
    config.proxy.no_proxy = ENV["no_proxy"]
  else
  end

  #config.vm.provision :hostmanager
  if Vagrant.has_plugin?("vagrant-hostmanager")
    config.hostmanager.enabled = true
    config.hostmanager.manage_host = true
    config.hostmanager.manage_guest = true
    config.hostmanager.ignore_private_ip = false
    config.hostmanager.include_offline = true
  else
  end

  config.vm.define "gitlab" do |gitlab|
    gitlab.vm.box = "geerlingguy/centos7"
    gitlab.vm.hostname = "gitlab"
    gitlab.vm.network :private_network, ip: "192.168.50.20"
    #gitlab.ssh.insert_key = false

    if Vagrant.has_plugin?("vagrant-hostmanager")
      gitlab.hostmanager.aliases = %w(gitlab.localdomain)
      gitlab.hostmanager.manage_guest = false
    else
    end

    gitlab.vm.provider :virtualbox do |v|
      #v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      #v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
      v.customize ["modifyvm", :id, "--memory", 1024]
      v.customize ["modifyvm", :id, "--cpus", "2"]
      v.customize ["modifyvm", :id, "--name", "gitlab"]
      v.customize ["modifyvm", :id, "--ioapic", "on"]
    end
  end

  config.vm.define "runner" do |runner|
    runner.vm.box = "geerlingguy/centos7"
    runner.vm.hostname = "runner"
    runner.vm.network :private_network, ip: "192.168.50.21"
    #runner.ssh.insert_key = false

    if Vagrant.has_plugin?("vagrant-hostmanager")
      runner.hostmanager.aliases = %w(runner.localdomain)
      runner.hostmanager.manage_guest = false
    else
    end

    runner.vm.provider :virtualbox do |v|
      #v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      #v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
      v.customize ["modifyvm", :id, "--memory", 1024]
      v.customize ["modifyvm", :id, "--cpus", "2"]
      v.customize ["modifyvm", :id, "--name", "runner"]
      v.customize ["modifyvm", :id, "--ioapic", "on"]
    end
  end

  config.vm.provision "shell" do |s|
      user = ENV["USER"]
      ssh_pub_key = File.readlines("/home/#{user}/.ssh/id_rsa.pub").first.strip
      s.inline = <<-SHELL
          echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
      SHELL
  end

  #config.vm.box_check_update = true

  config.vm.provision "shell", inline: <<-SHELL
    sudo yum update -y
  SHELL


  # config.vm.provider "virtualbox" do |vb|
  #      config.vm.network "private_network", :type => 'dhcp', :name => 'vboxnet0', :adapter => 2
  # end

    # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   sudo apt-get update
  #   sudo apt-get install -y apache2
  # SHELL



end
