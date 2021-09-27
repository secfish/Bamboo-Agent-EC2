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

if you only want to create/terminate  Bamboo Agent EC2 one instance without autoscaling, do: 

```sh
// create bamboo agent with default parameters in var.yml 
ansible-playbook -i hosts ec2-creation.yml

// terminate bamboo agent ec2 instance 
ansible-playbook -i hosts ec2-terminate.yml
```

## 3. Create/Terminate Bamboo Agent EC2 Autoscaling group 

There are two options for you to create  Bamboo Agent EC2 autoscaling group

### 3.1 Option 1

This option automatically generate new ami and autoscaling group, it takes longer time because it does not use existed ami.

```
// create new Bamboo Agent AMI and new Bamboo Agent ec2 autoscaling group 
// following command first delete all existed Bamboo Agent autoscaling group with tag: Bamboo-Agent-Asg
ansible-playbook -i hosts ec2-asg-creation.yml 

// if you do not want to use bamboo agent autoscaling group and ami anymore, you can do:

// delete all existed Bamboo agent ec2 autoscaling group 
ansible-playbook -i hosts ec2-asg-terminate.yml

// delete all bamboo agent ami
ansible-playbook -i hosts ec2-ami-terminate-all.yml 
```

### 3.2 Option 2

This option generate autoscaling group based on existed ami object, it takes shorter time but need more steps.

```
// create bamboo agent ec2 autoscaling group based on existed ami 
// 1. create new bamboo agent ami if bamboo agent ami is existed 
      ansible-playbook -i hosts ec2-ami-creation.yml

// 2. modify "aws_ec2_ami_name" in file: var.yml based on step 1 or your existed ami name
      aws_ec2_ami_name: <yourExistedBambooAgentAmiName> 

// 3. create new bamboo agent autoscaling group by ami 
      ansible-playbook -i hosts ec2-asg-creation-by-ami.yml

// if you do not want to use bamboo agent autoscaling group and ami anymore, you can do:

// delete all existed Bamboo agent ec2 autoscaling group 
ansible-playbook -i hosts ec2-asg-terminate.yml

// delete all bamboo agent ami 
ansible-playbook -i hosts ec2-ami-terminate-all.yml 

```

