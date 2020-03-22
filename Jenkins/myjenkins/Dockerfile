
# Pull base image
# ---------------
FROM openjdk:8-jdk

# Author
# ------
MAINTAINER Vamseekrishna Jamalla <vamseekrishna.jamalla.com>

# Build the container
# -------------------

USER root

# install wget vim sudo curl locate
RUN apt-get update
RUN apt-get install -y apt-utils wget vim sudo curl locate
RUN apt-get install apt-transport-https ca-certificates curl software-properties-common gnupg2
RUN curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
RUN add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"
RUN apt-get install docker-ce
RUN systemctl status docker

# get apache tomcat 8.5.35
RUN wget --no-verbose -O /tmp/apache-tomcat-8.5.35.tar.gz https://archive.apache.org/dist/tomcat/tomcat-8/v8.5.35/bin/apache-tomcat-8.5.35.tar.gz

# get maven 3.3.9
RUN wget --no-verbose -O /tmp/apache-maven-3.3.9-bin.tar.gz http://apache.cs.utah.edu/maven/maven-3/3.3.9/binaries/apache-maven-3.3.9-bin.tar.gz

# get tomcat 8.5.35
RUN tar xzf /tmp/apache-tomcat-8.5.35.tar.gz -C /opt/
RUN rm -f /tmp/apache-tomcat-8.5.35.tar.gz

# get jenkins 2.151
#RUN wget --no-verbose -O /opt/apache-tomcat-8.5.35/webapps/jenkins.war http://ftp-nyc.osuosl.org/pub/jenkins/war/2.151/jenkins.war
RUN wget --no-verbose -O /tmp/jenkins.war http://ftp-nyc.osuosl.org/pub/jenkins/war/2.205/jenkins.war
RUN cp /tmp/jenkins.war /opt/apache-tomcat-8.5.35/webapps/
RUN rm -f /tmp/jenkins.war

# install maven
RUN tar xzf /tmp/apache-maven-3.3.9-bin.tar.gz -C /opt/
RUN ln -s /opt/apache-maven-3.3.9 /opt/maven
RUN ln -s /opt/maven/bin/mvn /usr/local/bin
RUN rm -f /tmp/apache-maven-3.3.9-bin.tar.gz
ENV MAVEN_HOME /opt/maven

# create jenkins user
#RUN adduser jenkins
RUN useradd -ms /bin/bash jenkins

# set password for jenkins user and root user
#RUN echo "jenkins" | passwd --stdin jenkins
RUN echo jenkins:jenkins | chpasswd
RUN echo root:root | chpasswd

RUN echo "jenkins ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/jenkins && \
    chmod 0440 /etc/sudoers.d/jenkins


	
RUN chown -R jenkins:jenkins /opt/maven
RUN chown -R jenkins:jenkins /opt/apache-tomcat-8.5.35
#RUN chown -R jenkins:jenkins /home/jenkins/fcacode
#RUN chown -R jenkins:jenkins /home/jenkins/moparcode

# remove download archive files
# RUN apt-get clean

EXPOSE 8080

USER jenkins

RUN mkdir /home/jenkins/fcacode
RUN mkdir /home/jenkins/moparcode

CMD /opt/apache-tomcat-8.5.35/bin/catalina.sh run
#CMD ["su", "-", "jenkins", "-c", "/bin/bash"]

