LIN_BOX_IMAGE = "generic/centos7"
LIN_NODE_COUNT = 2
WIN_BOX_IMAGE = "StefanScherer/windows_2019"
WIN_NODE_COUNT = 2

$posh = <<-SCRIPT
Get-NetFirewallRule FPS-ICMP4-ERQ-In | Enable-NetFirewallRule
$url = "https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1"
$file = "$env:temp\\ConfigureRemotingForAnsible.ps1"
(New-Object -TypeName System.Net.WebClient).DownloadFile($url, $file)
powershell.exe -ExecutionPolicy ByPass -File $file
SCRIPT

$master = <<-SCRIPT
mkdir -p "/etc/ansible/group_vars/win"
export WIN_GROUP_VARS_FILE="/etc/ansible/group_vars/win/vars"
yum install python3 ansible -y
echo "---" >> $WIN_GROUP_VARS_FILE 
echo "ansible_user: vagrant" >> $WIN_GROUP_VARS_FILE
echo "ansible_password: vagrant" >> $WIN_GROUP_VARS_FILE
echo "ansible_connection: winrm" >> $WIN_GROUP_VARS_FILE
echo "ansible_winrm_server_cert_validation: ignore" >> $WIN_GROUP_VARS_FILE
echo "ansible_port: 5986" >> $WIN_GROUP_VARS_FILE
wget https://bootstrap.pypa.io/get-pip.py
python get-pip.py
pip install pywinrm
echo "[win]\n192.168.0.101\n192.168.0.102\n\n" >> /etc/ansible/hosts
echo "[lin]\n192.168.0.11\n192.168.0.12\n192.168.0.200\n\n" >> /etc/ansible/hosts
sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
cd .ssh
yes id_rsa |ssh-keygen -q -t rsa -N '' >/dev/null
chown vagrant:vagrant id_rsa id_rsa.pub
systemctl restart sshd
SCRIPT

$node = <<-SCRIPT
yum install sshpass -y
echo $(sshpass -p vagrant ssh -oStrictHostKeyChecking=no vagrant@192.168.0.200 'echo $(cat /home/vagrant/.ssh/id_rsa.pub)') >> /home/vagrant/.ssh/authorized_keys
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.define "ansible" do |subconfig|
    subconfig.vm.box = LIN_BOX_IMAGE
    subconfig.vm.hostname = "ansible"
    subconfig.vm.network :private_network, ip: "192.168.0.200"
    subconfig.vm.provision "shell", inline: $master
    subconfig.vm.provision "shell", inline: $node
  end

  (1..WIN_NODE_COUNT).each do |i|
    config.vm.define "win#{i}" do |subconfig|
      subconfig.vm.box = WIN_BOX_IMAGE
      subconfig.vm.hostname = "win#{i}"
      subconfig.vm.network :private_network, ip: "192.168.0.#{i + 100}"
      subconfig.vm.provision "shell", inline: $posh
    end
  end

  (1..LIN_NODE_COUNT).each do |i|
    config.vm.define "cent#{i}" do |subconfig|
      subconfig.vm.box = LIN_BOX_IMAGE
      subconfig.vm.hostname = "cent#{i}"
      subconfig.vm.network :private_network, ip: "192.168.0.#{i + 10}"
      subconfig.vm.provision "shell", inline: $node
    end
  end
end
