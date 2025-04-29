Vagrant.configure(2) do |config|
  config.vm.define "node01" do |node|
    node.vm.network "private_network", ip: "192.168.200.101"
    node.vm.hostname = "node01"
    node.vm.box = "ubuntu2204lts/minimal"
    node.vm.provision "shell", inline: <<-EOF
	sudo -s
	curl https://www.beegfs.io/release/beegfs_8.0/dists/beegfs-jammy.list > /etc/apt/sources.list.d/beegfs-jammy.list
	curl https://www.beegfs.io/release/beegfs_8.0/dists/jammy/Release.gpg | apt-key add -
	apt-get update
	apt-get install -yq beegfs-mgmtd 
	echo 192.168.200.101 node01 >> /etc/hosts
	echo 192.168.200.102 node02 >> /etc/hosts
	echo 192.168.200.103 node03 >> /etc/hosts
  EOF
  end

  config.vm.define "node02" do |node|
    node.vm.network "private_network", ip: "192.168.200.102"
    node.vm.hostname = "node02"
    node.vm.box = "ubuntu2204lts/minimal"
    node.vm.provision "shell", inline: <<-EOF
	sudo -s
	curl https://www.beegfs.io/release/beegfs_8.0/dists/beegfs-jammy.list > /etc/apt/sources.list.d/beegfs-jammy.list
	curl https://www.beegfs.io/release/beegfs_8.0/dists/jammy/Release.gpg | apt-key add -
	apt-get update
	apt-get install -yq beegfs-meta 
	echo 192.168.200.101 node01 >> /etc/hosts
	echo 192.168.200.102 node02 >> /etc/hosts
	echo 192.168.200.103 node03 >> /etc/hosts
  EOF
  end

  config.vm.define "node03" do |node|
    node.vm.network "private_network", ip: "192.168.200.103"
    node.vm.hostname = "node03"
    node.vm.box = "ubuntu2204lts/minimal"
    node.vm.provision "shell", inline: <<-EOF
	sudo -s
	cd /etc/apt/sources.list.d/
	curl https://www.beegfs.io/release/beegfs_8.0/dists/beegfs-jammy.list > /etc/apt/sources.list.d/beegfs-jammy.list
	curl https://www.beegfs.io/release/beegfs_8.0/dists/jammy/Release.gpg | apt-key add -
	apt-get update
	apt-get install -yq beegfs-storage
 	echo 192.168.200.101 node01 >> /etc/hosts
	echo 192.168.200.102 node02 >> /etc/hosts
	echo 192.168.200.103 node03 >> /etc/hosts
  EOF
  end
end
