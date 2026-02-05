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

# 7. Create Project

Step 1: Create Project

    1. Go to: Resources → Projects → Add

    2. Select:

        Source Control Type: Git

    3. Provide:

        Git repository URL

        Branch (e.g. main)
    
    4. Save and Sync

# 8. Playbook Variables (group_vars)

Create group_vars/all.yml:

ecr:
  name: demo-ecr
  region: ap-south-1

ec2:
  region: ap-south-1
  ami_id: ami-0abcd1234
  instance_type: t3.micro
  key_pair: my-key
  subnet_id: subnet-xxxx
  security_group: sg-xxxx
  name: demo-ec2

resource_provisioning:
  - region: ap-south-1
    athena_bucket: demo-athena-results
    database_name: demo_db
    table_name: dasda_logs

docdb:
  resource_provisioning:
    cluster_id: demo-docdb
    region: ap-south-1
    engine_version: "5.0.0"
    master_username: admin
    master_password: Password123!
    port: 27017
    subnet_ids:
      - subnet-aaa
      - subnet-bbb
    security_group_ids:
      - sg-aaa
    instance_class: db.r5.large
    instance_count: 2
    tags:
      Environment: Dev
      Owner: AAP

# 9. Create Job Templates (One per Playbook)

Example: EC2 Job Template

    1. Navigate to:
        Resources → Templates → Add → Job Template
    
    2. Fill:

        Name: AWS – EC2 Provision

        Inventory: AWS-Localhost

        Project: AWS Automation Project

        Playbook: playbooks/ec2.yml

        Credentials:

            AWS Credential (created earlier)

    3. Save

## Repeat for Other Playbooks

| Playbook     | Job Template Name          |
| ------------ | -------------------------- |
| `ecr.yml`    | AWS – ECR Provision        |
| `athena.yml` | AWS – Athena Provision     |
| `docdb.yml`  | AWS – DocumentDB Provision |


# 10. Launching the Job

    1. Go to:
            Resources → Templates

    2. Select desired job template

    3. Click Launch

    4. Monitor execution via Jobs → Output

# 11. Logs & Troubleshooting

    View live logs in Job Output

    AWS API failures will appear clearly in output

    Validate resources via AWS Console