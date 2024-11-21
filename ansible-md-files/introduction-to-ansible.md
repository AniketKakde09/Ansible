## Introduction to Ansible

- ### `Understanding Ansible`
    - Ansible is open source IT automation tool that helps to automate task like provisioning, configuration management, application deployment and many more other IT process. and it is free to use.
    - Ansible is agentless which means there is no need to install any software on target node which makes it lightweight and efficient.
- ### `Why Ansible is for DevOps?`
    - **Ansible is popular choice in DevOps for several reasons:**  

        | `Reason`                | `Description`                                                                                     |  
        |------------------------|--------------------------------------------|
        | Simplified Automation | Ansible’s declarative language allows teams to define the desired state of their infrastructure, reducing complexity. |
        | Agentless Architecture | No need for software agents on client machines, reducing management overhead. |
        | Streamlined CI/CD | By integrating with Jenkins, Git, and other DevOps tools, Ansible automates continuous integration and continuous deployment (CI/CD) pipelines effortlessly. |
        | Infrastructure as Code (IaC) | Ansible allows teams to define and manage infrastructure as code, improving consistency and making configuration repeatable. |

- ### `Ansible Vs Other IT Automation Tools`
    - When comparing Ansible to other popular IT automation tools like Chef, Puppet, and SaltStack, several factors make it stand out:  
        - Agentless: Unlike Puppet and Chef, Ansible doesn’t require agents, making it easier to manage.  
        - Ease of Use: Ansible’s playbooks are written in YAML, which is more human-readable compared to Puppet’s Ruby DSL or Chef’s recipes.  
        - Learning Curve: Ansible has a lower barrier to entry for beginners, making it accessible to teams with varying skill levels.  
        - Speed: Ansible’s push-based model uses SSH, making it faster to deploy changes.  
        -   | Feature	| Ansible |	Puppet	| Chef | SaltStack |
            | --------- | ------- | ------- | ---- | --------- |
            | Agentless	| Yes | No | No| Yes |
            | Ease of Use |	High | Medium | Low	Medium |
            | Language | YAML | Ruby | DSL | Ruby | Python |
            | Execution Model | Push-based | Pull-based | Pull-based | Push-based |
            | Learning Curve | Easy | Medium | High | Medium |

- ### `Installation of Ansible`
    #### This guide will guide you through the process of installing Ansible using pip with Python version 3.8.19. Please refer to the Ansible Installation Guide for more details.  [Ansible Documentation](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#pip-install)  

    - `Step 1: Install Ansible and Ansible Core Open your terminal and execute the following commands to install Ansible and its core components using pip:`  

        ```shell
        pip3 install --user ansible
        pip3 install --user ansible-core
        ```
        - pip3 install --user ansible: Installs the full Ansible package, including various tools and libraries.  
        - pip3 install --user ansible-core: Installs the core engine of Ansible.  

    - `Step 2: Update Your PATH Environment Variable After installation, you may need to add the Ansible binary location to your PATH environment variable to ensure that the ansible command is recognized.`  

        - Locate the installation path: Typically, Ansible is installed in ~/.local/bin/. Verify this by running:

            ```shell
            echo $PATH
            ```
        - Add to PATH: If the installation path is not in your PATH, add it by running:

            ```shell
            export PATH="$HOME/.local/bin:$PATH"
            ```
        - To make this change permanent, add the export line to your shell configuration file (e.g., .bashrc, .zshrc).

        - Verify the Installation: Finally, verify that Ansible is installed correctly by checking its version:

            ```shell
            ansible --version
            ```
        **This command should display the installed version of Ansible, confirming the successful installation.**

    > Note Ansible installation can also be done via package manager  
        ```bash
        sudo yum install ansible
        ```
- ### `Understanding Ansible Architecture`

    - ![Ansible Architecture](./ansible-architecture.png)  

    - **`1. Control Node:`**
        - This is the central machine where Ansible is installed.
        - It contains configuration files like inventory.ini and the automation logic in the form of playbook.yaml.
        - The control node sends commands to the target systems over SSH in the form of modules.

    - **`2. Inventory File (inventory.ini):`**
        - This file holds a list of target systems (IP addresses or hostnames) that the control node manages.
        - Ansible uses this inventory to know which machines it will execute tasks on.

    - **`3. Playbook (playbook.yaml):`**
        - A YAML file containing the automation tasks (plays).
        - Each task is executed on the target systems as per the instructions defined in the playbook.
        - Playbooks define the actions that Ansible should perform on the target systems, such as installing software or updating configuration files.

    - **`4. SSH Connection:`**
        - Ansible uses SSH (by default) to connect to the target systems.
        - SSH enables the control node to execute tasks on the remote machines without needing any agent installed on them.

    - **`5. Modules:`**
        - Ansible uses "modules" to perform tasks. These modules can be anything from managing files, installing packages, or interacting with services.
        - The control node sends these modules to the target systems where they are executed, and results are sent back to the control node.

    - **`6. Target Systems:`**  
        - These are the machines (servers or devices) that Ansible manages. Each one has an IP address or hostname (e.g., 172.18.1.2, 172.18.1.3).
        - No Ansible software is installed on these systems; only the necessary modules are temporarily transferred and executed via SSH.

