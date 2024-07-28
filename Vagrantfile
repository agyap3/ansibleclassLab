Vagrant.configure("2") do |config|
  config.vm.box = "almalinux/8"

  # Define the control node
  config.vm.define "control" do |control|
    control.vm.hostname = "control"
    control.vm.network "private_network", ip: "192.168.20.10"
    control.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
      vb.cpus = 1
    end

    control.vm.provision "shell", inline: <<-SHELL
      # Install necessary packages
      sudo dnf install epel-release --nogpgcheck -y
      sudo dnf install ansible sshpass --nogpgcheck -y

      # Create ansible_admin user
      sudo useradd ansible_admin
      echo "ansible_admin:ansible_admin" | sudo chpasswd

      # Set up SSH for ansible_admin
      sudo mkdir -p /home/ansible_admin/.ssh
      sudo chown ansible_admin:ansible_admin /home/ansible_admin/.ssh
      sudo chmod 700 /home/ansible_admin/.ssh

      # Add ansible_admin to sudoers
      echo 'ansible_admin ALL=(ALL) NOPASSWD: ALL' | sudo tee /etc/sudoers.d/ansible_admin

      # Add vagrant user to sudoers
      echo 'vagrant ALL=(ALL) NOPASSWD: ALL' | sudo tee /etc/sudoers.d/vagrant

      # Add managed nodes to /etc/hosts
      echo '192.168.20.10 control' | sudo tee -a /etc/hosts
      echo '192.168.20.11 node1' | sudo tee -a /etc/hosts
      echo '192.168.20.12 node2' | sudo tee -a /etc/hosts
      echo '8.8.8.8 google' | sudo tee -a /etc/hosts

      # Create Ansible inventory
      echo "[managed_nodes]
      node1 ansible_host=node1 ansible_user=ansible_admin ansible_become=yes ansible_become_method=sudo
      node2 ansible_host=node2 ansible_user=ansible_admin ansible_become=yes ansible_become_method=sudo" > /home/vagrant/inventory

      sudo chown vagrant:vagrant /home/vagrant/inventory
    SHELL
  end

  # Define the first managed node
  config.vm.define "node1" do |node1|
    node1.vm.hostname = "node1"
    node1.vm.network "private_network", ip: "192.168.20.11"
    node1.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
      vb.cpus = 1
    end

    node1.vm.provision "shell", inline: <<-SHELL
      # Install necessary packages
      sudo dnf install epel-release --nogpgcheck -y
      sudo dnf install ansible sshpass --nogpgcheck -y

      # Create ansible_admin user
      sudo useradd ansible_admin
      echo "ansible_admin:ansible_admin" | sudo chpasswd

      # Set up SSH for ansible_admin
      sudo mkdir -p /home/ansible_admin/.ssh
      sudo chown ansible_admin:ansible_admin /home/ansible_admin/.ssh
      sudo chmod 700 /home/ansible_admin/.ssh

      # Add ansible_admin to sudoers
      echo 'ansible_admin ALL=(ALL) NOPASSWD: ALL' | sudo tee /etc/sudoers.d/ansible_admin

      # Add vagrant user to sudoers
      echo 'vagrant ALL=(ALL) NOPASSWD: ALL' | sudo tee /etc/sudoers.d/vagrant

      # Add control and node2 to /etc/hosts
      echo '192.168.20.10 control' | sudo tee -a /etc/hosts
      echo '192.168.20.11 node1' | sudo tee -a /etc/hosts
      echo '192.168.20.12 node2' | sudo tee -a /etc/hosts
      echo '8.8.8.8 google' | sudo tee -a /etc/hosts
    SHELL
  end

  # Define the second managed node
  config.vm.define "node2" do |node2|
    node2.vm.hostname = "node2"
    node2.vm.network "private_network", ip: "192.168.20.12"
    node2.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
      vb.cpus = 1
    end

    node2.vm.provision "shell", inline: <<-SHELL
      # Install necessary packages
      sudo dnf install epel-release --nogpgcheck -y
      sudo dnf install ansible sshpass --nogpgcheck -y

      # Create ansible_admin user
      sudo useradd ansible_admin
      echo "ansible_admin:ansible_admin" | sudo chpasswd

      # Set up SSH for ansible_admin
      sudo mkdir -p /home/ansible_admin/.ssh
      sudo chown ansible_admin:ansible_admin /home/ansible_admin/.ssh
      sudo chmod 700 /home/ansible_admin/.ssh

      # Add ansible_admin to sudoers
      echo 'ansible_admin ALL=(ALL) NOPASSWD: ALL' | sudo tee /etc/sudoers.d/ansible_admin

      # Add vagrant user to sudoers
      echo 'vagrant ALL=(ALL) NOPASSWD: ALL' | sudo tee /etc/sudoers.d/vagrant

      # Add control and node1 to /etc/hosts
      echo '192.168.20.10 control' | sudo tee -a /etc/hosts
      echo '192.168.20.11 node1' | sudo tee -a /etc/hosts
      echo '192.168.20.12 node2' | sudo tee -a /etc/hosts
      echo '8.8.8.8 google' | sudo tee -a /etc/hosts
    SHELL
  end
end

