# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'vagrant-aws'
Vagrant.require_version '>= 1.6.0'
VAGRANTFILE_API_VERSION = '2'
ENV['VAGRANT_DEFAULT_PROVIDER'] = 'aws'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  if Vagrant.has_plugin?("vagrant-vbguest")
    config.vbguest.auto_update = false
  end

  config.vm.define "gs-rest-service"
  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.provider 'virtualbox' do |vb, override|
    vb.name = "gs-rest-service"

    override.vm.box = "ubuntu/xenial64"
    override.vm.hostname = "gs-rest-service.paulojeronimo.com"
    override.vm.network :private_network, ip: "192.168.0.10"
  end

  config.vm.provider 'aws' do |aws, override|
    aws.access_key_id = ENV['ACCESS_KEY_ID']
    aws.secret_access_key = ENV['SECRET_ACCESS_KEY']
    aws.keypair_name = 'vagrant'
    aws.region = 'eu-central-1'
    aws.ami = 'ami-7c412f13'
    aws.security_groups = ['vagrant']
    aws.instance_type = 't2.micro'

    override.vm.box = 'aws-dummy'
    override.ssh.username = 'ubuntu'
    override.ssh.private_key_path = 'vagrant.pem'
  end

  $script =<<-SCRIPT
  git clone https://github.com/paulojeronimo/gs-rest-service-provision-scripts provision-scripts
  ./provision-scripts/provision
  SCRIPT
  config.vm.provision "shell", inline: $script, privileged: false
end
