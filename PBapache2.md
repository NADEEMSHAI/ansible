### INSTALLING APACHE2 ON ANSIBLE WITH PLAYBOOK
---------------------
* We have setup ansible infra with ansible control node and NODE 1, NODE 2.
* Here we have done the ssh key pair for two nodes with ansible control node after that we have checked python availability and installed ansible in master node
* So we do connection check with ansible controlnode with --vi hosts--. In this we have given both nodes private ip address. Why because these nodes are in same network
  
  ![preview](IMAGES/A11.png)

* So ansible can communicate with nodes by using two apporaches
    * adhoc commands : It means to make ansible run you will type commands 
    * playbook : You will try to tell a desried state in the form of file. The file is a yaml mostly this is recommeded apporach for repetitive activies
    * EXAMPLE : If i need to restart all my servers onces in a year in that case i would be go with adhoc 
              : If i need to restart or update all my servers then i would prefer playbook it will maintain a desired state file.
* So i have written a sample update and installation of TREE 
  
  ![preview](IMAGES/A12.png)

* Here i have return this file in the local system after that i push to git hub and clone the github
* Other wise i was just copied the file and in ansible master created a folder of PLAYBOOKS/HELLO.YAML & INVENTORY/HOSTS
  
 ![PREVIEW](IMAGES/A14.png)

* Here you can see i was not created inventory/hosts after that i just applied the  command 
* " ansible-playbook -i inventory/hosts playbooks/hello.yaml "  here i got error 

 ![preview](IMAGES/A15.png) 
  
* As you can see i change is occur and the TREE installed now we can check in node 

 ![preview](IMAGES/A16.png)

* So this the sample tree installed here. Let us see authorized keys after that let me uninstall tree manually from node

 ![PREVIEW](IMAGES/A17.png)

* Now lets reapply the ansible command after that it will check first wheather it was present or not then it will creat a new tree again
* If it was present their the it dont create wheater you apply the same command 1000 times this is called IMDEPOTENCE
* PLAYBOOK--

 ----

- name: hello anisble           = name of the play

  hosts: all                    = where to run everthing that exists in inventory.

  become: yes                   = generally it means run as sudo user (with all premissions).

  tasks: 
   
   -name: update packages and install tree = name of the task.

    apt:                       = This module is were ansible work is done. The rest of all work is logic organization so that we can do multiple activities 
      
      name: tree               = what you want to install

      state: present           =`is it present or absent

      update_cache: yes        = work done or no 

### WAYS OF WORKING PLAYBOOK
--------------------

* LIST DOWN THE ALL MANUAL STEPS FIRST
* ENSURE THAT ALL THE MANUAL STEPS ARE WORKING 
* FOR EACH STEP FIND THE MODULE ( apt in ansible = ansible.builtin.apt module)
  
* So when we will see the errors you will first assume that your playbook is wrong. But some times it might not it should be also your manual steps. 
## Lets as write a playbook installation of apache2

 ![preview](IMAGES/A18.png)

* So i have checked the manual step in other machine and written the playbook for apache2 and applied the command.
*  " ansible-playbook -i inventory/hostss playbooks/apache2 "

 ![PREVIEW](IMAGES/A19.png)

* So after that take the NODE PUBLIC IP AND CHECK THE IP ACPACHE2 IS INSTALLED.
   
  ![PREVIEW](IMAGES/A20.png)

* As you can see the changed=0.if the work was present their then again if give same command to create it i dont create.

     
