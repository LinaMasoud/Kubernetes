# Kubernetes
# Installation of Kubernetes using Kubeadm (One Master/Node) 
This is a playbook with a couple of roles and tasks to install and configure k8s using kubeadm

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing.

### Prerequisites & Steps to Run the playbook
1 Centos7 Machine For Ansible Controller 
1 Ubuntu Machine For k8s Server

Install Ansible on the Controller using the following commands:
```
sudo yum install epel-release
sudo yum install ansible
```
Setup Ansible ServiceAccount & SSH Configuration:

  * On the Controller & K8s Server:
  ```
  sudo -i
  mkdir /ansible_svc
  useradd ansible_svc -md /ansible_svc
  chown -R ansible_svc:ansible_svc /ansible_svc
  usermod --shell /bin/bash ansible_svc
  ```
  * On the Controller:
  ```
  su - ansible_svc
  ssh-keygen
  Copy the Public Key exist in .ssh/id_rsa.pub
  ```
  * On K8s Server:
  ```
  su - ansible_svc
  mkdir .ssh
  chmod 700 .ssh
  touch authorized_keys
  Paste the public key in authorized_keys
  chmod 644 authorized_keys
  ```
From The controller Clone the Playbook & Change Directory to the files
Run the k8s.yml playbook after updating hosts & group_var/all.yml files carefully using the following command:
```
su - ansible_svc 
ansible-playbook -i hosts k8s.yml
```
Verify the provisioning
```
kubectl get nodes
kubectl get pods --all-namespaces
```
To Reset The cluster Run the k8s.yml playbook after updating hosts & group_var/all.yml files carefully using the following command:
```
su - ansible_svc 
ansible-playbook -i hosts clean.yml
```
