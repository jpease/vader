# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

## Web Server #########################
  config.vm.define :web do |web_config|

    web_config.vm.box = "ubuntu12.04"
    web_config.vm.box_url = "http://files.vagrantup.com/precise64_vmware.box"
    web_config.vm.network :private_network, ip: "192.168.2.10"

    web_config.vm.provider "vmware_fusion" do |v|
      v.vmx['displayName'] = 'web'
      v.vmx["numvcpus"] = '1'
      v.vmx['memsize'] = '512'
    end

    web_config.vm.provision :ansible do |ansible|
      ansible.playbook = "provisioning/webservers.yml"
      ansible.inventory_file = "provisioning/ansible_hosts"
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
      v.vmx['memsize'] = '512'
    end

    haproxy_config.vm.provision :ansible do |ansible|
      ansible.playbook = "provisioning/haproxy.yml"
      ansible.inventory_file = "provisioning/ansible_hosts"
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
      app_ruby_config.vm.network :forwarded_port, guest: port, host: 3000
      app_ruby_config.vm.synced_folder "../", "/srv/www"

      app_ruby_config.vm.provider "vmware_fusion" do |v|
        v.vmx['displayName'] = name
        v.vmx["numvcpus"] = '1'
        v.vmx['memsize'] = '512'
      end

      app_ruby_config.vm.provision :ansible do |ansible|
        ansible.playbook = "provisioning/app_ruby.yml"
        ansible.inventory_file = "provisioning/ansible_hosts"
      end
    end
  end
end
