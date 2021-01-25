Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.network "forwarded_port", guest: 3000, host: 3000, host_ip: "127.0.0.1"
  # for WinSCP
  config.vm.network "forwarded_port", guest: 22, host: 2727, id: "ssh"
  # 同期するとサーバが起動しなくなる
  #config.vm.synced_folder "./", "/home/vagrant/covid19", type:"virtualbox"
  config.vm.provider "virtualbox" do |vb|
    # Customize the amount of memory on the VM:
    vb.memory = "2048"
  end
  config.vm.provision "shell", path: "vagrant_provision.sh", privileged: false
end
