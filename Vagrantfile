# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'vagrant-aws'
Vagrant.require_version '>= 1.6.0'
VAGRANTFILE_API_VERSION = '2'
ENV['VAGRANT_DEFAULT_PROVIDER'] = 'aws'
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = 'aws-dummy'
  config.vm.provider 'aws' do |aws, override|
    aws.access_key_id = ENV['ACCESS_KEY_ID']
    aws.secret_access_key = ENV['SECRET_ACCESS_KEY']
    aws.keypair_name = 'vagrant'
    aws.region = 'eu-central-1'
    aws.ami = 'ami-7c412f13'
    aws.security_groups = ['vagrant']
    aws.instance_type = 't2.micro'
    override.vm.provision "shell", path: "provision", privileged: false
    override.vm.synced_folder ".", "/vagrant", disabled: true
    override.ssh.username = 'ubuntu'
    override.ssh.private_key_path = 'vagrant.pem'
  end
end
