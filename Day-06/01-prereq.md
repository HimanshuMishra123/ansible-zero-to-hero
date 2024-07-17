# Setup EC2 Collection and Authentication

## Install boto3

```
pip install boto3
```

## Install AWS Collection
goto Ansible Galaxy page>> Collections >> Search amazon (from aws)>> copy install command 
```
ansible-galaxy collection install amazon.aws
```

## Setup Vault 

Ansible comes with default vault and you don't have to install and configure unlike Terraform and jenkins.

1. Create a password for vault : Use the following command to generate a secure password and save it in a file named vault.pass

```
openssl rand -base64 2048 > vault.pass
```

The command(openssl rand -base64 2048) generates 2048 random bytes of data, encodes it in base64, and saves it to a file named vault.pass using openssl command line cryptographic tool.
But in real world you will not use openssl, org. will provide some other better encryption tool to generate password.

2. Create a encrypted Vault File and Add your AWS credentials using the below vault command:

```
ansible-vault create group_vars/all/pass.yml --vault-password-file vault.pass
```
Creates and Encrypts a File: It creates a new file named pass.yml and opens it in an editor. The file will be encrypted using the password specified in vault.pass when you save it(:wq!). <br/>

The command does not create a file inside the vault but rather creates and encrypts the pass.yml file using the vault's encryption mechanism. The file will be saved on your filesystem and will be readable only after decryption with the correct password.<br/>




