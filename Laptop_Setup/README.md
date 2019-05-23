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
`cinst -y graphviz`
`exit`  

## SSK Key, Git and Github 
(tested on Windows 10)

determine if Windows OpenSSH is installed
`Get-Command ssh*exe`

if any exes show Source as "C:\Windows\System32\OpenSSH\.."
`Remove-WindowsCapability -Online -Name "OpenSSH.Client~~~~0.0.1.0"`
`Remove-WindowsCapability -Online -Name "OpenSSH.Server~~~~0.0.1.0"`

verify we fixed the exes
`Get-Command ssh*exe`

stop and remove ssh agent windows service
`Get-Service ssh-agent | Stop-Service`
`sc.exe delete ssh-agent`

install openssh portable
`cinst -y openssh -params '"/SSHAgentFeature"'`  

verify exes are coming from "C:\Program Files\OpenSSH-Win64"
`Get-Command ssh*exe`

configure git client to use new exes
`git config --global core.sshCommand "'C:\Program Files\OpenSSH-Win64\ssh.exe'"`

create ssh key
`cd C:\Users\Don`   
`mkdir .ssh` 
`ssh-keygen -t rsa -b 4096 -C "donbecker@donbeckeronline.com"`   
* note that we must specify the full path if not using the default name

confirm ssh-agent service running and return pid  
`Get-Service ssh-agent`

add key to agent  
`ssh-add ./.ssh/id_rsa`  

confirm key added to agent
`ssh-add -l`

set git config   
`git config --global user.name "donbecker"`  
`git config --global user.email "donbecker@donbeckeronline.com"`  

list config to verify   
`git config --global --list`  

## GitHub setup

add pub key to clipboard  
`Get-Content .\.ssh\id_rsa.pub | clip`  

add pub key to github profile  
* Upper right profile picture -> Settings -> SSH and GPG tab
* Give name and paste public key into text box

test github
`ssh -T git@github.com`
* accept the fingerprint
* should say "successfully authenticated"




## AWS CLI setup
1. create aws key for iam account in aws  

1. create new profile (do not name this 'default')  
`aws configure --profile donbeckeradmin`  
(add AWS Access Key ID)  
(add AWS Secret Access Key)  
`Default region name: us-east-2`  
`Default output format: json`  

1. set the active profile and close Powershell window
`[System.Environment]::SetEnvironmentVariable('AWS_PROFILE', 'DonBeckerAdmin', [System.EnvironmentVariableTarget]::User)`  

1. verify active profile
`aws configure list`  


## VSCode setup

Open vscode
Extensions
Install:   
Terraform 1.3.4 (Mikael Olenfalk)

## Go dev setup

1. install go  
`cinst -y golang`  
`exit`  

1. verify  
`go version`

1. Go development uses the concept of a `workspace` with three subfolders
`bin` for command executables
`pkg` for packages
`src` for source, typically subfolders under this per repo

1. Create Go workspace  
`mkdir C:\Users\Don\code\Go-ws`  
`mkdir C:\Users\Don\code\Go-ws\bin`  
`mkdir C:\Users\Don\code\Go-ws\pkg`  
`mkdir C:\Users\Don\code\Go-ws\src`  

1. Set Sys env var  
`[System.Environment]::SetEnvironmentVariable('GOPATH', 'C:\Users\Don\code\Go-ws', [System.EnvironmentVariableTarget]::User)`  
`exit`

1. Verify env var
`$env:GOPATH`

1. Verify with a hello world  
`go get github.com/golang/example/hello`  
`cd $env:GOPATH\bin`  
`.\hello.exe`  
