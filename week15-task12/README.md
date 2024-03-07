# 12 Packer -> Ansible -> CloudFormation/Terraform

## Task 

Resources must be created within the `eu-central-1` region.

For task 12 you need to do following:

*   \[PACKER\] - Create Custom AMI image from Amazon Linux 3 AMI image where you will have needed yum repos installed and enabled to install nginx web server and mysql database.
*   \[IaC - CloudFormation\] Using an AMI image from step 1 create 2 new EC2 instances called `task-12-web-server-cf` and `task-12-db-server-cf`. For those instances create appropriate security groups and open needed ports. Please try to follow best practices for security groups. You can put your resources inside default VPC and public subnets.
*   \[IaC - Terraform\] Using an AMI image from step 1 create 2 new EC2 instances called `task-12-web-server-tf` and `task-12-db-server-tf`. For those instances create appropriate security groups and open needed ports. Please try to follow best practices for security groups. You can put your resources inside default VPC and public subnets.
*   \[Ansible\] By using ansible provisioner install nginx web server on `task-12-web-server-cf` and `task-12-web-server-tf` instances. By using ansible provisioner install mysql database on `task-12-db-server-cf` and `task-12-db-server-tf` instances.

You need to ensure that once when nginx is installed that it is enabled and started. Same goes for mysql database. Also your nginx web server instances needs to have index.html file with content `Hello from nginx web server created using CloudFormation and Ansible` `Hello from nginx web server created using Terrafrom and Ansible`. Mysql database needs to have database called `task-12-db` and user `task-12-user` with password `task-12-password` and all privileges on `task-12-db` database. For the instance `task-12-web-server-tf`

*   \[Ansible\] By using Ansible task verify connection via default mysql database port from `task-12-web-server-cf` and `task-12-web-server-tf` instances to `task-12-db-server-cf` and `task-12-db-server-tf` instances. For the connection verification you can use telnet tool.

Goal of this task is to create two paralel infrastructures, one using CloudFormation and one using Terrafrom. With 2 EC2 instances as Web and Database servers.

Please pay attention to tagging strategy and naming convention. For example your web server instance should have following tags:  
Name: task-12-web-server-cf  
CreatedBy: IAM User name  
Project: task-12  
IaC: CloudFormation

## Packer

### Installing Packer on Ubuntu

https://developer.hashicorp.com/packer/tutorials/docker-get-started/get-started-install-cli#installing-packer

