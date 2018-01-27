Vagrant.configure("2") do |config|
  #ansible workshop
  #create mgmt node
  config.vm.define "mgmt" do |mgmt_config|
           mgmt_config.vm.box = "ubuntu/xenial64"
           mgmt_config.vm.hostname = "mgmt"
           mgmt_config.vm.network "private_network", ip: "192.168.56.5"
           mgmt_config.vm.provider "virtualbox" do |vb|
                   vb.memory = 1024
                   vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
           end
           mgmt_config.vm.provision "shell", path: "bootstrap-mgmt.sh"
   end

   #create load balancer
   config.vm.define "lb" do |lb_config|
           lb_config.vm.box = "ubuntu/xenial64"
           lb_config.vm.hostname = "lb"
           lb_config.vm.network "private_network", ip: "192.168.56.6"
           lb_config.vm.network "forwarded_port", guest_ip: "192.168.56.6", guest: 80, host_ip: "127.0.0.1", host: 8080
           lb_config.vm.provider "virtualbox" do |vb|
                   vb.memory = 1024
                   vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
           end
   end

   #create web servers
   (1..2).each do |i|
   config.vm.define "web#{i}" do |node|
            node.vm.box = "ubuntu/xenial64"
            node.vm.hostname = "web#{i}"
            node.vm.network "private_network", ip: "192.168.56.1#{i}"
            node.vm.network "forwarded_port", guest: 80, host: "808#{i}"
            node.vm.provider "virtualbox" do |vb|
                   vb.memory = 1024
                   vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
         end
   end
 end
