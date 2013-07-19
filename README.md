composer-cakephp-sample
=======================

This is minimum example of installing CakePHP via Composer.
This repository includes Vagrantfile, You can try yourself on your machine.

#### 日本語での説明 ####
[http://www.engineyard.co.jp/blog/2013/install-cakephp-on-composer/](http://www.engineyard.co.jp/blog/2013/install-cakephp-on-composer/)


## quick course

## prerequirement

- install VirtualBox
- install Vagrant
- clone this repository
- cd into directory you cloned

## Overview


#### Vagrantfile

This Vagrantfile try to spinup instance with an image which is ready for PHP, Composer, Nginx and MySQL. You just need to kick <code>vagrant up</code>

<pre>
# -*- mode: ruby -*-
# vi: set ft=ruby :
 
Vagrant.configure("2") do |config|
  config.vm.box = "candycane"
  config.vm.box_url = "https://dl.dropboxusercontent.com/s/krarg1fxw1bllk8/candycane.box"
  src_dir = './'
  doc_root = '/vagrant_data/app/webroot'
  config.vm.network :forwarded_port, guest: 80, host: 8080
  config.vm.synced_folder src_dir, "/vagrant_data", :create => true, :owner=> 'vagrant', :group=>'www-data', :extra => 'dmode=775,fmode=775'
end
</pre>

#### composer.json

This composer file installs CakePHP via PEAR channel of CakePHP. so you do NOT need include CakePHP library in repository. once you kick <code>composer install</code>. CakePHP will get in the right place.

<pre>
{
    "name": "status-site",
    "repositories": [
        {
            "type": "pear",
            "url": "http://pear.cakephp.org"
        }
    ],
    "require": {
        "pear-cakephp/cakephp": "2.4.0-beta"
    },
    "config": {
        "vendor-dir": "Vendor/"
    }
}
</pre>

to try on this demo. follow step bellow.
<pre>
bash-3.2$ vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
[default] Importing base box 'candycane'...
[default] Matching MAC address for NAT networking...
[default] Setting the name of the VM...
[default] Clearing any previously set forwarded ports...
[default] Creating shared folders metadata...
[default] Clearing any previously set network interfaces...
[default] Preparing network interfaces based on configuration...
[default] Forwarding ports...
[default] -- 22 => 2222 (adapter 1)
[default] -- 80 => 8080 (adapter 1)
[default] Booting VM...
[default] Waiting for VM to boot. This can take a few minutes.
[default] VM booted and ready for use!
[default] Configuring and enabling network interfaces...
[default] Mounting shared folders...
[default] -- /vagrant
[default] -- /vagrant_data
bash-3.2$ vagrant ssh
Welcome to Ubuntu 12.04 LTS (GNU/Linux 3.2.0-23-generic x86_64)
 
 * Documentation:  https://help.ubuntu.com/
Welcome to your Vagrant-built virtual machine.
Last login: Sat Jul 13 17:16:28 2013 from 10.0.2.2
vagrant@precise64:~$ cd /vagrant_data/
vagrant@precise64:/vagrant_data$ composer install
Loading composer repositories with package information
Initializing PEAR repository http://pear.cakephp.org
Installing dependencies (including require-dev)
  - Installing pear-pear.cakephp.org/cakephp (2.4.0beta)
    Downloading: 100%         
Writing lock file
Generating autoload files
</pre>

Now CakePHP is with you.

<pre>
vagrant@precise64:/vagrant_data$ ls -l
total 20
-rwxrwxr-x 1 vagrant www-data  276 Jul 19 10:56 composer.json
-rwxrwxr-x 1 vagrant www-data 1291 Jul 19 11:10 composer.lock
-rwxrwxr-x 1 vagrant www-data 1078 Jul 19 10:56 LICENSE
-rwxrwxr-x 1 vagrant www-data   48 Jul 19 10:56 README.md
-rwxrwxr-x 1 vagrant www-data  459 Jul 19 10:56 Vagrantfile
drwxrwxr-x 1 vagrant www-data  204 Jul 19 11:10 Vendor
vagrant@precise64:/vagrant_data$ ls -l ./Vendor/pear-pear.cakephp.org/CakePHP/
total 0
drwxrwxr-x 1 vagrant www-data 170 Jul 19 11:09 bin
drwxrwxr-x 1 vagrant www-data 782 Jul 19 11:10 Cake
drwxrwxr-x 1 vagrant www-data 102 Jul 19 11:09 data
</pre>

Next you should generate your <code>app</code> direcotry with bake command. this also configure CAKE_CORE_INCLUDE_PATH automatically.


<pre>
vagrant@precise64:/vagrant_data$ ./Vendor/pear-pear.cakephp.org/CakePHP/bin/cake bake --app app
 
Welcome to CakePHP v2.4.0-beta Console
---------------------------------------------------------------
App : app
Path: /vagrant_data/app/
---------------------------------------------------------------
Warning Error: scandir(/vagrant_data/app/): failed to open dir: No such file or directory in [/vagrant_data/Vendor/pear-pear.cakephp.org/CakePHP/Cake/Console/Command/Task/ProjectTask.php, line 52]
 
Warning Error: scandir(): (errno 2): No such file or directory in [/vagrant_data/Vendor/pear-pear.cakephp.org/CakePHP/Cake/Console/Command/Task/ProjectTask.php, line 52]
 
Warning Error: array_diff(): Argument #1 is not an array in [/vagrant_data/Vendor/pear-pear.cakephp.org/CakePHP/Cake/Console/Command/Task/ProjectTask.php, line 52]
 
What is the path to the project you want to bake?  
[/vagrant_data/app] > 
What is the path to the directory layout you wish to copy?  
[/vagrant_data/Vendor/pear-pear.cakephp.org/CakePHP/Cake/Console/Templates/skel] > 
Skel Directory: /vagrant_data/Vendor/pear-pear.cakephp.org/CakePHP/Cake/Console/Templates/skel
Will be copied to: /vagrant_data/app
---------------------------------------------------------------
Look okay? (y/n/q) 
[y] > 
---------------------------------------------------------------
Created: app in /vagrant_data/app
---------------------------------------------------------------
 * Random hash key created for 'Security.salt'
 * Random seed created for 'Security.cipherSeed'
 * Cache prefix set
 * app/Console/cake.php path set.
CakePHP is not on your `include_path`, CAKE_CORE_INCLUDE_PATH will be hard coded.
You can fix this by adding CakePHP to your `include_path`.
 * CAKE_CORE_INCLUDE_PATH set to /vagrant_data/Vendor/pear-pear.cakephp.org/CakePHP in webroot/index.php
 * CAKE_CORE_INCLUDE_PATH set to /vagrant_data/Vendor/pear-pear.cakephp.org/CakePHP in webroot/test.php
   * Remember to check these values after moving to production server
Project baked successfully!
Your database configuration was not found. Take a moment to create one.
---------------------------------------------------------------
Database Configuration:
---------------------------------------------------------------
Name:  
[default] > ^C
</pre>

Then open <code>http://127.0.0.1:8080</code> on your browser. You must see the initial screen of CakePHP. Yes, now You've installed CakePHP via Composer.

##### note
<p>
specifying --app option to non existing directory cause warnings. run bake without --app shows clean result. but you need to give fullpath to the first question of bake processs.
</p>
