# Index

1. [Introduction to Ansible](#introduction)
    - [Understanding Ansible](#understanding-ansible)
    - [Why Ansible is for DevOps?](#why-ansible-is-for-devops)
    - [Ansible Vs Other IT Automation Tools](#ansible-vs-other-it-automation-tools)
    - [Installation of Ansible](#installation-of-ansible)
    - [Understanding Ansible Architecture](#understanding-ansible-architecture)
    - [Ansible Terminoly](#ansible-terminology)

2. [Ansible Ad-hoc Commands](#)
    - [What is an Ad-hoc Command?](#)
    - [Password-less Authentication](#)
    - [Executing Ad-hoc Commands](#)

2. [Ansible Playbooks](#)
    - [Introduction to Ansible Playbooks](#)
    - [Understanding YAML](#)
    - [Creating Your First Playbook](#)
    - [Executing the Playbook](#)
    - [Debugging the Playbook](#)
    - [Managing Erros in Playbooks](#)


[comment]: <> (----------------------------------------------------------------------------------------------------------------)
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


[comment]: <> (----------------------------------------------------------------------------------------------------------------)
## Ansible Ad-hoc Commands.
When working with Ansible, ad-hoc commands are a quick and efficient way to execute tasks without needing to create a full playbook. These commands are great for one-off tasks or making immediate changes across multiple machines.

- ### `What is an Ad-hoc Command?`
    - An ad-hoc command is a simple, single-line command used to perform a task on the fly, without having to write an entire Ansible playbook. Think of it as the equivalent of running a shell command but across multiple systems.  

        ```shell
        ansible all -m ping
        ```
- ### `Password-less Authentication.`
    >Currently, I do not have any other virtual machines, either in the cloud or on my local system, which is why I am using docker to create light-weight virtual machines, If you want to use the same [click here](https://hub.docker.com/)  
    - **Setp 1: Generate SSH Key: Run ssh-keygen on the master server, keeping all values default. This will generate a key pair.**

        ```shell
        ssh-keygen
        ```
    - **Step 2: Copy the Generated Key to the Control Node: Use the following command to copy the generated key to the control node:**

        ```shell
        ssh-copy-id -i <file_path> <user>@<ip/host>
        ```

    - **Step 3: Test SSH Connection: Try logging in using:**
        ```shell
        ssh <user>@<ip/host>
        ```

    - **Step 4: Creating Inventory file.**
        - Step 1: The inventory is a fundamental component of Ansible that defines details of remote systems to be managed.

        - Step 2. Purpose: It provides information about remote hosts during Ansible operations. below is an example of an inventory file:

            ```ini
            # File: inventory.ini
            [middleware_servers]
            <user>@<hostname/ip>
            <user>@<hostname/ip>

            [log_servers]
            <user>@<hostname/ip>
            <user>@<hostname/ip>
            ```
- ### `Executing Ad-hoc Commands`
    >Ad-hoc commands can be used for a variety of tasks such as installing packages, managing services, or gathering facts from servers.
    - **Example:**
        - **To install a package.**
            ```shell
            ansible -i inventory.ini -m apt -a "name=git state=present" middleware_servers
            ```
        - **To check if we are able to connect with target nodes.**
            ```shell
            ansible -i invenory.ini -m ping all
            ```


[comment]: <> (----------------------------------------------------------------------------------------------------------------)

## Ansible Playbooks
A playbook in Ansible is a YAML file that defines the steps to manage a system’s configuration or tasks.
- ### `Introduction to Ansible Playbooks`
    - Ansible Playbooks offer a more powerful approach to automation than simple command execution. By defining tasks, roles, and variables in an easily readable format, Playbooks help streamline tasks like configuration management, application deployment, and infrastructure orchestration.
    - A typical Playbook consists of one or more plays that map hosts to a set of defined tasks. Each play contains tasks that are executed in sequence on the specified hosts. This flexibility and clarity make Playbooks easy to manage and extend.

- ### `Understanding YAML`
    - If you've been working in DevOps or dealing with infrastructure as code (IaC), you've likely encountered YAML. But what exactly is it, and why is it so widely used? Let’s break it down.
    - **`What is YAML`**
        - YAML stands for "Yet Another Markup Language.
        - YAML is a data serialization language that is both human-readable and extremely popular, especially in the world of DevOps. You’ll find it used in tools like Docker, Kubernetes, and Prometheus, just to name a few.
        - It uses line separation and indentation to organize data, making it clean and easy to understand.
    - **Basic Syntax of YAML**
        - **`Key-Value Pairs`**  
            At its core, YAML consists of key-value pairs, where each key has a corresponding value.
            ```yaml
            name: "Aniket"
            age: 25
            gender: "male"
            ```
            In this example, name, age, and gender are keys, while "Aniket", 25, and "male" are the respective values. The use of colons and indentation defines these pairs.

        - **`Comments in YAML`**  
            Comments in YAML are straightforward. Just use the # symbol.
            ```yaml
            # This is a comment
            name: "Aniket"
            ```
            Anything following the # will be treated as a comment and ignored by the parser.

        - **`Objects in YAML`**  
            YAML also supports objects, which are key-value pairs grouped together.
            ```yaml
            student:
                name: "Aniket"
                age: 25
                gender: "male"
            ```
            In this case, student is an object with three properties: name, age, and gender.

        - **`Lists in YAML`**  
            YAML allows you to create lists, making it easy to represent data like arrays.
            ```yaml
            student:
              - name: "Aniket"
                age: 25
                gender: "male"
                boolean: true
                projects:
                  - project1
                  - project2
                  - project3
            ```
            Here, the student object contains a list of projects. Notice the use of dashes (-) to denote items in the list.

        - **`Multi-Line Strings`**  
            YAML also lets you define multi-line strings, making it useful for adding long text.
            ```yaml
            student:
              - name: "Aniket"
                age: 25
                gender: "male"
                boolean: true
                projects:
                  - project 1
                  - project 2
                  - project 3
                about: |
                  This is First Line
                  This is Second Line
                  This is Third Line
            ```
            The | symbol indicates that the following content is a multi-line string. Each line will be treated as part of the same string.

- ### `Creating Your First Playbook`
    Let’s create a basic Ansible playbook that installs Nginx on a group of servers.  

    - Step 1: Defining the Playbook Structure.  
    - Step 2: Here’s a simple playbook named install_nginx.yml.   
        ```yaml
        ---
        - name: Install and start Apache on web servers
        hosts: web
        become: yes
        tasks:
            - name: Ensure Apache/Nginx is installed
            apt:
                name: nginx
                state: present
            - name: Start Apache/Nginx service manually
            ansible.builtin.command:
                cmd: sudo service nginx restart
        ```
        - name: A human-readable description of what the playbook does.
        - hosts: Specifies the target hosts, in this case, the web group from your inventory file.
        - become: yes: This ensures that the tasks are executed with root privileges (using sudo).
        - tasks: Lists the tasks that Ansible will run on the target machines.
    - In the above example:  
        - The first task installs Nginx using the apt module (for systems like Ubuntu/Debian).
        - The second task ensures that Nginx is started and enabled to run on boot using the service module.

- ### `Executing the Playbook`
    - Step 1: Run the Playbook: Execute it using:

        ```shell
        ansible-playbook -i inventory.ini install_nginx.yml
        ```  

        **Sample Output**
        ```markdown
        PLAY [Install and start Apache on web servers] *******************************************************************************

        TASK [Gathering Facts] *******************************************************************************************************
        ok: [appadmin@172.18.1.3]

        TASK [Ensure Apache/Nginx is installed] **************************************************************************************
        ok: [appadmin@172.18.1.3]

        TASK [Start Apache/Nginx service manually] ***********************************************************************************
        changed: [appadmin@172.18.1.3]

        PLAY RECAP *******************************************************************************************************************
        appadmin@172.18.1.3        : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
        ```
- ### `Debugging the Playbook`
    Debugging an Ansible playbook involves using built-in Ansible modules and techniques to inspect the playbook's execution, capture errors, and identify issues in your tasks. Here are some effective methods to debug an Ansible playbook:

    - `Enable Verbose Output`  
        Run the playbook with increased verbosity to see detailed logs:  
        ```shell
        ansible-playbook -i inventory install_nginx.yml -v
        ```
        or increase the verbosity level for more detailed logs:  
        ```shell
        ansible-playbook -i inventory install_nginx.yml -vvv
        ```
        This will show detailed information about each task's execution, including SSH details and the output of each task.

    - `Use debug Module`  
        The debug module allows you to print variables or other information during playbook execution. You can insert debug tasks wherever necessary to inspect the state of your variables or command output.

        ```yaml
        ---
        - name: Install and start Apache/Nginx on web servers
        hosts: web
        become: yes

        tasks:
            - name: Ensure Nginx is installed
            apt:
                name: nginx
                state: present

            - name: Debug message after Nginx is installed
            debug:
                msg: "Nginx Installed successfully"

            - name: Start Nginx service manually
            ansible.builtin.command:
                cmd: sudo service nginx restart

            - name: Debug message after Nginx service restart
            debug:
                msg: "Service Restarted successfully"
        ```
        **Sample Output**
        ```markdown
        PLAY [Install and start Apache/Nginx on web servers] *************************************************************************

        TASK [Gathering Facts] *******************************************************************************************************
        ok: [appadmin@172.18.1.3]

        TASK [Ensure Nginx is installed] *********************************************************************************************
        ok: [appadmin@172.18.1.3]

        TASK [Debug message after Nginx is installed] ********************************************************************************
        ok: [appadmin@172.18.1.3] => {
            "msg": "Nginx Installed successfully"
        }

        TASK [Start Nginx service manually] ******************************************************************************************
        changed: [appadmin@172.18.1.3]

        TASK [Debug message after Nginx service restart] *****************************************************************************
        ok: [appadmin@172.18.1.3] => {
            "msg": "Service Restarted successfully"
        }

        PLAY RECAP *******************************************************************************************************************
        appadmin@172.18.1.3        : ok=5    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
        ```
    - `Use register to Capture Output`  
        The register keyword allows you to store the output of a task in a variable, which can be useful for debugging.  
        ```yaml
        ---
        - name: Install and start Apache/Nginx on web servers
        hosts: web
        become: yes

        tasks:
            - name: Ensure Nginx is installed
              apt:
                name: nginx
                state: present
              register: nginx_install_output

            - name: Debug message after Nginx is installed
              debug:
                var: nginx_install_output

            - name: Start Nginx service manually
              ansible.builtin.command:
                cmd: sudo service nginx restart
              register: nginx_service_output

            - name: Debug message after Nginx service restart
              debug:
                var: nginx_service_output
        ```

        **Sample Output**
        ```markdown
        PLAY [Install and start Apache/Nginx on web servers] *************************************************************************

        TASK [Gathering Facts] *******************************************************************************************************
        ok: [appadmin@172.18.1.3]

        TASK [Ensure Nginx is installed] *********************************************************************************************
        ok: [appadmin@172.18.1.3]

        TASK [Debug message after Nginx is installed] ********************************************************************************
        ok: [appadmin@172.18.1.3] => {
            "nginx_install_output": {
                "cache_update_time": 1726047994,
                "cache_updated": false,
                "changed": false,
                "failed": false
            }
        }

        TASK [Start Nginx service manually] ******************************************************************************************
        changed: [appadmin@172.18.1.3]

        TASK [Debug message after Nginx service restart] *****************************************************************************
        ok: [appadmin@172.18.1.3] => {
            "nginx_service_output": {
                "changed": true,
                "cmd": [
                    "sudo",
                    "service",
                    "nginx",
                    "restart"
                ],
                "delta": "0:00:01.137680",
                "end": "2024-09-20 12:26:21.467513",
                "failed": false,
                "msg": "",
                "rc": 0,
                "start": "2024-09-20 12:26:20.329833",
                "stderr": "",
                "stderr_lines": [],
                "stdout": " * Restarting nginx nginx\n   ...done.",
                "stdout_lines": [
                    " * Restarting nginx nginx",
                    "   ...done."
                ]
            }
        }

        PLAY RECAP *******************************************************************************************************************
        appadmin@172.18.1.3        : ok=5    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
        ```
    - `Check ignore_errors, ignore_unreachable`   
      - **Ignoring failed commands (ignore_errors)**   

        By default, Ansible stops the execution of a playbook when a task fails on a host. This behavior can interrupt workflows when certain failures are not critical and can be safely ignored. To allow the execution of tasks to continue even if a particular task fails, we use the ignore_errors directive.

        **How to Use**   
        When you set ignore_errors: yes in a task, it tells Ansible to proceed with the rest of the playbook, even if that specific task fails.

        **Example 1: YAML file example without using ignore_errors.**

        ```yaml
        ---
        - hosts: web
          tasks:
            - name: "Example of ignore_errors in ansible."
              ansible.builtin.shell: |
                echo "This is example of ignore_errors, in the below commmand we are setting exit code to 1 so that this module can fail"
                exit 1
              register: ShellModuleOutput

            - name: "Print Output of Module :- Example of ignore_errors in ansible."
              debug:
                var: ShellModuleOutput
          ```

          **Output**

          ```bash
          PLAY [web] *******************************************************************************************************************

          TASK [Gathering Facts] *******************************************************************************************************
          ok: [appadmin@172.18.1.3]

          TASK [Example of ignore_errors in ansible.] **********************************************************************************
          fatal: [appadmin@172.18.1.3]: FAILED! => {"changed": true, "cmd": "echo \"This is example of ignore_errors, in the below commmand we are setting exit code to 1 so that this module can fail\"\nexit 1\n", "delta": "0:00:00.004080", "end": "2024-10-21 10:45:44.611746", "msg": "non-zero return code", "rc": 1, "start": "2024-10-21 10:45:44.607666", "stderr": "", "stderr_lines": [], "stdout": "This is example of ignore_errors, in the below commmand we are setting exit code to 1 so that this module can fail", "stdout_lines": ["This is example of ignore_errors, in the below commmand we are setting exit code to 1 so that this module can fail"]}

          PLAY RECAP *******************************************************************************************************************
          appadmin@172.18.1.3        : ok=1    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0
          ```

          **Example 2: YAML file example with using ignore_errors.**

          ```yaml
          ---
          - hosts: web
            tasks:
              - name: "Example of ignore_errors in ansible."
                ansible.builtin.shell: |
                  echo "This is example of ignore_errors, in the below commmand we are setting exit code to 1 so that this module can fail"
                  exit 1
                ignore_errors: yes
                register: ShellModuleOutput

              - name: "Print Output of Module :- Example of ignore_errors in ansible."
                debug:
                  var: ShellModuleOutput
          ```

          **Output**

          ```bash
          PLAY [web] *******************************************************************************************************************

          TASK [Gathering Facts] *******************************************************************************************************
          ok: [appadmin@172.18.1.3]

          TASK [Example of ignore_errors in ansible.] **********************************************************************************
          fatal: [appadmin@172.18.1.3]: FAILED! => {"changed": true, "cmd": "echo \"This is example of ignore_errors, in the below commmand we are setting exit code to 1 so that this module can fail\"\nexit 1\n", "delta": "0:00:00.004102", "end": "2024-10-21 10:49:55.752339", "msg": "non-zero return code", "rc": 1, "start": "2024-10-21 10:49:55.748237", "stderr": "", "stderr_lines": [], "stdout": "This is example of ignore_errors, in the below commmand we are setting exit code to 1 so that this module can fail", "stdout_lines": ["This is example of ignore_errors, in the below commmand we are setting exit code to 1 so that this module can fail"]}
          ...ignoring

          TASK [Print Output of Module :- Example of ignore_errors in ansible.] ********************************************************
          ok: [appadmin@172.18.1.3] => {
              "ShellModuleOutput": {
                  "changed": true,
                  "cmd": "echo \"This is example of ignore_errors, in the below commmand we are setting exit code to 1 so that this module can fail\"\nexit 1\n",
                  "delta": "0:00:00.004102",
                  "end": "2024-10-21 10:49:55.752339",
                  "failed": true,
                  "msg": "non-zero return code",
                  "rc": 1,
                  "start": "2024-10-21 10:49:55.748237",
                  "stderr": "",
                  "stderr_lines": [],
                  "stdout": "This is example of ignore_errors, in the below commmand we are setting exit code to 1 so that this module can fail",
                  "stdout_lines": [
                      "This is example of ignore_errors, in the below commmand we are setting exit code to 1 so that this module can fail"
                  ]
              }
          }

          PLAY RECAP *******************************************************************************************************************
          appadmin@172.18.1.3        : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=1
          ```
          
      - **Ignoring unreachable host errors (ignore_unreachable)**   

        In Ansible, when a host is unreachable (for example, due to a network issue or an SSH failure), the default behavior is to stop running tasks for that host. However, if you want to continue executing tasks on unreachable hosts or control how unreachable hosts are handled, you can use the ignore_unreachable directive.

        **How ignore_unreachable Works:**

        - **ignore_unreachable: true**   
           Ansible will attempt to run all tasks on all hosts, including those that become unreachable. Tasks will be skipped for unreachable hosts, but Ansible continues with other tasks.

        - **ignore_unreachable: false**   
            Ansible stops executing tasks for unreachable hosts. Tasks will only run on hosts that are reachable.

        **Example 1: ignore_unreachable: true**

        ```yaml
        ---
        - hosts: all
          ignore_unreachable: true
          tasks:
            - name: "Example of handling unreachable hosts"
              ansible.builtin.shell: |
                echo "This is example of handling unreachable hosts"
        ```

        **Output:**   
        In this scenario, Ansible continues executing tasks on reachable hosts, even if some hosts are unreachable

        ```bash
        PLAY [all] *******************************************************************************************************************

        TASK [Gathering Facts] *******************************************************************************************************
        ok: [appadmin@172.18.1.3]
        fatal: [appadmin@172.18.1.2]: UNREACHABLE! => {"changed": false, "msg": "Failed to connect to the host via ssh: ssh: connect to host 172.18.1.2 port 22: No route to host", "skip_reason": "Host appadmin@172.18.1.2 is unreachable", "unreachable": true}
        fatal: [appadmin@172.18.1.1]: UNREACHABLE! => {"changed": false, "msg": "Failed to connect to the host via ssh: ssh: connect to host 172.18.1.1 port 22: No route to host", "skip_reason": "Host appadmin@172.18.1.1 is unreachable", "unreachable": true}

        TASK [Example of handling unreachable hosts] *********************************************************************************
        changed: [appadmin@172.18.1.3]
        fatal: [appadmin@172.18.1.2]: UNREACHABLE! => {"changed": false, "msg": "Failed to connect to the host via ssh: ssh: connect to host 172.18.1.2 port 22: No route to host", "skip_reason": "Host appadmin@172.18.1.2 is unreachable", "unreachable": true}
        fatal: [appadmin@172.18.1.1]: UNREACHABLE! => {"changed": false, "msg": "Failed to connect to the host via ssh: ssh: connect to host 172.18.1.1 port 22: No route to host", "skip_reason": "Host appadmin@172.18.1.1 is unreachable", "unreachable": true}

        PLAY RECAP *******************************************************************************************************************
        appadmin@172.18.1.1        : ok=0    changed=0    unreachable=2    failed=0    skipped=2    rescued=0    ignored=0
        appadmin@172.18.1.2        : ok=0    changed=0    unreachable=2    failed=0    skipped=2    rescued=0    ignored=0
        appadmin@172.18.1.3        : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
        ```

        Here, Ansible skips tasks for unreachable hosts (172.18.1.1, 172.18.1.2), but continues on the reachable one (172.18.1.3).

        **Example 2: ignore_unreachable: false**

        ```yaml
        ---
        - hosts: all
          ignore_unreachable: false
          tasks:
            - name: "Example of handling unreachable hosts"
              ansible.builtin.shell: |
                echo "This is example of handling unreachable hosts"
        ```

        **Output:**   
        In this case, Ansible only runs tasks on the reachable host (172.18.1.3), and stops further tasks for unreachable hosts.

        ```bash
        PLAY [all] *******************************************************************************************************************

        TASK [Gathering Facts] *******************************************************************************************************
        ok: [appadmin@172.18.1.3]
        fatal: [appadmin@172.18.1.1]: UNREACHABLE! => {"changed": false, "msg": "Failed to connect to the host via ssh: ssh: connect to host 172.18.1.1 port 22: No route to host", "unreachable": true}
        fatal: [appadmin@172.18.1.2]: UNREACHABLE! => {"changed": false, "msg": "Failed to connect to the host via ssh: ssh: connect to host 172.18.1.2 port 22: No route to host", "unreachable": true}

        TASK [Example of handling unreachable hosts] *********************************************************************************
        changed: [appadmin@172.18.1.3]

        PLAY RECAP *******************************************************************************************************************
        appadmin@172.18.1.1        : ok=0    changed=0    unreachable=1    failed=0    skipped=0    rescued=0    ignored=0
        appadmin@172.18.1.2        : ok=0    changed=0    unreachable=1    failed=0    skipped=0    rescued=0    ignored=0
        appadmin@172.18.1.3        : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
        ```

        In this case, Ansible only runs tasks on the reachable host (172.18.1.3), and stops further tasks for unreachable hosts.

        **ignore_unreachable: true** allows you to handle scenarios where some hosts might be temporarily unreachable without stopping task execution.

        **ignore_unreachable: false** ensures that tasks only run on reachable hosts, and unreachable hosts are ignored after failing.






    - `Use pause to Step Through Playbooks`

    - `Use ansible-playbook --check (Dry Run)`

    - `Use ansible.builtin.fail to Force a Task to Fail`

    - `Run the Playbook on a Subset of Hosts`

    - `Ansible Debug Mode (ANSIBLE_DEBUG=true)`

    - `Run Playbook in Step Mode`