- ### `Ansible Terminology`
    - **`1. Control Node:`**
        - The machine where Ansible is installed.
        - It manages the automation tasks and orchestrates communication with the target systems (also called managed nodes).
        - No agents need to be installed on the target systems; communication occurs over SSH.

    - **`2. Managed Nodes (Target Systems):`**
        - These are the machines that Ansible manages.
        - Also known as "hosts", they are defined in the inventory file.
        - Ansible can perform tasks such as configuration management, deployment, and orchestration on these nodes.

    - **`3. Inventory:`**
        - A file that lists the managed nodes (servers, devices) that Ansible will manage.
        - It can be a simple .ini file, a YAML file, or even dynamically generated using inventory scripts.
        - Groups of hosts can be defined to organize them for more efficient task execution.
            ```ini
            [webservers]
            192.168.1.1
            192.168.1.2

            [dbservers]
            192.168.1.3
            ```

    - **`4. Playbook:`**
        - A YAML file that contains a set of instructions (plays) for Ansible to execute on the managed nodes.
        - It defines tasks, which can be as simple as installing a package or as complex as setting up an entire application stack.
            ```yaml
            - hosts: webservers
              tasks:
                - name: Install Apache
                  yum:
                    name: httpd
                    state: present
            ```

    - **`5. Tasks:`**
        - The individual actions in a playbook.
        - A task represents a single unit of work, such as installing a package, copying a file, or starting a service.

    - **`6. Plays:`**
        - A play is a set of tasks that are executed on a defined set of hosts (managed nodes).
        - Playbooks can contain multiple plays, each targeting different groups of hosts with different tasks.

    - **`7. Modules:`**
        - Reusable scripts that perform specific tasks such as installing software, managing files, or configuring services.
        - Ansible ships with many built-in modules (e.g., yum, apt, service, copy), but custom modules can also be created.

    - **`8. Roles:`**
        - A way to organize playbooks into reusable components.
        - Roles contain related tasks, variables, files, templates, and handlers that define a particular functionality (e.g., setting up a web server).
        - Roles help in organizing complex playbooks.
            ```markdown
            roles/
            └── webserver/
            ├── tasks/
            ├── handlers/
            ├── templates/
            ├── files/
            └── vars/
            ```
    - **`9. Handlers:`**
        - Special tasks that are triggered only when notified by other tasks.
        - Typically used for actions like restarting a service after a configuration file has been changed.
            ```yaml
            - name: Restart Apache
              service:
                name: httpd
                state: restarted
              notify:
              - Restart Apache service

            handlers:
            - name: Restart Apache service
              service:
                name: httpd
                state: restarted
            ```


    - **`10. Facts:`**
        - System variables collected from managed nodes before executing any tasks.
        - Ansible automatically gathers facts such as IP address, hostname, OS type, etc., unless explicitly disabled.
        - Facts can be used within playbooks to create more dynamic configurations.

    - **`11. Variables::`**
        - Ansible variables allow playbooks to be more dynamic.
        - Variables can be defined in playbooks, roles, or inventory files and passed between tasks.
        - They can also be templated using Jinja2 for even more flexibility.
            ```yaml
            vars:
                http_port: 80
                max_clients: 200
            ```

    - **`12. Templates:`**
        - Templates are text files that contain placeholders for variables and are processed using the Jinja2 templating engine.
        - This allows you to create dynamic configuration files based on the values of variables.

            Example (Nginx configuration template):
            ```jinja2
            server {
                listen {{ http_port }};
                server_name {{ domain }};
            }
            ```

    - **`13. Vault:`**
        - Ansible Vault is used to securely store sensitive information, such as passwords or API keys, by encrypting variables and files.
        - Vault-encrypted data can only be decrypted by authorized users with the correct password.

    - **`14. Galaxy:`**
        - Ansible Galaxy is a community hub where users can share roles.
        - You can download roles created by others to save time and use them in your projects.

    - **`15. Callback Plugins:`**
        - Ansible uses callback plugins to control how the output of playbooks is displayed.
        - They are often used to send notifications or log the results of a playbook run.

    - **`16. Dynamic Inventory:`**
        - Unlike a static inventory file, a dynamic inventory is generated at runtime and can be pulled from external sources like cloud - providers (AWS, Azure, GCP) or CMDBs.

    - **`17. Idempotency:`**
        - Ansible modules are designed to be idempotent, meaning running them multiple times will have the same result.
        - This ensures that tasks will not make changes if the system is already in the desired state.