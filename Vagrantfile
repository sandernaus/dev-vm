Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"

  config.vm.hostname = "dev-machine"
  config.vm.network :private_network, ip: "192.168.45.2"
  config.vm.network "forwarded_port", guest: 5901, host: 5901
  config.vm.network "forwarded_port", guest: 5986, host: 5986

  # config.customize ["modifyvm", :id, "--nested-hw-virt", "on"]

  username = "#{ENV['USERNAME'] || `whoami`}"
  ssh_pub_key = File.readlines("C:/Users/#{username}/.ssh/id_rsa.pub").first.strip

  config.vm.provision 'shell', inline: "echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys", privileged: true

  config.vbguest.iso_path = "C:/Program Files/Oracle/VirtualBox/VBoxGuestAdditions.iso"

  config.vm.synced_folder "./", "/vagrant",
      id: "vagrant",
      automount: true,
      mount_options: ["dmode=755,fmode=777"]

  config.vm.synced_folder "C:/Data/Git", "/git",
      id: "git",
      automount: true,
      mount_options: ["dmode=755,fmode=777"]

  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "playbook.yml"
  end

end
