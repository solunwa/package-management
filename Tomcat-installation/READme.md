#  **<span style="color:green">Landmark Technologies, Ontario, Canada.</span>**
### **<span style="color:green">Contacts: +1437 215 2483<br> WebSite : <http://mylandmarktech.com/></span>**
### **Email: mylandmarktech@gmail.com**

## Apache Tomcat Installation And Setup In AWS EC2 Redhat Instance.
##### Prerequisite
+ AWS Acccount.
+ Create Redhat EC2 T2.micro Instance.
+ Create Security Group and open Tomcat ports or Required ports.
   + 8080 ..etc
+ Attach Security Group to EC2 Instance.
+ Install java openJDK 1.8+

### Install Java JDK 1.8+ 

``` sh
# change hostname to tomcat
sudo hostnamectl set-hostname tomcat
sudo su - ec2-user
cd /opt 
# install Java JDK 1.8+ as a pre-requisit for tomcat to run.
sudo yum install git wget -y
sudo yum install java-11-openjdk-devel -y
# install wget unzip packages.
sudo yum install wget unzip -y
```
## Install Tomcat version 9.0.75
### Download and extract the tomcat server
``` sh
sudo yum install wget unzip -y
export VER="9.0.64"
sudo wget https://archive.apache.org/dist/tomcat/tomcat-9/v${VER}/bin/apache-tomcat-${VER}.tar.gz
sudo tar xvf apache-tomcat-${VER}.tar.gz 
sudo rm -rf apache-tomcat-9.0.64.tar.gz
### rename tomcat for good naming convention
sudo mv apache-tomcat-9.0.64 tomcat9  
### assign executable permissions to the tomcat home directory
sudo chmod 777 -R /opt/tomcat9
sudo chown ec2-user -R /opt/tomcat9
### start tomcat
sh /opt/tomcat9/bin/startup.sh
# create a soft link to start and stop tomcat
# This will enable us to manage tomcat as a service
# sysmbolic link creation did not work below we may have to use the direct absolute path to perform service
sudo ln -s /opt/tomcat9/bin/startup.sh /usr/bin/starttomcat  # did not work
sudo sh /opt/tomcat9/bin/startup.sh  # worked!!!
# sysmbolic link creation did not work below we may have to use the direct absolute path to perform service
sudo ln -s /opt/tomcat9/bin/shutdown.sh /usr/bin/stoptomcat  # did not work
sudo sh /opt/tomcat9/bin/shutdown.sh  # worked!!!
sudo su - ec2-user
```
## Linux tomcat server and user configuration after the completion of installation  
## Plesae this is only for my personal use and the information here will need to be modified 

configure tomcat server:
 1.  Enable external tomcat mgt access
  vi /opt/tomcat9/webapps/manager/META-INF/context.xml


 <Valve className="org.apache.catalina.valves.RemoteAddrValve"
   allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />

<!--
 <Valve className="org.apache.catalina.valves.RemoteAddrValve"
    allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />
--> 


2. create tomcat users with access roles  :
vi /opt/tomcat9/conf/tomcat-users.xml  =# to add user
vi /opt/tomcat9/conf/tomcat-users.xml

<user username="landmark" password="admin123" roles="manager-gui,admin-gui,manager-script"/>
<user username="simon" password="admin123" roles="admin-gui"/> 

