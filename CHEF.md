#### CHEF DOCS: https://docs.chef.io/

## Setting Up Chef Server
* Open an account on CHEF Manage
* This will be your hosted server foe CHEF
* Link: https://manage.opscode.com

## Creating a WorkStation
* Provision an ubuntu vm on aws/azure 
* Open Link: https://docs.chef.io/workstation/install_workstation/
* Head over to Linux Docs Part
* Copy those two commands for Debian/Ubuntu one by one and run on vm
* Run These Commands
```
sudo apt update

wget https://packages.chef.io/files/stable/chef-workstation/21.2.278/ubuntu/20.04/chef-workstation_21.2.278-1_amd64.deb

dpkg -i chef-workstation_21.2.278-1_amd64.deb

```
* Verify Installation
```
chef -v
```

## Connecting workstation with CHEF Server
* Head over to CHEF Manage Server
* Select the Adminstration Tab
* On Actions select 'Starter Kit'
* Download the starter kit on local system
* UnZip that File
* Then copy the unzipped File into the workstation
* Make sure you have the .pem file in the same folder from where u want to copy a file
```
scp -r -i "awskeypair.pem" chef-repo/ ubuntu@ec2-3-12-36-226.us-east-2.compute.amazonaws.com:~/^
```
* Copy the .pem file to that folder and make sure the path to be pasted in is correct
```
scp -r -i "awskeypair.pem" awskeypair.pem ubuntu@ec2-3-12-36-226.us-east-2.compute.amazonaws.com:~/chef-repo
```
* This will help to communicate workstation and server


## Bootstrap a node
* Create two node vms on aws/azure and tag them as Node1 and node2
* Remember to add the inbound access for HTTP 80 in the sec groups of both nodes
```
knife bootstrap 3.143.255.10 --ssh-user ubuntu --sudo --ssh-identity-file awskeypair.pem -N DevNode

knife node list
```
* Head over to CHEF Server 
* Click on Nodes Tab
* You will be able to see the node list
* Link: https://docs.chef.io/install_bootstrap/ (Not So Imp)


## Creating a Sample CookBook (From Workstation)
* Either create a cookbook inside 'cookbooks folder' or create a new directory and create cookbooks inside them
* Run the command to create cookbook inside chef-repo/cookbooks
```
chef generate cookbook lamp_stack
```
* Sample Scripts
```
package 'apache2'  do
  action :install
end
file '/var/www/html/index.html' do
  content '<html>This is a placeholder for the home page.</html>'
  action :create
end
service 'apache2' do
  action [:enable,:start]
end
```
```
package 'nginx' do
 action :install
end
service 'nginx' do
 action [:enable,:start]
end
```

#### Chef Cookbook Recipe Tutorial for beginners
https://www.youtube.com/watch?v=wY6xg7CI5Xw&list=PLsgnv1SN76IJIiBg0e1lAIIAW1xZXPHF1&index=4


## Uploading a CookBook (From WorkStation)
* For Syntax Check of ruby script
```
chef exec ruby -c recipes/default.rb
```
* Command to push CookBook (Always work from chef-repo folder)
* If cookbooks are present inside default folder 'cookbooks'
```
knife cookbook upload -a

OR

knife cookbook upload simplecookbook

```
* If cookbooks are present in user created folders then specifying path is mandatory for uploading
```
knife cookbook upload --cookbook-path ~/chef-repo/democook/ -a

OR

knife cookbook upload --cookbook-path ~/chef-repo/democook/ simplecookbook

```
* Add the cookbook to the Node's Run List
```
knife node run_list add Node1 recipe[<cookbook name>::<ruby file name inside recipe>]

Ex:
knife node run_list add Node1 recipe[simplecookbook::default]

```
* Once added then all future pushes of cookbooks will automatically update in the run list of the node.
* Note: You dont have to specifically run this commandn and again




