### ANSIBLE FACTS
---------------

@ How would ansible knows the current node details. When we running in other operating system it gets failed ..?

* Inventory in ansible represents the hosts which we need to connect.
* Ansible inventory is broadly classified in two types
   * STATIC INVENTORY: Where we can mantion the list of nodes to connect to in some files
   * DYNAMIC INVENTORY: Where we mention some script / plugin. which will dynamically find out to connect.
* As of now lets focus on the static inventory
  
  ![pre](IMAGES/f7.png)

* And now can we see the list of host. Here in hosts we have some many details about that hosts like ansible_facts, os_family, distribution, ipv4,etho and so many details we have in hosts 
    * " ansible-playbook -i inventory/hostss playbook/lamp/main.yaml --list-hosts "
    * " ansible-playbook -i inventory/hostss playbook/lamp/redmain.yaml --list-hosts "
* # FACTS 
* If you remember when ever we run ansible play book their was an extra facts was running over their it is called GATHER FACTS
  
  @ problem why we are writing two yaml files one for each like main.yaml redmain.yaml

* Gather facts was added by ansible. Ansible collects information about the node which it is executing by the help of modules called SETUP
   * " ansible -m setup -i 'localhosts, ' all "   .  m= module , i=inventory (localhosts or nodes)
* Here ansible try to get the information about the machine which we given -i 'localhost or node ip,' to setup
   * " ansible -m 'setup' -i '172.23.232.0,localhosts,' -a 'filter=*os*' all "
   * " ansible -m 'setup' -i '172.23.232.0,localhosts,' -a 'filter=*distribution*' all " 
* So like that we can search the what distribution or os_family,,,,etc. here we have some examples 
    ![pre](IMAGES/F1.png)
    ![pre](IMAGES/F2.png)
    ![pre](IMAGES/F3.png)
    ![pre](IMAGES/F4.png)
    ![pre](IMAGES/F5.png)
    ![pre](IMAGES/F6.png)


### COMBINED FILE OF ANSIBLE
--------------------------------


* So here i'm are combining the both main.yaml (ubuntu) redmain.yaml (redhat) files into combo.yaml 
* After that i'm runnign the combo file with a condition because it should run according the machine which tasks should be executed on which machine.
* For thar i'm writing a when condition it tells that when should be that task have to be run in which machine 
   * when: ansible_facts['os_family'] == 'Debian'
   * when: ansible_facts['os_family'] == 'RedHat'
* Here the " Debian = ubuntu" " redhat= RedHat " according to that i'm applying this condition for each task as you see below
 
  ![pre](IMAGES/C1.png)
 
  ![pre](IMAGES/C2.png)

* After that the task was executed sucessfully. We have seen skipped=3 , skipped=4. Those are nothing but it check the condition when ubuntu machine task is running then it skipped redhat task same as when redhat machine it skipped ubuntu tasks.
  
 ![pre](IMAGES/C3.png)

* Here i have uninstalled all apache2 and httpd and php all infra.
   
   ![PRE](IMAGES/U1.png)

* Now i'm going to run the combo.yaml so it created all if you observer past ansible dont changed=0 but now changed =3 changed =4

  ![pre](IMAGES/C4.png)

  ![PRE](IMAGES/C5.png)

  ![PRE](IMAGES/C6.png)


####  ANSIBLE VARIABLES
----------------------

* As of now we have created ansible infrastructure and run the yaml files to install the softwares apache2, httpd, php,mysql.
* If you observer we have written the inventory/hosts file which contains the information about the ip address of nodes  which  we  written in yaml format.
* But now onwords i'm writting in " INI " format which looks like below 
  
  ![PRE](IMAGES/C7.png)

* Here something new i have added right, So those are called ANISBLE VARAIBLES.

       * Here it means that were ever i tell apache2 to install or restart it will be replace by packages_name         " packages_name=apache2 "
       * Here also the same case but in one variable i'm passing three installation of ( php, php-mysql, libapache2-mod-php ) were ever i tell to install  it will also replace by "  php_packages='["php","libapache2-mod-php","php-mysql"]'  " 
       * So here in one variable i have 3 task to do in ubuntu and in redhat one varible have only one task so like that you can pass the task her this method is called " LOOPING "
  
*  @ GENERIC OS PACKAGE MANAGER

* Generic os package manager does if running this module in ubuntu it will automatically run " apt " or in redhat " yum ". So here you no need to specify the package manager "yum, apt ". So that you can go a head with your task with multiple operating systems
* So like this we pass the varibles in the files this is starting stage later on will change lot data into varaibles.
  

   ![PRE](IMAGES/C8.png)
   ![PRE](IMAGES/C9.png)

* So i made two changesets one in hosts file two in combined file.


* So if u observer here when i run the file first time changes was made by ansible because i have given the varaibles so that i created them onces after that when i run again it dosn't creat anything beacuse those all already created.
  