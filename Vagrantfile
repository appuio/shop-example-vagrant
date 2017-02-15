# -*- mode: ruby -*-
# vi: set ft=ruby :

$script = <<SCRIPT
wget https://github.com/openshift/source-to-image/releases/download/v1.1.4/source-to-image-1.1.4-870b273-linux-amd64.tar.gz
wget https://github.com/openshift/origin/releases/download/v1.4.1/openshift-origin-client-tools-v1.4.1-3f9807a-linux-64bit.tar.gz
tar -xvf source-to-image-1.1.4-870b273-linux-amd64.tar.gz
tar -xvf openshift-origin-client-tools-v1.4.1-3f9807a-linux-64bit.tar.gz
mv openshift-origin-client-tools-v1.4.1+3f9807a-linux-64bit/oc /usr/local/bin/
mv s2i /usr/local/bin/
rm -rf *.tar.gz openshift-origin-client-tools-v1.4.1+3f9807a-linux-64bit sti
SCRIPT

Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/xenial64"

  config.vm.provision "docker"

  config.vm.provision "shell", inline: $script

  config.vm.synced_folder "C:/Users/rolan/Documents/Git/VSHN", "/opt/git"

end
