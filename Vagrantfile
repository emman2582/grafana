# Code review comments:
# - The mysql_server configuration is placed inside the grafana_server block, which is incorrect
# - Missing end statement for the outer Vagrant.configure block
# - The mysql_server configuration should be in its own config.vm.define block

Vagrant.configure("2") do |config|
  config.vm.define "grafana-server" do |grafana_server|
	grafana_server.vm.box = "bento/ubuntu-24.04"
	grafana_server.vm.box_version = "202502.21.0"
	grafana_server.vm.hostname = "grafana-server"
	grafana_server.vm.network "private_network", ip: "192.168.56.36"
	grafana_server.vm.provider "virtualbox" do |vb|
	  vb.memory = "2048"
	  vb.cpus = 2
	end

	grafana_server.vm.provision "shell", inline: <<-SHELL
	  sudo apt-get update
	  sudo apt-get install -y adduser libfontconfig1 musl
	  if [ ! -f grafana_11.2.0_amd64.deb ]; then
		wget https://dl.grafana.com/oss/release/grafana_11.2.0_amd64.deb
	  fi
	  sudo dpkg -i grafana_11.2.0_amd64.deb
	  sudo apt-get install -f -y
	  sudo systemctl start grafana-server.service
	  # Optionally log service status for debugging
	  sudo systemctl status grafana-server.service > /vagrant/grafana-status.log 2>&1
	  sudo systemctl enable grafana-server.service
	SHELL
  end

  config.vm.define "mysql-server" do |mysql_server|
	mysql_server.vm.box = "bento/ubuntu-24.04"
	mysql_server.vm.box_version = "202502.21.0"
	mysql_server.vm.hostname = "mysql-server"
	mysql_server.vm.network "private_network", ip: "192.168.56.37"
	mysql_server.vm.provider "virtualbox" do |vb|
	  vb.memory = "2048"
	  vb.cpus = 2
	end

	mysql_server.vm.provision "shell", inline: <<-SHELL
	  sudo apt-get update
	  sudo apt-get install -y mysql-server
	  sudo systemctl status mysql-server.service > /vagrant/mysql-status.log 2>&1
	  wget https://raw.githubusercontent.com/meob/my2Collector/master/my2_80.sql
	SHELL
  end
end
