#### WEBSITES :

#### Azure Portal :
portal.azure.com

#### DevOps Portal :
dev.azure.com


# Resource Groups in Azure
All similar kinds of resources are mapped under a single RG.

Helps in easy identification and deletion

* Search Resource Group on Azure Portal
* + New
* Name : DemoRG
* Location : US EAST
* Review and Create
* Create  


# Creating a VM in Azure
* Virtual Machines > Add +
* Assign a RG
* Give it a name
* Choose Location (USEAST2) , No redundancy ,Ubuntu Version , Size 
* Select Password Option
* Give Username and Password  (Login credentials)
* Allow Selected Port : Also choose HTTP(80) and HTTPS(443) along with SSH(22)
* Review and Create 
* Create




# Azure App Services
* Search App Services on Azure Portal
* + New
* Choose RG
* Select an unique name for webapp name
* Choose Code for .jar/.war files or else Docker Containers
* Runtime stack : JAVA 8
* Java Web Server Stack : Java SE (Emb)
* OS : Windows (for free tier)
* Region: EAST US 2 (comparatively less cost)
* App Service Plan : Default
* Review and create
* Create
* Click on browse button on top left 
* Default web page by microsoft will open.


# CI Pipeline
* Pipelines > Pipelines > New Pipeline
* Select Use Classical Editor from below
* Select a source : Azure Repos Git / Github
* Select a template : Search for Maven 
* Save $ queue


### Maven Task
* In Pipeline Tab
```
Select the pom.xml file location from drop down
```
* Get Sources Tab
```
Leave it as default
```
* In AgentJob Tab
```
Check for Demands if you are running on Self Hosted Agent
```
* In Maven Tab
```
Goal(s) : package or clean package or clean package -DskipTests
Integrated JUnit Tests Publish to Azure Pipelines
Code Coverage : Cobertura
```
* In CopyFilesTo Tab
```
Source : $(system.defaultworkingdirectory)
Target : $(build.artifactstagingdirectory)
Leave it as default
```
* In PublishArtifact:Drop Tab
```
Source : $(build.artifactstagingdirectory)
Target : drop/AzurePipelines
Leave it as default
```
### WhiteSource Bolt Task
#### LINK :
https://whitesource.atlassian.net/wiki/spaces/WD/pages/1641644045/WhiteSource+Bolt+for+Azure+Pipelines
* On Agent Job Tab click + Add button
* Search for WhiteSource Bolt
* Add it to task list
* For the first time
* If it asks for Active Directory Permission then
```
Go to Organization Settings > Azure Active Dir
Connect with default directory
Sign Out and Sign In again
Create an WhiteSource Essentials Account
``` 

### Docker Task (Build or Push Images)
```
Container Registry : Give the docker service connection token
Container Repo : harvey2504/AzImage
Command : Build and Push
Select the path of Dockerfile from menu
Rest as Default
```
* Once Pipeline is completed head over to katacoda playground and pull the image 
```
$ docker run harvey2504/imagename:tag 
```

# Service Connection
## For App Services
#### LINKS :
https://www.youtube.com/watch?v=o9oeMZjrZp4


* Project Settings > Service Connections > New Service Connection > Azure Resource Manager > Service Principal(Auto)
* Choose the ResourceGroup(RG) from the dropdown menu
* Give the name of Service Connection same as RG
* Save

## For Docker
* Project Settings > Service Connections > New Service Connection > Docker Registry
* Registry Type : Docker Hub
* Put your Docker Hub ID
* Put your Docker Hub Password
* Click on verify to check connection
* Service Name : DockerToken



# CD Pipeline

