# Ansible


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
