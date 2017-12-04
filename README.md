level27tech.aws_vpc
=============

This role allows you to create VPCs in Amazon Web Services (AWS) following a number of different design patterns. It supports combinations of single or multi AZ, single or multi subnet.
It will automatically create:

* the internet gateway for public subnets with associate route
* NAT gateways for private subnets with associate route
* security groups to allow SSH to these subnets from a given CIDR address/block
* network ACLs for public and private subnets to allow SSH

Optionally, it will create a private DNS hosted zone attached to the VPC.
If DNS is created then a DHCP option set is associated with the VPC for the purposes of setting the domain_name and dns_servers.  The AmazonProvidedDNS reserved word is used to specify Amazon DNS servers in this case and this cannot be changed.
All objects, where possible will include a Name tag and a chosen Project tag.
The terms "public" and "private" are used within this role as follows:

* Public objects relate to the internet facing subnet(s) and have assign public IP address set to true
* Private objects relate to the non-internet facing subnet(s)

**Note that this role only works within a single AWS region.**

**Note that the following parameters of the ec2_vpc_net module are not supported by this role.**
- dns_hostnames
- dns_support
- ec2_url
- multi_ok
- profile
- security_token
- tenancy
- validate_certs

Requirements
-------------------

In order to use this role you need an AWS account and an AWS user with appropriate permissions. 
To make things easier an AWS policy document is included with this role.

>AnsibleVPCPolicy.json

As this role interfaces with the AWS API, the boto3 Python module is also required.
One way to install boto is to use pip:
> pip install boto

Role Variables
--------------------

The role includes the following defaults:

**AWS Credentials**
*Note that these credentials should be kept private as they can be used to gain access to your AWS environment.
It is recommended that these variables be encrypted with Ansible Vault.  See https://docs.ansible.com/ansible/2.4/ansible-vault.html for details.*
aws_vpc_aws_access_key: "THISISMYAWSACCESSKEY"
aws_vpc_aws_secret_key: "ThisIsMyAwSSecretKey"

**AWS Region**
aws_vpc_aws_region:     "eu-west-1"

**Control Parameters**
aws_vpc_include_private: False
aws_vpc_multi_az: False
aws_vpc_create_dns_zone: False
aws_vpc_dns_domain: "example.com"

**Security**
*Note that this default should be overridden as it is used in the security groups intended for SSH access.*
aws_vpc_cidr_for_access:          "1.2.3.4/32"

**Tags**
aws_vpc_tag_project: "My Project"

**VPC Information**
aws_vpc_vpc_name:       "MyVPC"
aws_vpc_vpc_cidr_block: "10.0.0.0/21"

**Subnets**
aws_vpc_public_subnet_1_name:  "sub_public_a"
aws_vpc_public_subnet_1_cidr:  "10.0.0.0/24"
aws_vpc_private_subnet_1_name:  "sub_private_a"
aws_vpc_private_subnet_1_cidr: "10.0.1.0/24"
aws_vpc_public_subnet_2_name:  "sub_public_b"
aws_vpc_public_subnet_2_cidr:  "10.0.4.0/24"
aws_vpc_private_subnet_2_name:  "sub_private_b"
aws_vpc_private_subnet_2_cidr: "10.0.5.0/24"

The role includes no variables (vars).
The role does not depend on any global variables or variables from other roles.

Dependencies
-------------------

This role does not depend on any other Ansible roles.

Example Playbook
-------------------------

Here is an example of how you might use the role.  In this example, the default region is overriden as is the CIDR address used in the security group for access.
Given the defaults above, this example would yield a VPC in a single AZ us-east-1a, with a single internet facing subnet "sub_public_a" with a CIDR of 10.0.0.0/24.  
No DNS would be created.

    - hosts: localhost
      connection: local
      gather_facts: False
      tasks:
      - name: Create VPC
        include_role:
          level27tech.aws_vpc
        vars:
          aws_vpc_aws_access_key: "AVIBI4QDEWFZOC2T3K4A"
          aws_vpc_aws_secret_key: "Br9t-/gEN+OYfrbDmr7f63NyliCPIrDvTdcTUMHf"
          aws_vpc_region: "us-east-1"
          aws_vpc_cidr_for_access: "192.168.0.1/32"
          

License
----------

GPLv3.0

Author Information
---------------------------

Name: Dean Harris
Organisation: Level 27 Technology Ltd

