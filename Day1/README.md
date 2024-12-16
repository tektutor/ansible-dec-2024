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

## Chef Overview
<pre>
- is a configuration management tool
- it follows client/server architecture
- the domain specific language (DSL) - the automation language used by Chef
- the DSL used by Chef is Ruby ( scripting language )
- Chef installation is very complex and time consuming
- learning curve is quiet steep
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

## Lab - Install Ansible Core in Ubuntu
```
sudo apt update
sudo apt install -y ansible-core
sudo apt install python3-pip -y
pip install "pywinrm>=0.3.0"
```

Expected output
![image](https://github.com/user-attachments/assets/d053e30e-303e-49df-9663-5e3d0a60991a)
