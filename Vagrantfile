Vagrant.configure("2") do |config|
  config.vm.box = "generic/ubuntu2004"
  config.ssh.insert_key = false
  config.ssh.forward_agent = true
  config.vm.define "os0" do |os|
   os.vm.provider :libvirt do |libvirt|
     libvirt.management_network_name = 'public'
     libvirt.management_network_address = "192.168.122.0/24"
     libvirt.cpus = 4
     libvirt.memory = 8192
     libvirt.default_prefix = ""
   end
   os.vm.network "private_network", ip: "172.29.236.10", auto_config: false
   os.vm.network "private_network", ip: "172.31.1.10", auto_config: false
   os.vm.network "private_network", ip: "172.29.240.10", auto_config: false
   os.vm.provision :ansible do |ansible|
    ansible.limit = "all"
    ansible.playbook = "provisioning/setup.yml"
    ansible.groups = {
      "openstack" => ["os0"],
      "osds" => ["osd-[0:1]"],
      "all_groups:children" => ["openstack", "osds"]
    }
   end
  end

  config.vm.define "osd-0" do |osd0|
   osd0.vm.provider :libvirt do |libvirt|
    libvirt.management_network_name = 'public'
    libvirt.management_network_address = "192.168.122.0/24"
    libvirt.cpus = 1
    libvirt.memory = 1024
    libvirt.default_prefix = ""
    libvirt.storage :file, :size => '10G'
   end
   osd0.vm.network "private_network", ip: "172.29.236.11", auto_config: false
   osd0.vm.network "private_network", ip: "172.31.1.11", auto_config: false
   osd0.vm.network "private_network", ip: "172.31.2.10", auto_config: false
  end
  
  config.vm.define "osd-1" do |osd1|
   osd1.vm.provider :libvirt do |libvirt|
    libvirt.management_network_name = 'public'
    libvirt.management_network_address = "192.168.122.0/24"
    libvirt.cpus = 1
    libvirt.memory = 1024
    libvirt.default_prefix = ""
    libvirt.storage :file, :size => '10G'
   end
   osd1.vm.network "private_network", ip: "172.29.236.12", auto_config: false
   osd1.vm.network "private_network", ip: "172.31.1.12", auto_config: false
   osd1.vm.network "private_network", ip: "172.31.2.11", auto_config: false
  end
end

