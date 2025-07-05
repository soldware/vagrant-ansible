# Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.box = "debian/bookworm64"

  config.vm.define "flask" do |flask|
    flask.vm.hostname = "flask.local"
    flask.vm.network "private_network", ip: "192.168.56.10"
  end

  config.vm.define "nginx" do |nginx|
    nginx.vm.hostname = "nginx.local"
    nginx.vm.network "private_network", ip: "192.168.56.11"
  end

  config.vm.define "mongodb" do |mongodb|
    mongodb.vm.hostname = "mongodb.local"
    mongodb.vm.network "private_network", ip: "192.168.56.12"
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "site.yml"
    ansible.inventory_path = "inventory.ini"
    ansible.verbose = "v"
  end
end
