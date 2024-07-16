# Ansible Roles

An Ansible role is a reusable, self-contained unit of automation that is used to 
organize and manage tasks, variables, files, templates, and handlers in a structured way. 

Roles help to encapsulate and modularize the logic and configuration needed to manage 
a particular system or application component. 

This modular approach promotes reusability, maintainability, and consistency across different 
playbooks and environments.

## Key Components of an Ansible Role

### Tasks
The main list of actions that the role performs.

### Handlers
Ansible Tasks that are triggered by changes in other tasks (means you want to execute an action after some task so just notify the handler to run), typically used for actions like restarting services.
```yaml
tasks:
- name: Template configuration file
  ansible.builtin.template:
    src: template.j2
    dest: /etc/foo.conf
  notify:                         # call handlers
    - Restart apache
    - Restart memcached

handlers:
  - name: Restart memcached
    ansible.builtin.service:
      name: memcached
      state: restarted

  - name: Restart apache
    ansible.builtin.service:
      name: apache
      state: restarted
```
### Files
These are static files that you can copy to remote hosts using the 'copy' module. They do not support variable substitution or any form of dynamic content generation. Use `files when you need to deploy a static file exactly as it is, without any need for customization. You can keep any no. of files here which need to be transferred to managed hosts. As keeping files in the root location creates confusions. <br/>

```yaml
- name: Copy file with owner and permissions
      ansible.builtin.copy:
        src: files/index.html          #in src block just say files/file_name
        dest: /var/www/html
        owner: root
        group: root
        mode: '0644'
```

### Templates
Notes- Files and Template serves same purpose to keep files. Files used to keep static files whereas Templates is used to keep Dynamic files(where you want to keep some values in the file Dynamic).<br/>
Folder in an Ansible role is used to store Jinja2 templating language files, which can be used to dynamically generate configuration files.<br/>
This enable dynamic content generation where You can include variables, control structures, and other dynamic elements in a template. Use `templates` when you need to customize the content of the file based on variables or other conditions at runtime. (Example - used to dynamically generate configuration files)

### Vars
Variables that are used within the role.

### Defaults
Default variables for the role, which can be overridden. These are used by the Role if no Vars are given.

### Meta
Metadata about the role, including dependencies on other roles.

### Library
Custom modules or plugins used within the role.

### Module_defaults
Default module parameters for the role.

### Lookup_plugins
Custom lookup plugins for the role.

## Directory Structure of an Ansible Role

An Ansible role follows a specific directory structure:

```
<role_name>/
  ├── defaults/
  │   └── main.yml
  ├── files/
  ├── handlers/
  │   └── main.yml
  ├── meta/
  │   └── main.yml
  ├── tasks/
  │   └── main.yml
  ├── templates/
  ├── vars/
  │    └── main.yml
  ├── templates/
│   └── my_config.conf.j2
```

## Why Use Ansible Roles?

### Modularity
Roles allow you to break down complex playbooks into smaller, reusable components. 
Each role handles a specific part of the configuration or setup.

### Reusability
Once created, roles can be reused across different playbooks and projects. This saves time 
and effort in writing redundant code.

### Maintainability
By organizing related tasks into roles, it becomes easier to manage and maintain the code. 
Changes can be made in one place and applied consistently wherever the role is used.

### Readability
Roles make playbooks cleaner and easier to read by abstracting away the details into logically
named roles.

### Collaboration
Roles facilitate collaboration among team members by allowing them to work on different parts
of the infrastructure independently.

### Consistency
Using roles ensures that the same setup and configuration procedures are applied uniformly across
multiple environments, reducing the risk of configuration drift.