* Pipelines > Releases > New Pipeline
* Add Artifacts > Select the CI Build pipeline to access
* Give Specific Version or latest as per requirements
* Enable Continous Deployment Trigger 
* Edit Stage Name in Stages of Release
* Click on 1job,0task
* At Production Level unlink all subscriptions (as we will connect through Service Connection)
* Select Deploy azure app services template
* On Agent Tab select your agent configs
* On Deploy Azure App Service Tab 
* Select Conn Type Azure Resource Manager (Default)
* In Azure Sub Choose the Service Connection Token
* Web App on Windows (As we created a specific windows stack in app service on azure)
* Give path of .jar / .war in Package/Folder Space
Note : dont leave it as default **/*.zip
* Save
* Then create a release.


# Self Hosted Agent
#### LINKS : 
https://www.youtube.com/watch?v=a1tWj3ytVSQ
https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/v2-windows?view=azure-devops


* GoTo Org Settings > Agent pool > Add pool
* Select New
* Select Self Hosted
* Agent pool Name : MyAgent
* Grant Access Permission to all pipelines

* Open Agent pool > New Agent
* Follow documentation for Windows/Linux


NOTE : If you already have an Agent pool and its not showing for a specific project then select Existing Option


## Setting up ubuntu machines

#### For Demands
```
sudo apt-get update
sudo apt-get install default-jdk
sudo apt-get install maven
maven -v/mvn -v
```
#### For Connection
```
wget https://vstsagentpackage.azureedge.net/agent/2.184.2/vsts-agent-linux-x64-2.184.2.tar.gz
ls
mkdir myagent && cd myagent
tar zxvf ../vsts-agent-linux-x64-2.184.2.tar.gz (REMOVE Download Path as in Documentation)
ls
./config.sh
./run.sh
```
#### For Re-Listening or Re-Config
```
cd myagent
./run.sh

./config remove
```
## Setting up windows machines
Download the file and make sure its present in downloads folder
```
PS C:\> mkdir agent ; cd agent

PS C:\agent> Add-Type -AssemblyName System.IO.Compression.FileSystem ; [System.IO.Compression.ZipFile]::ExtractToDirectory("$HOME\Downloads\vsts-agent-win-x64-2.184.2.zip", "$PWD")

PS C:\agent> .\config.cmd

PS C:\agent> .\run.cmd
```


### NOTE :
#### Server URL
(https://dev.azure.com/{your_organization})
#### Generating PAT (Personal Access Token)
* User Settings > Personal Acess Token
* New Token
* Give Name of Token 
* Select Full Access
* Copy the generated PAT and save in a txt file
* Upon demand give it during config stage of setting up host




## Connect Azure and Az DevOps without same Ids
#### Part01:
* Subscription > FreeTrail > Access control (IAM) > 
* Click on Add Role Assignments
* Select a role : Owner
* Assign Access : User,Group or service principal
* Search for email address you want to add
* Save
* Accept the invitation through that mail

#### Part02:
* Azure Active Dir > Manage Tab > Users 
* Click on the user you added
* Open Assigned Roles
* Add Assignment +
* Search for global administrator in drop down and add




## Deploying into aws beanstalk
### STEP01 (Setup)
* Open AWS Dashboard
* Search for Elastic Beanstalk
* Click on Create a new application
* Application Name : PleaseDeploy
* Click on Create

* Create a new Environment
* Select Web Server App
* Leave everything as default
* Select : Managed Platform
* Platform : Java
* Platform branch : Java 8 running on Amazon Linux
* Platform version : Default
* Select Application Code : Sample Application
* Create Environment

#### Wait for 5 mins till everything is up and running
EC2 , S3 bucket , Security Groups , etc will be created once the step is finished .

(This is to match the port that env is listening to with the port that application is running on)

* Open the environment
* On left tab select configuration
* Edit Software tab
* Environment Properties
```
Name     Value
PORT     8080
```

* Open IAM 
* Open the user which which will be used in az Devops Session Connection
* Add Permission : Attach existing policies directly
```
AmazonEC2FullAccess
AmazonS3FullAccess
AWSElasticBeanstalkCustomPlatformforEC2Role
AdministratorAccess-AWSElasticBeanstalk
```
* Open EC2 (Maybe Optional)
* Click on the instace > Open IAM Role
```
AWSElasticBeanstalkWebTier
AWSElasticBeanstalkMulticontainerDocker
AWSElasticBeanstalkWorkerTier
AdministratorAccess-AWSElasticBeanstalk
```

### STEP02(Az DevOps)
* Install aws toolkit
* Link : https://marketplace.visualstudio.com/items?itemName=AmazonWebServices.aws-vsts-tools
* Open the project
* Project : https://github.com/Harvey2504/gs-spring-boot.git
* Create a service connection in project settings
```
New Service Connection > AWS
Give your IAM Credentials for user
Access Key ID : **********
Secret Access Key : *************
Service connection name : give token name
```
* Open the pipeline
* Select YAML Type
* Select Maven Task
```
- task: Maven@3
  inputs:
    mavenPomFile: 'app/pom.xml'
    mavenOptions: '-Xmx3072m'
    javaHomeOption: 'JDKVersion'
    jdkVersionOption: '1.8'
    jdkArchitectureOption: 'x64'
    publishJUnitResults: true
    testResultsFiles: '**/surefire-reports/TEST-*.xml'
    goals: 'package -DskipTests'
```
* Add Copy Task
```
- task: CopyFiles@2
  inputs:
    SourceFolder: '$(system.defaultworkingdirectory)'
    Contents: 'app/target/*.jar'
    TargetFolder: '$(build.artifactstagingdirectory)'
```
* Add Publish artifact task
```
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(Build.ArtifactStagingDirectory)'
    ArtifactName: 'drop'
    publishLocation: 'Container'
```
* Add Amazon S3 upload

Select the name of bucket that was created by beanstalk

Filename Patterns : choose what to copy to S3
```
- task: S3Upload@1
  inputs:
    awsCredentials: 'aws_token'
    regionName: 'us-east-2'
    bucketName: 'elasticbeanstalk-us-east-2-256424498744'
    sourceFolder: '$(Build.ArtifactStagingDirectory)'
    globExpressions: '**/target/*.jar'
```
* Select AWS Elastic Beanstalk Deploy Application

deploymentBundleType : ExistingDepBundleS3
deploymentBundleBucket : bucket name
deploymentBundleKey : path of object to deploy
```
- task: BeanstalkDeployApplication@1
  inputs:
    awsCredentials: 'aws_token'
    regionName: 'us-east-2'
    applicationName: 'LetsDeploy'
    environmentName: 'Letsdeploy-env'
    applicationType: 's3'
    deploymentBundleBucket: 'elasticbeanstalk-us-east-2-256424498744'
    deploymentBundleKey: 'app/target/gs-spring-boot-0.1.0.jar'
```
* Run the pipeline

#### Link : https://youtu.be/NfsecKpfGjc
#### Link : https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/tutorials.html
#### Note : Check that you are selecting out same location for all resources like beanstalk , s3 , etc.



## Some Reference Links :

Azure Artifacts
https://docs.microsoft.com/en-us/learn/modules/manage-build-dependencies/

Release Cadence
https://docs.microsoft.com/en-us/learn/modules/manage-release-cadence/

Maven Java CI/CD
https://www.youtube.com/watch?v=QXIxKZINJV4

ASP.Net App CI/CD
https://www.youtube.com/watch?v=MoFAR_6KDFo

Angular CI/CD
https://www.youtube.com/watch?v=NFqrWsUPCAM

Angular (Docker)
https://www.youtube.com/watch?v=K4uNl6JA7g8


