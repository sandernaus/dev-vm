Vagrant.configure("2") do |config|

  username = "#{ENV['USERNAME'] || `whoami`}"
  ssh_pub_key = File.readlines("C:/Users/#{username}/.ssh/id_rsa.pub").first.strip

  config.vm.box = "centos/7"

  config.vm.define "dev" do |dev|

    dev.vm.provider "virtualbox" do |vb|
      vb.name = "dev-machine"
      vb.memory = "4096"
      vb.cpus = "2"
      vb.customize ["modifyvm", :id, "--nested-hw-virt", "on"]
    end

    dev.vm.hostname = "dev-machine"
    # dev.vm.network :private_network, ip: "192.168.45.2"
    dev.vm.network "forwarded_port", guest: 5901, host: 5901
    dev.vm.network "forwarded_port", guest: 5986, host: 5986
    dev.vm.network "forwarded_port", guest: 80, host: 80
    dev.vm.network "forwarded_port", guest: 443, host: 443

    dev.vm.provision 'shell', inline: "echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys", privileged: true

    dev.vbguest.iso_path = "C:/Program Files/Oracle/VirtualBox/VBoxGuestAdditions.iso"

    dev.vm.synced_folder "./", "/vagrant",
        id: "vagrant",
        automount: true,
        mount_options: ["dmode=755,fmode=777"]

    dev.vm.synced_folder "C:/Data/Git", "/git",
        id: "git",
        automount: true,
        mount_options: ["dmode=755,fmode=777"]

    dev.vm.synced_folder "C:/Data/quickies", "/quickies",
        id: "quickies",
        automount: true,
        mount_options: ["dmode=755,fmode=777"]

    dev.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "playbook.yml"
    end
  end

end
