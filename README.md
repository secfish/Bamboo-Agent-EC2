# Ansible - Bamboo Agent AWS EC2

#### 1. Setup

```sh
// Install and config AWS Cli
aws config

// get key pair or create your key pair if there is no key pair for Bamboo
aws ec2 create-key-pair --key-name <YourKeyPairName>

// install ansible 

// verify "ansible_python_interpreter" value in host file based on your local python environment 

```

#### 2. Create  Bamboo Agent EC2 instance

```sh
// change vars in ec2-creation.yml based on your environment 

// create bamboo agent  
ansible-playbook -i hosts ec2-creation.yml
```

#### 3. Terminate Bamboo Agent EC2 instance

```
// verify vars in ec2-terminate.yml 

// terminate bamboo agent ec2 instance 
ansible-playbook -i hosts ec2-terminate.yml
```
