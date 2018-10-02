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

    # Provision the VM with ansible
    config.vm.provision "ansible" do |ansible|
        ansible.playbook = "ansible/playbook.yml"
    end
end
