# Day 1

## Configuration Management Tool Overview
<pre>
- is used to automate system administration activities
- the assumptions we already a machine with some OS ( Unix, Linux, Windows or Mac )
- this tool is used to install/configure stuffs on a already provisioned machine
- examples
  - Chef
  - Puppet
  - Ansible
</pre>

## Puppet Overview
<pre>
- is the oldest configuration management tool
- it follows client/server architecture
- every configuration management supports a specific domain specific language (DSL) to automate stuffs 
- the DSL used by Puppet is Puppet language ( a proprietary declarative language )
- Puppet installation is very complex and time consuming
- learning curve is quite steep
- uses proprietary tools on the servers that needs to be managed by chef
- Puppet architecture is very complex
</pre>


## Chef Overview
<pre>
- is a configuration management tool
- it follows client/server architecture
- the domain specific language (DSL) - the automation language used by Chef
- the DSL used by Chef is Ruby ( scripting language )
- Chef installation is very complex and time consuming as Puppet
- Chef provides loads of tools, hence its very powerful and confusing
- learning curve is quite steep
- uses proprietary tools on the servers that needs to be managed by chef
- chef architecture is very complex
</pre>

## Ansible Overview
<pre>
- is a configuration management tool
- it is agent-less
- easy to learn
- easy to install
- follows simple architecture
- ansible nodes
  - these are servers we can perform automation using ansible 
  - dependent softwares
    - Unix/Linux/Mac Server
      - Python
      - SSH Server
    - Windows Server
      - Powershell
      - WinRM
- Ansible Controller Machine
  - the machine where ansible is installed is called Ansible Controller Machine(ACM)
  - it could a laptop/desktop
  - officially Ansible is only supported on Linux machines, but it works in Unix/Mac
  - Windows machine can't be used as a Ansible Controller Machine
  - Windows machine can be managed by Ansible
- Inventory
  - is a plain text file which follows an INI style format
  - captures connectivity details, IP address/hostname, username, password, ssh-key's etc
- comes in 3 flavours
  1. Ansible core - open source variant supports only command line
  2. AWX - supports webconsole, opensource, built on top of Ansible core
  3. Red Hat Ansible Tower - enterprise commercial product, built on top of AWX 
</pre>

## Ansible Core
<pre>
- this is developed in Python by Michael Deehan
- Michael Deehan is a former employee of Red Hat
- Michael Deehan founded a company called Ansible Inc and developed Ansible core as an open source product
- perfect alternate to Puppet/Chef
- supports only command line
- very well documented open source product
- agent less
- can be installed in Linux, Unix and Mac
- can manage Windows, Linux, Mac, etc., ansible nodes
- doesn't support role based access ( can't create different types of ansible users )
- doesn't historial logging mechanism
</pre>

## AWX
<pre>
- is developed on top of Ansible core
- supports webconsole but no command line
- it can be installed on a centralized server within your organization
- can be accessed from web browser only
- supports role based access control
- supports logs for each playbook execution
- you don't get any support
- can't develop ansible playbook, you can only run them
- which means we need ansible core to develop/write playbook
</pre>

## Red Hat Ansible Tower
<pre>
- it is developed on top of AWX
- functionally AWX and Ansible Tower(Ansible Automation Platform) are same
- you will world-wide support from Red Hat (an IBM company)
- which means we need ansible core to develop/write playbook
</pre>

## Ansible Modules
<pre>
- ansible supports many built-in ansible modules to automate
- for instance 
  - file module helps in creating files and folders with specific permissions
  - copy module helps in copying from/to ACM to ansible nodes and vice versa
  - all unix/linux/mac ansible modules are developed as Python scripts
  - all windows ansible modules are developed as Powershell scripts
  - we can also write out own custom ansible modules, when there is no readily available module to automate certain rare stuffs
</pre>

## Ansible Plugins
<pre>
- ansible plugins helps us extend the core functionality of ansible
- for instance
  - become plugin helps us perform certain tasks as sudo(administrative) users
</pre>

## Ansible Roles
<pre>
- is way we could follow best practices and ensure our automation code can be reused across many ansible playbooks
- ansible roles can't be executed directly, while they can be invoked via ansible playbooks
- ansible roles can be downloaded and installed via ansible-galaxy tool
- we could also develop our own ansible role
- For example
  - we could develop an ansible role to install Oracle Database in Windows 2016/2019 Server, Ubuntu Linux, etc
