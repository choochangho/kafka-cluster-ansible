# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
    Z = 3 # max number of zookeeper nodes
    K = 3 # max number of kafka nodes

    (1..Z).each do |i|
        config.vm.define "zk0#{i}" do |config|
            config.vm.box = "gbailey/amzn2"
            config.vm.provider "virtualbox" do |vb|
                vb.name = "zk0#{i}.foo.bar"
                vb.cpus = 2
                vb.memory = 4096
            end
            config.vm.hostname = "zk0#{i}.foo.bar"
            config.vm.network "private_network", ip: "172.41.12.1#{i}"
        end
    end

    (1..K).each do |i|
        config.vm.define "kafka0#{i}" do |config|
            config.vm.box = "gbailey/amzn2"
            config.vm.provider "virtualbox" do |vb|
                vb.name = "kafka0#{i}.foo.bar"
                vb.cpus = 2
                vb.memory = 4096
            end
            config.vm.hostname = "kafka0#{i}.foo.bar"
            config.vm.network "private_network", ip: "172.41.12.2#{i}"
        end
    end

    config.vm.define "ansible01" do |config|
        config.vm.box = "gbailey/amzn2"
        config.vm.provider "virtualbox" do |vb|
            vb.name = "ansible01.foo.bar"
            vb.cpus = 2
            vb.memory = 4096
        end
        config.vm.hostname = "ansible01.foo.bar"
        config.vm.network "private_network", ip: "172.41.12.31"
    end

    # Hostmanager plugin
    config.hostmanager.enabled = true
    config.hostmanager.manage_guest = true

    # Enable SSH Password Authentication
    config.vm.provision "shell", inline: <<-SHELL
    sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
    sed -i 's/PermitRootLogin no/PermitRootLogin yes/g' /etc/ssh/sshd_config
    sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/g' /etc/ssh/sshd_config
    systemctl restart sshd
  SHELL
end