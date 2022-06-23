# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV['VAGRANT_DEFAULT_PROVIDER'] = 'libvirt'
ENV['VAGRANT_NO_PARALLEL'] = 'yes'

$script = <<-'SCRIPT'
sed -i s/^BLACKLIST_RPC=/\\#BLACKLIST_RPC=/ /etc/sysconfig/qemu-ga
sed -i s/^SELINUX=.*/SELINUX=disabled/ /etc/selinux/config
shutdown -r now
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.provider :libvirt do |libvirt|
    libvirt.uri = 'qemu:///system'
    libvirt.driver = "qemu"
    libvirt.host = "localhost"
    libvirt.connect_via_ssh = false
    libvirt.username = "root"
    libvirt.password = ""
    libvirt.storage_pool_name = "VMs"
    libvirt.cpus = 1
    libvirt.memory = 1024
    libvirt.machine_virtual_size = 20
    libvirt.channel :type => 'unix', :target_name => 'org.qemu.guest_agent.0', :target_type => 'virtio'
  end
  config.vm.provision "shell" do |s|
    ssh_pub_key = File.readlines("./files/ansible-skillup.pub").first.strip
    s.inline = <<-SHELL
    echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
    SHELL
  end  
  
  ##### DEFINE VMS #####
 
  (1..2).each do |i|

    ##Create web VM##
    config.vm.define "web_#{i}" do |web|
      if (i % 2 == 0) 
        web.vm.box = "generic/ubuntu1804"
      else
        web.vm.box = "centos/7"
        web.vm.provision "shell", inline: $script
      end
      web.vm.hostname = "web_#{i}"
      web.vm.guest = :linux
      web.vm.box_check_update = false
      web.vm.hostname = "web#{i}.local"
    end

   ##Create DB VM##
    config.vm.define "db_#{i}" do |db|
      if (i % 2 == 0)
        db.vm.box = "generic/ubuntu1804"
      else
        db.vm.box = "centos/7"
        db.vm.provision "shell", inline: $script
      end
      db.vm.hostname = "db_#{i}"
      db.vm.guest = :linux
      db.vm.box_check_update = false
      db.vm.hostname = "db#{i}.local"
    end

  end

end
