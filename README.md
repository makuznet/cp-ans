# Confluent Platform: cp-ansible: confluent-community
> Terraform rolls out an internal DNS zone and four VPSes in the Yandex.Cloud, runs Ansible that configures ansible VPS a Kafka cluster is installed from.

## Kafka cluster
Cluster is installed with default var settings.  
See `ansible/files/roles/ans/templates/hosts.yml.j2` to get a clue what cluster components are assigned to what VPSes.

## Usage
### ssh to the first Kafka broker
```bash
cd $HOME/cp-ans
ansible-vault decrypt ansible/files/id_rsa* --vault-password-file=ansible/.vault_pass
ssh -o "IdentitiesOnly=yes" -i ansible/files/id_rsa one.makuznet.pub.IP -l theuser
```
### Create Kafka topic
```bash
kafka-topics --create --topic quickstart-events --bootstrap-server localhost:9092
```
### List the created topic
```bash
kafka-topics --describe --topic quickstart-events --bootstrap-server localhost:9092
```
### Write events into the topic
```bash
kafka-console-producer --topic quickstart-events --bootstrap-server localhost:9092
```
### Read the events
```bash
kafka-console-producer --topic quickstart-events --bootstrap-server localhost:9092
```

## Installation
### Clone this repo
```bash
git clone https://github.com/makuznet/cp-ans
```

### Get your credentials
#### Get your ssh key and Yandex.Cloud credentials
See `terraform-ya/t02.auto.tfvars.sample` template file to understand what credentials you need to proceed.
- [Yandex.Cloud: Getting started with Terraform](https://cloud.yandex.com/en-ru/docs/solutions/infrastructure-management/terraform-quickstart)  

### Yandex 
#### OAuth token
[Yandex.OAuth](https://oauth.yandex.com)

#### Yandex CLI / MacOS
```bash
curl https://storage.yandexcloud.net/yandexcloud-yc/install.sh | bash
brew install bash-completion
echo 'source /Users/makuznet/yandex-cloud/completion.zsh.inc' >>  ~/.zshrc
source "/Users/makuznet/.bash_profile"
yc init # provide your yandex token
yc config profile get <your_profile_name> 
```
### Terraform / MacOS
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install terraform
```   

### Ansible / MacOS
```bash
https://www.python.org/ftp/python/3.9.5/python-3.9.5-macosx10.9.pkg
python get-pip.py
pip install ansible
```
#### Roles
- ans —  configure ansible.makuznet host to rollout a Kafka cluster using hostnames;  
- theuser — configure all the VPSes to let ansible.makuznet ssh to them;  

### Rollout your virtual empire 
```shell
cd terraform-ya
terraform init
terraform apply --auto-approve
```
### ssh to the ansible.makuznet
```bash
cd cp-ans
ansible-vault decrypt ansible/files/id_rsa* --vault-password-file=ansible/.vault_pass
ssh -o "IdentitiesOnly=yes" -i ansible/files/id_rsa ansible.makuznet.pub.IP -l theuser
``` 
### Intall the Kafka cluster
```bash
ansible-playbook -i hosts.yml all.yml
```

## Extra
### See CentOS release
```bash
cat /etc/centos-release
```

## Also see
- [Confluent Platform Ansible Installer](https://www.confluent.io/installer)    
- [Kafka intro: my notes](https://docs.google.com/document/d/164obgYypwvWCwDaK01-S1c5Abu0lqAgJ2WvUKymfxKI/edit?usp=sharing)  
- [Confluent: Configure Ansible Playbooks for Confluent Platform](https://docs.confluent.io/ansible/current/ansible-configure.html)  
 - [Ansible: Directory Layout](https://docs.ansible.com/ansible/2.8/user_guide/playbooks_best_practices.html#directory-layout)  
 - [GitHub: cp-ansible repo](https://github.com/confluentinc/cp-ansible)  
 - [Confluent: Migrate to Confluent Server](https://docs.confluent.io/platform/current/installation/migrate-confluent-server.html#migrate-to-cs)
- [Podcast: Installing Apache Kafka with Ansible ft. Viktor Gamov](https://podcasts.apple.com/us/podcast/installing-apache-kafka-ansible-ft-viktor-gamov-justin/id1401509765?i=1000457117179)  

## License
Follow all involved parties licenses terms and conditions.