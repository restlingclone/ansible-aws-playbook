# AWS Provisioning using Red Hat Ansible Automation Platform (AAP)

# 1. Overview

This project provisions AWS resources using Red Hat Ansible Automation Platform (AAP).

AWS services covered:

    Amazon ECR

    Amazon EC2

    Amazon Athena (S3 + Database + Table)


    Amazon DocumentDB (Subnet Group, Cluster, Instances)

Key design principles:

    No AWS credentials in playbooks

    AWS authentication handled via AAP Credentials

    Playbooks are idempotent

    Ready for enterprise automation

# 2. Prerequisites

    Red Hat Ansible Automation Platform 2.x

    AWS Account

    IAM User or Role with required permissions

    Git repository containing Ansible playbooks

    AWS region access enabled

# 3. Repository Structure

aws-aap-automation/

├── playbooks/

│   ├── ecr.yml

│   ├── ec2.yml

│   ├── athena.yml

│   ├── docdb.yml

│
├── group_vars/

│   └── all.yml

│
├── inventories/

│   └── localhost.yml

│
└── README.md

# 4. AWS IAM Permissions

Ensure the AWS IAM user or role has permissions for:

ECR

EC2

S3

Athena

DocumentDB

VPC (subnets, security groups)

# 5. Configure AWS Credential in AAP

Step 1: Create AWS Credential

1 Login to AAP Web Console

2 Navigate to: 
        Resources → Credentials → Add

3 Select:

    Credential Type: Amazon Web Services

    Fill:

    Access Key

    Secret Key

    Region

Save

✅ AWS credentials are now securely stored in AAP

# 6. Create Inventory

Step 1: Create Inventory

1 Go to:
        Resources → Inventories → Add → Inventory

2 Name: AWS-Localhost

Save

Step 2: Add Host

1 Open inventory → Hosts

2 Add host:

Name: localhost

Variables: "ansible_connection: local"

# 