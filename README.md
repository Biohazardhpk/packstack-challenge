# packstack-challenge
This repository intends to share lessons learned in a challenge regarding Vagrant, OpenStack, Ansible and Python

Decisions Taken on the challenge
This file describes the reasons why I decided to go on a specific direction. I will try to collect only the important choices taken. I have four evenings to finish the challenge, therefore time is a constant to take into account.

Once that I got the challenge from Richard, I started studying the tasks and requirements. We need to use Vagrant to deploy Openstack with POC tool called Packstack. I never used it before but I have had used DevStack so I guessed it would be similar. An ansible project must be created, with a role to automate openstack operations and a playbook to orchestrate VMs creation. Input to these ops must be on YAML format. Finally, I need to create a dynamic inventory in ansible to query environment information and group VMs based on attributes.

Decision 00
Before to start deploying anything, I check the files of the project:

Vagrantfile: recipe used by Vagrant to spin Centos up and execute Packstack.
answers.txt: recipe used by Packstack to spin OST environment up.
Going through the Vagrantfile, on line #31, packstack needs answers.txt, and this file is not added anywhere on the Vagrantfile, Vagrant will fail.

Going through answers.txt, on line #32, Nova component appears as "n", which means that will not be installed. This is definitely the second step fix.

Decision 01
Git workflow to use: on the first steps, I will be commiting and pushing on the master branch, as it is easier and faster for me. There is no CI in place nor collaboration with other people needed. Therefore, I have taken the decision to only push to master for now. If situation changes, I will adapt.
Decision 02
Install pre-commit githook: I am going to be working with Ansible a lot, therefore I want to be sure that my code is Lint compliant and syntax clean. I could set a .travis.yml file for that but I have not used Travis before and it is too much work for the same results. Also, I am not collaborating with anyone so integration is not much needed at this point. For that, I installed a pre-commit hook on the repository, so it is triggered before commiting a change.
Decision 03
Add infrastructure tests: I am going to use Ansible to modify the state of OST environment, therefore I need to test the infrastructure deployments. I am not sure yet which tool/platform to use for this matters as I am not very familiart with OST. Also, it is not a mandatory task, so I will not waste all my time here. Something simple and effective should work.
Decision 04
Add vagrant ports to open connections with the guest. Added "forwarded port" mapping 80:8080.
Decision 05
Open the necessary ports in vagrant to allow a VM on the same network to access the API endpoints showed by issuing $ openstack endpoint list --interface public. I could simply change NAT to public_network in vagrant but that seems less secure. I cannot spend much more time here. Truth is, I am not sure which way is the best, so I will just go for the way it works. Also, if I am not wrong, a VM on the same subnet will not be able to access any port in my Windows laptop unless I open the ports in the firewall.
Decision 06
Use Ansible instead of AWX: considering my experience with AWX and the tasks that need to be done, this project fits quite well with AWX. Specially the dynamic inventory querying information from Openstack environment. However, I decided to execute them on Ansible standalone as I am not 100% certain how to deploy correctly AWX on Packstack and time is not in my favor. On the other side, I will create the role and playbooks taking into account that could be migrated to AWX for a future project, if needed.
Decision 07
using one role instead of modularity: I decided to use one role instead of many because the exercise said so. However, the more I am developing, the more I regret not adding more granularity to the roles.
Decision 08
import_role and from_tasks: because I was using one only role, and I was never a friend of tags, I decided to use latest feature import_role and call the tasks separately. Probably not the best decision ever.
Decision 09
vars_files to include vars: because the requirements said to use a YML file, I am using vars_files for that.
Decision 10
I am aware that I am doing things wrong. It is too late to come back. If I was working without so much time pressure, I would change mostly everything: how to deal with roles, how to deal with vars, the main playbook, etc.
Decision 11
I cannot SSH/login into the VMs from packstack. I have tried everything, I am not sure what I am missing. I will keep going and focus on the last task as I cannot get stuck longer here. Tic tac.
Decision 12
dynamic inventory: I have worked with VMware dynamic inventory and Ansible Tower but I have never used Openstack one. I know there is an official dynamic inventory and I will check that one as I have barely 3 hours left to finish last task.
Decision 13
Query instance information: I am going to use openstack_inventory.py official script for such a task. I know the dynamic inventory is meant to be built manually but I do not know the openstack sdk library in python good enough.
Decision 14
Group VMs based on metadata: I understand the purpose and I have an idea of how this would be done, but I cannot finish it today. I rather get some sleep and be ready and fresh tomorrow, than start coding the answer right now.
