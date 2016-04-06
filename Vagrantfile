# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile for bootstrapping a development Consul cluster with
# VirtualBox provider and Ansible provisioner

VAGRANTFILE_API_VERSION = "2"
BOX_NAME = ENV['BOX_NAME'] || "centos/7"
BOX_MEM = ENV['BOX_MEM'] || "1024"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

	config.vm.define :swarm1 do |swarm1_config|
		swarm1_config.vm.box = BOX_NAME
		swarm1_config.vm.network :private_network, ip: "10.1.42.210"
		swarm1_config.vm.network "forwarded_port", guest: 8500, host: 8500
		swarm1_config.vm.network "forwarded_port", guest: 2376, host: 2376
		swarm1_config.vm.network "forwarded_port", guest: 5000, host: 5000
		swarm1_config.vm.hostname = "swarm1.local"
		swarm1_config.ssh.forward_agent = true
		swarm1_config.vm.provider "virtualbox" do |v|
			v.name = "swarm1"
			v.customize ["modifyvm", :id, "--memory", BOX_MEM]
			v.customize ["modifyvm", :id, "--ioapic", "on"]
			v.customize ["modifyvm", :id, "--cpus", "2"]
			v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
			v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
		end
		swarm1_config.vm.provision :hosts do |provisioner|
			provisioner.add_host '10.1.42.210', ['swarm1.local']
			provisioner.add_host '10.1.42.220', ['swarm2.local']
			provisioner.add_host '10.1.42.230', ['swarm3.local']
		end
	end

	config.vm.define :swarm2 do |swarm2_config|
		swarm2_config.vm.box = BOX_NAME
		swarm2_config.vm.network :private_network, ip: "10.1.42.220"
		swarm2_config.vm.hostname = "swarm2.local"
		swarm2_config.ssh.forward_agent = true
		swarm2_config.vm.provider "virtualbox" do |v|
			v.name = "swarm2"
			v.customize ["modifyvm", :id, "--memory", BOX_MEM]
			v.customize ["modifyvm", :id, "--ioapic", "on"]
			v.customize ["modifyvm", :id, "--cpus", "2"]
			v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
			v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
		end
		swarm2_config.vm.provision :hosts do |provisioner|
			provisioner.add_host '10.1.42.210', ['swarm1.local']
			provisioner.add_host '10.1.42.220', ['swarm2.local']
			provisioner.add_host '10.1.42.230', ['swarm3.local']
		end
	end

	config.vm.define :swarm3 do |swarm3_config|
		swarm3_config.vm.box = BOX_NAME
		swarm3_config.vm.network :private_network, ip: "10.1.42.230"
		swarm3_config.vm.hostname = "swarm3.local"
		swarm3_config.ssh.forward_agent = true
		swarm3_config.vm.provider "virtualbox" do |v|
			v.name = "swarm3"
			v.customize ["modifyvm", :id, "--memory", BOX_MEM]
			v.customize ["modifyvm", :id, "--ioapic", "on"]
			v.customize ["modifyvm", :id, "--cpus", "2"]
			v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
			v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
		end
		swarm3_config.vm.provision :hosts do |provisioner|
			provisioner.add_host '10.1.42.210', ['swarm1.local']
			provisioner.add_host '10.1.42.220', ['swarm2.local']
			provisioner.add_host '10.1.42.230', ['swarm3.local']
		end
		swarm3_config.vm.provision :ansible do |ansible|
			ansible.limit = 'all'
			ansible.playbook = "site.yml"
			ansible.inventory_path = "development"
			ansible.extra_vars = { ansible_ssh_user: 'vagrant', private_network_iface: 'ansible_eth1' }
			ansible.sudo = true
		end
	end


end
