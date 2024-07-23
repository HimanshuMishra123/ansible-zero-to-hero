## Detailed Notes on Ansible Variables

### Introduction
Variables in Ansible allow you to write more flexible and reusable playbooks. Hardcoding values can lead to limitations and rigidity in your playbooks. By utilizing variables, you can share your playbooks with others who might have different requirements, such as different instance types or operating systems.

### Declaring Variables
Variables in Ansible can be declared in multiple places, with each location having a different precedence. This flexibility can sometimes lead to complexity. The most common places to declare variables include:

1. **Role Defaults**
2. **Role Vars**
3. **Extra Vars**
4. **Playbook Level**
5. **Group Vars**

### Role Defaults
Variables can be declared in the `defaults/main.yml` file within a role. These variables have the lowest precedence. For example:
```yaml
# defaults/main.yml
type: t2.micro
```
To use this variable in a playbook:
```yaml
# tasks/main.yml
- name: Launch EC2 instance
  ec2:
    instance_type: "{{ type }}"
```
This uses Jinja2 templating to reference the variable.

### Role Vars
Variables can also be declared in the `vars/main.yml` file within a role. These variables have a higher precedence than defaults:
```yaml
# vars/main.yml
type: t2.medium
```
If the same variable is declared in both `defaults/main.yml` and `vars/main.yml`, the value in `vars/main.yml` will take precedence.

### Extra Vars
Extra Vars are passed at the command line and have the highest precedence. For example:
```sh
ansible-playbook playbook.yml -e "type=t2.micro"
```
This will override any values set in `defaults` or `vars`.

### Playbook Level Variables
Variables can also be declared directly within a playbook:
```yaml
# playbook.yml
- name: Launch EC2 instance
  hosts: localhost
  vars:
    type: t2.micro
  tasks:
    - name: Launch EC2 instance
      ec2:
        instance_type: "{{ type }}"
```

### Group Vars
Group variables are declared in the `group_vars` directory and are applied to groups of hosts:
```yaml
# group_vars/all.yml
password: aws_secret

# group_vars/app.yml
password: app_password

# group_vars/db.yml
password: db_password
```
These variables are used based on the group the host belongs to, as defined in the inventory file.

### Variable Precedence
Understanding the precedence of variables is crucial for managing complex playbooks. The general order from lowest to highest precedence is:
1. Role defaults
2. Inventory file variables
3. Playbook variables
4. Role vars
5. Block vars
6. Task vars (only for the task)
7. Extra vars (always win)

### Practical Example
Let's create an EC2 instance using variables declared at different levels:
```yaml
# defaults/main.yml
type: t2.micro

# vars/main.yml
type: t2.medium

# playbook.yml
- name: Launch EC2 instance
  hosts: localhost
  vars:
    region: us-west-2
  tasks:
    - name: Launch EC2 instance
      ec2:
        region: "{{ region }}"
        instance_type: "{{ type }}"
```
Command to run the playbook with Extra Vars:
```sh
ansible-playbook playbook.yml -e "type=t2.large"
```
In this scenario:
- `region` is defined at the playbook level.
- `type` in `vars/main.yml` will override `defaults/main.yml`.
- `type` passed through `-e` will override all other `type` declarations.

### Conclusion
Using variables in Ansible allows you to write flexible, reusable, and maintainable playbooks. Understanding where and how to declare variables, along with their precedence, enables you to manage complex configurations effectively. Always follow the best practice of using variables to avoid hardcoding values, ensuring that your playbooks remain adaptable and robust.