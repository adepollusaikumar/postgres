$script = <<-'SCRIPT'
echo "Installing pre-requisites(takes few minutes ...)"
sudo bash -c 'apt-get update  >/dev/null 2>&1 ' 
sudo bash -c 'apt-get install python3 python3-distutils curl postgresql-client -y >/dev/null 2>&1 '
sudo bash -c 'curl -s https://bootstrap.pypa.io/get-pip.py -o get-pip.py >/dev/null 2>&1 '
sudo bash -c 'python3 get-pip.py >/dev/null 2>&1 '
echo "Installing ansible"
sudo bash -c 'python3 -m pip install ansible >/dev/null 2>&1 '
mkdir -p  /root/.ssh/ 
sudo bash -c "sed -i 's/deprecation_warnings = True/deprecation_warnings = False/g' /etc/ansible/ansible.cfg >/dev/null 2>&1 "
echo "Running playbook ...."
ansible-playbook   /vagrant/playbook/site.yml 2>/dev/null 
SCRIPT

Vagrant.configure("2") do |config|
       config.vm.box = "bento/ubuntu-20.04"
       config.ssh.insert_key = false
       config.hostmanager.enabled = true
       config.vm.synced_folder ".", "/vagrant", type: "virtualbox"
       config.vm.provider "virtualbox" do |vb|
     vb.customize ["modifyvm", :id, "--memory", 4096]
     vb.customize ["modifyvm", :id, "--cpus", 4]
 end

  (  ["POSTGRESHACLUSTER"] ).each_with_index do |role, i|
    name = "%s"     % [role, i]
    addr = "10.0.11.%d" % (100 + i)
    config.vm.define name do |node|
      node.vm.hostname = name
      node.vm.network "private_network",ip: addr
      node.vm.provision "shell", inline: $script
        end
    end

end
