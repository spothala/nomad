# -*- mode: ruby -*-
# vi: set ft=ruby :

$script = <<SCRIPT
# Update apt and get dependencies
sudo apt-get update
sudo apt-get install -y unzip curl wget vim

# Install Docker
sudo curl -sSL https://get.docker.com/ | sh

# Download Nomad
echo Fetching Nomad...
cd /tmp/
wget https://dl.bintray.com/mitchellh/nomad/nomad_0.1.0_linux_amd64.zip -O nomad.zip

echo Installing Nomad...
unzip nomad.zip
sudo chmod +x nomad
sudo mv nomad /usr/bin/nomad

sudo mkdir /etc/nomad.d
sudo chmod a+w /etc/nomad.d

SCRIPT

Vagrant.configure(2) do |config|
  config.vm.box = "puphpet/ubuntu1404-x64"
  config.vm.hostname = "nomad"
  config.vm.provision "shell", inline: $script, privileged: false
  config.vm.network :forwarded_port, guest: 8080, host: 4567
  config.vm.network :forwarded_port, guest: 80, host: 1234

  # Increase memory for Virtualbox
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "10240"
  end

  # Increase memory for VMware
  ["vmware_fusion", "vmware_workstation"].each do |p|
    config.vm.provider p do |v|
      v.vmx["memsize"] = "1024"
    end
  end
end
