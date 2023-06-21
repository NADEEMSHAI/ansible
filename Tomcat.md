### MANUAL  INSTALLATION  OF  TOMCAT
----------------------------

* As we know first we have to gather the manual steps of tomcat installation. So here thier are 2 parts of installation 
     * One is tomcat server installation.
     * Second is tomcat server  configuring  Firewall.

* first steps 
    * sudo apt update 
    * sudo apt install openjdk-11-jdk
    * java -version
    * sudo useradd -m -U -d /opt/tomcat -s /bin/false tomcat
    * VERSION=10.1.9...... CHECK THE VERSION UPTODATE
    * wget https://www-eu.apache.org/dist/tomcat/tomcat-10/v${VERSION}/bin/apache-tomcat-${VERSION}.tar.gz -P /tmp
    * sudo tar -xf /tmp/apache-tomcat-${VERSION}.tar.gz -C /opt/tomcat/
    * sudo ln -s /opt/tomcat/apache-tomcat-${VERSION} /opt/tomcat/latest
    * sudo chown -R tomcat: /opt/tomcat
    * sudo sh -c 'chmod +x /opt/tomcat/latest/bin/*.sh'
    * sudo nano /etc/systemd/system/tomcat.service

       *[Unit]

Description=Tomcat 10 servlet container
After=network.target

[Service]
Type=forking

User=tomcat
Group=tomcat

Environment="JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64"
Environment="JAVA_OPTS=-Djava.security.egd=file:///dev/urandom -Djava.awt.headless=true"

Environment="CATALINA_BASE=/opt/tomcat/latest"
Environment="CATALINA_HOME=/opt/tomcat/latest"
Environment="CATALINA_PID=/opt/tomcat/latest/temp/tomcat.pid"
Environment="CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC"

ExecStart=/opt/tomcat/latest/bin/startup.sh
ExecStop=/opt/tomcat/latest/bin/shutdown.sh

[Install]
WantedBy=multi-user.target


[refer]https://linuxize.com/post/how-to-install-tomcat-10-on-ubuntu-22-04/  ...... refer this document for text here to put in nano editor

     * sudo systemctl daemon-reload
     * sudo systemctl enable --now tomcat
     * sudo systemctl status tomcat
     * NOTE :AFTER THIS STEPS YOU CAN CHECK ON WE PAGE WITH IP IF IT IS NOT WORKING PLEASE CHECK FIRST PORT 8080 IS ALLOWING.
* So we have done first part in manual steps. Here if you observe on the tomcat image i have marked 3 black dot we access those it was second task.
  
![pre](Playbooks/Tomcat/imagestom/T1.png)
 
* Second Configuring steps
   
  
   * sudo nano /opt/tomcat/latest/conf/tomcat-users.xml
   * <tomcat-users>
   # <role rolename="admin-gui"/>
   # <role rolename="manager-gui"/>
   # <user username="tomcat" password="TOMCAT" roles="admin-gui,manager-gui"/>

   * So here please refer the installation doucment
   * sudo nano /opt/tomcat/latest/webapps/manager/META-INF/context.xml
   * sudo nano /opt/tomcat/latest/webapps/host-manager/META-INF/context.xml
   * 

   * <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" /> ...... so here we remove the content between both " "  and put  ".*" 

     
  * sudo systemctl restart tomcat

 * So here in the first step we given the input data in nano file we mention username="tomcat" password="TOMCAT"
 * In the second and third step we give the input in nano and also edit the allow".*" before that their was some data we remove it and give just ".*"
 * In the final step we do systemctl restart the tomcat so now if we login in manager section this it will ask admin and password.
   ![pre](PLAYBOOKS/Tomcat/imagestom/t2.png)
   ![pre](PLAYBOOKS/Tomcat/imagestom/t3.png)

### PLAYBOOK  INSTALLATION  OF  TOMCAT
---------------------------------------

* As of now we can see that we succesfully created up end runnning tomcat server and also tomcat configuration manger service.
* Let us do it with the anisble-playbook. For that we have to setup the ansible infrastructre first and a new folder of tomcat in that we need one of Playbook/ubuntu.yaml folder and Group_vars/tomcat.yaml/tomcat.service.j2 folder and inventory/hosts.

### STEP .1. INSTALL JAVA
------------

