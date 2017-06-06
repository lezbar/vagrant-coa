# -*- mode: ruby -*-
# vi: set ft=ruby :
# We set the last octet in IPV4 address here
nodes = {
 'controller' => [1, 200],
 'network' => [1, 201],
 'compute' => [1, 202],
 'cinder' => [1, 203],
 'swift-proxy' => [1, 204],
 'swift-01' => [1, 211],
 'swift-02' => [1, 212],
 'swift-03' => [1, 213],
 'swift-04' => [1, 214],
 'swift-05' => [1, 215],

}

Vagrant.configure("2") do |config|
  # Virtualbox
  config.vm.box = "bento/ubuntu-16.04"
  config.ssh.insert_key = false
  config.vm.box_check_update = false

  # Default is 2200..something, but port 2200 is used by forescout NAC agent.
  config.vm.usable_port_range= 2800..2900

  nodes.each do |prefix, (count, ip_start)|
    count.times do |i|
      hostname = "%s" % [prefix, (i+1)]

      config.vm.define "#{hostname}" do |box|
        box.vm.hostname = "#{hostname}.book"
        box.vm.network :private_network, ip: "172.15.0.#{ip_start+i}", :netmask => "255.255.0.0"
        box.vm.network :private_network, ip: "172.16.0.#{ip_start+i}", :netmask => "255.255.0.0"
        box.vm.network :private_network, ip: "172.17.0.#{ip_start+i}", :netmask => "255.255.255.0"

        # Otherwise using VirtualBox
        box.vm.provider :virtualbox do |vbox|
          # Defaults
          vbox.memory = 1024
          vbox.cpus = 1
          vbox.customize ["modifyvm", :id, "--nicpromisc3", "allow-all"]
          vbox.customize ["modifyvm", :id, "--nicpromisc4", "allow-all"]
          if prefix == "compute" or prefix == "controller" or prefix == "swift"
            vbox.memory = 2048
            vbox.cpus = 2
          end # if
        end # box.vm virtualbox
      end # config.vm.define
    end # count.times
  end # nodes.each
end # Vagrant.configure("2")
