# vagrant-ansible-sandbox

**Built for windows PC sandbox**

**This should leave you with 1 ansible controller, two centos boxes and two windows boxes ready for playbooks and adhoc commands**

**Install latest VirtualBox**

https://www.virtualbox.org/wiki/Downloads

**Install latest git for Windows**

https://git-scm.com/download/win

**Install latest Vagrant**

https://www.vagrantup.com/downloads.html

**Clone repo and bring up sandbox**

    git clone https://github.com/mccbryan3/vagrant-ansible-sandbox.git

    cd vagrant-ansible-sandbox

    vagrant up

**SSH into the ansible server**

    vagrant ssh ansible1

**Run some adhoc commands against the groups**

    ansible lin -m ping

    ansible win -m win_ping

**Feel free to change the vagrant boxes that are used but do so at your own risk**

<h3>Environment Information</h3>


    default inventory file is used with groups [lin] and [win] for the respective platforms

    vm: ansible1
    - ip_adress = 192.168.0.200  
    vm: cent1
    - ip_adress = 192.168.0.11
    vm: cent2
    - ip_adress = 192.168.0.12
    vm: win1
    - ip_adress=192.168.0.101
    vm: win2
    - ip_adress=192.168.0.102
  
**No internal name resolution at the moment**
