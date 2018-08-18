Execute all below in Powershell Admin window

## Chocolatey and packages

1. Install Chocolately  
`Set-ExecutionPolicy Bypass -Scope Process -Force`  
`iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))`  
`exit`  

1. install packages    
`cinst -y googlechrome`  
`cinst -y steam -ignore-checksums`  
`cinst -y lastpass`  
`cinst -y lastpass-for-applications`  
`cinst -y lastpass-chrome`  

1. more packages  
`cinst -y vscode`  
`cinst -y github-desktop`  
`cinst -y terraform`  
`cinst -y packer`  
`cinst -y awscli`  
`cinst -y putty`  
`cinst -y git`  
`cinst -y cygwin`  
`cinst -y 7zip`  
`cinst -y notepadplusplus`  
`cinst -y winmerge`  
`cinst -y slack`  
`cinst -y poshgit`  
`cinst -y openssh -params '"/SSHAgentFeature"'`  
`cinst -y graphviz`  

1. Git setup    
`cd C:\Users\Don`   
`mkdir .ssh`    

## SSK Key, Git and Github 
`ssh-keygen -t rsa -b 4096 -C "donbecker@donbeckeronline.com"`   

confirm ssh-agent service running and return pid  
`WMIC Service WHERE "Name = 'ssh-agent'" GET ProcessId`  

add key to agent  
`ssh-add ./.ssh/id_rsa`  

add pub key to clipboard  
`Get-Content .\.ssh\id_rsa.pub | clip`  

add pub key to github profile  

set git config   
`git config --global user.name "donbecker"`  
`git config --global user.email "email"`  

list config to verify   
`git config --global --list`  


## AWS CLI setup
1. create aws key for iam account in aws  

1. create new profile (do not name this 'default')  
`aws configure --profile donbeckeradmin`  
(add AWS Access Key ID)  
(add AWS Secret Access Key)  
`Default region name: us-east-2`  
`Default output format: json`  

1. set the active profile  
`$env:AWS_PROFILE="donbeckeradmin"`  

1. verify active profile
`aws configure list`  


## VSCode setup

Open vscode
Extensions
Install: 
Terraform 1.3.4 (Mikael Olenfalk)
