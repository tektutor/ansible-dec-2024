# Day 2

## Info - Installing AWX in Ubuntu ( Please don't do this in our lab environment - this is just for your reference )

First, let's install minikube
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
minikube start  --vm-driver=docker --addons=ingress
```

Expected output
![image](https://github.com/user-attachments/assets/5425231d-7e57-4e6c-a46d-e233e3268c31)

Let's install kubectl 
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin
```

Check if minikube is installed properly
```
kubectl get nodes
```

Expected output
![image](https://github.com/user-attachments/assets/054ee1ed-a260-40fa-911f-f16d0f669db3)

Let's install AWX operator in minikube
```
sudo apt install make -y
cd ~
git clone https://github.com/ansible/awx-operator.git
cd awx-operator/
git checkout 2.19.0
export NAMESPACE=ansible-awx
make deploy

kubectl get pods -n ansible-awx
```

Expected output
![image](https://github.com/user-attachments/assets/3166b632-5cd9-43b7-846f-fe34669743fc)
![image](https://github.com/user-attachments/assets/3ee52ff0-7042-4649-8c32-c1ea0794db75)
![image](https://github.com/user-attachments/assets/1d396402-d35b-409d-96a1-5acacd1a026a)

Track the progress of awx installation
```
kubectl logs -f deployments/awx-operator-controller-manager -c awx-manager -n ansible-awx
```

Expected output
![image](https://github.com/user-attachments/assets/97929864-4d17-490f-8531-0955ac569bef)
![image](https://github.com/user-attachments/assets/0a849d40-309e-4199-b33c-03a09e4b32cc)
![image](https://github.com/user-attachments/assets/5ee43576-026f-4878-b2e7-264aa3f7cf1b)

Access the AWX dashboard
```
kubectl get nodes
kubectl get svc -n ansible-awx
minikube service awx-ubuntu-service --url -n ansible-awx
```
Expected output
![image](https://github.com/user-attachments/assets/908f2b16-aaa5-44b3-99b6-a4de9b9b6dc8)
![image](https://github.com/user-attachments/assets/0f2d6c6b-9958-427e-93b7-d8909af6bc37)

AWX Login Credentials
```
username - admin
```

To retrieve password
```
kubectl get secret -n ansible-awx | grep -i password
kubectl get secret awx-ubuntu-admin-password -o jsonpath="{.data.password}" -n ansible-awx | base64 --decode; echo
```

Expected output
![image](https://github.com/user-attachments/assets/fda1a4ac-c16f-4890-b70b-73fac5a7680b)
![image](https://github.com/user-attachments/assets/133d98f7-c315-45e4-a2af-16b2f2800bbf)

If everything went smooth, you are expected to see similar page
![image](https://github.com/user-attachments/assets/353bcbaa-e837-4d84-b851-69da778ffc82)

## Lab - Installing 7zip software utility via ansible playbook
```
cd ~/ansible-dec-2023
git pull
cd Day2/ansible
ansible-playbook -i inventory install-7zip-playbook.yml
```

Expected output
![image](https://github.com/user-attachments/assets/583ff4b1-f224-40ff-ba1f-1db43651f4fc)
![image](https://github.com/user-attachments/assets/f9cff51d-ccda-4b56-8d0f-d2dfe09c0523)

## Lab - Ansible vault
<pre>
- ansible-vault is a tool that can be used to create a text file with sensitive data  
- the data stored in the file will be encrypted by ansible-vault tool with AES 256 bit algorithm
- playbook can retrieve the sensitive data at runtime from the vault protected file securely
- this helps us avoid hard-coding sensitive information like login credentials, certs, etc
</pre>

Let's store some credentials in a vault encrypted file as shown below, when prompts for password I gave rps@123 as the password
```
ansible-vault create machine-credentials.yml
ls -l machine-credentials.yml
cat machine-credentials.yml
```

Let's view the machine-credentials.yml data
```
ansible-vault view machine-credentials.yml
```

Let's edit the machine-credentials.yml
```
ansible-vault edit machine-credentials.yml
```

Let's encrypt an existing file
```
cat vault-playbook.yml
ansible-vault encrypt vault-playbook.yml
cat vault-playbook.yml
```

Let's view the data from the encrypted playbook
```
ansible-vault view vault-playbook.yml
```

Let's decrypt the playbook
```
ansible-vault decrypt vault-playbook.yml
```

Expected output
![image](https://github.com/user-attachments/assets/4c66bdbd-ef01-44db-97f4-f0cff98404f4)
![image](https://github.com/user-attachments/assets/e8c986d8-1c93-45fb-9aa6-c20e02aca9f7)
![image](https://github.com/user-attachments/assets/1065a6f5-0246-46c8-86a5-15bd5fa4ac3a)
![image](https://github.com/user-attachments/assets/ae6d8531-1790-4849-b522-ca3f255f9586)
![image](https://github.com/user-attachments/assets/f3df58b8-6662-4f54-9038-5d8cef6a4bb3)

## Lab - Refactoring the inventory to move the login credentials to a vault protected file
```
cat inventory
ansible view machine-credentials.yml
ansible windows -m win_ping --ask-vault-pass
```

Expected output
![image](https://github.com/user-attachments/assets/74bed6c8-db53-4535-bd3d-f8105c73b638)
![image](https://github.com/user-attachments/assets/ece6c634-c58b-4036-8dec-ddbfdf0bf00d)

## Lab - Retrieving machine credentials from vault protected file and using it in playbook
My vault password is rps@123

```
cat inventory
ansible-vault view machine-credentials.yml
cat vault-playbook.yml
cat ansible.cfg
ansible-playbook vault-playbook.yml --ask-vault-pass
```

Expected output
![image](https://github.com/user-attachments/assets/940662f1-d2b2-4dc5-9395-239d6fba2ebf)
![image](https://github.com/user-attachments/assets/b16af7cf-27af-4427-8f34-b905699c549e)
![image](https://github.com/user-attachments/assets/712b9558-4c1a-4222-a664-cddeded24a48)

## Lab - Invoking win_ping ansible module via ad-hoc command without hard coded login credentials
```
cat ansible.cfg
cat inventory
cat ~/.my-vault-password
ansible windows -m win_ping -e @machine-credentials.yml
```

Expected output
![image](https://github.com/user-attachments/assets/43345098-5cc9-4f67-8589-00f074416cbb)

## Lab - Creating an Inventory in Ansible Automation Platform
![image](https://github.com/user-attachments/assets/03e7f4c4-85d2-41d9-9260-ca6f94163972)
![image](https://github.com/user-attachments/assets/51901122-d710-499e-b962-b5bf0c97b075)
![image](https://github.com/user-attachments/assets/3e53d503-07a3-4b32-a439-54090d00f421)
![image](https://github.com/user-attachments/assets/3d4dce65-4868-4683-9630-cda9c8644ad4)


## Lab - Creating a project in Ansible Automation Platform ( formerly Ansible Tower )
![image](https://github.com/user-attachments/assets/11b3c5fd-e207-42f0-b340-ef2e2ca30562)
![image](https://github.com/user-attachments/assets/c0a09771-20e8-48fb-ac3d-7f4b33d4a649)
![image](https://github.com/user-attachments/assets/debb4e89-d83d-4259-aac7-37f532ab8cba)

## Lab - Creating a Job Template to invoke an ansible playbook
![image](https://github.com/user-attachments/assets/764cd200-1f38-4f56-84b4-6e6067a61fce)
![image](https://github.com/user-attachments/assets/4661bcc2-7b19-4238-90b0-2e795ab075a3)
![image](https://github.com/user-attachments/assets/e268a2e4-13ff-47c7-9936-de4560b5b5a6)
![image](https://github.com/user-attachments/assets/b099acd2-1843-459f-afa7-651ab25fc2b4)
![image](https://github.com/user-attachments/assets/1362b71e-d762-44e4-850e-2ca2ae50afd1)

## Lab - Rebooting windows node and waiting until becomes ready for further configuration management
```
cd ~/ansible-dec-2024
git pull
cd Day2/ansible
ansible-playbook wait-for-windows-machine-ready-state-playbook.yml
```

Expected output
![image](https://github.com/user-attachments/assets/553479a5-a710-4f68-ba99-5ea62c5d5151)
![image](https://github.com/user-attachments/assets/cbfef43c-d858-4f61-b9ba-a49754d857e6)

## Lab - Enable firewall in all profiles
```
cd ~/ansible-dec-2024
git pull
cd Day2/ansible
ansible-playbook enable-firewall-playbook.yml
```

## Lab - Disable firewall in all profiles
```
cd ~/ansible-dec-2024
git pull
cd Day2/ansible
ansible-playbook disable-firewall-playbook.yml
```

## Lab - Add firewall rule to allow ICMP ping
```
cd ~/ansible-dec-2024
git pull
cd Day2/ansible
ansible-playbook allow-icmp-ping-playbook.yml
```

## Lab - Remove firewall rule to deny ICMP ping
```
cd ~/ansible-dec-2024
git pull
cd Day2/ansible
ansible-playbook deny-icmp-ping-playbook.yml
```

