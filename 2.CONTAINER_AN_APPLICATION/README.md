# Jenkins_SpringBoot_Project_ONLY BUILDING_JARFILE_USING_MAVEN


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

2. Create The Pipeline in a Jenkins and Choose the Github as a SCM.

Plugins: Maven Needed for this.

Jenkins Groovy Script: Placed in the following Path.

Once Maven Added Just set the Maven in the Global Configuration Section.

3. Intiate the Build.

Start the BUild. Once Build completed JAR File will be located in the following directory: target/spring-boot-web.jar

4. Check the Apllication Status.

Note: Both Jenkins and Spring Boot listens Same Port 8080, So we need to stop the Jenkins. (systemctl stop jenkins)

Then Run the JAR File: nohup java -jar target/spring-boot-web.jar &

5. Check On the Brwoser, You will get the Page.