* So now i am going step by step writting the playbook refers of manual commands. Now i have completed the fisrt step of java installation with playbook. 
* Refer here for changes and tomcat infra structure
  
  * sudo apt update 
  * sudo apt install openjdk-11-jdk
  * java -version
 
   [refer]https://github.com/NADEEMSHAI/ansible/commit/2b0ac9fd0ac604d24305c8c319c3dfc23a9e019e
 
   ![pre](PLAYBOOKS/Tomcat/imagestom/t4.png)

### STEP .2. CREATE USER GROUP
-------------------

* Now lets us create the a group and user for tomcat and along with  shell in home directry of /opt/tomcat.
  
      * sudo useradd -m -U -d /opt/tomcat -s /bin/false tomcat
      * Here -m : create a new user home directory if it does not exists
      * -u : create a group with the same name as the user and add the user into this group
      * -d : the new user will be created using HOME_DIR as the value for the user login directory. the deafult is append thelogin name ti BASE_DIR and use that as the login directly 
      * -s : the name of user login shell. the default is to leave this filed blank, which causes the system to select the default login shell specified by the shell variable in /etc/default/useradd or and empty string by defult.
      * create_home :Unless set to false, a home directory will be made for the user when the account is created or if the home directory does not exist. " Changed from createhome to create_home "
  * Here you can see the changes for the user add and group add of tomcat.
     Refer here [refer]https://github.com/NADEEMSHAI/ansible/commit/705bc9c2f939ace23770d3913403b0d5700d8c53
               ![pre](PLAYBOOKS/Tomcat/imagestom/t5.png)

### STEP .3. INSTALL TOMCAT
-----------------

* Lets move to the next command installation of tomcat here you need to check the version up to date.

    * VERSION=10.1.9...... CHECK THE VERSION UPTODATE
    * wget https://www-eu.apache.org/dist/tomcat/tomcat-10/v${VERSION}/bin/apache-tomcat-${VERSION}.tar.gz -P /tmp
* So here we have wget which will be download from the link. In ansible we use url instead of wget
* If you oberver we have " ${VERSION} "  " -${VERSION} " here in both condition $ is present it means that remember when we use the echo hello then it will print hello similarly here also it will print version but we need to run it * So lets us check in ansible we use JINJA script like {{ version }} in both places we define the version in vars file. Lets do it refer here [refer]https://github.com/NADEEMSHAI/ansible/commit/fcaa06325a51dabe800a1650953d232c1f1b180a
   ![pre](PLAYBOOKS/Tomcat/imagestom/t6.png)
    
    * So here we got error because i had given the total url which got error here if you observe the last " -p /tmp " it means that after download the tomcat put in temp destination folder. which is not known to ansible it will work in manually 
    * For that i'm writting dest: "/temp/apache-tomcat-{{ VERSION }}.tar.gz" ....after i remove that -p it works
     ![pre](PLAYBOOKS/Tomcat/imagestom/t7.png)

### STEP .4. EXTRACT TOMCAT
--------------------------------------

* Lets move to next step. Here we it is extracting the tomcat file from tar to /opt/tomcat/ homedirectory
    *  sudo tar -xf /tmp/apache-tomcat-${VERSION}.tar.gz -C /opt/tomcat/.... tar  what is in ansible ansible.builtin.unarchive:
    * remote_src: Set to true to indicate the archived file is already on the remote system and not local to the Ansible controller.This option is mutually exclusive with copy. this would be default in false condition.
* Refer here for the changes [refer]https://github.com/NADEEMSHAI/ansible/commit/5c4b40a47b6ce36e2c8a2411822f46bc6eb506ee
* Here we are facing error
  ![pre](PLAYBOOKS/Tomcat/imagestom/t9.png)
* ERROR resloved
  [refer]https://github.com/NADEEMSHAI/ansible/commit/be9e3089f88d7cd06fac906b589488c790539ff9
  ![pre](PLAYBOOKS/Tomcat/imagestom/t8.png)

   * sudo ls /opt/tomcat/

### STEP .5. CREATE LINK
-----------------------  

