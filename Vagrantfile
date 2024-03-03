Vagrant.configure("2") do |config|

  config.vm.define "control" do |control|
      control.vm.box = "bento/ubuntu-22.04"
      control.vm.network "private_network", ip: "192.168.11.51"
      control.vm.hostname = "control"
      control.vm.synced_folder "C:/kubespray-centOS", "/vagrant"
      control.vm.provider "virtualbox" do |vb|
          vb.memory = "2048"
          vb.name = "control"
      end
  end


  config.vm.define "node1" do |node1|
    node1.vm.box = "bento/ubuntu-22.04"
    node1.vm.network "private_network", ip: "192.168.11.52"
    node1.vm.hostname = "node1"
    node1.vm.synced_folder "C:/kubespray-centOS", "/vagrant"
    node1.vm.provider "virtualbox" do |vb|
        vb.memory = "2048"
        vb.name = "node1"
    end
end


config.vm.define "node2" do |node2|
    node2.vm.box = "bento/ubuntu-22.04"
    node2.vm.network "private_network", ip: "192.168.11.53"
    node2.vm.hostname = "node2"
    node2.vm.synced_folder "C:/kubespray-centOS", "/vagrant"
    node2.vm.provider "virtualbox" do |vb|
        vb.memory = "2048"
        vb.name = "node2"
    end
end

config.vm.define "cool" do |node3|
    node3.vm.box = "bento/ubuntu-22.04"
    node3.vm.network "private_network", ip: "192.168.11.54"
    node3.vm.hostname = "node3"
    node3.vm.synced_folder "C:/kubespray-centOS", "/vagrant"
    node3.vm.provider "virtualbox" do |vb|
        vb.memory = "2048"
        vb.name = "node3"
    end
end

end