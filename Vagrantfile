Vagrant.configure(2) do |config|
  config.vm.synced_folder ".", "/vagrant", type: "nfs", disabled: true
  config.vm.provider :libvirt do |libvirt|
    libvirt.qemu_use_session = false
    libvirt.memory = 4096
    libvirt.cpus = 2
    # if the above doesn't work, try uncommenting the following instead
    #libvirt.uri = 'qemu:///system'
  end
  config.vm.define "node01" do |node|
    node.vm.network "private_network", ip: "192.168.200.101"
    node.vm.hostname = "node01"
    node.vm.box = "fedora/41-cloud-base"
    node.vm.provision "shell", inline: <<-EOF
    sudo -s
    echo 192.168.200.101 node01 >> /etc/hosts
    echo 192.168.200.102 node02 >> /etc/hosts
    echo 192.168.200.103 node03 >> /etc/hosts
    echo 192.168.200.104 node04 >> /etc/hosts
    
    curl https://www.beegfs.io/release/beegfs_8.0/dists/beegfs-rhel9.repo > /etc/yum.repos.d/beegfs-rhel9.repo
    dnf repolist
    dnf install -y beegfs-mgmtd 
    sed -i 's/# tls-disable.*/tls-disable=true/g' /etc/beegfs/beegfs-mgmtd.toml
    sed -i 's/# auth-disable.*/auth-disable=true/g' /etc/beegfs/beegfs-mgmtd.toml
    /opt/beegfs/sbin/beegfs-mgmtd --init
    mkdir -p /mnt/beegfs_mgmtd
    systemctl start beegfs-mgmtd
    systemctl enable beegfs-mgmtd
    EOF
  end

  config.vm.define "node02" do |node|
    node.vm.network "private_network", ip: "192.168.200.102"
    node.vm.hostname = "node02"
    node.vm.box = "fedora/41-cloud-base"
    node.vm.provision "shell", inline: <<-EOF
    sudo -s
    echo 192.168.200.101 node01 >> /etc/hosts
    echo 192.168.200.102 node02 >> /etc/hosts
    echo 192.168.200.103 node03 >> /etc/hosts
    echo 192.168.200.104 node04 >> /etc/hosts
    
    curl https://www.beegfs.io/release/beegfs_8.0/dists/beegfs-rhel9.repo > /etc/yum.repos.d/beegfs-rhel9.repo
    dnf repolist
    dnf install -yq beegfs-meta 
    mkdir -p /data/beegfs/beegfs_meta
    /opt/beegfs/sbin/beegfs-setup-meta -p /data/beegfs/beegfs_meta -s 2 -m node01
    systemctl start beegfs-meta
    systemctl enable beegfs-meta
    EOF
  end

  config.vm.define "node03" do |node|
    node.vm.network "private_network", ip: "192.168.200.103"
    node.vm.hostname = "node03"
    node.vm.box = "fedora/41-cloud-base"
    node.vm.provision "shell", inline: <<-EOF
    sudo -s
    echo 192.168.200.101 node01 >> /etc/hosts
    echo 192.168.200.102 node02 >> /etc/hosts
    echo 192.168.200.103 node03 >> /etc/hosts
    echo 192.168.200.104 node04 >> /etc/hosts
    
    curl https://www.beegfs.io/release/beegfs_8.0/dists/beegfs-rhel9.repo > /etc/yum.repos.d/beegfs-rhel9.repo
    dnf repolist
    dnf install -yq beegfs-storage
    mkdir -p /mnt/beegfs/beegfs_storage
    /opt/beegfs/sbin/beegfs-setup-storage -p /mnt/myraid1/beegfs_storage -s 3 -i 301 -m node01 
    systemctl start beegfs-storage
    systemctl enable beegfs-storage
    EOF
  end

  # Client node configuration
  config.vm.define "node04" do |node|
    node.vm.network "private_network", ip: "192.168.200.104"
    node.vm.hostname = "node04"
    node.vm.box = "fedora/41-cloud-base"
    node.vm.provision "shell", inline: <<-EOF
    sudo -s
    echo 192.168.200.101 node01 >> /etc/hosts
    echo 192.168.200.102 node02 >> /etc/hosts
    echo 192.168.200.103 node03 >> /etc/hosts
    echo 192.168.200.104 node04 >> /etc/hosts
    
    curl https://www.beegfs.io/release/beegfs_8.0/dists/beegfs-rhel9.repo > /etc/yum.repos.d/beegfs-rhel9.repo
    dnf repolist
    dnf install -y beegfs-client beegfs-utils kernel-devel
    /opt/beegfs/sbin/beegfs-setup-client -m node01
    mkdir /mnt/beegfs
    systemctl start beegfs-client
    systemctl enable beegfs-client
    EOF
  end
end
