$script_init_ansible = <<-SCRIPT
  apt-get update && \
  apt-get install -y ansible && \
  apt-get install -y software-properties-common && \
  apt-add-repository --yes --update ppa:ansible/ansible && \
  mv /home/vagrant/configs/id_ansible /home/vagrant && \
  chmod 600 /home/vagrant/id_ansible && \
  chown vagrant:vagrant /home/vagrant/id_ansible
SCRIPT

$script_run_ansible = <<-SCRIPT
  ansible-playbook -i /vagrant/configs/ansible/hosts /vagrant/configs/ansible/playbook.yml
SCRIPT

Vagrant.configure("2") do |config|

    config.vm.box = "ubuntu/hirsute64"
    config.vm.boot_timeout = 1440

    config.vm.provider "virtualbox" do |v|
        v.memory = 512
        v.cpus = 1
    end

    config.vm.define "ansible" do |ansible|
        ansible.vm.network "private_network", ip: "172.17.177.39"
        ansible.vm.provision "shell", inline: $script_init_ansible
        # ansible.vm.provision "shell", inline: $script_run_ansible
        ansible.vm.synced_folder "./configs/ansible", "/home/vagrant/configs"
        ansible.vm.synced_folder ".", "/vagrant", disabled: true
    end

    config.vm.define "wordpress" do |m|
        m.vm.network "private_network", ip: "172.17.177.40"
        config.vm.provider "virtualbox" do |v|
            v.memory = 1024
            v.cpus = 2
        end
        m.vm.provision "shell", inline: "cat /vagrant/configs/id_ansible.pub >> .ssh/authorized_keys"
    end

end