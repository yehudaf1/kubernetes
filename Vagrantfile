# -*- mode: ruby -*-
# vi: set ft=ruby :
# Where to store the disk file


disk = '.\k8s-master_u01_disk.vdi'



VAGRANTFILE_API_VERSION = "2"

cluster = {
   "k8s-master.local.box" => { :ip => "192.168.56.110", :cpus => 2, :mem => 2048 , :boot_size=>100}
  #,"worker-node1.local.box" => { :ip => "192.168.56.111", :cpus => 2, :mem => 2048 , :boot_size=>100}
  #,"worker-node2.local.box" => { :ip => "192.168.56.112", :cpus => 2, :mem => 2048 , :boot_size=>100}
  #,"mysql8n4.local.box" => { :ip => "192.168.56.91", :cpus => 2, :mem => 1024 , :boot_size=>100}
}
 
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  cluster.each_with_index do |(hostname, info), index|

    config.vm.define hostname do |cfg|
      cfg.vm.provider :virtualbox do |vb, override|
        #config.vm.box = "ubuntu/trusty64"
		#config.vm.box = "generic/rhel7"
		config.vm.box = "bento/centos-7.5"

		override.vm.network :private_network, ip: "#{info[:ip]}"
        override.vm.hostname = hostname
        vb.name = hostname
        vb.customize ["modifyvm", :id, "--memory", info[:mem], "--cpus", info[:cpus], "--hwvirtex", "on" ]
		disk =  './'+ hostname +'_mysql8_u01_disk.vdi'
		

	    unless File.exist?(disk)
			vb.customize ['createhd', '--filename', disk, '--size', 1 * 2048 ]
		end
		vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', disk]


		# add DVD 
#		vb.customize ["storageattach", :id, 
#                "--storagectl", "SATA Controller", 
#                "--port", "2", "--device", "20", 
#                "--type", "dvddrive", 
#                "--medium", "emptydrive"]

		#vb.customize [ modifyhd ,0 , --resize , 100000 ]
      end # end provider
    end # end config

  end # end cluster
end


#    vb.customize ["modifyhd", "<disk id>", "--resize", "<size in megabytes>"]
