# ####################################################################
# ################### CONFIGURATION VARIABLES ########################
# ####################################################################
IMAGE_NAME = "bento/ubuntu-22.04"   # Image to use
MEM = 2048                          # Amount of RAM
CPU = 1                             # Number of processors
SLAVE_NBR = 2                       # Number of slaves node


Vagrant.configure("2") do |config|
    # RAM and CPU config
    config.vm.provider "virtualbox" do |v|
        v.memory = MEM
        v.cpus = CPU
    end

    # Slave node config
    (1..SLAVE_NBR).each do |i|
        config.ssh.insert_key = false
        config.vm.define "slave-#{i}" do |slave|
            # OS and Hostname
            slave.vm.box = IMAGE_NAME
            slave.vm.hostname = "slave-#{i}"
            slave.vm.network "private_network", ip: "192.168.56.14#{i}"            
        end
    end
end