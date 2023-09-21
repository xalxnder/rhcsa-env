disk1 = 'disk-0-1.vdi'
disk2 = 'disk-0-2.vdi'

Vagrant.configure("2") do |config|    
    config.ssh.insert_key = false
    config.vm.synced_folder ".", "/vagrant", disabled: true


    config.vm.define "repos" do |repos|
        repos.vm.box = "hixavier/rhel9repos"
        repos.vm.network "private_network", ip: "192.168.55.200"
    end



    config.vm.define "node1" do |node1|
        node1.vm.box = "hixavier/rhel9node"
        node1.vm.network "private_network", ip: "192.168.55.201"
    end


    config.vm.define "node2" do |node2|
        node2.vm.box = "hixavier/rhel9node"
        node2.vm.network "private_network", ip: "192.168.55.202"
        node2.vm.provider "virtualbox" do |node2|
            unless File.exist?(disk1)
                node2.customize ['createhd', '--filename', disk1, '--variant', 'Fixed', '--size', 16 * 1024]
              end
        
              unless File.exist?(disk2)
                node2.customize ['createhd', '--filename', disk2, '--variant', 'Fixed', '--size', 16 * 1024]
              end
        
              node2.memory = "2048"
              node2.customize ['storageattach', :id,  '--storagectl', 'SATA', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', disk1]
              node2.customize ['storageattach', :id,  '--storagectl', 'SATA', '--port', 2, '--device', 0, '--type', 'hdd', '--medium', disk2]
            end
    end

    config.vm.provision "ansible" do |ansible|
        ansible.playbook = "playbooks/main.yml"
        ansible.compatibility_mode = "2.0"
        ansible.inventory_path = "inventory"
        ansible.config_file = "ansible.cfg"
        # ansible.limit = "all"
    end

end
