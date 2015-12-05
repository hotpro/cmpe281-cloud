# cmpe281-cloud

ssh -i aws-ubuntu.pem ubuntu@xxx.com
scp -i /path/my-key-pair.pem SampleFile.txt ubuntu@xxx.com:~

sudo shutdown -h now

sudo apt-get update
sudo apt-get upgrade
sudo apt-get install git
sudo apt-get install vim


#nginx
sudo apt-get install nginx
sudo /etc/init.d/nginx start
sudo /etc/init.d/nginx stop
sudo /etc/init.d/nginx restart

#nginx config
sudo vim /etc/nginx/conf.d/tomcat.conf
```
upstream mywebapp1 {
		ip_hash;
        server   127.0.0.1:8080;
        server   127.0.0.1:8181;
}

server {
    listen 80;

    location / {
        proxy_pass http://mywebapp1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

mkdir package
cd package/

#java 7
sudo apt-get install python-software-properties
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java7-installer #or sudo apt-get install oracle-java8-installer
which java
echo $JAVA_HOME
ls /usr/lib/jvm/java-7-oracle/


#tomcat
tar -zxvf apache-tomcat-8.0.28.tar.gz
ls package/apache-tomcat-8.0.28
vim ~/.bashrc

#tomcat
export JAVA_HOME=/usr/lib/jvm/java-7-oracle/
export CATALINA_HOME=/home/ubuntu/package/apache-tomcat-8.0.28
source .bashrc

#tomcat
```
$CATALINA_HOME/bin/startup.sh
$CATALINA_HOME/bin/shutdown.sh
```
package/startup-instance1.sh
package/shutdown-instance1.sh 

cd package
mkdir tomcat8
cd tomcat8/
mkdir tomcat-instance1

Create one folder named “tomcat-instance1” and copy conf, logs, temp, webapps, work folder from CATALINA_HOME folder and change conf/server.xml file in tomcat-instance1. Change these ports: shutdown port, connector port, ajp port and redirect port as follow:
```
<server port="8105" shutdown="SHUTDOWN">
	.....
	<connector
		connectiontimeout="20000" port="8181"
		protocol="org.apache.coyote.http11.Http11NioProtocol"
		redirectport="81443" />
	<connector port="8109" protocol="AJP/1.3" redirectport="81443" />
</server>
```

vim startup-instance1.sh
export CATALINA_BASE=/home/ubuntu/package/tomcat8/tomcat-instance1
cd $CATALINA_HOME/bin
./startup.sh

vim shutdown-instance1.sh 
export CATALINA_BASE=/home/ubuntu/package/tomcat8/tomcat-instance1
cd $CATALINA_HOME/bin
./shutdown.sh
chmod a+x startup-instance1.sh shutdown-instance1.sh

#tomcat cluster, enable
<Cluster className="org.apache.catalina.ha.tcp.SimpleTcpCluster"/>

#install maven
apt-cache search maven
sudo apt-get install maven
mvn --version



<Engine name="Catalina" defaultHost="localhost">
<Engine name="Catalina" defaultHost="localhost" jvmRoute="node1">
<Cluster className="org.apache.catalina.ha.tcp.SimpleTcpCluster"/>

#enable user
vim apache-tomcat-8.0.28/conf/tomcat-users.xml
<role rolename="manager-gui"/>
<user username="hotpro" password="Maxwell4" roles="manager-gui"/>

#deploy
mkdir development/github
git clone
mvn clean package
mv cmpe281.war package/apache-tomcat-8.0.28/webapps/

# test
mkdir webapps/test
vim index.jsp
cp test/* ~/package/tomcat8/tomcat-instance1/webapps/