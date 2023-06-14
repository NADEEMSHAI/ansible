
###  ANSIBEL INSTALLATION & INFRASTRUCTER
--------------------------------------------------

* Lets us first create two virtual machines of t2micro in AWS.

      NAME OF VMS * ANSIBLE CONTROL NODE (master) 
                  * NODE ONE  (node)
* After that create a user with same name in both VM's. So let us assume our user name would be ansible.
* By using this command you can check is your user created or not  
    "cat /etc/passwd"
  
  ![preview](IMAGES/A1.png)

* Now we need to give sudoers permission to the user to do administration work using sudo & i want to avoid asking passwd every time.

       sudo visudo

  ![preview](IMAGES/A2.png)


* After that we need to give password configuration yes for login into machine as a user ansible
  
   sudo vi /etc/ssh/sshd_config
  
  ![preview](IMAGES/A3.png)

* Now just restart the service changes   " sudo service sshd restart ". EXIT the machine and relogin as a ansible user.

  ![preview](IMAGES/A4.png)

* After login as ansible uer do "  sudo apt update  ". Then it don't ask for passwd.

* So do the same steps in the node 1 machine also.

### SSH-KEYGEN CONFIG OF BOTH MACHINES
--------------------------------------
* As we have created a user called ansible and given sudo permission so in master node create a ssh key  " ssh-keygen "
  
 ![preview](IMAGES/A5.png)

* Now we need to copy the public key into node machine to connect the both machines. For that we need node private IP ADDS 
* So ideally if i want to connect the local laptop i will use public ip but here the both machines are in the same network. So they will have private ip for internal communications & private ip don't need internet this is  called internal communication (computerlabs, room env etc..) 
  
  ![preview](IMAGES/A6.png)

* Now you can see i'm able to login node from master node now we established the connection between both nodes. We can see ansible infra was created now we can install ansible 

### INSTALLING ANSIBLE
-----------------

* For ansible installation python is needed you can check if python is available in your machine or not "  whereis python ". some time python is not but if you see python3 it will be present mostly " whereis python3".
  
  ![preview](IMAGES/A7.png)

* If python is not present in vm then you have to install python first. Follow steps installation of ansible
  
  [refer here]https://docs.ansible.com/ansible/latest/installation_guide/installation_distros.html#installing-ansible-on-ubuntu  

* Remember these step should be done on ansible control node.

 ![preview](IMAGES/A8.png)


* After installation completed now lets add inventry file name called HOSTS file in that pravite ip of node.

  vi hosts

  172.31.46.63      (you can give name also)

* check connectivity by apply this command "  ansible -m ping -i hosts all "

    -m ping   ( Just to check the ping)      -i  ( inventary file )

* If this command works  without any error then your ansible installation and nodes configuration was succesfull.

  ![preview](IMAGES/A9.png)

* So lets us change the inventry file let me give some machine  " local host " here local host means i want to connect from ansible control to ansible control node.
   
  ![PREVIEW](IMAGES/A10.png)

* Here you can see i can't find the local host because i didn't setup for that so if there is any configuration issues also the errors shown will be like this means that your not satisfied.
