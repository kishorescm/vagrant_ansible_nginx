Purpose
=======

### Vagrant-Ansible-Nginx
This project uses vagrant to create virtual machines and configure nginx loadbalancer on another virtual machine. Vagrant Ansible provisioner is used configure provisioned virtual machines.


### Requirements
You will need Vagrant version 2.2.6 to run, I have made this compatiable with older versions but have not tested with version lesser than 2.2.3

### Usage
=====
````
git clone https://kishoregarlapati@dev.azure.com/kishoregarlapati/vagrant_ansible_nginx/_git/vagrant_ansible_nginx
cd vagrant-ansible-nginx
````
Spin up your environment
````
vagrant up
````


#### Test

Once the vagrant run followed by ansible play is complete
Open a browser of your choice and browse to http://10.0.15.11/