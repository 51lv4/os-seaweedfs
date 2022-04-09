# -*- mode: ruby -*-
# vi: set ft=ruby :

nodes = [
  { :hostname => "os-sw1", :ip => "172.16.0.10", :box => "ubuntu/focal64" },
  { :hostname => "os-sw2", :ip => "172.16.0.11", :box => "ubuntu/focal64" },
  { :hostname => "os-sw3", :ip => "172.16.0.12", :box => "ubuntu/focal64" },
]

Vagrant.configure("2") do |config|
  nodes.each_with_index do |node, index|
    config.vm.define node[:hostname] do |nodeconfig|
      nodeconfig.ssh.insert_key = false
      nodeconfig.vm.box_check_update = false
      nodeconfig.vbguest.auto_update = false 
      nodeconfig.vm.box = node[:box]
      nodeconfig.vm.hostname = node[:hostname]
      nodeconfig.vm.network :private_network, ip: node[:ip]

      config.vm.provider :virtualbox do |vb|
        vb.memory = 1024
        vb.cpus = 1
        #vb.name = nodeconfig.vm.hostname                                         
      end

      if index == nodes.size - 1
        nodeconfig.vm.provision :ansible do |ansible|
          ansible.verbose = true #"vvvv"
          ansible.playbook = "provision.yml"
          ansible.limit = 'all'# "#{info[:ip]}" # Ansible hosts are identified by ip
          ansible.groups = { "seaweed" => ["os-sw1", "os-sw2", "os-sw3"] }
        end # end provision
      end #end if
    end
  end
end

