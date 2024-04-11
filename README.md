[![Provision EC2 with Ansible](https://github.com/EzioDEVio/ansible-ec2-tutorial/actions/workflows/ansible-ec2.yml/badge.svg)](https://github.com/EzioDEVio/ansible-ec2-tutorial/actions/workflows/ansible-ec2.yml)  [![Ansible Lint](https://github.com/EzioDEVio/ansible-ec2-tutorial/actions/workflows/ansible-lint.yml/badge.svg)](https://github.com/EzioDEVio/ansible-ec2-tutorial/actions/workflows/ansible-lint.yml)  

# ansible-ec2-tutorial



## Project Overview

This project uses Ansible to automate the provisioning of an AWS EC2 instance and the setup of a security group. It demonstrates the use of Ansible playbooks, roles, and Ansible Vault for secure management of sensitive variables.

## Prerequisites

- AWS Account
- Ansible installed on your machine
- AWS CLI installed and configured
- SSH key pair created in AWS (referred to as `ansible-keypair` in this guide)

## Files and Directory Structure

- `site.yml`: The main playbook that orchestrates the provisioning process.
- `inventory/hosts.yml`: Inventory file specifying the target host.
- `group_vars/all.yml`: Contains non-sensitive variable definitions.
- `secret.yml`: Encrypted file containing sensitive variables like `ec2_vpc_id` and `ec2_subnet_id`.
- `roles/ec2-setup`: Role containing tasks to create a security group and provision EC2 instances.

## Setting Up

1. **Clone the repository:**

   ```
   git clone <repository-url>
   cd ansible-ec2-tutorial
   ```

2. **Configure AWS CLI:**

   Make sure you have AWS CLI installed and configured:

   ```
   aws configure
   ```

3. **Create an Ansible Vault:**

   To securely store sensitive information like VPC and subnet IDs:

   ```
   ansible-vault create secret.yml
   ```

   Enter the sensitive data:

   ```yaml
   ec2_vpc_id: vpc-xxxxxxx
   ec2_subnet_id: subnet-xxxxxxx
   ```

4. **Update `group_vars/all.yml`:**

   Remove sensitive data and ensure it only contains non-sensitive information:

   ```yaml
   ec2_keypair: ansible-keypair
   ec2_instance_type: t2.micro
   ec2_ami: ami-051f8a213df8bc089
   ec2_region: us-east-1
   ec2_security_group_name: ansible-tutorial-sg
   http_port: 80
   ```

5. **Running the Playbook:**

   Execute the main playbook:

   ```
   ansible-playbook -i inventory/hosts.yml site.yml --ask-vault-pass
   ```

   Enter the vault password when prompted.

## Understanding the Playbook

- The `site.yml` playbook references the `ec2-setup` role, which includes tasks to create a security group and provision an EC2 instance using variables defined in `group_vars/all.yml` and `secret.yml`.
- The `ec2_group` task creates a security group allowing all inbound traffic on specified ports.
- The `ec2_instance` task provisions an EC2 instance with the specified configuration and tags.

## Conclusion

This project provides a foundational understanding of using Ansible for AWS resource management while emphasizing best practices for secure variable management with Ansible Vault. For detailed Ansible documentation, visit [Ansible Documentation](https://docs.ansible.com/).
