# Chapter 5 - 如何制作vagrant image box

1. 通过ks自动化安装虚拟机
https://git.centos.org/centos/kickstarts/blob/master/f/CentOS-8-Stream-x86_64-Vagrant.ks

2. 使用脚本 create_box.sh qcow2 打包为box
https://github.com/vagrant-libvirt/vagrant-libvirt/blob/master/tools/create_box.sh