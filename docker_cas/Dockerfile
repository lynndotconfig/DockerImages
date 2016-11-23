FROM ubuntu:14.04

MAINTAINER lynndotconfig <lynn.config@gmail.com>

# Get dependencies
RUN apt-get update
RUN apt-get install -y wget
RUN apt-get install -y tomcat7
RUN apt-get install -y maven2
RUN apt-get install -y default-jdk
RUN apt-get install -y libtcnative-1
RUN apt-get install -y libssl-dev

# Download and install cas server
RUN wget http://downloads.jasig.org/cas/cas-server-4.0.0-release.zip
RUN unzip cas-server-4.0.0-release.zip

# Add binaries to path
ENV CATALINA_HOME=/usr/share/tomcat7
ENV CATALINA_BASE=/var/lib/tomcat7
ENV JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64

RUN rm -rf /var/lib/tomcat7/webapps/*
RUN cp cas-server-4.0.0/modules/cas-server-webapp-4.0.0.war /var/lib/tomcat7/webapps/ROOT.war
RUN cd /usr/share/tomcat7
RUN ls -l
RUN ln -s /var/lib/tomcat7/common/ /usr/share/tomcat7/common
RUN ln -s /var/lib/tomcat7/server/ /usr/share/tomcat7/server
RUN ln -s /var/lib/tomcat7/shared/ /usr/share/tomcat7/shared

# Generate .keystore and remember your keystore password
# avoid interactive command by a long command with options
# RUN .$JAVA_HOME/bin/keytool -genkey -alias tomcat -keyalg RSA -keypass password -storepass password -dname "CN=Unknown, OU=Unkown, L=Unkown, ST=Unkown, C=Unkown"
# cp keystore file to directory /usr/share/tomcat7
# On general, file ".keystore" in user's home directory
# RUN cp ~/.keystore /usr/share/tomcat7/

RUN apt-get install -y vim
# edit ssl connection

# edit SSL connection on /var/lib/tomcat7/conf/server.xml
# modify connection 8080 and 8443 as follows
# <Connector port="8080" protocol="HTTP/1.1" connectionTimeout="20000" URIEncoding="UTF-8" redirectPort="8443" address="0.0.0.0" />
RUN cd /var/lib/tomcat7/conf/

RUN sed -i '74a address="0.0.0.0"' /var/lib/tomcat7/conf/server.xml
RUN sed -i '88d;92d' /var/lib/tomcat7/conf/server.xml
RUN sed -i 's/sslProtocol="TLS"/sslProtocol="TLSv1"/' /var/lib/tomcat7/conf/server.xml
RUN sed -i '89a keystoreFile="/usr/share/tomcat7/.keystore" keystorePass="password"' /var/lib/tomcat7/conf/server.xml
RUN sed -i 's/UseConcMarkSweepGC/& -Dhttps.protocols=TLSv1/' /etc/default/tomcat7
RUN sed -i '4608a <security-constraint><web-resource-collection><web-resource-name>Protect Context</web-resource-name><url-pattern>/*</url-pattern></web-resource-collection><!--auth-constraint goes here if your require authentication --><user-data-constraint><transport-guarantee>CONFIDENTIAL</transport-guarantee></user-data-constraint></security-constraint>' /var/lib/tomcat7/conf/web.xml

# RUN /etc/init.d/tomcat7 start
CMD ping 8.8.8.8
# CMD tail -f /var/lib/tomcat7/logs/catalina.out