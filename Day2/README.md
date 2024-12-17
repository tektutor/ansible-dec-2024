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

