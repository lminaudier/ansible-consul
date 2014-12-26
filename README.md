# Consul

## Setup

### Prerequisites

You'll need virtualbox, vagrant and ansible setup on you local to use this project

On MacOS you can install them via Homebrew and Homebrew Cask

    brew cask install virtualbox vagrant

and

    brew install ansible

Then we will need some vagrant plugin and setup on your local machine to mimic
cloud providers.

    vagrant plugin install vagrant-hostsupdater

Will add/remove a host in `/etc/hosts` each time you boot up or destroy a VM.

This will ask for your `sudo` password. **DON'T PANIC**.

Add the following configuration into `~/.ssh/config`

    Host 10.0.100.* *.consulcluster.dev
      StrictHostKeyChecking no
      UserKnownHostsFile=/dev/null
      User root
      LogLevel ERROR

We will use `.consulcluster.dev` to access our VMs. VMs will get an ip in
`[10.0.100.0,10.0.100.255]` range

### Launch

Simply run

    vagrant up

This will boot up some VMs in Virtualbox and upload your public SSH key to it (like
it would happen on Digital Ocean for instance)

### Provision

Use ansible to provision your freshly booted VMs

    ansible-playbook -i provisioning/inventory provisioning/playbook.yml

# Resources

- [6 practices for super smooth Ansible experience](http://hakunin.com/six-ansible-practices#build-a-convenient-local-playground)
