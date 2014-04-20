# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

## Web Server #########################
  config.vm.define :webserver do |web_config|

    web_config.vm.box = "ubuntu12.04"
    web_config.vm.box_url = "http://files.vagrantup.com/precise64_vmware.box"
    web_config.vm.network "private_network", ip: "192.168.2.10"

    web_config.vm.provider "vmware_fusion" do |v|
      v.vmx['displayName'] = 'webserver'
      v.vmx["numvcpus"] = '1'
      v.vmx['memsize'] = '128'
    end

    web_config.vm.provision :ansible do |ansible|
      ansible.inventory_path = "provisioning/ansible_hosts"
      ansible.playbook = "provisioning/webserver.yml"
    end
  end

## HAProxy ############################
   config.vm.define :haproxy do |haproxy_config|

    haproxy_config.vm.box = "ubuntu12.04"
    haproxy_config.vm.box_url = "http://files.vagrantup.com/precise64_vmware.box"
    haproxy_config.vm.network :private_network, ip: "192.168.2.20"

    haproxy_config.vm.provider "vmware_fusion" do |v|
      v.vmx['displayName'] = 'haproxy'
      v.vmx["numvcpus"] = '1'
      v.vmx['memsize'] = '128'
    end

    haproxy_config.vm.provision :ansible do |ansible|
      ansible.inventory_path = "provisioning/ansible_hosts"
      ansible.playbook = "provisioning/haproxy.yml"
    end
  end
 
## App Server #########################
app_hosts = {
  "app_ruby_1" => "00",
  "app_ruby_2" => "01"
}

  app_hosts.each do |name, id|
    config.vm.define name do |app_ruby_config|
      ip = "192.168.2.1"+id
      port = "30"+id

      app_ruby_config.vm.box = "ubuntu12.04"
      app_ruby_config.vm.box_url = "http://files.vagrantup.com/precise64_vmware.box"
      app_ruby_config.vm.network :private_network, ip: ip
      app_ruby_config.vm.network :forwarded_port, guest: 3000, host: port 
      app_ruby_config.vm.synced_folder "../", "/srv/www"

      app_ruby_config.vm.provider "vmware_fusion" do |v|
        v.vmx['displayName'] = name
        v.vmx["numvcpus"] = '1'
        v.vmx['memsize'] = '256'
      end

      app_ruby_config.vm.provision "ansible" do |ansible|
        ansible.inventory_path = "provisioning/ansible_hosts"
        ansible.playbook = "provisioning/app_ruby.yml"
      end
    end
  end

## PostgreSQL Server #########################
  config.vm.define :postgres do |postgres_config|

    postgres_config.vm.box = "ubuntu12.04"
    postgres_config.vm.box_url = "http://files.vagrantup.com/precise64_vmware.box"
    postgres_config.vm.network :private_network, ip: "192.168.2.30"

    postgres_config.vm.provider "vmware_fusion" do |v|
      v.vmx['displayName'] = 'PostgreSQL'
      v.vmx["numvcpus"] = '1'
      v.vmx['memsize'] = '512'
    end

    postgres_config.vm.provision :ansible do |ansible|
      ansible.inventory_path = "provisioning/ansible_hosts"
      ansible.playbook = "provisioning/postgres.yml"
    end
  end
  
end
