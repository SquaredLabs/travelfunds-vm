# Boilerplate VM development environment setup

Boilerplate to create a local development environment using Vagrant + Ansible.

* [Ansible](http://docs.ansible.com/ansible/index.html)
* [Vagrant](https://www.vagrantup.com)

## Requirements
* [Python >= 2.6](https://www.python.org)
* [Ansible](http://docs.ansible.com/ansible/intro_installation.html#installation)
    - [Ubuntu Instructions](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#latest-releases-via-apt-ubuntu)
    - [Mac Instructions](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#latest-releases-on-mac-osx)
* [virtualbox](https://www.virtualbox.org/wiki/Downloads)
    - Ubuntu: `sudo apt install virtualbox`
    - Mac: `brew cask install virtualbox`
* [Vagrant](https://www.vagrantup.com/downloads.html) >= 2.0
    - system package managers often provide outdated versions of vagrant, so downloading from their site is recommended for best results

## Getting Started
- Make sure you have all the requirements listed above.
- Clone this repository
- `cd` to the root directory of this repository
- run `vagrant up` to build your VM
- run `vagrant ssh` to SSH into your running VM

This will give you a basic VM (Ubuntu 16.04 by default) with a few useful packages installed.

The root directory of this project will be mounted in the VM at `/vagrant`. I recommend using the `workspace` directory (`/vagrant/workspace` on the VM) to hold your projects.

This boilerplate is meant to be extended to provide a development environment suitable for your project. See [Extending](#extending) below.

### Other Vagrant Commands
- To stop your VM: `vagrant halt`
- To restart your VM: `vagrant reload`
- To run ansible again (i.e., after making changes to the playbook): `vagrant provision`

## Basic Customization
Configuration can be changed in `ansible/vars/all.yml`.

Base server packages, timezone, and language are set in the `server` key:
```yaml
server:
    packages:
      - curl
      - wget
      - python-software-properties
      - ca-certificates
      - build-essential
      - libxml2-dev
      - unzip
      - git
      - zsh
    timezone: America/New_York
    locale: en_US.UTF-8
```

VM settings (base box, hostname, IP, memory) are set in the `vagrant_local` key:
```yaml
vagrant_local:
    vm:
      base_box: 'ubuntu/xenial64'
      hostname: dev.project.vm
      ip: 192.168.10.10
      memory: 2048
```

## Extending
Vagrant is used to create and configure your development VM. It then uses Ansible to handle provisioning of your development environment.

In order to extend this boilerplate to create a suitable development environment for your project, you will need to extend the ansible playbook provided in the `ansible` directory.

This will require a basic understanding of how to use Ansible. Although using this boilerplate may be a useful tool to use while learning Ansible, teaching how to use Ansible is outside the scope of this document. There is an abundance of learning materials made freely available by the community. A good place to start is [the official docs](https://docs.ansible.com/ansible/latest/user_guide/index.html) and [the getting started page](https://www.ansible.com/resources/get-started).

If you're willing to pay $10, I recommend [this book](https://www.ansiblefordevops.com). I found it very useful.