* Lets move to the next step. Here we are try to create a symbol link (just like a short cut if you have a folder in c drive in that some other folder is present we make that folder short cut on desktop )
* Lets do it link in ansible. ansible.builtin.file:

  *  sudo ln -s /opt/tomcat/apache-tomcat-${VERSION} /opt/tomcat/latest

  * here ln -s means softlink. A soft link isn't a seprate file, it points to the name if the original file, rather than to a spot on the harddrive.
  * state: HARD:  If hard, the hard link will be created or changed.
  * Stare: SOFT:  If link, the symbolic link will be created or changed. 
  * src :Path of the file to link to. This applies only to state=link and state=hard. PRESENT TOMCAT DESTINATION /opt/tomcat/apache-tomcat-${VERSION}
  * dest:  /opt/tomcat/latest we need to copy (or) creat a shortcut this dest.
* REFER HERE FOR CAHANGES 
      [REFER] https://github.com/NADEEMSHAI/ansible/commit/7faf39a9a80b69723dca69f11135751b20e168e7

    ![pre](PLAYBOOKS/Tomcat/imagestom/t12.png)
    ![PRE](PLAYBOOKS/Tomcat/imagestom/T10.png)
     
     * sudo ls -al /opt/tomcat/.... you can check here in node latest is created 
     ![pre](PLAYBOOKS/Tomcat/imagestom/t11.png)


### STEP .6. OWNERSHIP TO TOMCAT USER
-------------------------------------------


* So now we are changing the all ownership premission from all in tomcat to user of tomcat only.
    * sudo chown -R tomcat: /opt/tomcat
* Lets do it chown in ansible. ansible.builtin.file:
  
    * owner: Name of the user that should own the filesystem object, as would be fed to chown. 
    * recurse : Recursively set the specified file attributes on directory contents. This applies only when state is set to directory.
    * path : Path to the file being managed.
* REFER HERE FOR CAHANGES 
    [REFER]https://github.com/NADEEMSHAI/ansible/commit/b9487b5829b3590f86eb7fb57c0ac39b44bb34ce

    ![pre](PLAYBOOKS/Tomcat/imagestom/t13.png)
    ![pre](PLAYBOOKS/Tomcat/imagestom/t14.png)

    * sudo ls -al /opt/tomcat
* So here and before task if you observe the extract task was skipping every time we will  if it NOTE THIS PROBLEM.
  
### STEP .7. SHELL CHMOD
------------------------

* So now to explain this commond we will see two manul commands first
*  sudo sh -c 'chmod +x /opt/tomcat/latest/bin/*.sh'

   * sudo ls -al /opt/tomcat/latest/bin/
   * sudo ls -al /opt/tomcat/latest/bin/ | grep sh
  
* So now basically in the first command you would see all the files  premissions where as second command it will only show sh file all premissions 
* Apply second command in ansible also premission will not same.
* So let me directly run the linux command from ansible. when you running directly a linux command it will never be an idempotent.
* when ever you will run a playbook it will execute. Their will be a low level command like cmd,shell,run it will execute everytime you run playbook.
    * sudo ls -al /opt/tomcat/latest/bin/ | grep sh
    *  You can see all premission are same.But it not a good idea because it will not maintain state. Their is other way lets see 
* REFER HERE FOR CHANGES
    [REFER]https://github.com/NADEEMSHAI/ansible/commit/25291f36f64596f5b48b8ca87324d0ce5bd16d82
    ![PRE](PLAYBOOKS/Tomcat/imagestom/t15.png)

### STEP .8. CREATE SERVICE JINJA2
------------------------------------

* We need to create a service file but it has dynamic content, so copying static file is not an option, Ansible has tempenting 
   * For expression we use jinja templates.
   * For modules we use templates modules.
* So we use to see in our yaml files. "{{ var_name }}" this type is comes from jinja templates.
* So here for passing the nano file data we will create a serivce file in playbook -> tomcat.service.j2
* In that we replace user = {{ user }} ..... group = {{ group }} also /opt/tomcat= {{ homedir }} 
* Lets see the how should be it done.
    ![pre](PLAYBOOKS/Tomcat/imagestom/t16.png)

* So lets check this how to check if see cat this path sudo ls -al /etc/systemd/system/tomcat.service in node it will show the tomcat service file is add and if we cat the path it should print the serive file.
   
    ![pre](PLAYBOOKS/Tomcat/imagestom/t17.png)
   
    ![pre](PLAYBOOKS/Tomcat/imagestom/t19.png)
   
    ![pre](PLAYBOOKS/Tomcat/imagestom/t18.png)

* So the tomcat is up end running we done the first process 