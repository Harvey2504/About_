# Jenkins Insatllation as Docker Image
### General Image Pull
```
docker run -p 8080:8080 -p 50000:50000 jenkins
```
This will store the workspace in /var/jenkins_home. All Jenkins data lives in there - including plugins and configuration. You will probably want to make that a persistent volume (recommended):
```
docker run -p 8080:8080 -p 50000:50000 -v /your/home:/var/jenkins_home jenkins
```
This will store the jenkins data in /your/home on the host. Ensure that /your/home is accessible by the jenkins user in container (jenkins user - uid 1000) or use -u some_other_user parameter with docker run.

You can also use a volume container:
```
docker run --name myjenkins -p 8080:8080 -p 50000:50000 -v /var/jenkins_home jenkins
```
Then myjenkins container has the volume (please do read about docker volume handling to find out more).

Copy the passkey generated in cmd and use it during running Jenkins for the first time.

Set up the jenkins with name mail and password.

### Docker in Docker (For Jenkins) :Recommended
stop all docker images running on port 8080 or change the port number

1.Go to your cmd run this command
```

docker run -p 8080:8080 -v /var/run/docker.sock:/var/run/docker.sock --name jenkins jenkins/jenkins:lts

```
we will get an initial admin password do the jenkins installation steps 

After completion of jenkins installation

2.To enter into your docker image run this command
```
docker exec -it -u root jenkins bash

```

3.install docker in docker
```
apt-get update && \
apt-get -y install apt-transport-https \
     ca-certificates \
     curl \
     gnupg2 \
     software-properties-common && \
curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg > /tmp/dkey; apt-key add /tmp/dkey && \
add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") \
   $(lsb_release -cs) \
   stable" && \
apt-get update && \
apt-get -y install docker-ce
```


now you will be inside your docker and run '''docker ps''' you will get something like this
```
root@cf1fd5908a1c:/# docker ps
CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS                               NAMES
cf1fd5908a1c        jenkins/jenkins:lts   "/sbin/tini -- /usr/…"   2 hours ago         Up 12 minutes       0.0.0.0:8080->8080/tcp, 50000/tcp   jenkins
root@cf1fd5908a1c:/#   
```
this means you have succesfully created the image 

Now to give jenkins actions access to all users for that run this command
```
chmod 777 /var/run/docker.sock
```
run your jenkins pipeline
```
pipeline{
  agent {
    docker {
      image 'maven:3-alpine'
      args '-v /root/.m2:/root/.m2'
    }
  }
  stages {
    stage('Build'){
      steps{
        sh 'mvn -B -DskipTests clean package'
      }
     }
    stage('Test'){
      steps{
        sh 'mvn test'
      }
      
  }
  }
}
```
# Setting Up Jenkins on AWS Ubuntu Instance
#### Link :
https://www.jenkins.io/doc/tutorials/tutorial-for-installing-jenkins-on-AWS/

* Follow the steps here till "Download and Install Jenkins"

### Inside Ubuntu Instance Through Putty
* Install Java
* Link : https://www.jenkins.io/doc/book/installing/linux/
```
sudo apt update
sudo apt search openjdk (for selection)
sudo apt install openjdk-11-jdk
sudo apt install openjdk-11-jdk (for confirmation)
java -version

```

* Install Jenkins
* Link : https://www.jenkins.io/doc/book/installing/linux/
```

wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian binary/ > \
    /etc/apt/sources.list.d/jenkins.list'
sudo apt-get update
sudo apt-get install jenkins

```
```

cd ..
cd ..
sudo chmod 777 var/lib/jenkins/secrets/initialAdminPassword
cat initialAdminPassword

```
* Copy the Password
* Open ec2-13-58-37-51.us-east-2.compute.amazonaws.com:8080 on browser
* Then Install Jenkins and Add More Users If Required

#### Installing docker on Ubuntu Machines
https://docs.docker.com/engine/install/ubuntu/
```
After Installing :
chmod 777 /var/run/docker.sock
```
#### Installing docker compose on Ubuntu Machines
https://docs.docker.com/compose/install/
#### Installing Kubectl on Ubuntu Machines
https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/



### Installing Jenkins on Azure Ubuntu VM
Link : https://docs.microsoft.com/en-us/azure/developer/jenkins/configure-on-linux-vm
```
Rememnber to open the port as per documentation
```


# VS Code Integration for Pipeline Script

Install Jenkins Pipeline Linter Connector Plugin in VsCode

Configure As Follows :
Crumb URL
```
http://localhost:8080/crumbIssuer/api/xml?xpath=concat
```
Pass
```
Protagonist@2504
```
URL
```
http://localhost:8080/pipeline-model-converter/validate
```
User
```
Harvey25
```
Shortcut to validate a Jenkinsfile
```
Shift+Alt+V
```

# Basic Plugins Installation
Manage Jenkins > Manage Plugins

Install Basic plugins for :
*   Dark Theme
*   Email Ext
*   Git
*   Sonarqube
*   Artifactory
*   Amazon Web Services
*   SSH Plugins
*   Terraform
*   Docker
*   Kubernetes

# Creating New Users
```
Login into Jenkins.
Go to Manage Jenkins.
Go to Create Users.
Enter all the details – Username, Password, Confirm Pwd, FullName, Email.
Select Create User.
```

# Set Up Java and Maven Configs
### Configure Jenkins > Global Tools Config

#### JDK > JDK Installations

* Add JDK
* Give name as java11
* Check Install automatically Box
* Delete default installer
* Add Installer
* Chose Extract *zip/.tar.gz as installer
* Label : Leave it as blank
* Download URL
```
https://download.java.net/java/GA/jdk11/13/GPL/openjdk-11.0.1_linux-x64_bin.tar.gz
```
* Set Sub Directory Value as : jdk-11.0.1
* Save and Apply


#### Maven > Maven Installations

* Add Maven
* Give name as maven3
* Check Install automatically Box
* Install from Apache Version 3.6.3
* Save and Apply

```
pipeline {
    agent any
    tools{
            maven 'maven-3'
            jdk 'java11'
    }

    stages {
        stage('Git-Checkout') {
            steps {
                git 'https://github.com/Harvey2504/maven-samples.git'
            }
        }
        stage('Maven-Package') {
            steps {
                bat 'mvn package'
            }
        }
    }
}
```

# Sample Config : Mail Config
### Configure Jenkins > Config System
* GoTo E-mail Notification
```
SMTP Server : smtp.gmail.com

Check Use SMPT Authentication Box

UserName : samalatib96@gmail.com
Password : *******

Check Use SSL Box
SMTP port : 465
Reply-To-Address : noreply@gmail.com
Charset : UTF-8
```
* Test Config By Sending a Test Email.


## Throttle Build
Link : https://plugins.jenkins.io/throttle-concurrents/#documentation






