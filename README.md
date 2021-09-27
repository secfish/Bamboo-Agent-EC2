# Ansible - Bamboo Agent AWS EC2

## 1. Setup

```sh
// Install and config AWS Cli
aws config

// get key pair or create your key pair if there is no key pair for Bamboo Agent SSH
aws ec2 create-key-pair --key-name <YourKeyPairName>

// modify keypair parameters in file var.yml 
aws_keypair_name: YourKeypairName
aws_keypair_keyfile: <YourKeypairPrivateFile>

// install ansible 
pip3 install ansible 
pip3 install boto 

// verify "ansible_python_interpreter" value in file:hosts based on your local python environment 

```

## 2. Create/Terminate  Bamboo Agent EC2 single instance

if you  want to create   Bamboo Agent EC2 single  instance , do: 

```sh
// create bamboo agent with default name "Bamboo-Agent-Test" 
ansible-playbook -i hosts ec2-creation.yml

// terminate bamboo agent ec2 instance with default name "Bamboo-Agent-Test" 
ansible-playbook -i hosts ec2-terminate.yml
```

## 3. Create/Terminate Bamboo Agent EC2 Autoscaling group 

### 3.1 Create

There are two options for you to create  Bamboo Agent EC2 autoscaling group. 

```
// verify all parameters in var.yml file based on your environment

// +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
// Option 1: execute playbook: ec2-asg-creation.yml
// automatically create new ami/autoscaling group, it takes longer time because it does not use existed ami
// first create ami and create autoscaling group 
ansible-playbook -i hosts ec2-asg-creation.yml 

// +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
// Option2: execute playbook: ec2-asg-creation-by-ami.yml
// generate autoscaling group based on existed ami object, it takes shorter time but need 3 steps
// 1. create new bamboo agent ami if bamboo agent ami is existed 
      ansible-playbook -i hosts ec2-ami-creation.yml
      
// 2. modify "aws_ec2_ami_name" in file: var.yml based on step 1 or your existed ami name
      aws_ec2_ami_name: <yourExistedBambooAgentAmiName> 
      
// 3. create new bamboo agent autoscaling group by ami 
      ansible-playbook -i hosts ec2-asg-creation-by-ami.yml
```

### 3.2 Terminate 

```
// if you do not want to use bamboo agent autoscaling group and ami anymore, you can do:

// delete all existed Bamboo agent ec2 autoscaling group 
ansible-playbook -i hosts ec2-asg-terminate.yml

// delete all bamboo agent ami
ansible-playbook -i hosts ec2-ami-terminate-all.yml 
```

