# 使用vagrant创建虚拟机centos 8示例




## Download CentOS 8 Vagrant box

使用下面的命令下载适合自己虚拟环境的Vagrant box 

```bash
# kvm 虚拟机
vagrant box add centos/8 --provider=libvirt

# virtualbox
vagrant box add centos/8 --provider=virtualbox

# VMware
vagrant box add generic/centos8 --provider=vmware_desktop
```

由于外网访问慢，可以下载后再添加

```bash
wget https://cloud.centos.org/centos/8/vagrant/x86_64/images/CentOS-8-Vagrant-8.3.2011-20201204.2.x86_64.vagrant-libvirt.box

vagrant box add centos/8 ./CentOS-8-Vagrant-8.3.2011-20201204.2.x86_64.vagrant-libvirt.box
```

## Create Vagrantfile for CentOS 8

创建Vagrantfile

```bash
$ mkdir -p ~/vagrant/centos8 && cd ~/vagrant/centos8
$ vim Vagrantfile
```

- kvm

```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV['VAGRANT_DEFAULT_PROVIDER'] = 'libvirt'

Vagrant.configure("2") do |config|

  ##### DEFINE VMS #####
  config.vm.define "centos8" do |config|
  config.vm.hostname = "centos8"
  config.vm.box = "centos/8"
  config.vm.box_check_update = false
  end
  config.vm.provider :libvirt do |v|
    v.memory = 1024
    v.cpus = 2
  end
end
```

- virtualbox

```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "centos/8"
  config.vm.box_check_update = false
  config.vm.hostname = "centos8"
  config.vm.provider "virtualbox" do |vb|
  # Display the VirtualBox GUI when booting the machine
     vb.gui = false
     vb.memory = "2048"
     vb.cpus = 2
  end
end
```

创建好 Vagrantfile，启动vm

```bash
vagrant up
```

测试ssh

```bash
vagrant ssh
```