The `templates` folder in an Ansible role is used to store Jinja2 template files, which can be used to dynamically generate configuration files. Below is an example of how to use the `templates` folder in an Ansible role.

### Directory Structure
```
my_role/
├── tasks/
│   └── main.yml
├── templates/
│   └── my_config.conf.j2
├── vars/
│   └── main.yml
```

### Example Template (`templates/my_config.conf.j2`)
```jinja
# my_config.conf.j2

[settings]
server_name = {{ server_name }}
port = {{ server_port }}

[database]
db_name = {{ db_name }}
db_user = {{ db_user }}
db_password = {{ db_password }}
```

### Variables File (`vars/main.yml`)
```yaml
# vars/main.yml

server_name: example.com
server_port: 80
db_name: my_database
db_user: my_user
db_password: my_password
```

### Tasks File (`tasks/main.yml`)
```yaml
# tasks/main.yml

- name: Deploy configuration file from template
  template:
    src: my_config.conf.j2
    dest: /etc/myapp/my_config.conf
  vars:
    server_name: "{{ server_name }}"
    server_port: "{{ server_port }}"
    db_name: "{{ db_name }}"
    db_user: "{{ db_user }}"
    db_password: "{{ db_password }}"
```

### Playbook Example
You would use the role in a playbook like this:

```yaml
# playbook.yml

- hosts: all
  roles:
    - my_role
```

### Explanation
1. **Template File (`my_config.conf.j2`)**: This is a Jinja2 template file with placeholders for variables.
2. **Variables File (`vars/main.yml`)**: This file contains the variables that will be used in the template.
3. **Tasks File (`tasks/main.yml`)**: This Ansible task uses the `template` module to process the Jinja2 template, replacing the placeholders with actual values from the variables file. The result is saved to the specified destination on the managed node.
4. **Playbook**: This playbook applies the role `my_role` to all hosts.

When the playbook is run, the `template` module reads `my_config.conf.j2`, replaces the placeholders with the values defined in `vars/main.yml`, and writes the output to `/etc/myapp/my_config.conf` on the managed nodes.