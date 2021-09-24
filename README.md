# Ansible - Bamboo Agent AWS EC2

## 1. Setup

```sh
// Install and config AWS Cli
aws config

// get key pair or create your key pair if there is no key pair for Bamboo
aws ec2 create-key-pair --key-name <YourKeyPairName>

// install ansible 
pip3 install ansible 
pip3 install boto 

// verify "ansible_python_interpreter" value in file:hosts based on your local python environment 

// modify variables in file: var.yml based on your environment and requirement

```

## 2. Create/Terminate  Bamboo Agent EC2 single instance

if you only want to create/terminate one Bamboo Agent EC2 single instance without autoscaling, do: 

```sh
// verify parameters in file var.yml based on your environment 

// create bamboo agent  
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
// 1. create new bamboo agent ami if you want 
      ansible-playbook -i hosts ec2-ami-creation.yml

// 2. get existed bamboo agent ami name from aws console, then
      set aws_ec2_ami_name = <yourExistedBambooAgentAmiName> in file var.yml

// 3. create new bamboo agent autoscaling group based on existed ami
      ansible-playbook -i hosts ec2-asg-creation-by-ami.yml

// if you do not want to use bamboo agent autoscaling group and ami anymore, you can do:
// delete all existed Bamboo agent ec2 autoscaling group 
ansible-playbook -i hosts ec2-asg-terminate.yml
// delete all bamboo agent ami 
ansible-playbook -i hosts ec2-ami-terminate-all.yml 

```

