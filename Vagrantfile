Vagrant.configure("2") do |config|
    config.vm.box = "ubuntu/focal64"

    config.vm.synced_folder ".", "/vagrant", disabled: true

    config.vm.boot_timeout = 60000

    config.vm.provider "virtualbox" do |vb|
        vb.gui = true
        vb.linked_clone = true
        vb.customize [ "modifyvm", :id, "--uartmode1", "disconnected" ]
    end

    config.vm.define "backend" do |backend|
        backend.vm.provider "virtualbox" do |vb|
            vb.name = "BACKEND"
	end
	backend.vm.network "private_network", ip: "192.168.150.10"
	backend.vm.network "forwarded_port", guest: 80, host: 8090
        backend.vm.provision "ansible" do |ansible|
            ansible.playbook = "tutorials.yaml"
        end
    end
    config.vm.define "frontend" do |frontend|
        frontend.vm.provider "virtualbox" do |vb|
            vb.name = "FRONTEND"
        end
	frontend.vm.network "private_network", ip: "192.168.150.11"
        frontend.vm.network "forwarded_port", guest: 80, host: 8080
	frontend.vm.provision "ansible" do |ansible|
            ansible.playbook = "tutorials.yaml"
        end
    end
    config.vm.define "database" do |database|
        database.vm.provider "virtualbox" do |vb|
            vb.name = "DATABASE"
        end
	database.vm.network "private_network", ip: "192.168.150.12"
	database.vm.network "forwarded_port", guest: 3306, host: 12002
        database.vm.provision "ansible" do |ansible|
            ansible.playbook = "tutorials.yaml"
        end
    end
end
