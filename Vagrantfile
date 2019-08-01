# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'fileutils'
require 'yaml'

## Read VM settings from "Vagrant.yml". These settings can be overridden by
## values in an optional "Vagrant.local.yml" file, which is ignored by git.

dir = File.dirname(File.expand_path(__FILE__))

# Copy the provision script into place.
if !File.exists?("#{dir}/provision.sh")
  FileUtils.copy_file("#{dir}/dist/provision.sh", "#{dir}/provision.sh")
end

# Copy the Vagrant.yml file into place.
if !File.exists?("#{dir}/Vagrant.yml")
  FileUtils.copy_file("#{dir}/dist/Vagrant.yml", "#{dir}/Vagrant.yml")
  puts "Vagrant.yml copied into place. You should edit this file and run this command again."
  abort
end

settings = YAML::load_file("#{dir}/Vagrant.yml")

# Require vagrant 1.8.1 or higher
Vagrant.require_version ">= 1.8.1"

Vagrant.configure(2) do |config|
  config.vagrant.plugins = ["vagrant-disksize"]
  config.vm.hostname = settings["vb"]["hostname"]
  config.vm.box = settings["vb"]["box"]
  config.vm.network "private_network", type: "dhcp"
  config.vm.synced_folder ".", "/vagrant", type: settings["vb"]["sync"]
  config.disksize.size = settings["vb"]["disksize"]

  if settings["vb"]["ports"]
    settings["vb"]["ports"].each do |mapping|
      ports = mapping.split(":")
      if ports.length != 2
          puts "The port syntax #{mapping} is incorrect. It should be 'xx:yy'."
          abort
      end
      config.vm.network "forwarded_port", guest: ports[1], host: ports[0]
    end
  end

  config.vm.provider "virtualbox" do |v|
    v.memory = settings["vb"]["memory"]
    v.cpus = settings["vb"]["cpus"]
  end

  # Run the init script.
  config.vm.provision "shell", path: "./provision.sh", run: "always", env: { "LANDO_VERSION" => settings["vb"]["lando_version"] }
  config.vm.post_up_message = "Ready to go! Just vagrant ssh and run lando start to get going."
  config.ssh.extra_args = ["-t", "cd /vagrant && exec bash --login"]
end
