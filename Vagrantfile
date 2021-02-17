VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  #config.vm.box = "debian/buster64"
  #config.vm.box_version = "9.99.1" #9gb but OLD
  config.vm.box = "debian/stretch64" #20gb, can't downsize
  #vagrant plugin install vagrant-disksize
  config.disksize.size = '10GB' #if box less than 10gb 
  config.ssh.insert_key = true

  config.vm.provider :virtualbox do |v|
    v.name = "samson"
    v.memory = 2048
    v.cpus = 2
    v.gui = false
 #   v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
 #   v.customize ["modifyvm", :id, "--ioapic", "on"]
  end

  config.vm.hostname = "samson"
 # config.vm.network :private_network, ip: "192.168.33.3"
  config.vm.network :public_network, bridge: "wlan0"

  # Set the name of the VM. See: http://stackoverflow.com/a/17864388/100134
  config.vm.define :samson do |samson|
  end
  #My ssh pub key
  config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "/tmp/me.pub"
  config.vm.provision "shell", inline: <<-SHELL
    mkdir /root/.ssh
    cat /tmp/me.pub >> /root/.ssh/authorized_keys
    cat /tmp/me.pub >> /home/vagrant/.ssh/authorized_keys
    rm /tmp/me.pub
  SHELL

  # Ansible provisioner.
  config.vm.provision "ansible" do |ansible|
    ansible.compatibility_mode = "2.0"
    ansible.playbook = "provisioning/playbook.yml"
    ansible.inventory_path = "provisioning/inventory"
    ansible.become = true
  end
end