# Passwordless Authentication

**Importance**
- Passwordless authentication allows Ansible to interact with managed nodes without requiring password input during each connection. This is crucial for automation, as manual password entry would negate the benefits of automation.



**Two Methods for setting up Passwordless Authentication**
-Using SSH keys
-USing Password (aws does not support this method, you can try on Azure VM)

Any method can be used based on your organization(how they prefer).

1(a). **Copying SSH Keys (Public Key) to target VM**
   - **Step 1**: Generate an SSH key pair if not already available.
     ```bash
     ssh-keygen -t rsa
     ```
   - **Step 2**: Copy the public key to the managed node.
     ```bash
     ssh-copy-id -i ~/.ssh/id_rsa.pub user@managed_node_ip
     ```
   - **Step 3**: Verify passwordless login.
     ```bash
     ssh user@managed_node_ip
     ```

1(b). **Copying SSH Keys using PEM File (for AWS instances) to Target VM**
   - **Step 1**: Ensure you have the PEM file (private key) which is used to create the AWS instance.
   - **Step 2**: Copy the SSH public key to the managed node using the PEM file.
     ```bash
     ssh-copy-id -f "-o IdentityFile <PATH TO PEM FILE>" ubuntu@<INSTANCE-PUBLIC-IP>
     ```
   - **Step 3**: Verify passwordless login.
     ```bash
     ssh ubuntu@<INSTANCE-PUBLIC-IP>
     ```


- ssh-copy-id: This is the command used to copy your public key to a remote machine.
- -f: This flag forces the copying of keys, which can be useful if you have keys already set up and want to overwrite them.
- "-o IdentityFile <PATH TO PEM FILE>": This option specifies the identity file (private key) to use for the connection. The -o flag passes this option to the underlying ssh command.
- ubuntu@<INSTANCE-IP>: This is the username (ubuntu) and the IP address of the remote server you want to access.

2. **Using Password Authentication (for environments where SSH keys are not used)**

- **Step 1**: First login to Target VM(using SSH) to Enable password authentication (if not already enabled).
     - Go to file and do ...sudo vim `/etc/ssh/sshd_config.d/60-cloudimg-settings.conf` (or) `/etc/ssh/sshd_config.d`:
     - Update `PasswordAuthentication yes`
     - Now Restart the SSH service:
       ```bash
       sudo systemctl restart ssh
       ```
   - **Step 2**: Setup a password for the target VM user (if not already set).
     ```bash
     sudo passwd user
     ```
     -replace user with ubuntu

     - logout from target VM

   - **Step 3**: Use `ssh-copy-id` to copy the SSH key, which will prompt for the password one last time.
     ```bash
     ssh-copy-id user@target_node_ip
     ```
     >> enter your password

   - **Step 4**: Verify passwordless login.
     ```bash
     ssh user@target_node_ip
     ```


You have successfully configured your control node (your laptop with Ansible) to communicate with your managed/target nodes without requiring a password. This enables Ansible to execute user instructions through YAML files(playbooks) or ad-hoc commands seamlessly. Now, Ansible operations can be automated through Jenkins, scheduled to run at night using Cron jobs, or you can schedule your anible playbooks they will execute without any Interruption and they will just notify you that the execution is successful.