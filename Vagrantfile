# provisioning script for setting up s2i and openshift cli
$script = <<SCRIPT
wget https://github.com/openshift/source-to-image/releases/download/v1.1.4/source-to-image-1.1.4-870b273-linux-amd64.tar.gz
wget https://github.com/openshift/origin/releases/download/v1.4.1/openshift-origin-client-tools-v1.4.1-3f9807a-linux-64bit.tar.gz
mkdir untar
tar -xzvf source-to-image-1.1.4-870b273-linux-amd64.tar.gz -C untar/
tar -xzvf openshift-origin-client-tools-v1.4.1-3f9807a-linux-64bit.tar.gz -C untar/
mv untar/openshift-origin-client-tools-v1.4.1+3f9807a-linux-64bit/oc /usr/local/bin/
mv untar/s2i /usr/local/bin/
rm -rf *.tar.gz untar
SCRIPT

Vagrant.configure("2") do |config|

  # use an ubuntu lts box as a base
  config.vm.box = "ubuntu/xenial64"

  # specify the resources for the vm
  config.vm.provider "virtualbox" do |v|
    v.memory = 2560
    v.cpus = 2
    
    # fix for slow docker builds: don't use the intel NAT
    v.customize ["modifyvm", :id, "--nictype1", "virtio"]
  end

  # provision docker inside the vm
  config.vm.provision "docker"

  # provision the script inside the vm (as defined above)
  config.vm.provision "shell", inline: $script

  # expose the folder with git projects inside the vm
  config.vm.synced_folder "C:/Users/rolan/Documents/Git/VSHN", "/opt/git"

end
