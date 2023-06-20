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

* 

      
   

