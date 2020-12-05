Vagrant.configure("2") do |config|
  config.vm.box = "generic/debian10"

  config.vm.network "private_network", ip: "192.168.99.100"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "4096"
    vb.cpus = 4
    vb.name = "k3d"
  end

  config.vm.provision "shell", inline: <<-SHELL
    # init vm dependencies
    apt-get update
    apt-get install -y apt-transport-https ca-certificates curl gnupg2 software-properties-common
    
    # install ansible
    echo "deb http://ppa.launchpad.net/ansible/ansible/ubuntu trusty main" | tee /etc/apt/sources.list.d/ansible-ubuntu.list
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 93C4A3FD7BB9C367
    apt update
    apt install -y ansible

    # install docker
    curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add -
    add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"
    apt update
    apt install -y docker-ce docker-ce-cli containerd.io
    usermod -aG docker vagrant

    # install k3d
    curl -s https://raw.githubusercontent.com/rancher/k3d/main/install.sh | bash
    
    # install kubectl
    curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
    chmod +x ./kubectl
    mv ./kubectl /usr/local/bin/kubectl

    # install helm
    curl https://baltocdn.com/helm/signing.asc | sudo apt-key add -
    echo "deb https://baltocdn.com/helm/stable/debian/ all main" | tee /etc/apt/sources.list.d/helm-stable-debian.list
    apt-get update
    apt-get install -y helm

    # install k9s
    curl -LO https://github.com/derailed/k9s/releases/download/v0.24.1/k9s_Linux_x86_64.tar.gz
    tar -zxvf k9s_Linux_x86_64.tar.gz
    chmod +x ./k9s
    mv ./k9s /usr/local/bin/k9s
    rm k9s_Linux_x86_64.tar.gz LICENSE README.md

    # install kubectx
    apt install kubectx

    # install skaffold
    curl -Lo skaffold https://storage.googleapis.com/skaffold/releases/latest/skaffold-linux-amd64 && \
	  install skaffold /usr/local/bin/ && \
    rm skaffold

    # install Kustomize
    curl -s "https://raw.githubusercontent.com/kubernetes-sigs/kustomize/master/hack/install_kustomize.sh"  | bash
    chmod +x ./kustomize
    mv ./kustomize /usr/local/bin/kustomize

    # install devspace
    curl -s -L "https://github.com/devspace-cloud/devspace/releases/download/v5.4.1/devspace-linux-amd64" -o devspace
    install devspace /usr/local/bin
    rm devspace
  SHELL

  # vagrant ansible provisioner seems not working
  #config.vm.provision "shell", inline: <<-SHELL
  #  export ANSIBLE_HOST_KEY_CHECKING=False
  #  ssh-keygen -f ~/.ssh/id_rsa -t rsa -N ''
  #  cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
  #  ansible-playbook -i /vagrant/inventory/single /vagrant/images.yml
  #  ansible-playbook -i /vagrant/inventory/single /vagrant/site.yml
  #SHELL
end
