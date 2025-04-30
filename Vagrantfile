Vagrant.configure(2) do |config|
  config.vm.define "node01" do |node|
    node.vm.network "private_network", ip: "192.168.200.101"
    node.vm.hostname = "node01"
    node.vm.box = "ubuntu2204lts/minimal"
    node.vm.provision "shell", inline: <<-EOF
      sudo -s
      echo 192.168.200.101 node01 >> /etc/hosts
      echo 192.168.200.102 node02 >> /etc/hosts
      echo 192.168.200.103 node03 >> /etc/hosts
      echo 192.168.200.104 node04 >> /etc/hosts
      
      curl https://www.beegfs.io/release/beegfs_8.0/dists/beegfs-jammy.list > /etc/apt/sources.list.d/beegfs-jammy.list
      curl https://www.beegfs.io/release/beegfs_8.0/dists/jammy/Release.gpg | apt-key add -
      apt-get update
      apt-get install -yq beegfs-mgmtd 
      sed -i 's/storeMgmtdDirectory\ =.*/storeMgmtdDirectory = \/mnt\/beegfs_mgmtd/g' /etc/beegfs/beegfs-mgmtd.conf
      mkdir -p /mnt/beegfs_mgmtd
      systemctl start beegfs-mgmtd
      systemctl enable beegfs-mgmtd
  EOF
  end

  config.vm.define "node02" do |node|
    node.vm.network "private_network", ip: "192.168.200.102"
    node.vm.hostname = "node02"
    node.vm.box = "ubuntu2204lts/minimal"
    node.vm.provision "shell", inline: <<-EOF
      sudo -s
      echo 192.168.200.101 node01 >> /etc/hosts
      echo 192.168.200.102 node02 >> /etc/hosts
      echo 192.168.200.103 node03 >> /etc/hosts
      echo 192.168.200.104 node04 >> /etc/hosts
      
      curl https://www.beegfs.io/release/beegfs_8.0/dists/beegfs-jammy.list > /etc/apt/sources.list.d/beegfs-jammy.list
      curl https://www.beegfs.io/release/beegfs_8.0/dists/jammy/Release.gpg | apt-key add -
      apt-get update
      apt-get install -yq beegfs-meta 
      sed -i 's/storeMetaDirectory\ =.*/storeMetaDirectory = \/mnt\/beegfs_meta/g' /etc/beegfs/beegfs-meta.conf
      sed -i 's/sysMgmtdHost\ =.*/sysMgmtdHost = 192.168.200.101/g' /etc/beegfs/beegfs-meta.conf
      
      mkdir -p /mnt/beegfs_meta
      systemctl start beegfs-meta
      systemctl enable beegfs-meta
  EOF
  end

  config.vm.define "node03" do |node|
    node.vm.network "private_network", ip: "192.168.200.103"
    node.vm.hostname = "node03"
    node.vm.box = "ubuntu2204lts/minimal"
    node.vm.provision "shell", inline: <<-EOF
      sudo -s
      echo 192.168.200.101 node01 >> /etc/hosts
      echo 192.168.200.102 node02 >> /etc/hosts
      echo 192.168.200.103 node03 >> /etc/hosts
      echo 192.168.200.104 node04 >> /etc/hosts
      
      cd /etc/apt/sources.list.d/
      curl https://www.beegfs.io/release/beegfs_8.0/dists/beegfs-jammy.list > /etc/apt/sources.list.d/beegfs-jammy.list
      curl https://www.beegfs.io/release/beegfs_8.0/dists/jammy/Release.gpg | apt-key add -
      apt-get update
      apt-get install -yq beegfs-storage
      sed -i 's/storeStorageDirectory\ =.*/storeStorageDirectory = \/mnt\/beegfs_storage/g' /etc/beegfs/beegfs-storage.conf
      sed -i 's/sysMgmtdHost\ =.*/sysMgmtdHost = 192.168.200.101/g' /etc/beegfs/beegfs-storage.conf
      
      mkdir -p /mnt/beegfs_storage
      systemctl start beegfs-storage
      systemctl enable beegfs-storage
  EOF
  end

  # Client node configuration
  config.vm.define "node04" do |node|
    node.vm.network "private_network", ip: "192.168.200.104"
    node.vm.hostname = "node04"
    node.vm.box = "ubuntu2204lts/minimal"
    node.vm.provision "shell", inline: <<-EOF
      sudo -s
      echo 192.168.200.101 node01 >> /etc/hosts
      echo 192.168.200.102 node02 >> /etc/hosts
      echo 192.168.200.103 node03 >> /etc/hosts
      echo 192.168.200.104 node04 >> /etc/hosts
      
      curl https://www.beegfs.io/release/beegfs_8.0/dists/beegfs-jammy.list > /etc/apt/sources.list.d/beegfs-jammy.list
      curl https://www.beegfs.io/release/beegfs_8.0/dists/jammy/Release.gpg | apt-key add -
      apt-get update
      apt install beegfs-client beegfs-helperd beegfs-utils
      sed -i 's/sysMgmtdHost\ =.*/sysMgmtdHost\ = 192.168.200.101' /etc/beegfs/beegfs-client.conf
      
      mkdir /mnt/beegfs
      echo "beegfs /mnt/beegfs beegfs defaults 0 0" >> /etc/fstab
      mount /mnt/beegfs
      systemctl start beegfs-helperd
      systemctl start beegfs-client
      systemctl enable beegfs-helperd
      systemctl enable beegfs-client
  EOF
  end
end
