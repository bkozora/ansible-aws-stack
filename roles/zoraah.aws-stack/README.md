# Zoraah.aws-stack Role


This role is responsible for all of the AWS intermingling and finesse, from VPC creation via CloudFormation template to configuration of all associated resources of said VPC in preparation of the EC2 instance launching into it to serve web traffic.

Requirements
------------

- boto
- boto3
- awscli

Role Variables
--------------

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



License
-------

BSD

Author Information
------------------

Bobby Kozora

https://github.com/bkozora

bobby@kozora.me
