# TOMCAT10.1.15
   ### Apache tomcat installation on ubuntu. 
prerequisite:
AWS Acccount.
Create ubuntu EC2 T2.micro Instance.
Create Security Group and open Tomcat ports or Required ports.
8080 ..etc
Attach Security Group to EC2 Instance.
Install java openJDK 1.8+
# the command below is to ensure we cant login as tomcat for security reasons
sudo useradd -m -d /opt/tomcat -U -s /bin/false tomcat 
sudo apt update 
sudo apt install default-jdk
java -version 
cd /tmp
# the two commands below install git and tomcat tar file from interbnet that need to be extracted
sudo apt install git wget -y 
sudo wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.15/bin/apache-tomcat-10.1.15.tar.gz 
sudo tar xzvf apache-tomcat-10*tar.gz -C /opt/tomcat --strip-components=1
sudo nano /opt/tomcat/conf/tomcat-users.xml
sudo chmod 777 -R /opt/tomcat

# from here, if you cant get access, switch to roots and
# IMPORTANT: USE THE MANAGER SCRIPT USERNAME AND PASSWORD FOR JENKINS. MANAGER SCRIPT IS THE ONLY WITH PERMISSION FOR DEPLOYMENT
vi /opt/tomcat/conf/tomcat-users.xml and add the credentials at the bottom
-->
  <user username="admin" password="admin123" roles="manager-gui"/>
  <user username="manager" password="manager123" roles="manager-script"/>
  </tomcat-users>

sudo vi /opt/tomcat/webapps/manager/META-INF/context.xml
and comment the valve portion as shown below. this gives us access to the tomcat from the gui.
 <!-- <Valve className="org.apache.catalina.valves.RemoteAddrValve"
 allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" /> -->


sudo vi /opt/tomcat/webapps/host-manager/META-INF/context.xml
and do the same thing for host-manager

  <!-- <Valve className="org.apache.catalina.valves.RemoteAddrValve"
  allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" /> -->

# run this command to find where tomcat resides
sudo update-java-alternatives -l
/usr/lib/jvm/java-1.11.0-openjdk-amd64

# this command is important to run after changes
sudo systemctl daemon-reload

# we now create a soft link to start and stop tomcat

sudo ln -s /opt/tomcat/bin/startup.sh /usr/bin/starttomcat 
sudo ln -s /opt/tomcat/bin/shutdown.sh /usr/bin/stoptomcat

# we can simply run starttomcat and stoptomcat
# TOMCAT 9.0.83 on redhat server COMPATIBLE WITH JENKINS
cd /opt  
sudo yum install git wget -y 
sudo yum install java-1.8.0-openjdk-devel -y 
sudo yum install wget unzip -y 
sudo wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.83/bin/apache-tomcat-9.0.83.tar.gz 
sudo tar -xvf apache-tomcat-9.0.83.tar.gz 
sudo rm -rf apache-tomcat-9.0.83.tar.gz 
sudo mv apache-tomcat-9.0.83 tomcat9 
sudo chmod 777 -R /opt/tomcat9 
sudo chown ec2-user -R /opt/tomcat9 
sh /opt/tomcat9/bin/startup.sh 
sudo ln -s /opt/tomcat9/bin/startup.sh /usr/bin/starttomcat 
sudo ln -s /opt/tomcat9/bin/shutdown.sh /usr/bin/stoptomcat 
sudo vi /opt/tomcat9/conf/tomcat-users.xml
# add manager script user because he can be the only to deploy using jenins, and make sure the credentials are the same with the jenkins
sudo vi /opt/tomcat9/webapps/manager/META-INF/context.xml
# comment the valse
sudo vi /opt/tomcat9/webapps/host-manager/META-INF/context.xml
# comment valve too

