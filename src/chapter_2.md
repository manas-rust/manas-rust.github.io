# 安装vagrant

### 安装vagrant

要安装Vagrant，请先找到适合您系统的软件包并[下载](https://www.vagrantup.com/downloads)。Vagrant被打包为特定于操作的程序包。运行系统的安装程序。安装程序将自动添加 vagrant到您的系统路径，以便在终端中可用。

```bash
wget wget https://releases.hashicorp.com/vagrant/2.2.14/vagrant_2.2.14_x86_64.deb
dpkg -i vagrant_2.2.14_x86_64.deb
```


### 安装libvirt插件

```bash
# 需要安装 libvirt 开发包
sudo apt install libvirt-dev
vagrant plugin install vagrant-libvirt --plugin-clean-sources --plugin-source https://gems.ruby-china.com/
```

### 设置VAGRANT_HOME

vagrant在执行子命令box add、init、up等命令时，都可能会去下载所需的虚拟机镜像文件，即Box image。

这些镜像文件默认放在~/.vagrant.d目录(Linux)

```bash
echo 'export VAGRANT_HOME="/data/.vagrant.d"' >>~/.bashrc
```

### 命令自动补全

```bash
vagrant autocomplete
source ~/.bashrc
```

### 一些常用的插件

vagrant-hostmanager - 各个虚拟机之间，及虚拟机与host之间互信认证

vagrant-rsync-back - guest虚拟机数据同步回主机

