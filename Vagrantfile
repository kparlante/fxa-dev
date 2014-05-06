# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box_url = "packer/sl64-virtualbox.box"
  config.vm.box = "sl64"
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.network "private_network", type: "dhcp"

  config.vm.provider "virtualbox" do |vb, override|
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    vb.memory = 2048
    vb.cpus = 2
  end

  config.vm.provider "vmware_fusion" do |vw, override|
    override.vm.box_url = "packer/sl64-vmware.box"
    vw.vmx["memsize"] = "2048"
    vw.vmx["numvcpus"] = "2"
  end

  config.vm.define "fxa" do |fxa|
    fxa.vm.hostname = 'fxa'
    fxa.vm.provision "ansible" do |ansible|
      ansible.playbook = "local2.yml"
      #ansible.verbose = 'vvv'
      ansible.limit = 'all'
      ansible.groups = {
        "db" => ["fxa"],
        "content" => ["fxa"],
        "customs" => ["fxa"],
        "auth" => ["fxa"]
      }
    end
  end
end