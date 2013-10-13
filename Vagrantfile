# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  vagrant_version = Vagrant::VERSION.sub(/^v/, '')
  config.vm.box = "candycane"
  config.vm.box_url = "https://dl.dropboxusercontent.com/s/krarg1fxw1bllk8/candycane.box"
  src_dir = './'
  doc_root = '/vagrant_data/app/webroot'
  config.vm.network :forwarded_port, guest: 80, host: 8080
  if vagrant_version >= "1.3.0"
    config.vm.synced_folder src_dir, "/vagrant_data", :create => true, :owner=> 'vagrant', :group=>'www-data', :mount_options => ['dmode=775,fmode=775']
  else
    config.vm.synced_folder src_dir, "/vagrant_data", :create => true, :owner=> 'vagrant', :group=>'www-data', :extra => 'dmode=775,fmode=775'
  end
end
