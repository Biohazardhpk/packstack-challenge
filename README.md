# packstack-challenge
This repository intends to share lessons learned in a challenge regarding Vagrant, OpenStack, Ansible and Python

# Notes
During the challenge I have encoutered some "situations" here my thoughts.

## Vagrantfile
- The file suggested to use port-forwarding to the machine (very nasty thing to do in VirtualBox).
- In a controlled environment I suggest to go with a bridge network, it eases work alot.
- Added installation of a ftp server client in order to get the packstack configuration file to the newly created Cloud Host.

## Cloud Environment
- The Cloud is mostly functional there were two configurations I performed to obtain the desired configuration:
 - The Horizion interface was not reachable beacuse of the public network configuration I did before thus:
   + vim /etc/httpd/conf.d/15-horizon_vhost.conf (Here you find the Hostname Alias as an IP)
 - The VMs will not be reachable from the outside environment. Studied the network overlay configuration and figured out what I needed to modify:
   + vim /etc/neutron/plugins/ml2/openvswitch_agent.ini (the packstack installation is using a OVS agent as a network plugin)
     + in the file above there is a network mapping to the outside environment, that needs to be configured as the interface configured at the first chapter:
     - bridge_mappings=extnet:eth1

## Ansible environment
- Installed it first on a different host (Ubuntu machine) and got issues with the remote permissions.
- Decided to install it on the Cloud Host (no issues with permissions). 
- Used:
  - https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-ansible-on-centos-7mnn
  - pip install git+https://github.com/larsks/ansible-toolbox
  - sudo pip install rackspace-novaclient
  - https://docs.ansible.com/ (used for everything else)

## Ansible configuration
- The first instance I created was without defining roles, I made a .yml with all the variables and tags I needed for the task.
- However the requirement was to use roles, used "ansigle-galaxy" to create the roles previously defined in the single file.
- Added a role for creation of speciic VM groups based on meta info (i.e. web_server, db_servers)
- I started the dynamic inventory from the provided example in the documentation, removed some unecessary code and added new code for grouping machine based on state.
