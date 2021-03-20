# Chapter 4 - 批量创建虚拟机示例


## 创建虚拟机集群

## 编写Vagrantfile配置文件

```rb
Vagrant.configure("2") do |config|
	config.vm.define "master" do |master|

	end
	config.vm.define "node-1" do |node_1|

	end
	config.vm.define "node-2" do |node_2|

	end
	config.vm.define "docker-register" do |docker-register|

	end
end
```



最终结果

```rb
Vagrant.configure("2") do |config|
	#####################################################################
	# 全局配置
	#####################################################################
	config.vm.box = "centos/8"
	config.vm.network "public_network", bridge: "eth0"

	#===================================================================#
	# 互信认证
	#===================================================================#
	# 激活hostmanager插件
	config.hostmanager.enabled = true

	# 在宿主机上的hosts文件中添加虚拟机的主机名解析信息
	config.hostmanager.manage_host = true

	# 在各自虚拟机中添加各虚拟机的主机名解析信息
	config.hostmanager.manage_guest = true

	# 不忽略私有网络的地址
	config.hostmanager.ignore_private_ip = false
	#===================================================================#

	# libvirt相关配置
	config.vm.provider "libvirt" do |v|
		# 设置虚拟机的内存大小
		v.memory = 2048
		# 设置虚拟机的CPU个数
		v.cpus = 1
	end

	# 使用shell脚本进行软件安装和配置
	config.vm.provision "shell", inline: <<-SHELL
     echo "PermitRootLogin yes" >> /etc/ssh/sshd_config
     sed -in 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
     systemctl restart sshd
	SHELL
	#####################################################################

	#####################################################################
	# 局部配置
	#####################################################################
	config.vm.define "master" do |master|
		# 设置虚拟机的主机名
		master.vm.hostname="k8s-master"
		# 设置虚拟机的IP 192.168.121.*/24
		master.vm.network "private_network", ip: "192.168.121.11"
		# 使用shell脚本进行软件安装和配置
		master.vm.provision "shell", inline: <<-SHELL
			yum -y install docker
		SHELL
	end
	#####################################################################
	config.vm.define "node-1" do |node_1|
		node_1.vm.hostname="k8s-node-01"
		node_1.vm.network "private_network", ip: "192.168.121.12"
	end
	config.vm.define "node-2" do |node_2|
		node_2.vm.hostname="k8s-node-02"
		node_2.vm.network "private_network", ip: "192.168.121.13"
	end
	config.vm.define "docker-register" do |register|
		register.vm.hostname="k8s-docker-register"
		register.vm.network "private_network", ip: "192.168.121.14"
	end
end


```