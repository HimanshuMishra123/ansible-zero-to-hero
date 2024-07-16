# Push your Ansible roles to Ansible Galaxy

1. Make sure your role is structured correctly. The basic structure should look like this:

```
my_role/
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
├── tests/
│   ├── inventory
│   └── test.yml
└── vars/
    └── main.yml
```

2. Make sure ansible-galaxy CLI exists

```
ansible-galaxy --version
```

3. Push Your Role to GitHub

```
cd <role-name>
git init
Now go to github and create a repository for push this Ansible role to it >> copy repo https URL
git remote add origin <https://github.com/your_github_username/my_role.git> 
git add .
git commit -m "Initial commit"
git push -u origin main
```

4. Import the Role to Ansible Galaxy(means to share it on Ansible Galaxy we need to put that in Github first).

```
ansible-galaxy role import <your_github_username> <repo-name> --token <API Token of Ansible Galaxy Account>
```
- FOR API Token: go to ansible galaxy>> left sidepane goto collections>> API token >> load and copy it.<br/>

You can create/login Ansible galaxy account with your github account.


**Ansible Galaxy role install** - to use role from ansible galaxy in your local

```adhoc
ansible-galaxy role install bsmeding.docker
```

-bsmeding is user
-bsmeding.docker is role to install docker on linux which we are using....link https://github.com/bsmeding/ansible_role_docker

role will be installed in location  ~/.ansible/roles
now you can use the role in your playbook.
```yaml
# in playbook.yml

- hosts: all
  roles:
    - bsmeding.docker
```

```
ansible-playbook -i inventory.ini playbook.yml
```
Docker will be install on managed nodes(all hosts).
