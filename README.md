# MY CI/CD DevOps Approach ( jenkins, github, docker, ssh, scp, IIS, Virtualbox )

This Docs explain the steps use to put in place my Devops CI/CD process for fast delivery of nodejs App (reactjs vite)

This project contains the following skills :
- System Administration
- Computer Network Administration
- SoftWare developement

## Stacks and Uses
**Jenkins** : The ultimate tool use for putting in place pipeline for the CI/CD of our application
**Docker** : The tool use to containerise and build our application
**Github** : The tool use for code versionnate
**VirtualBox** : The toll use to create our windows server machine to host the app
**SSh** : Secure Shel is the protocol use to securely tranfer our files from host to host
**SCP** : Secure Copy Protocol relies on ssh to securely transfer files from host to hosts
**IIS** : internet Information Service use to host web app on windows server

## Step 1
*Installation process of virtual machine of type Windows Server 2019*
1. install virtual box
2. install windows server iso from microsoft site
3. create a virtual machine with virtaul box and import the server image
4. configure the network to use same networks as yopur local machine
5. test the connection between the to host
6. If good connection, then we have a ready server to be use

## Step 2
*installation of docker desktop*
1. install docker desktop installer in the official docker site
2. install docker
3. then restart your host

## Step 3
*Installation of jenkins docker*
1. open docker in your host
2. open a cmd and pull the docker image
3. make sure the image contains a volum, an unused port, a docker socket to use docker commands
4. then log the jenkins container and get the password from your cmd
5. open yout jenkins running on a localhost:your_jenkins_port
6. then use the password provided in the cmd
7. then created your logging information for feature authentifications
8. then we have a ready jenkins server runing on docker

## Step 4
*creation of a git repository*
1. create a git repository and push your javascript project

## Step 5
*create jenkins credentials to connected to virtaul machine where it is going to host the app in our iis server*
1. create a new credential in the dashboard/credentials menu to be use to coonect to our vm
2. make sure you uses ssh kind
3. go to the jenkins conatainer on docker desktop then click on exc to acces the jenkins cmd
4. generte an ssh key
5. copy the ssh key private key and paste it in the input private key in the credential menu
6. enter your others informations then save the credebtial

## Step 6
*connect jenkins with vm host server*
1. make sure OpenSSH is install in your VM host if not, go to settings/apps/features then app a new openssh server
2. now check the installation of openssh with poweshell as admin with the cmd : ```Get-WindowsCapability-Online | Where-Object name-like 'OpenSSH*'```
3. if install, automatically run the ssh server with the cmd : ``` Set-Service -name sshd -Startuptype 'Automatic' ```



