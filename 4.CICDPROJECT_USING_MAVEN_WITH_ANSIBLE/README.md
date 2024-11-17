# USED JENKINS AND CREATED JAR FILE USING  MAVEN AND DEPLOYED USING ANSIPLAYBOOK

In this project Used Three Servers. One for Jenkins Server to build jar file, Second is for ansible to Deploy to deploy the jar file from jenkins server to Production Server, Finally the Production server which running the old version of the app.

# Steps to Setup a Jenkins Server

1. Install Jenkins in a Linux Machine using the below Commands:

```
sudo apt update

sudo apt install openjdk-17-jre

java -version

curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt-get update
sudo apt-get install jenkins

```

2. Create The Pipeline project in Jenkins and Choose the Github as a SCM.

Maven Plugin needed for this, After install the Maven just Set it in the Global Configuration tools and call it in our groovy Script.

My pipeline Groovy Script Placed in the following Path: 3.CICDPROJECT_USING_MAVEN/spring-boot-app/JenkinsFile (Should be placed in the pox.xml folder)

3. Start the BUild. Once Build completed JAR File would be located in the Jenkins WorkSpace Folder and Path would be /var/lib/jenkins/workspace/<buildname>/spring-boot-app/target/


# Ansible Server Setup for Jar file Deployment

```
sudo apt update

sudo apt upgrade

sudo apt-get install ansible ansible-doc

ansible --version (To check the ansible version)
```

Enable the Password less authentication from ansible Server to Jenkins server as well as Production Server by using ssh-keygen command.

My ansible YAML Script in the Following Path, Please Copy and use it For Your project:  4.CICDPROJECT_USING_MAVEN_WITH_ANSIBLE/deploy_spring_app.yml

My hosts file sample Mentioned in the Following path: 4.CICDPROJECT_USING_MAVEN_WITH_ANSIBLE/inventoryfile

In this Project for testing I used the root user for enable the passwordless authentication, If you are working in production system use respective users.

I created 3 VM's in AWS and profermed the activity, So i mentioned those IP addresses in inventory, Replace it with your IPs.