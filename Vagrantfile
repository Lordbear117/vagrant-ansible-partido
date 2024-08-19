Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64" # Imagen base de Ubuntu
  config.vm.network "private_network", ip: "192.168.56.2"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
    vb.cpus = 2
  end

  # Provisionamiento usando Ansible Local
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "ansible/playbook.yml"
  end
end
