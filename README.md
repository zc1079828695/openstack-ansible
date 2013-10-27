# OpenStack on Ansible with Vagrant

This repository contains script that will deploy OpenStack into Vagrant virtual
machines. These scripts are based on the [OpenStack Install and Deploy
Manual](http://docs.openstack.org/grizzly/openstack-compute/install/apt/content/),
grizzly release.

See also [Vagrant, Ansible and OpenStack on your laptop](http://www.slideshare.net/lorinh/vagrant-ansible-and-openstack-on-your-laptop)
on SlideShare.

## Install prereqs

You'll need to install:

 * [Vagrant](http://vagrantup.com)
 * [Ansible](http://ansible.github.com)
 * [python-novaclient](http://pypi.python.org/pypi/python-novaclient/)
    so you can control your instances with the `nova` command-line tool.
 * [python-netaddr](https://pypi.python.org/pypi/netaddr/)
    this is used to match ip addresses and interfaces to networks.

The simplest way to get started with Ansible is to install the prerequisites,
grab the git repo and source the appropriate file to set your environment
variables, no other installation is required:

	sudo pip install paramiko PyYAML Jinja2
	git clone git://github.com/ansible/ansible.git
	cd ./ansible
	source ./hacking/env-setup

## Get an Ubuntu 12.04 (precise) Vagrant box

Download a 64-bit Ubuntu Vagrant box:

	vagrant box add precise64 http://files.vagrantup.com/precise64.box

## Grab this repository

This repository uses a submodule that contains some custom Ansible modules for
OpenStack, so there's an extra command required after cloning the repo:

    git clone http://github.com/lorin/openstack-ansible
    cd openstack-ansible
    git submodule update --init

## Bring up the cloud

    make standard

This will boot three VMs (controller, network, and a compute node), install
OpenStack, and attempt to boot a test VM inside of OpenStack.

If everything works, you should be able to ssh to the instance from the 
network host:

 * username: `cirros`
 * password: `cubswin:)`

Note: You may get a "connection refused" when attempting to ssh to the instance.
It can take several minutes for the ssh server to respond to requests, even
though the cirros instance has booted and is pingable.

## Vagrant hosts

The hosts for the standard configuration are:

 * 10.0.0.10 (our cloud controller)
 * 10.0.0.9 (the quantum network host)
 * 10.0.0.11 (compute host #1)

You should be able to ssh to these VMs (username: `vagrant`, password:
`vagrant`). You can also authenticate  with the vagrant private key, which is
included here as the file `vagrant_private_key` (NOTE: git does not manage file
permissions, these must be set to using "chmod 0600 vagrant_private_key" or ssh
and ansible will fail with an error).

## Interacting with your cloud

You can interact with your cloud directly from your desktop, assuming that you
have the [python-novaclient](http://pypi.python.org/pypi/python-novaclient/)
installed.

Note that the openrc file will be created on the controller by default.
