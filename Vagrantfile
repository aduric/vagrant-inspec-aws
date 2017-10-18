# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/zesty64"

  config.vm.provider "virtualbox" do |vb|
    vb.name = "inspec-aws"
  end

  config.vm.provision "file", source: "etc/aws-vars", destination: "/tmp/aws-vars" 

  config.vm.synced_folder "../inspec-aws", "/inspec-aws"
  config.vm.synced_folder "../inspec", "/inspec"

  config.vm.provision "shell", inline: <<-SHELL
    chmod 400 /home/ubuntu/.ssh/id_rsa

    apt-get update
    apt-get install -y git unzip zlib1g-dev bundler python3.6 python3-pip

    pushd /tmp
      wget -q https://packages.chef.io/files/stable/chefdk/1.1.16/ubuntu/14.04/chefdk_1.1.16-1_amd64.deb
      dpkg -i chefdk_1.1.16-1_amd64.deb 
      rm chefdk_1.1.16-1_amd64.deb

      wget -q https://releases.hashicorp.com/terraform/0.10.7/terraform_0.10.7_linux_amd64.zip
      unzip terraform_0.10.7_linux_amd64.zip -d /usr/bin
      rm terraform_0.10.7_linux_amd64.zip

      cat aws-vars >> /etc/environment
      rm aws-vars
    popd

    sed -ie 's#"$#:/opt/chefdk/bin"#' /etc/environment
    sed -ie 's#PATH="#PATH="/home/ubuntu/.local/bin:#' /etc/environment
  SHELL
end
