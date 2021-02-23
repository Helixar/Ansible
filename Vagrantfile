Vagrant.configure("2") do |config|
  config.vm.box = "bento/debian-10"
  config.env.enable
  config.vm.define ENV['VM_NAME']
  config.vm.network "private_network", ip: ENV['VM_IP']
  config.vm.provision "shell" do |s|
    ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
    s.inline = <<-SHELL
      echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
      sudo mkdir -p /root/.ssh
      echo #{ssh_pub_key} >> /root/.ssh/authorized_keys
      sudo apt-get install -y python
    SHELL
  end
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "install.yml"
  end
  config.vm.provider "virtualbox" do |v|
    v.memory = ENV['VM_MEMORY']
    v.cpus = ENV['VM_CPU']
    v.name = ENV['VM_NAME']
  end
end