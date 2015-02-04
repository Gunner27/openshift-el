# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'json'

def generate_ansible_groups(num_minions)
  minions = Array.new
  1.upto(num_minions) do |n|
    minions.push("minion-#{n}")
  end

  groups = { 
    "minions" => minions,
    "masters" => ["master"],

    "all_groups:children" => ["masters", "minions"]
  }

  return groups
end

# Require a recent version of vagrant otherwise some have reported errors setting host names on boxes
Vagrant.require_version ">= 1.6.2"

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

  # attempt to read config in this repo's .vagrant-openshift.json if present
  if File.exist?('.vagrant-openshift.json')
    json = File.read('.vagrant-openshift.json')
    begin
      vagrant_openshift_config = JSON.parse(json)
    rescue JSON::ParserError => e
      raise VFLoadError.new "Error parsing .vagrant-openshift.json:\n#{e}"
    end
  else
    # this is only used as default when .vagrant-openshift.json does not exist
    vagrant_openshift_config = {
      "dev_cluster" => true,
      "num_minions" => ENV['OPENSHIFT_NUM_MINIONS'] || 2,
      "libvirt" => {
        "box_name" => "rhel-7.0",
        "box_url" => "http://file.rdu.redhat.com/~jshubin/vagrant/rhel-7.0/rhel-7.0.box"
      },
    }
  end

  # write back the config
  File.open('.vagrant-openshift.json','w') do |f|
    f.write(JSON.pretty_generate(vagrant_openshift_config))
  end

# The number of minions to provision.
$num_minion = (vagrant_openshift_config['num_minions'] || ENV['OPENSHIFT_NUM_MINIONS'] || 2).to_i

$nodes = [];
$ansible_groups = {
  "masters" => ["master"],
  "minions" => $nodes
}
$num_minion.times do |i|
  $nodes.push("minion-#{i+1}")
end

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.provider :libvirt do |libvirt|
    libvirt.memory = 1024
  end 
 
  # and num_minions minions, or nodes...
  dev_cluster = vagrant_openshift_config['dev_cluster'] || ENV['OPENSHIFT_DEV_CLUSTER']
  if dev_cluster
    # Start an OpenShift cluster
    # Currently this only works with the (default) VirtualBox provider.

    # OpenShift minion
    $num_minion.times do |n|
      config.vm.define "minion-#{n+1}" do |minion|
        minion_index = n+1

        minion.vm.box = "rhel-7.0" 
        minion.vm.box_check_update = false
        
        minion.vm.hostname = "minion-#{minion_index}.goern.example.com"

        minion.vm.synced_folder ".", "/vagrant", type: "rsync", rsync_exclude: ".git/"

        # this is required to subscribe RHEL7
        minion.registration.subscriber_username = ENV['SUB_USERNAME']
        minion.registration.subscriber_password = ENV['SUB_PASSWORD']

      end # minion
    end # do
  end # if dev_cluster

  # setup a OpenShift master
  config.vm.define "master" do |master|
    master.vm.box = "rhel-7.0"
    master.vm.box_check_update = false

    master.vm.hostname = 'master.goern.example.com'

    # this is required to subscribe RHEL7
    master.registration.subscriber_username = ENV['SUB_USERNAME']
    master.registration.subscriber_password = ENV['SUB_PASSWORD']

    master.vm.synced_folder ".", "/vagrant", type: "rsync", rsync_exclude: ".git/"

    # provision all machines using ansible
    config.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning/playbook.yml"

      ansible.groups = $ansible_groups # generate_ansible_groups(vagrant_openshift_config['num_minions'])
      ansible.limit = :all

#      ansible.verbose = 'vv'
    end # ansible
 
  end # master



end # config
