# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'

current_dir    = File.dirname(File.expand_path(__FILE__))
configs        = YAML.load_file("#{current_dir}/config.yaml")
vagrant_config = configs['configs']
hosts          = ""
vagrant_config['projectnames'].each do |project|
    hosts << project << "." << vagrant_config['servername'] << " "
end

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

# Detect host OS for different folder share configuration
module OS
    def OS.windows?
        (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
    end

    def OS.mac?
        (/darwin/ =~ RUBY_PLATFORM) != nil
    end

    def OS.unix?
        !OS.windows?
    end

    def OS.linux?
        OS.unix? and not OS.mac?
    end
end

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "bento/debian-8.7"

  config.vm.provider "parallels"
  config.vm.provider "virtualbox"

  # Check available Plugins
  if OS.windows?
      if !Vagrant.has_plugin?('vagrant-winnfsd')
          puts "The vagrant-winnfsd plugin is required. Please install it with \"vagrant plugin install vagrant-winnfsd\""
          exit
      end
  end

  if Vagrant.has_plugin?('vagrant-vbguest')
      config.vbguest.auto_update = true
  end

  if !Vagrant.has_plugin?('vagrant-hostmanager')
     puts "The vagrant-hostmanager plugin is required. Please install it with \"vagrant plugin install vagrant-hostmanager\""
     exit
  end

  config.vm.provider "virtualbox" do |vb|
      vb.gui = true
      vb.customize ['modifyvm', :id, '--memory', 6096]
      vb.customize ["modifyvm", :id, "--cpus", 2]
      vb.customize ["modifyvm", :id, "--vram", 50]
      vb.customize ["modifyvm", :id, "--accelerate2dvideo", "on"]
      vb.customize ["modifyvm", :id, "--accelerate3d", "on"]
      vb.customize ["modifyvm", :id, "--name", vagrant_config['vmname']]
  end

  config.vm.provider "parallels" do |v|
      v.memory = 2048
      v.cpus = 2
  end

  # Configure the VM
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true
  config.hostmanager.aliases = hosts
  config.vm.hostname = vagrant_config['servername']
  config.vm.network :private_network, ip: vagrant_config['private_ip']

  # Configure shared folder
  if OS.windows?
    config.vm.synced_folder ".", "/vagrant", type: "nfs"
  else
    config.vm.synced_folder ".", "/vagrant", :owner => "vagrant", :group => "vagrant"
  end

  # Run the provisioning
  ## Install Ansible
  config.vm.provision "shell", path: "ansible/tools/install_ansible_in_Vagrantbox.sh"

  ## Install and configure software
  config.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "ansible/playbook.yml"
      ansible.sudo = true
      ansible.verbose = ""
      ansible.extra_vars = {
        servername: vagrant_config['servername'],
        projectname: 0,
        testing_mode: 0
      }
  end
  
  ## Configure Symfony projects
  vagrant_config['projectnames'].each do |project|
      config.vm.provision "ansible_local" do |ansible|
          ansible.playbook = "ansible/playbook.yml"
          ansible.start_at_task = "Download Symfony Installer"
          ansible.sudo = true
          ansible.verbose = ""
          ansible.extra_vars = {
              servername: vagrant_config['servername'],
              projectname: project,
              testing_mode: 0
          }
      end
  end

end
