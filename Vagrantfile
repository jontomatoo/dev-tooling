$shellProvisionScript = <<SCRIPT
    yum install -y epel-release
    yum install -y python-pip
    pip install docker
    curl -fsSL get.docker.com -o get-docker.sh && sh get-docker.sh && rm get-docker.sh
SCRIPT

Vagrant.configure("2") do |config|

    config.vm.box = "centos/7"

    config.vm.provider "virtualbox" do |v|
        v.memory = 4096
    end

    config.vm.synced_folder "ansible/", "/ansible"

    # Jenkins
    config.vm.network "forwarded_port", guest: 8080, host: 8080

    # SonarQube
    config.vm.network "forwarded_port", guest: 9000, host: 9000
    config.vm.network "forwarded_port", guest: 9092, host: 9092

    # Artifactory
    config.vm.network "forwarded_port", guest: 8081, host: 8081

    # Basic bootstrapping
    config.vm.provision "shell", inline: $shellProvisionScript

    config.vm.provision "ansible_local" do |ansible|
        ansible.playbook = "/ansible/site.yml"
        ansible.verbose  = true
    end
end