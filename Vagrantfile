Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"
  config.vm.hostname = "yolo"
  config.vm.network "forwarded_port", guest: 3000, host: 3000
  config.vm.network "forwarded_port", guest: 5000, host: 5000
  config.vm.network "forwarded_port", guest: 27017, host: 27017
  config.vm.network "forwarded_port", guest: 80, host: 8080

  # Sync your project folder with the VM
  config.vm.synced_folder ".", "/vagrant", type: "virtualbox"

  # Provision using Ansible
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yaml"
    ansible.inventory_path = "inventory.yaml"
  end
end
