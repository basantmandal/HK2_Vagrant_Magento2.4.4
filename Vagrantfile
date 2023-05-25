# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
    config.vm.box = "basantmandal/HK2_Vagrant_Magento2.4.4"
    config.vm.box_version = "1.0.1"
    config.vm.network "private_network", ip: "192.168.2.183"

    # Default HTTP port
    config.vm.network "forwarded_port", guest: 80, host: 8080, auto_correct: true
    config.vm.network "forwarded_port", guest: 443, host: 8443, auto_correct: true

    # MySQL Port
    config.vm.network "forwarded_port", guest: 3306, host: 3306

    # Redis Port
    config.vm.network "forwarded_port", guest: 6379, host: 6379, auto_correct: true

    # OpenSearch Port
    config.vm.network "forwarded_port", guest: 9200, host: 9200, auto_correct: true
    config.vm.network "forwarded_port", guest: 9300, host: 9300, auto_correct: true

    config.vm.provider "virtualbox" do |vb|
      # Customize CPU, Ram, Vbox Name & Disable Audio
      vb.cpus = 2
      vb.memory = "8192"
      vb.name = "HK2_magento244"
      vb.customize ["modifyvm", :id, "--audio", "none"]

      # Fixing issue of VM loosing sync of the date and time while sleeping.
      vb.customize ['guestproperty', 'set', :id, '/VirtualBox/GuestAdd/VBoxService/--timesync-set-threshold', 10000]
    end

    # Share Local Folder with Vagrant Shared Folder
    # config.vm.synced_folder "/var/www/magento244/app/code", "/var/www/magento244/app/code", owner: "vagrant", group: "www-data"

    # Provision - Clean Magento Logs
    config.vm.provision "shell", privileged: true, inline: <<-SHELL
        echo $(tail -5000 /var/www/magento244/var/log/exception.log) >/var/www/magento244/var/log/exception.log
        echo $(tail -5000 /var/www/magento244/var/log/system.log) >/var/www/magento244/var/log/system.log
        echo $(tail -5000 /var/www/magento244/var/log/cron.log) >/var/www/magento244/var/log/cron.log
        echo $(tail -5000 /var/www/magento244/var/log/debug.log) >/var/www/magento244/var/log/debug.log
    SHELL
end
