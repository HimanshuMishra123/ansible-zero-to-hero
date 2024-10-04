Ansible handlers are special tasks that are triggered by the `notify` directive in other tasks. They are typically used for actions that should only be run once, at the end of the playbook, even if multiple tasks notify them. Common use cases include restarting services or reloading configuration files after changes.

### Example Scenario: Using Ansible Handlers

#### Directory Structure
```
my_role/
├── tasks/
│   └── main.yml
├── handlers/
│   └── main.yml
├── templates/
│   └── my_config.conf.j2
├── vars/
│   └── main.yml
```

### Step 1: Define the Handler

Create the handler file `handlers/main.yml`:

```yaml
# handlers/main.yml

- name: restart nginx
  service:
    name: nginx
    state: restarted
```

### Step 2: Define the Task with Notify

Create the main task file `tasks/main.yml` that includes a task notifying the handler:

```yaml
# tasks/main.yml

- name: Deploy configuration file from template
  template:
    src: my_config.conf.j2
    dest: /etc/nginx/nginx.conf
  notify: restart nginx
```

### Step 3: Define the Template

Create the template file `templates/my_config.conf.j2`:

```jinja
# my_config.conf.j2

server {
    listen 80;
    server_name {{ server_name }};
    root /var/www/html;
}
```

### Step 4: Define Variables

Create the variables file `vars/main.yml`:

```yaml
# vars/main.yml

server_name: example.com
```

### Step 5: Define the Role in the Playbook

Include the role in your playbook `playbook.yml`:

```yaml
# playbook.yml

- hosts: webservers
  roles:
    - my_role
```

### Explanation

1. **Handler Definition (`handlers/main.yml`)**: Defines a handler named `restart nginx` that restarts the nginx service.
2. **Task with Notify (`tasks/main.yml`)**: Deploys a configuration file using the `template` module and notifies the `restart nginx` handler if the task changes the file.
3. **Template (`templates/my_config.conf.j2`)**: A Jinja2 template for the nginx configuration.
4. **Variables (`vars/main.yml`)**: Defines a variable `server_name` used in the template.
5. **Role in Playbook (`playbook.yml`)**: Uses the role `my_role` in a playbook targeting hosts in the `webservers` group.

### Running the Playbook

When you run the playbook, the following happens:
1. The `Deploy configuration file from template` task runs and updates the `/etc/nginx/nginx.conf` file.
2. If the file changes, the task triggers the `restart nginx` handler using the `notify` directive.
3. The handler `restart nginx` is executed at the end of the playbook, ensuring that nginx is restarted only once, even if multiple tasks notify it.