</pre>

## Ansible Collections
<pre>
- is a reusable code that has many different kinds of reusable code in ansible
- it could have one or more roles, custom modules, plugins, filters, etc.,
- it's a way we could package and distribute all the related playbooks, modules, plugins, etc in a single collection
</pre>


## Lab - Install git in Ubuntu
```
sudo apt update
sudo apt install -y git
```

Expected output
![image](https://github.com/user-attachments/assets/3146d933-d75a-45c4-9c44-7a2501cf4c34)


## Lab - Cloning the Training repository( one time activity )
```
cd ~
git clone https://github.com/tektutor/ansible-dec-2024.git
cd ansible-dec-2024
```

## Lab - Install Ansible Core in Ubuntu
```
sudo apt update
sudo apt install -y ansible-core
sudo apt install python3-pip -y
pip install "pywinrm>=0.3.0" --break-system-packages
```

Expected output
![image](https://github.com/user-attachments/assets/d053e30e-303e-49df-9663-5e3d0a60991a)

# Lab - Configuring windows nodes to allow ansible connectivity (KVM VM)
### We need to configure WinRM connectivity in the Windows Ansible Node

#### Windows Node Ansible Requirments	
- PowerShell 3.0 or latest
- .Net Framework 4.5 or latest

### Finding PowerShell version from Windows Powershell prompt
```
$PSVersionTable
```
Expected output
![image](https://github.com/tektutor/ansible-sep-2023/assets/12674043/a9722c67-f55f-482c-a4f1-1fa4beb95b1b)


### Finding .Net Framework Version

1. Open regedit

2. HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full

Expected output
![image](https://github.com/tektutor/ansible-sep-2023/assets/12674043/8d97cfef-0cc0-4d81-a303-1d7d1d6ca7ac)


### Configuring WinRM on Windows machine
Download the file at below URL and save it in C:/Users/Administrator/Downloads/ConfigureRemotingForAnsible.ps1
```
https://github.com/ansible/ansible-documentation/blob/devel/examples/scripts/ConfigureRemotingForAnsible.ps1
```

Now execute the below command
```
powershell.exe -ExecutionPolicy ByPass -File C:/Users/Administrator/Downloads/ConfigureRemotingForAnsible.ps1

winrm enumerate winrm/config/Listener
```

Expected output
![image](https://github.com/tektutor/ansible-sep-2023/assets/12674043/08821a8e-2b7b-47d7-8ab9-6b8581781ae1)


### Configuring Windows node with Basic authentication
```
Set-Item -Path WSMan:\localhost\Service\Auth\Basic -Value $true
```

### Verify if WinRM Listeners are running ( 2 listerners one for Http and other for Https expected )
```
winrm enumerate winrm/config/Listener
```

Expected output
![image](https://github.com/tektutor/ansible-sep-2023/assets/12674043/23e5d406-e105-4c12-8786-215df8af72a1)


### On the Ansible Controller machine (Ubuntu machine), make sure pywinrm is installed
```
pip install "pywinrm>=0.3.0"
```


## Lab - Install git in Ubuntu
![image](https://github.com/user-attachments/assets/3146d933-d75a-45c4-9c44-7a2501cf4c34)

## Lab - Install Ansible Core in Ubuntu ( on the Ansible Controller Machine )
```
sudo apt update
sudo apt install -y ansible-core
sudo apt install python3-pip -y
pip install "pywinrm>=0.3.0"
```

Expected output
![image](https://github.com/user-attachments/assets/d053e30e-303e-49df-9663-5e3d0a60991a)

## Lab - Pinging the windows machine using ansible ad-hoc command
You need to create an inventory file with the your windows machine credentials 
<pre>
[2016-servers]
152.67.176.35

[2019-server]
144.24.152.144

[windows]
144.24.152.144
152.67.176.35

[windows:vars]
ansible_connection=winrm
ansible_user=opc
ansible_password=Nagejmca**7752
ansible_winrm_server_cert_validation=ignore  
</pre>

```
cd ~/ansible-dec-2024
git pull
cd Day1/ansible
ansible -i inventory all -m win_ping
```

Expected output
![image](https://github.com/user-attachments/assets/94ddb166-9023-4dfc-922b-d08a1175b7e1)
