# ---- Define variables below ----

box = "ubuntu/bionic64"

subnet = "10.100.1."
subnet_ip_last_octat = 100

deployer_hostname = "deployer"
deployer_cpus = 2
deployer_memory_gb = 4096
PROVISION_SCRIPT = "./bootstrap.sh"

master_hostname = "master"
master_count = 1
master_cpus = 2
master_memory_gb = 8192

worker_hostname = "worker"
worker_count = 3
worker_cpus = 2
worker_memory_gb = 4096

Vagrant.configure("2") do |config|

  config.vm.define "#{deployer_hostname}" do |deployer|
      deployer.vm.box = box
      deployer.vm.host_name = "#{deployer_hostname}"
      deployer.vm.network 'private_network', ip: "#{subnet}#{subnet_ip_last_octat}"
      subnet_ip_last_octat = subnet_ip_last_octat + 1
      deployer.vm.synced_folder "./", "/shared"
      deployer.vm.provision :shell, path: PROVISION_SCRIPT
      deployer.vm.provider :virtualbox do |vb, override|
        vb.memory = deployer_memory_gb
        vb.cpus = deployer_cpus
      end
    end
  
  (1..master_count).each do |node_id|
    config.vm.define "#{master_hostname}#{node_id}" do |master|
      master.vm.box = box
      master.vm.host_name = "#{master_hostname}#{node_id}"
      master.vm.network 'private_network', ip: "#{subnet}#{subnet_ip_last_octat}"
      subnet_ip_last_octat = subnet_ip_last_octat + 1
      master.vm.synced_folder "./", "/shared"
      master.vm.provider :virtualbox do |vb, override|
        vb.memory = master_memory_gb
        vb.cpus = master_cpus
      end
    end
  end

  (1..worker_count).each do |node_id|
    config.vm.define "#{worker_hostname}#{node_id}" do |worker|
      worker.vm.box = box
      worker.vm.host_name = "#{worker_hostname}#{node_id}"
      worker.vm.network 'private_network', ip: "#{subnet}#{subnet_ip_last_octat}"
      subnet_ip_last_octat = subnet_ip_last_octat + 1
      worker.vm.synced_folder "./", "/shared"
      worker.vm.provider :virtualbox do |vb, override|
        vb.memory = master_memory_gb
        vb.cpus = master_cpus
      end
    end
  end
end
