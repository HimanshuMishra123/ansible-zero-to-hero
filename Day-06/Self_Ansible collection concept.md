## New Use Case: API-Based Management

Now, let's say we want to manage resources not only on AWS but also on other platforms like Azure, GCP, or network appliances like Cisco. Instead of connecting to virtual machines, we want Ansible to interact with APIs to create resources. For example, we might want to create an EC2 instance, an S3 bucket, or manage network configurations directly through API calls.

![Day-06 _ Create Resources on AWS using Ansible _ Ansible Variables Demo _ Ansible Vault Demo 20-19 screenshot](https://github.com/user-attachments/assets/baeb9cf1-8dde-447a-a159-74c5f27cb896)
![Day-06 _ Create Resources on AWS using Ansible _ Ansible Variables Demo _ Ansible Vault Demo 37-29 screenshot](https://github.com/user-attachments/assets/5b3921be-d74b-4aea-ad63-fee8f1305a60)

- Collections allow Ansible to manage different types of infrastructure by interacting with their APIs.
- Examples include collections for AWS, Azure, GCP, Cisco, OpenStack, etc.

**API-based Execution:**
- Ansible runs modules on the control node when working with collections for infrastructure provisioning.
- These modules contain Python code that interacts with APIs of the target services so we need python(+boto3 for aws).
- But in case of working with managed nodes Ansible uses SSH to connect to managed nodes and runs modules but you can not do SSH to AWS so API call is done.

----------

### How Ansible Handles API-Based Management

When using Ansible for API-based management:

1. **Modules**: Ansible uses specific modules designed for interacting with APIs (e.g., AWS modules, Azure modules, Cisco modules).
2. **Local Execution**: Unlike managing EC2 instances via SSH, these modules are executed on your control node (your local machine).
3. **API Interaction**: These modules contain Python code that Ansible runs locally. This Python code communicates with the respective APIs (e.g., AWS API, Cisco API) to manage resources.

### Collections in Ansible

To know which platforms and services are supported, Ansible provides **collections**. Collections are groups of related Ansible modules and plugins. Here are some examples:

- **AWS Collection**: For managing AWS resources.
- **Azure Collection**: For managing Azure resources.
- **GCP Collection**: For managing Google Cloud resources.
- **Cisco Collection**: For managing Cisco network appliances.

Each collection contains modules that allow you to perform specific tasks via API. For example:

- **F5 Networks**: Manages F5 BIG-IP appliances.
- **OpenStack**: Manages OpenStack resources.
- **Kubernetes**: Manages Kubernetes clusters.

### User Workflow

**As a user, your steps remain the same regardless of whether you are managing VMs or interacting with APIs**:

1. **Write YAML**: Create a YAML file in the format of a playbook or role.
2. **Execution**: Ansible executes the playbook or role. If it involves managing VMs, Ansible connects to them via SSH. If it involves APIs, Ansible runs the module locally to interact with the API.

### Summary

- When managing VMs: Ansible connects to the VMs, installs modules, and executes them on the VMs.
- When interacting with APIs: Ansible runs the modules on your control node, and these modules communicate with the APIs to manage resources.

By understanding these differences, you can effectively use Ansible to manage not just VMs but a wide range of resources across different platforms.

---

I hope this makes it clearer! Let me know if you have any questions.
