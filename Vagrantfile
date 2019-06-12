IMAGE_NAME = "bento/ubuntu-18.04"
WORKERS = 2
IP_PREFIX = "192.168.50."
HOST_USER_ID = ""
POD_NETWORK_CIDR = "192.168.0.0/16"

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

    config.vm.provider "virtualbox" do |v|
        v.memory = 2048
        v.cpus = 2
    end
      
    config.vm.define "k8s-master" do |master|
        master.vm.box = IMAGE_NAME
        master.vm.network "private_network", ip: IP_PREFIX + "50"
        master.vm.hostname = "k8s-master"
        master.vm.provider "virtualbox" do |v|
            v.name = "#{master.vm.hostname}"
        end
        master.vm.provision "ansible" do |ansible|
            ansible.playbook = "k8s/master.yml"
            ansible.extra_vars = {
                node_ip: IP_PREFIX + "50",
                host_user_id: HOST_USER_ID,
                pod_network_cidr: POD_NETWORK_CIDR,
            }
        end
    end

    (1..WORKERS).each do |i|
        config.vm.define "k8s-node#{i}" do |node|
            node.vm.box = IMAGE_NAME
            node.vm.network "private_network", ip: IP_PREFIX + "#{i + 50}"
            node.vm.hostname = "k8s-node#{i}"
            node.vm.provider "virtualbox" do |v|
                v.name = "#{node.vm.hostname}"
            end
            node.vm.provision "ansible" do |ansible|
                ansible.playbook = "k8s/node.yml"
                ansible.extra_vars = {
                    node_ip: IP_PREFIX + "#{i + 50}",
                }
            end
        end
    end
end
