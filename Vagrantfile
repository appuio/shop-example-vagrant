# provisioning script for setting up s2i and openshift cli
$script = <<SCRIPT
set -x
wget https://github.com/openshift/source-to-image/releases/download/v1.1.5/source-to-image-v1.1.5-4dd7721-linux-amd64.tar.gz \
  && wget https://github.com/openshift/origin/releases/download/v1.4.1/openshift-origin-client-tools-v1.4.1-3f9807a-linux-64bit.tar.gz \
  && mkdir untar \
  && tar -xzvf source-to-image-v1.1.5-4dd7721-linux-amd64.tar.gz -C untar/ \
  && tar --strip-components=1 -xzvf openshift-origin-client-tools-v1.4.1-3f9807a-linux-64bit.tar.gz -C untar/ \
  && mv untar/oc /usr/local/bin/ \
  && mv untar/s2i /usr/local/bin/ \
  && rm -rf *.tar.gz untar
wget https://packages.erlang-solutions.com/erlang-solutions_1.0_all.deb \
  && dpkg -i erlang-solutions_1.0_all.deb \
  && rm -f erlang-solutions_1.0_all.deb
apt-add-repository ppa:ansible/ansible
apt-key adv \
  --keyserver hkp://ha.pool.sks-keyservers.net:80 \
  --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
echo "deb https://apt.dockerproject.org/repo ubuntu-yakkety main" | sudo tee /etc/apt/sources.list.d/docker.list
apt-get update
apt-get install -y \
      ansible \
      apt-transport-https \
      build-essential \
      ca-certificates \
      curl \
      docker-engine=1.13.1-0~ubuntu-yakkety \
      elixir \
      esl-erlang \
      linux-image-extra-$(uname -r) \
      linux-image-extra-virtual \
      python \
      software-properties-common
usermod -aG docker ubuntu
apt-mark hold docker-engine
echo DOCKER_OPTS="--insecure-registry 172.30.0.0/16" >> /etc/default/docker
set +x
SCRIPT

Vagrant.configure("2") do |config|

  # use an ubuntu lts box as a base
  config.vm.box = "ubuntu/yakkety64"

  # specify the resources for the vm
  config.vm.provider "virtualbox" do |v|
    v.memory = 3072
    v.cpus = 2
  end

  # provision docker inside the vm
  # config.vm.provision "docker"

  # provision the script inside the vm (as defined above)
  config.vm.provision "shell", inline: $script

  # expose the folder with git projects inside the vm
  config.vm.synced_folder "C:/Users/rolan/Documents/Git/VSHN", "/opt/git"
  config.vm.synced_folder "C:/Users/rolan/Documents/Vagrant", "/opt/share"
  
  # enable a private network for the vm
  config.vm.network "private_network", type: "dhcp"

end
