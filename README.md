# AWS Provisioned nginx Web Stack Using Ansible and CloudFormation

The following Ansible playbook will deploy a self contained environment to run a simple nginx web server running on AWS. A brief rundown of some of the nuts and bolts:

* Uniquely dynamic to deal with automated generation and configuration of entire environment, VPC and all interworking components
* Based on a modified version of Ubuntu 17.04 Server that I installed Python 2.7 on (AMI made publicly available as ami-5d2e0e38)
* EC2 instance deployed in a public subnet, dual availability zone virtual private cloud created by a CloudFormation stack
* SSH access restricted via Security Group to user's detected IP address according to http://ifconfig.me/ip
* SSL Certificates not addressed for portability

__*Note: The CloudFormation VPC stack will persist between runs, however, a new EC2 instance will be created and provisioned each time the playbook is run. The old EC2 instance will remain in the VPC. Please be advised.*__


## Setup
If on a new system, we'll need to do some preliminary tasks. Assuming we have git, Python, pip and other obvious essentials like Ansible installed, let's get started...

At a terminal, enter the following:

```bash
# Clone our project
git clone https://github.com/bkozora/ansible-aws-web

# Make sure awscli and boto are installed
pip install awscli boto boto3

# Configure aws credentials
aws configure
```

Help with AWS configuration can be found here: http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html

## Usage

To launch the infrastructure, use the following command. Substitute the `--private-key`'s proceeding value with the path to your own private key **that you've generated from AWS**

```bash
$ ansible-playbook -i inventory/hosts --private-key=~/.ssh/mykey -u ubuntu -vv deploy.yml
```

Generating an SSH Key in AWS: http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#having-ec2-create-your-key-pair


## What's Next?

There are many possible enhancements to implement from here, this is just a starting point. For example, the VPC is much more full-featured than we're using it for. We should have an application ELB that spans multiple AZs, with multiple EC2 instances in each Availability Zone to make use of a high availability infrastructure design. We would also benefit from placing the nginx instances in private subnets and restricting their access to only allowing inbound communication from the ELB, not the internet as a whole. We'd then need to add in a NAT instance, gateway or managed service to allow outbound internet communication so the instances can retrieve software updates and other internet access related tasks. Another good security consideration would be placing a bastion host to ~intercept~ or more accurately tunnel all SSH communications into the infrastructure. All of the aforementioned steps are somewhat present in this playbook, albeit unfinished. 

## Requirements

- Ansible 2
- Boto 2
- Boto 3
- awscli
    

## Role Variables

* region: us-east-2
* ami_id: ami-5d2e0e38
* instance_type: t2.nano
* instance_name: simple-web-server
* volumes:
  - device_name: /dev/sda1
  - device_type: 
  - volume_size: 5
  - delete_on_termination: true
* keypair: us-east
* security_groups: ['simple-web-sg']

## Dependencies

I modified them slightly so they're included in my source code but shoutouts to Cloudonaut.io for the CloudFormation templates and Jeff Geerling (geerlingguy) for his nginx and security roles.

#### Ansible Roles
- https://galaxy.ansible.com/geerlingguy/nginx/
- https://galaxy.ansible.com/geerlingguy/security/

#### CloudFormation Templates from Cloudonaut.io

- http://templates.cloudonaut.io/en/stable/vpc/#vpc-with-private-and-public-subnets-in-two-availability-zones
- http://templates.cloudonaut.io/en/stable/vpc/#nat-gateway
- http://templates.cloudonaut.io/en/stable/vpc/#ssh-bastion-hostinstance



## Author Information

Bobby Kozora

https://github.com/bkozora

Bobby@Kozora.me