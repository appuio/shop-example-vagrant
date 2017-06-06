# PARAM: path where the shop-example repository is located
$gitFolder = "C:/Users/rolan/Documents/Git/VSHN"

# PARAM: additional shares
# $shareFolder = "C:/Users/rolan/Documents/Vagrant"

# provisioning script for setting up s2i and openshift cli
$script = <<SCRIPT
set -x

# PARAM: S2I
export stiVersion="v1.1.5"
export stiRelease="source-to-image-v1.1.5-4dd7721-linux-amd64.tar.gz"

# PARAM: OC cli
export ocVersion="v1.4.1"
export ocRelease="openshift-origin-client-tools-v1.4.1-3f9807a-linux-64bit.tar.gz"

# PARAM: docker
export dockerVersion="1.13.1-0~ubuntu-yakkety"
export composeVersion="1.11.2"
export insecureRegistries='{ "insecure-registries": [ "172.30.0.0/16", "172.17.0.0/16", "172.28.128.3/24"] }'

wget https://github.com/openshift/source-to-image/releases/download/${stiVersion}/${stiRelease} \
  && wget https://github.com/openshift/origin/releases/download/${ocVersion}/${ocRelease} \
  && mkdir untar \
  && tar -xzvf ${stiRelease} -C untar/ \
  && tar --strip-components=1 -xzvf ${ocRelease} -C untar/ \
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
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash - \
  && curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add - \
  && echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
apt-get update
apt-get install -y \
  ansible \
  apt-transport-https \
  build-essential \
  ca-certificates \
  curl \
  docker-engine=${dockerVersion} \
  elixir \
  esl-erlang \
  linux-image-extra-$(uname -r) \
  linux-image-extra-virtual \
  nodejs \
  python \
  software-properties-common \
  yarn
apt-mark hold docker-engine \
  && usermod -aG docker ubuntu \
  && echo $insecureRegistries >> /etc/docker/daemon.json \
  && systemctl enable docker \
  && curl -L "https://github.com/docker/compose/releases/download/${composeVersion}/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose \
  && chmod +x /usr/local/bin/docker-compose
ln -s /opt/git /home/ubuntu/git
ln -s /opt/share /home/ubuntu/share
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

  # provision the script inside the vm (as defined above)
  config.vm.provision "shell", inline: $script

  # expose the folder with git projects inside the vm
  config.vm.synced_folder $gitFolder, "/opt/git"
  config.vm.synced_folder $shareFolder, "/opt/share"
  
  # enable a private network for the vm
  config.vm.network "private_network", type: "dhcp"

end
