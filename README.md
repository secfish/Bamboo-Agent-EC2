# Ansible - Bamboo Agent AWS EC2

#### 1. Setup

```sh
// Install and config AWS Cli
aws config

// get key pair or create your key pair if there is no key pair for Bamboo
aws ec2 create-key-pair --key-name <YourKeyPairName>

// install ansible 
pip3 install ansible 
pip3 install boto 

// change "ansible_python_interpreter" value in file:hosts based on your local python environment 

// if your subnet is private, change following items value in file: ./bamboo-agent-create/tasks/main.yml
//   - ec2_group/rules/cidr_ip: <yourPrivateIpRange>
//   - wait_for/host: "{{ item.private_ip }}" 
//   - add_host/host: "{{ item.public_ip }}" 
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
