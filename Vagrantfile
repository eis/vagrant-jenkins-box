# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

# Plugin installation procedure from http://stackoverflow.com/a/28801317
required_plugins = %w(vagrant-omnibus)

plugins_to_install = required_plugins.select { |plugin| not Vagrant.has_plugin? plugin }
if not plugins_to_install.empty?
  puts "Installing plugins: #{plugins_to_install.join(' ')}"
  if system "vagrant plugin install #{plugins_to_install.join(' ')}"
    exec "vagrant #{ARGV.join(' ')}"
  else
    abort "Installation of one or more plugins has failed. Aborting."
  end
end


Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "nrel/CentOS-6.5-x86_64"

  # Jenkins
  config.vm.network :forwarded_port, guest: 8080, host: 8082
  config.omnibus.chef_version = :latest
  config.vm.provision :shell, :privileged => true, :inline => "yum install java-1.7.0-openjdk -y"
  config.vm.provision :shell, :privileged => true, :inline => "service iptables stop"

  config.vm.provision "chef_solo" do |chef|
    chef.add_recipe "jenkins::master"
    #chef.json = { :mysql_password => "foo" }
  end

end
