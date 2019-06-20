# Boilerplate VM development environment setup

Creates a local development VM for travelfunds.core.uconn.edu

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
- run `ansible-galaxy install -r ansible/requirements.yml`
- run `vagrant up` to build your VM
- run `vagrant ssh` to SSH into your running VM

The root directory of this project will be mounted in the VM at `/vagrant`. I recommend using the `workspace` directory (`/vagrant/workspace` on the VM) to hold your projects.

### Other Vagrant Commands
- To stop your VM: `vagrant halt`
- To restart your VM: `vagrant reload`
- To run ansible again (i.e., after making changes to the playbook): `vagrant provision`

### Setting up the project

* SSH into your VM: `vagrant ssh`
* Move to the shared workspace directory: `cd /vagrant/workspace`
* Clone the project: `git clone git@github.com:SquaredLabs/travelfunds.core.uconn.edu.git travelfunds`
* Move into your project directory: `cd travelfunds`
* From there, follow installation instructions in the [travelfunds readme](https://github.com/SquaredLabs/travelfunds.core.uconn.edu/blob/develop/README.md)

Since the `workspace` directory is shared with the VM, you can edit the files within `workspace` on your host machine and the changes will automatically be reflected in the VM. There is no need to rebuild or restart the VM after making changes to your application code.

### Note on webpack-dev-server

If you are using a local dev server (such as webpack-dev-server) you need to specify the host as `0.0.0.0` (due to Vagrant's networking setup, `localhost` will not work). For example:

```
webpack-dev-server --host 0.0.0.0 --mode=development
```

### Mailhog

This VM include an installation of [mailhog](https://github.com/mailhog/MailHog) for capturing emails sent by the application during development. You can access the mailhog HTTP server at http://travelfunds.core.vm:8025 to view the emails.

Note that you should have the following as your SMTP configuration in `.env.local`:
```
SMTP_HOST=localhost
SMTP_PORT=1025
```
The other `SMTP_*` variables can be left at their defaults.

## Customization
Configuration can be changed in `ansible/vars/all.yml`.

VM settings (hostname, IP, memory) are set in the `vagrant_local` key:
```yaml
vagrant_local:
    vm:
      base_box: 'ubuntu/bionic64'
      hostname: travelfunds.core.vm
      ip: 192.168.80.13
      memory: 4096
```

## Extending
Vagrant is used to create and configure your development VM. It then uses Ansible to handle provisioning of your development environment.

In order to extend this boilerplate to create a suitable development environment for your project, you will need to extend the ansible playbook provided in the `ansible` directory.

This will require a basic understanding of how to use Ansible. Although using this boilerplate may be a useful tool to use while learning Ansible, teaching how to use Ansible is outside the scope of this document. There is an abundance of learning materials made freely available by the community. A good place to start is [the official docs](https://docs.ansible.com/ansible/latest/user_guide/index.html) and [the getting started page](https://www.ansible.com/resources/get-started).

If you're willing to pay $10, I recommend [this book](https://www.ansiblefordevops.com). I found it very useful.
