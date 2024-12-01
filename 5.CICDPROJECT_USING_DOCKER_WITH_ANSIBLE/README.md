# USED JENKINS TO CREATE A DOCKER IMAGE, STORED IT IN DOCKER HUB, AND DEPLOYED THE IMAGE USING AN ANSIBLE PLAYBOOK.

In this project Used Three Servers. One for Jenkins Server to build docker image and store it into the dockerhub, Second is for ansible server to Deploy to docker image from Docker hub to Production Server, Finally the Production server which running the old version of the app.

Note: People Always use the Jenkins master and Slave concept for Splitting workload. In this project we used
docker as an agent.

Advantages if we use docker as an agent: Will encounters cost for slave servers even developers don't commit any codes for a week.  

# 1. Steps to Setup a Jenkins Server along with Docker Engine:

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

2. Install Docker in the Same Jenkins Machine:

```
apt update

sudo apt install apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings docker-archive-keyring.gpg

echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update

sudo apt install docker-ce docker-ce-cli containerd.io

usermod -aG docker jenkins

usermod -aG docker ubuntu

systemctl restart docker

systemctl status docker

Optional : To check the docker status: docker run hello-world

```

2. Create The Pipeline project in Jenkins and Choose the Github as a SCM.

Maven & Docker Pipeline Plugins are needed for this, After install the Maven just Set it in the Global Configuration tools and call it in our groovy Script. 

Note: Name Should be same in the Groovy script and Global configuration tools page.

Add the Docker Hub Credentials in the credentials manager page's global Option. (ID name should be same in both credentials page and groovy script. In my case it is "docker-hub-credentials")

My pipeline Groovy Script Placed in the following Path: 5.CICDPROJECT_USING_DOCKER_WITH_ANSIBLE/spring-boot-app/JenkinsFile (Should be placed in the pox.xml folder)

My Docker build file also in the same path to build the Docker Image: 5.CICDPROJECT_USING_DOCKER_WITH_ANSIBLE/spring-boot-app/Dockerfile

Note: Before Start build, Just restart it. We made many changes: http://<ec2-instance-public-ip>:8080/restart

3. Start the BUild. Once Build completed Docker image will be Stored in my Docker hub repository.


# 2. Ansible Server Setup For Docker Image deployment:

```
sudo apt update

sudo apt upgrade

sudo apt update

sudo apt install -y apt-transport-https ca-certificates curl software-properties-common

sudo apt install -y software-properties-common

sudo apt-get install ansible

ansible-galaxy collection install community.docker

ansible-galaxy collection list

ansible --version (To check the ansible version)
```

Enable the Password less authentication from ansible Server to Production Docker Server by using ssh-keygen command.

My ansible YAML Script in the Following Path in server (/etc/ansible/playbooks/appdeploy.yml), Please Copy and use it For Your project from Github:  5.CICDPROJECT_USING_DOCKER_WITH_ANSIBLE/deploy_spring_app.yml

My hosts file also Mentioned in the Following path: 5.CICDPROJECT_USING_DOCKER_WITH_ANSIBLE/inventoryfile

My ansible.cfg file mentioned in the following path: 5.CICDPROJECT_USING_DOCKER_WITH_ANSIBLE/ansible.cfg

We need to Store the Docker Hub credentials in the encrypted format in the same path where our playbook is present.
In my case(/etc/ansible/playbook/docker_credentials.yml)

```
ansible-vault create docker_credentials.yml

```

Password file Format should like below:

docker_user: "your_docker_username"

docker_password: "your_docker_password"

In this Project for testing I used the root user for enable the passwordless authentication, If you are working in production system use respective users.

I created 3 VM's in AWS and proformed the activity, So i mentioned those IP addresses in inventory, Replace it with your IPs.

# 3. Production Docker Server Setup For Docker Image deployment:

Already production server Running with Docker with Old version(Installation Steps are same like Jenkins Server)

If we want to deploy docker image from ansible server to Production server remotely we must require the docker plugin in the production Docker Server. Steps mentioned below.

```
sudo apt install python3-pip
sudo pip3 install docker
```


