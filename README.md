# `Ansible`


### What is ansible?
Ansible is open source IT automation tool that helps to automate task like provisioning, configuration management, application deployment and many more other IT process. and it is free to use.

### How Ansible Works
1. Agentless Architecture: No software installation is required on managed nodes.
2. SSH Communication: Ansible uses SSH to communicate with managed nodes.
3. Task Execution: It connects via SSH and pushes out small programs called Ansible modules to perform tasks. Once executed, these modules are removed automatically.

### How to Install Ansible Using Python's pip (Version 3.8.19)
In this guide, I'll walk you through installing Ansible using pip with Python version 3.8.19. For more details, refer to the Ansible Installation Guide.
Step 1: Install Ansible and Ansible Core Open your terminal and execute the following commands to install Ansible and its core components using pip:
```shell
pip3 install --user ansible
pip3 install --user ansible-core
```
pip3 install --user ansible: Installs the full Ansible package, including various tools and libraries.
pip3 install --user ansible-core: Installs the core engine of Ansible.
Step 2: Update Your PATH Environment Variable After installation, you may need to add the Ansible binary location to your PATH environment variable to ensure that the ansible command is recognized.

1. Locate the installation path: Typically, Ansible is installed in ~/.local/bin/. Verify this by running:
```shell
echo $PATH
```
2. Add to PATH: If the installation path is not in your PATH, add it by running:
```shell
export PATH="$HOME/.local/bin:$PATH"
```
To make this change permanent, add the export line to your shell configuration file (e.g., .bashrc, .zshrc).
3. Verify the Installation: Finally, verify that Ansible is installed correctly by checking its version:
```shell
ansible --version
```
This command should display the installed version of Ansible, confirming the successful installation.

### Creating a Passwordless SSH Connection with the Control Node
1. Generate SSH Key: Run ssh-keygen on the master server, keeping all values default. This will generate a key pair.
2. Copy the Generated Key to the Control Node: Use the following command to copy the generated key to the control node:
```shell
ssh-copy-id -i <file_path> <user>@<ip/host>
```
3. Test SSH Connection: Try logging in using:
```shell
ssh <user>@<ip/host>
```
Note: The control node is where Ansible is installed, and the managed node is your target node.

### Inventory
1. Definition: The inventory is a fundamental component of Ansible that defines details of remote systems to be managed.
2. Purpose: It provides information about remote hosts during Ansible operations.
Here’s an example of an inventory file:
```ini
# File: inventory.ini
[middleware_servers]
<user>@<hostname/ip>
<user>@<hostname/ip>

[log_servers]
<user>@<hostname/ip>
<user>@<hostname/ip>
```

### Playbook
What is an Ansible Playbook? A playbook in Ansible is a YAML file that defines the steps to manage a system’s configuration or tasks.
Playbook Structure:
YAML files start with ---.
You declare name, hosts, and remote-user.
Under tasks, define your tasks as required.
Here’s an example:

```text
# File: first_playbook.yml
---
- hosts: all
  tasks:
   - name: "Create Hello_World.txt file on all managed nodes"
     ansible.builtin.shell: |
      cd "/home/ansible"
      touch Hello_World.txt
      ls -l
```
This example creates a Hello_World.txt file in the /home/ansible path on all managed nodes.

### Ansible Modules
1. Definition: Ansible modules are units of code that can control system resources or execute system commands.
2. Module Library: Ansible provides a library of modules that can be executed directly on remote hosts or through playbooks. Custom modules can also be created.
3. Commonly Used Modules: Examples include ansible.builtin.copy, ansible.builtin.command, ansible.builtin.file.

### Executing Your First Ansible Ad-Hoc Command
1. Create inventory.ini file: List all your target servers in it.
2. Execute a Ping Command:
```bash
ansible -i inventory.ini -m ping all
```
expected output
```json
<User>@<IP/HostName> | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}
```
This indicates that the control and managed nodes can communicate with each other.


### Executing Your First Ansible Playbook
1. Create inventory.ini file: Include all your target servers.
2. Create YAML Playbook: Save the following content as first-playbook.yml:
```yaml
---
- hosts: all
  tasks:
    - name: "Create File on all managed nodes"
      ansible.builtin.shell: |
        echo "creating file Ansible.txt"
        touch Ansible.txt
```
3. Run the Playbook: Execute it using:
```bash
ansible-playbook -i inventory.ini first-playbook.yml 
```

expected output:
```markdown
PLAY [all] *******************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************
ok: [<user>@<IP/Hostname>]

TASK [Create file on all managed servers] ************************************************************************************
changed: [<user>@<Hostname>]

PLAY RECAP *******************************************************************************************************************
<user>@<Hostname>      : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

### Conclusion
Ansible is a powerful tool that simplifies IT automation tasks through its agentless architecture and straightforward playbook system. By following this guide, you’ve learned how to install Ansible, configure passwordless SSH connections, create an inventory file, and write and execute your first playbook. These foundational steps will help you automate various IT processes efficiently and effectively.
