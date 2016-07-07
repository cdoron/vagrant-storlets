# -*- mode: ruby -*-

VAGRANTFILE_API_VERSION = "2"

LIBVIRT_POOL = "default"

# <http://docs.openstack.org/developer/devstack/guides/single-vm.html#virtual-machine>
# DevStack should run in any virtual machine running a supported Linux
# release. It will perform best with 4GB or more of RAM.

# TODO: check if there is some reproducibile provision ordering
# https://www.vagrantup.com/docs/multi-machine/

LIBVIRT_MEMORY_SMALL = 1024  # MB
LIBVIRT_MEMORY = 2048  # MB
LIBVIRT_MEMORY_BIG = 3096  # MB
LIBVIRT_MEMORY_HUGE = 4096  # MB

LIBVIRT_CPUS = 1
LIBVIRT_CPUS_BIG = 2
LIBVIRT_CPUS_HUGE = 4

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "ubuntu/trusty64"
  config.vm.box_check_update = false

  # See: <https://github.com/fgrehm/vagrant-cachier>
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
    config.cache.synced_folder_opts = {
      type: :nfs,
      mount_options: ['rw', 'vers=3', 'tcp', 'nolock']
    }
  end

  config.vm.provider "libvirt" do |dm|
    dm.storage_pool_name = LIBVIRT_POOL
    dm.memory = LIBVIRT_MEMORY
    dm.cpus = LIBVIRT_CPUS
  end

  config.vm.provider "virtualbox" do |vb|
    vb.memory = LIBVIRT_MEMORY
    vb.cpus = LIBVIRT_CPUS
  end

  config.vm.define "storlets" do |vmc|
    vmc.vm.hostname = "storlets"
    vmc.vm.network "private_network", ip: "192.168.52.101"
    vmc.vm.provision "shell" do |s|
      s.path = "provision/provision_root"
      s.args = ["storlets"]
    end
    vmc.vm.provider :libvirt do |domain|
      domain.memory = LIBVIRT_MEMORY_HUGE
      domain.cpus = LIBVIRT_CPUS_BIG
    end
    vmc.vm.provider :virtualbox do |vb|
      vb.memory = LIBVIRT_MEMORY
      vb.cpus = LIBVIRT_CPUS
    end

  end

  config.vm.define "devstackstorlets" do |vmc|
    vmc.vm.hostname = "devstackstorlets"
    vmc.vm.network "private_network", ip: "192.168.52.102"
    vmc.vm.provision "shell" do |s|
      s.path = "provision/provision_root_devstackstorlets"
      s.args = ["devstackstorlets"]
    end
    vmc.vm.provider :libvirt do |domain|
      domain.memory = LIBVIRT_MEMORY_HUGE
      domain.cpus = LIBVIRT_CPUS_BIG
    end
    vmc.vm.provider :virtualbox do |vb|
      vb.memory = LIBVIRT_MEMORY
      vb.cpus = LIBVIRT_CPUS
    end
  end

end
