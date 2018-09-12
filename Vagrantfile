Vagrant.configure("2") do |config|

  config.vm.define "ansible-master" do |master|
    master.vm.box = "centos/7"
    master.vm.network "private_network", ip: "192.168.50.51"
    master.vm.provision "shell", path: "install-ansible.sh"
    master.vm.synced_folder ".", "/vagrant"
    master.vm.synced_folder ".vagrant", "/vagrant/.vagrant"
  end

  config.vm.define "web" do |web|
    web.vm.box = "centos/7"
    web.vm.network "private_network", ip: "192.168.50.52"
    web.vm.synced_folder ".", "/vagrant", disabled: true
  end

  config.vm.define "db" do |db|
    db.vm.box = "debian/stretch64"
    db.vm.network "private_network", ip: "192.168.50.53"
    db.vm.synced_folder ".", "/vagrant", disabled: true
  end

end
