###jenkins-master######
 
# Starting off with the Jenkins base Image
FROM jenkins/jenkins:2.151 
MAINTAINER FCADEVOPS Team
 
#Running update command to make sure machine is updated with laest packages.
USER root
RUN yum update -y && \
clean all
 
#Given the jenkins user sudo privileges in order to be able to run Docker commands inside the container
RUN echo "jenkins ALL=NOPASSWD: ALL" >> /etc/sudoers
 
USER jenkins #Make sure all operations peformed with jenkins user
 
#Installing required jenkins plugins
COPY plugins.txt /usr/share/jenkins/plugins.txt
RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/plugins.txt
 
#Default jenkins port
EXPOSE 8080
