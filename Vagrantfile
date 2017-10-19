unless Vagrant.has_plugin?("vagrant-docker-compose")
  system("vagrant plugin install vagrant-docker-compose")
  puts "Dependencies installed, please try the command again."
  exit
end

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.provider "virtualbox"
  config.vm.synced_folder './projects', '/home/vagrant/projects'
  config.vm.provider :virtualbox do |vb|
    vb.customize ['modifyvm', :id, '--memory', '2048', '--cpus', '2']
	vb.customize ["modifyvm", :id, "--usb", "on"]
	vb.customize ["usbfilter", "add", "0", "--target", :id, "--name", "android", "--vendorid", "0x18d1"]
  end
  
  config.vm.network "private_network", ip: "192.168.33.10"
  config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.network "forwarded_port", guest: 3306, host: 3306
  config.vm.network "forwarded_port", guest: 8100, host: 8100
  config.vm.network "forwarded_port", guest: 35729, host: 35729
  
  config.vm.provision :shell, path: "bootstrap.sh"
 
  config.vm.provision "docker" do |d|
    d.pull_images "ubuntu"
    d.pull_images "nginx"
	d.pull_images "mysql"
	d.pull_images "redis"
	d.pull_images "php:7-fpm"
  end
  
  config.vm.provision :docker_compose
end