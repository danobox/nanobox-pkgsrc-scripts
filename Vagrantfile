# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure('2') do |config|
  config.vm.provider 'virtualbox' do |v|
    v.memory = 4096
    v.cpus   = 4
  end

  config.vm.network "private_network", type: "dhcp"

  config.vm.define "Ubuntu" do |ubuntu|
    ubuntu.vm.box = 'ubuntu/trusty64'
  end

  # config.vm.define "SmartOS" do |smartos|
  #   smartos.vm.box = 'livinginthepast/smartos-base64'
  #   smartos.vm.network "private_network", ip: "33.33.33.10"
  #   smartos.vm.communicator = 'smartos'
  #   smartos.ssh.insert_key = false
  #   smartos.global_zone.platform_image = 'latest'
  #   smartos.zone.name = 'base64'
  #   smartos.zone.brand = 'joyent'
  #   smartos.zone.image = '0edf00aa-0562-11e5-b92f-879647d45790'
  #   smartos.zone.memory = 4096
  #   smartos.zone.disk_size = 20
  # end

  nanobox_user = ENV["NANOBOX_USER"]
  nanobox_base_project = ENV["NANOBOX_BASE_PROJECT"] || "base"
  nanobox_gonano_project = ENV["NANOBOX_GONANO_PROJECT"] || "gonano"
  nanobox_base_secret = ENV["NANOBOX_#{nanobox_user.upcase}_#{nanobox_base_project.upcase}_SECRET"]
  nanobox_gonano_secret = ENV["NANOBOX_#{nanobox_user.upcase}_#{nanobox_gonano_project.upcase}_SECRET"]

  config.vm.synced_folder "../distfiles", "/content/distfiles", type: "nfs"
  config.vm.synced_folder "../packages", "/content/packages", type: "nfs"
  config.vm.synced_folder "../pkgsrc-lite", "/content/pkgsrc", type: "nfs"
  config.vm.synced_folder "../pkgsrc-base", "/content/pkgsrc/base", type: "nfs"
  config.vm.synced_folder "../pkgsrc-gonano", "/content/pkgsrc/gonano", type: "nfs"

  $script = <<-SCRIPT
  echo # Vagrant environment variables > /etc/profile.d/vagrant.sh
  echo export NANOBOX_USER=#{nanobox_user} >> /etc/profile.d/vagrant.sh
  echo export NANOBOX_BASE_PROJECT=#{nanobox_base_project} >> /etc/profile.d/vagrant.sh
  echo export NANOBOX_BASE_SECRET=#{nanobox_base_secret} >> /etc/profile.d/vagrant.sh
  echo export NANOBOX_GONANO_PROJECT=#{nanobox_gonano_project} >> /etc/profile.d/vagrant.sh
  echo export NANOBOX_GONANO_SECRET=#{nanobox_gonano_secret} >> /etc/profile.d/vagrant.sh
  echo umask 022 >> /etc/profile.d/vagrant.sh
  chmod +x /etc/profile.d/vagrant.sh
  SCRIPT

  config.vm.provision "shell", inline: $script

end