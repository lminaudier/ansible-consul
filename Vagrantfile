# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/trusty64"
  authorize_key_for_root config, '~/.ssh/id_rsa.pub'

  [
    {:short_host => "node1", :ip => "10.0.100.2"},
    {:short_host => "node2", :ip => "10.0.100.3"}
  ].each do |node|
    config.vm.define node[:short_host] do |machine|
      machine.vm.hostname = "#{node[:short_host]}.consulcluster.dev"
      machine.vm.network "private_network", ip: node[:ip]
    end
  end
end

def authorize_key_for_root(config, key_path)
  if key_path.nil?
    fail "Public key not found at following paths: #{key_paths.join(', ')}"
  end

  full_key_path = File.expand_path(key_path)

  if File.exists?(full_key_path)
    config.vm.provision 'file',
      run: 'once',
      source: full_key_path,
      destination: '/home/vagrant/root_pubkey'

    config.vm.provision 'shell',
      privileged: true,
      run: 'once',
      inline:
      "echo \"Creating /root/.ssh/authorized_keys with #{key_path}\" && " +
      'rm -f /root/.ssh/authorized_keys && ' +
      'mv /home/vagrant/root_pubkey /root/.ssh/authorized_keys && ' +
      'chown root:root /root/.ssh/authorized_keys && ' +
      'chmod 600 /root/.ssh/authorized_keys && ' +
      'rm -f /home/vagrant/root_pubkey && ' +
      'echo "Done!"'
  end
end