Add the HashiCorp [GPG key](https://apt.releases.hashicorp.com/gpg "HashiCorp GPG key").
```bash
$ curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
```

Add the official HashiCorp Linux repository.
```bash
$ sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
```

Update and install.
```bash
$ sudo apt-get update && sudo apt-get install packer
```

### Verify the installation

```bash
$ packer --version
Packer v1.10.1
```

### Installation and configuration

Install Amazon plugin:
```bash
$ packer plugins install github.com/hashicorp/amazon
```

Install AWS CLI on Linux:
```bash
$ curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
$ unzip awscliv2.zip
$ sudo ./aws/install
$ aws --version
aws-cli/2.15.25 Python/3.11.8 Linux/6.5.0-1014-aws exe/x86_64.ubuntu.22 prompt/off
```

Configure the profile:
```bash
$ aws configure --profile adin
AWS Access Key ID [None]: AKIAWXLK35W7G2JSVZCV
AWS Secret Access Key [None]: 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
Default region name [None]: eu-central-1
Default output format [None]: json
```

### Create packer.json file

Task: Create Custom AMI image from Amazon Linux 3 AMI image where you will have needed yum repos installed and enabled to install nginx web server and mysql database.

We need to develop scripts to enable and install YUM repositories, facilitating subsequent installations of the Nginx web server and MySQL database:
- [packer_install_nginx.sh](./packer_install_nginx.sh)
- [packer_install_mysql.sh](./packer_install_mysql.sh)

We need to create [`packer.json`](./packer.json) file, and then execute the command:
```bash
$ packer build packer.json
```

<details>
<summary>Screenshots</summary>

![packer1](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/b2cf5769-cc28-4861-98bd-08867a647445)
![packer2](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/dace8df6-99fe-4782-aa1e-167fc46dc340)
</details>

## CloudFormation

We need to create the template [`cloudformation.yaml`](./cloudformation.yaml).

Execute the command:
```bash
$ aws cloudformation create-stack --stack-name task12-cf --template-body file://cloudformation.yaml
{
    "StackId": "arn:aws:cloudformation:eu-central-1:462469983678:stack/task12-cf/1ae4a670-daf1-11ee-94b0-020aa4f89a6d"
}
```

Verify:
```bash
$ aws cloudformation describe-stacks --stack-name task12-cf
{
    "Stacks": [
        {
            "StackId": "arn:aws:cloudformation:eu-central-1:462469983678:stack/task12-cf/1ae4a670-daf1-11ee-94b0-020aa4f89a6d",
            "StackName": "task12-cf",
            "Description": "Setup environment with EC2 instances using CF",
            "Parameters": [
                {
                    "ParameterKey": "KeyName",
                    "ParameterValue": "task12"
                }
            ],
            "CreationTime": "2024-03-05T13:05:56.883000+00:00",
            "RollbackConfiguration": {},
            "StackStatus": "CREATE_COMPLETE",
            "DisableRollback": false,
            "NotificationARNs": [],
            "Outputs": [
                {
                    "OutputKey": "WebInstancePublicIP",
                    "OutputValue": "3.71.49.249",
                    "Description": "Web Instance Public IP adress"
                },
                {
                    "OutputKey": "DbInstancePublicIP",
                    "OutputValue": "3.123.36.234",
                    "Description": "DB Instance Public IP adress"
                },
                {
                    "OutputKey": "StackName",
                    "OutputValue": "task12-cf",
                    "Description": "CF Stack Name"
                }
            ],
            "Tags": [],
            "EnableTerminationProtection": false,
            "DriftInformation": {
                "StackDriftStatus": "NOT_CHECKED"
            }
        }
    ]
}
```

After all the tasks, we need to clean the resources:
```bash
$ aws cloudformation delete-stack --stack-name task12-cf
```

<details>
<summary>Screenshots</summary>

![cf](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/b4f2c50c-a622-434b-a686-4ac0eb8e9b4d)
</details>

## Terraform

### Installation on Ubuntu

Ensure that your system is up to date and you have installed the `gnupg`, `software-properties-common`, and `curl` packages installed. You will use these packages to verify HashiCorp's GPG signature and install HashiCorp's Debian package repository.
```shell-session
$ sudo apt-get update && sudo apt-get install -y gnupg software-properties-common
```

Install the HashiCorp [GPG key](https://apt.releases.hashicorp.com/gpg "HashiCorp GPG key").
```shell-session
$ wget -O- https://apt.releases.hashicorp.com/gpg | \
gpg --dearmor | \
sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg > /dev/null
```

Verify the key's fingerprint.
```shell-session
$ gpg --no-default-keyring \
--keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg \
--fingerprint
```

Add the official HashiCorp repository to your system. The `lsb_release -cs` command finds the distribution release codename for your current system, such as `buster`, `groovy`, or `sid`.
```shell-session
$ echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
sudo tee /etc/apt/sources.list.d/hashicorp.list
```

Download the package information from HashiCorp.
```shell-session
$ sudo apt update
```

Install Terraform from the new repository.
```shell-session
$ sudo apt-get install terraform
```

Check the version:
```
$ terraform --version
Terraform v1.7.4
on linux_amd64
```

### Create .tf files

We need to create a [`providers.tf`](./providers.tf) file wherein we define the region, paths to credentials, and the profile name.

Then we create the [`main.tf`](./main.tf) file to specify the resources to be created.

```bash
$ terraform init #initializes a Terraform working directory, downloading necessary plugins
$ terraform plan #dry run, showing proposed changes without applying them
$ terraform apply #applies changes described in the execution plan, modifying infrastructure accordingly
$ terraform destroy #destroys all resources managed by Terraform, cleaning up the infrastructure
```

<details>
<summary>Screenshots</summary>

![tf1](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/41baf2c0-9d69-46bc-8213-53d5213e85a4)
![tf2](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/3ffddc7d-33b6-46db-8dee-a41f11af3caa)
</details>


## Ansible

### Install on Ubuntu

```bash
$ sudo apt update
$ sudo apt install software-properties-common
$ sudo add-apt-repository --yes --update ppa:ansible/ansible
$ sudo apt install ansible
$ ansible --version
ansible [core 2.16.4]
```

### Task

Create [`ansible.cfg`](./ansible/ansible.cfg) file:
```bash
ansible-config init --disabled -t all > ansible.cfg
```

Create [`prod.ini`](./ansible/prod.ini) file.

Create playbook [`1-nginx-install-playbook.yml`](./ansible/1-nginx-install-playbook.yml) and execute command `$ ansible-playbook -i prod.ini 1-nginx-install-playbook.yml`.

<details>
<summary>Screenshots</summary>

![ansi1](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/b7f101f6-d7b5-4d47-b911-dc06cf8eec16)
![ansi2](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/5e47164e-be8c-4bcf-ba5d-8c8825930ad2)
![ansi3](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/1e2ec03c-b595-4910-808e-174735f31ab0)
</details>

Create the playbook [`2-mysql-install-playbook.ym`](./ansible/2-mysql-install-playbook.yml) and execute command `$ ansible-playbook -i prod.ini 2-mysql-install-playbook.yml`.

Connect to the db-server ec2 and check the temporary password with `$ sudo grep 'temporary password' /var/log/mysqld.log` then execute `$ sudo mysql_secure_installation`. We need to add `bind-address = 0.0.0.0` into `my.cnf`, then restart database with `$ service mysqld restart`. Connect to mysql with `$ mysql -h localhost -u root -p` and update the privileges with `UPDATE mysql.user SET host='%' WHERE user='root';` and `FLUSH PRIVILEGES;`.

To create the database, execute `$ ansible-playbook -i prod.ini 3-mysql-configure-playbook.yml`. 

<details>
<summary>Screenshots</summary>

![ansi4](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/3a6fede1-9f0e-4e5b-b79e-eab9a70d613c)
![ansi5](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/804b8ba2-6a95-4fec-bb00-0b99233b5c48)
</details>
