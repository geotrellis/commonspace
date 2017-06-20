# -*- mode: ruby -*-
# vi: set ft=ruby :

ANSIBLE_VERSION = "2.3.1.0"
Vagrant.configure("2") do |config|

    # Ubuntu 14.04 LTS
    config.vm.box = "ubuntu/trusty64"

    # Ports to the services
    config.vm.network :forwarded_port, guest: 9999, host: 9999  # gt-transit
    config.vm.network :private_network, ip: ENV.fetch("GT_TRANSIT_WEB_IP",  "10.10.10.10")
    # VM resource settings
    config.vm.provider :virtualbox do |vb|
        vb.memory = 4096
        vb.cpus = 2
    end

    config.vm.synced_folder "~/.aws", "/home/vagrant/.aws"
    config.vm.synced_folder "./", "/home/vagrant/geotrellis-transit", type: "nfs"

    # Provisioning
    # Ansible is installed automatically by Vagrant.
    config.vm.provision "ansible_local" do |ansible|
        ansible.install = true
        ansible.install_mode = :pip
        ansible.version = "#{ANSIBLE_VERSION}"
        ansible.playbook = "deployment/ansible/playbook.yml"
        ansible.galaxy_role_file = "deployment/ansible/roles.yml"
        ansible.galaxy_roles_path = "deployment/ansible/roles"
    end

    config.vm.provision "shell" do |s|
        s.path = 'deployment/vagrant/cd_shared_folder.sh'
        s.args = "/home/vagrant/geotrellis-transit"
    end

end
