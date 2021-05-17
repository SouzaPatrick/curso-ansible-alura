$script_init_ansible = <<-SCRIPT
  apt-get update && \
  apt-get install -y software-properties-common && \
  apt-add-repository --yes --update ppa:ansible/ansible && \
  apt-get install -y ansible && \
  cp /vagrant/id_ansible /home/vagrant && \
  chmod 600 /home/vagrant/id_ansible && \
  chown vagrant:vagrant /home/vagrant/id_ansible
SCRIPT

$script_run_ansible = <<-SCRIPT
  ansible-playbook -i /vagrant/configs/ansible/hosts /vagrant/configs/ansible/playbook.yml
SCRIPT

$script_install_python36 = <<-SCRIPT
  apt-get update && \
  apt-add-repository --yes ppa:deadsnakes/ppa && \
  apt-get update && \
  apt-get install -y python3.6 && \
  ln -sf /usr/bin/python3.6 /usr/bin/python3  && \
  ln -s /usr/lib/python3/dist-packages/apt_inst.cpython-34m-x86_64-linux-gnu.so /usr/lib/python3/dist-packages/apt_inst.cpython-36m-x86_64-linux-gnu.so  && \
  ln -s /usr/lib/python3/dist-packages/apt_pkg.cpython-34m-x86_64-linux-gnu.so /usr/lib/python3/dist-packages/apt_pkg.cpython-36m-x86_64-linux-gnu.so
SCRIPT

Vagrant.configure("2") do |config|

    config.vm.box = "ubuntu/trusty64"
    config.vm.boot_timeout = 2000

    config.vm.provider "virtualbox" do |v|
        v.memory = 512
        v.cpus = 1
    end

    config.vm.define "ansible" do |ansible|
        ansible.vm.box = "ubuntu/bionic64"
        ansible.vm.network "private_network", ip: "172.17.177.39"
        ansible.vm.provision "shell", inline: $script_init_ansible
        # ansible.vm.provision "shell", inline: $script_run_ansible
        ansible.vm.synced_folder "./configs/ansible", "/home/vagrant/configs"
    end

    config.vm.define "wordpress" do |m|
        m.vm.network "private_network", ip: "172.17.177.40"
        m.vm.provider "virtualbox" do |v|
            v.memory = 1024
            v.cpus = 2
        end
        m.vm.provision "shell", inline: "cat /vagrant/configs/id_ansible.pub >> .ssh/authorized_keys"
        m.vm.provision "shell", inline: $script_install_python36
    end

end