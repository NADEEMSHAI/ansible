### INSTALLING LAMP SERVER ( LINUX APACHE2 MYSQL PHP ) 
------------------------------------------------------

* Now lets us set the ansible setup first ansible control node, node 1 , manual.
* After that login into manual and find for lamp  installing on ubuntu and do the manual steps.
* HERE THE MANUAL STEPS FOR INSTALLATION OF LAMP 
*  Manual steps for lamp server installation
 * sudo apt update

 * sudo apt install apache2 -y

 * sudo apt install mysql-server -y

 * sudo apt install php libapache2-mod-php php-mysql -y

 * sudo -i 

   * /var/www/html/info.php " <?php phpinfo (); ?> " 

   * These command for to set the php path in sudoers user

   * exit

* sudo systemctl restart apache2

* check the node public ip /info.php

* So afrer this if you  go to your website server and give node public ip/info.php . you should be able to see a differnet looking web page were it would tell about something of your server
* Lets do it in ansible create a playbook folder which have tree and apache2 in that create a lamp floder in that create two files main.yaml and info.php
  
  ![PREVIEW](IMAGES/A23.png)

* So now before apply the ansible command. Here we have a --SYNTAX-CHECK option for that and also --CHECK
* SYNTAX: It will just wheather check your playbook is ready according to syntax or not.
* CHECK: check is not implementing the playbook but it try to predit when you execute the playbook what would happend when you see something is ook it is present in ansible when you see something is different that will be executed by ansible 

     * ansible-playbook -i inventory/hostss --syntax-check playbooks/lamp/main.yaml
 
     * ansible-playbook -i inventory/hostss --check playbooks/lamp/main.yaml

  ![preview](IMAGES/A21.png)

* After syntax check we can see what ansible is creating. Now we can apply the ansible command lets see is it create same as check
   * ansible-playbook -i inventory/hostss  playbooks/lamp/main.yaml

 ![preview](IMAGES/A22.png)

* Here you can see how the page works 
   
   ![preview](IMAGES/A24.png)

PROBLEM: During every playbook execution the apache service is getting restarted



### REDHAT LAMP 
----------------

* So we have take a REDHAT machine and written a lamp using yum package and checked for syntax and checks after that we applied the playbook it was showing error because in " hosts: all "
* So we cant give all in hosts because thier  a ubuntu machine so redhat playbook will fail. So we need to need redhat.
![PREVIEW](IMAGES/RA4.png)
![PREVIEW](IMAGES/RA1.png)
![PREVIEW](IMAGES/RA2.png)
![PREVIEW](IMAGES/RA3.png)
![PREVIEW](IMAGES/RA5.png)