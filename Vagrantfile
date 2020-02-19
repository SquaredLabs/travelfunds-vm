require "yaml"
require "pp"

Vagrant.require_version ">= 2.0"

Vagrant.configure("2") do |config|

    # Load the configuration from the ansible vars file
    # this allows us to share certain configuration needed by both vagrant and ansible (e.g., IP and hostname)
    _conf = YAML.load(
        File.open(
            File.join(File.dirname(__FILE__), "ansible/vars/all.yml"),
            File::RDONLY
        ).read
    )["vagrant_local"]["vm"]
    puts "Using this configuration (edit ansible/vars/all.yaml to modify):"
    pp _conf
    _hostname = _conf["hostname"]
    _ip = _conf["ip"]

    # Configure the VM setttings
    config.vm.box = _conf["base_box"]
    config.vm.network :private_network, ip: _ip
    config.ssh.forward_agent = true
    config.vm.provider :virtualbox do |v|
        v.customize [
            "modifyvm", :id,
            "--name", _hostname,
            "--memory", _conf["memory"],
            "--natdnshostresolver1", "on",
            "--cpus", 1,
        ]
    end

    # Provision the VM with ansible. Using the ansible_local provisioner means we don't
    # need to install ansible on the host OS which is quite convenient.
    config.vm.provision "ansible_local" do |ansible|
        ansible.playbook = "/vagrant/ansible/playbook.yml"
        ansible.provisioning_path  = "/vagrant/ansible"
        ansible.galaxy_role_file = "/vagrant/ansible/requirements.yml"
        ansible.galaxy_roles_path = "/home/vagrant/ansible/roles"
    end
end
