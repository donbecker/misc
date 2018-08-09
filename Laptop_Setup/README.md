1. Install Chocolately
PSA> Set-ExecutionPolicy Bypass -Scope Process -Force
PSA> iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
PSA> exit

2. install packages

PSA> cinst -y googlechrome
PSA> cinst -y steam -ignore-checksums


PSA> cinst -y lastpass
PSA> cinst -y lastpass-for-applications
PSA> cinst -y lastpass-chrome


3. more packages

PSA> cinst -y vscode
PSA> cinst -y github-desktop
PSA> cinst -y terraform
PSA> cinst -y packer
PSA> cinst -y awscli
PSA> cinst -y putty
PSA> cinst -y git
PSA> cinst -y cygwin
PSA> cinst -y 7zip
PSA> cinst -y notepadplusplus
PSA> cinst -y winmerge
PSA> cinst -y slack
PSA> cinst -y poshgit
PSA> cinst -y openssh -params '"/SSHAgentFeature"'


4. Git setup

PS> 
cd C:\Users\Don
mkdir .ssh

# gen key
ssh-keygen -t rsa -b 4096 -C "donbecker@donbeckeronline.com"

#confirm ssh-agent service running and return pid
WMIC Service WHERE "Name = 'ssh-agent'" GET ProcessId

#add key to agent
ssh-add ./.ssh/id_rsa

#add pub key to clipboard
Get-Content .\.ssh\id_rsa.pub | clip

#add pub key to github profile

#set git config 
git config --global user.name "donbecker"
git config --global user.email "email"

#list config to verify 
git config --global --list
