# vagrant-ansible-sandbox

**Install latest VirtualBox**

https://www.virtualbox.org/wiki/Downloads

**Install latest git for Windows**

https://git-scm.com/download/win

**Install latest Vagrant**

https://www.vagrantup.com/downloads.html

**clone repo and bring up sandbox**

git clone https://github.com/mccbryan3/vagrant-ansible-sandbox.git

cd vagrant-ansible-sandbox

vagrant up

vagrant ssh ansible

ansible lin -m ping

ansible win -m win_ping
