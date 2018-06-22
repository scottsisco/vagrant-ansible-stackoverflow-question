# This guide is optimized for Vagrant 1.7 and above.
# Although versions 1.6.x should behave very similarly, it is recommended
# to upgrade instead of disabling the requirement below.
Vagrant.require_version ">= 1.7.0"

Vagrant.configure(2) do |config|
  N = 8
        (1..N).each do |hpc_id|
          config.vm.define "hpc#{hpc_id}" do |hpc|
            hpc.vm.hostname = "hpc#{hpc_id}"
            hpc.vm.network "private_network", ip: "192.168.30.#{200+hpc_id}"
            hpc.vm.box = "ubuntu/xenial64"
            hpc.ssh.insert_key = false

            hpc.vm.provider 'virtualbox' do |vb|
              vb.name = "hpc#{hpc_id}"
              vb.memory = 512
              vb.cpus = 1
              vb.gui = false

          if hpc_id == N
            hpc.vm.provision :ansible do |ansible|
              # Disable default limit to connect to all the hpcs
              ansible.limit = "all"
              ansible.playbook = "site.yml"
              ansible.become_user = "root"
              ansible.groups = {
                 "Slurm_Primary_Controller" => ["hpc1"],
                 "Slurm_Backup_Controller" => ["hpc2"],
                 "Slurm_Primary_Database" => ["hpc3"],
                 "Slurm_Backup_Database" => ["hpc4"],
                 "My_SQL_Database" => ["hpc5"],
                 "Slurm_Worker" => ["hpc6", "hpc7", "hpc8"],
              }
           end
         end
       end
     end
   end
end
