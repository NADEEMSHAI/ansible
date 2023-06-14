### ANSIBLE HANDLERS 
------------------

* Handlers are something they do not run all the time they respond to something which has changed
* when ansible executing this playbook it will install apache first. After that when you are running over the first time when php get installed then only it trriged other wise it won't trriged 
* Now you can try to tell the handlers when the certain task get executed  than only it get trigged 
  
* Here we are using the handlers to avoidong  the restart every time.
* So for that i have written the " notify " where i need to be restart all notify will ecexute at last of when the work is done.
    
  ![PRE](IMAGES/H1.png)

* Here we done the syntax check we have found given " hosts: all " but we need to run in the ubuntu machine only 
  
  ![PRE](IMAGES/H2.png)

* After that i jsut applied  the ansible playbook lets see what happens 
    * "ansible-playbook -i inventory/hostss playbooks/lamp/main.yaml
  
  ![PRE](IMAGES/H3.png)

 * So here ansible is done my work without error in ubuntu but in redhat it will fail because the package manager is apt. So i just modified the main.yaml fine " hosts: ubuntu ".
  
 ![PRE](IMAGES/H4.png)

* After modification when i reapply the ansible command it wont work out again and again it just shown that work is already done  CHANGES=0

* Same we have added handler over redhat also
  
  ![pre](IMAGES/H5.png)
  ![PRE](IMAGES/H6.png)
  
* HENCE WE RESLOVED FIRST PROBLEM OF RESTARTING EVERY TIME 

