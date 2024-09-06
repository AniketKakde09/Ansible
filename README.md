# `Ansible`


### **What is ansible?**
```Ansible is open source IT automation tool that helps to automate task like provisioning, configuration management, application deployment and many more other IT process. and it is free to use.```

### **How Ansible Works?**
| `Feature`                | `Description`                                                                                     |
|------------------------|-------------------------------------------------------------------------------------------------|
| **Agentless Architecture** | *No software installation is required on managed nodes.*                                          |
| **SSH Communication**       | *Ansible uses SSH to communicate with managed nodes.*                                             |
| **Task Execution**          | *Connects via SSH, pushes out small programs (Ansible modules) to perform tasks, then removes them automatically.* |


### **How to Install Ansible Using Python's pip (Version 3.8.19)**
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
    This command should display the installed version of Ansible, confirming the successful installation.

### **Creating a Passwordless SSH Connection with the Control Node**
- `Setp 1: Generate SSH Key: Run ssh-keygen on the master server, keeping all values default. This will generate a key pair.`
  
- `Step 2: Copy the Generated Key to the Control Node: Use the following command to copy the generated key to the control node:`
  ```shell
  ssh-copy-id -i <file_path> <user>@<ip/host>
  ```
- `Step 3: Test SSH Connection: Try logging in using:`
  ```shell
  ssh <user>@<ip/host>
  ```
- `Note: The control node is where Ansible is installed, and the managed node is your target node.`

### **Inventory**
- `Step 1: Definition: The inventory is a fundamental component of Ansible that defines details of remote systems to be managed.`
  
- `Step 2. Purpose: It provides information about remote hosts during Ansible operations. below is an example of an inventory file:`
  ```ini
  # File: inventory.ini
  [middleware_servers]
  <user>@<hostname/ip>
  <user>@<hostname/ip>

  [log_servers]
  <user>@<hostname/ip>
  <user>@<hostname/ip>
  ```

### **Playbook**
`What is an Ansible Playbook?`  
- A playbook in Ansible is a YAML file that defines the steps to manage a system’s configuration or tasks.

`Playbook Structure:`  
- YAML files start with ---, You declare name, hosts, and remote-user.  
- Under tasks, define your tasks as required.
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
  >*This example creates a Hello_World.txt file in the /home/ansible path on all managed nodes.*

### **Ansible Modules**
```1. Definition: Ansible modules are units of code that can control system resources or execute system commands.```  
  
```2. Module Library: Ansible provides a library of modules that can be executed directly on remote hosts or through playbooks. Custom modules can also be created.```

```3. Commonly Used Modules: Examples include ansible.builtin.copy, ansible.builtin.command, ansible.builtin.file.```

### **Executing Your First Ansible Ad-Hoc Command**
```1. Create inventory.ini file: List all your target servers in it.```  
  
`2. Execute a Ping Command:`  
  ```bash
  ansible -i inventory.ini -m ping all
  ```
  **expected output**
  ```json
  <User>@<IP/HostName> | SUCCESS => {
      "ansible_facts": {
          "discovered_interpreter_python": "/usr/bin/python"
      },
      "changed": false,
      "ping": "pong"
  }
  ```
  >*This indicates that the control and managed nodes can communicate with each other.*


### **Executing Your First Ansible Playbook**
- ```1. Create inventory.ini file: Include all your target servers.```  

- ```2. Create YAML Playbook: Save the following content as first-playbook.yml:```
  ```yaml
  ---
  - hosts: all
    tasks:
      - name: "Create File on all managed nodes"
        ansible.builtin.shell: |
          echo "creating file Ansible.txt"
          touch Ansible.txt
  ```
- ```3. Run the Playbook: Execute it using:```
    ```bash
    ansible-playbook -i inventory.ini first-playbook.yml 
    ```
    **expected output:**
    ```markdown
    PLAY [all] *******************************************************************************************************************

    TASK [Gathering Facts] *******************************************************************************************************
    ok: [<user>@<IP/Hostname>]

    TASK [Create file on all managed servers] ************************************************************************************
    changed: [<user>@<Hostname>]

    PLAY RECAP *******************************************************************************************************************
    <user>@<Hostname>      : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
  ```  

### **Conclusion**
- `Ansible is a powerful tool that simplifies IT automation tasks through its agentless architecture and straightforward playbook system. By following this guide, you’ve learned how to install Ansible, configure passwordless SSH connections, create an inventory file, and write and execute your first playbook. These foundational steps will help you automate various IT processes efficiently and effectively.`


***

### **Ansible Role**
#### **Defination**
  `1. Ansible roles is a structured way to organize tasks, files, variables, template and handlers.`  
  `2. It promotes reuseability and modularity and consistancy across different playbooks.`  
  `3. It helps to encapsulate and modularize tasks.`  
#### **Scenario**
- `Imagine we have an Ubuntu node on which we want to install the Apache2 service and then deploy an index.html page. We can achieve this using Ansible in two ways: by writing a straightforward playbook or by creating a structured role.`
- **1st Way: Using a Simple Ansible Playbook**
  
  - Start by creating a YAML file named apache2.yaml, which will define the tasks required to install Apache2, start the service, and deploy the index.html file. Below is the content of the apache2.yaml file:
    ```yaml
    # apache2.yaml
    ---
    - hosts: app
      become: true
      become_user: root
      become_method: sudo
      tasks:
        - name: Install apache httpd  (state=present is optional)
          ansible.builtin.package:
            name: apache2
            state: present
        - name: Start service httpd, if not started
          ansible.builtin.command:
            cmd: sudo service apache2 restart
        - name: Copy file with owner and permissions
          ansible.builtin.copy:
            src: index.html
            dest: /var/www/html
            owner: appadmin
            group: appadmin
            mode: '0644'
      ```
    **Explanation of the Playbook:**  
    | **Task Name** | **Module** | **Description** |
    |-----------|--------|-------------|
    | **Install Apache HTTPD** | `ansible.builtin.package` | Installs the Apache2 package on the target node. The state: present ensures that the package is installed. |
    | **Start Service HTTPD** | `ansible.builtin.command` | Restarts the Apache2 service on the target node using a shell command. Ensures the service is running. |
    | **Copy File with Owner and Permissions** | `ansible.builtin.copy` | Copies the `index.html` file from the local machine to the target directory `(/var/www/html)` with specific ownership `(appadmin)` and permissions `(0644)`. |
    


  - **Step 2: Create the Inventory File**
    - Create an inventory file named inventory.ini to define the target hosts where the playbook will be executed.Below is an example of the inventory.ini file.
      ```ini
      # inventory.ini
      [app]
      appadmin@172.18.1.1
      ```
      `'[app]': Defines a group named app.`  
      `appadmin@172.18.1.1: Specifies the managed node (replace with your actual IP address and user).`
    

  - **Step 3: Create the `index.html` Page**
    - Next, create the index.html file with the content you want to deploy. Save it in the same directory as your playbook. Below is an example.  
      
      ```html
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
          <meta name="viewport" content="width=device-width, initial-scale=1.0">
          <title>Explore Ansible</title>
          <style>
              body {
                  font-family: Arial, sans-serif;
                  display: flex;
                  justify-content: center;
                  align-items: center;
                  height: 100vh;
                  margin: 0;
                  background-color: #f0f0f0;
              }
              h1 {
                  color: #333;
              }
          </style>
      </head>
      <body>
          <h1>Hello Ansible!!</h1>
      </body>
      </html>
      ```
    - **Execute ansible playbook using below mentioned command**
      ```bash
      ansible-playbook -i inventory.ini apache2.yaml
      ```
      **expected output**
      ```markdown
      `PLAY [app] *******************************************************************************************************************

      TASK [Gathering Facts] *******************************************************************************************************
      ok: [appadmin@172.18.1.1]

      TASK [Install apache httpd  (state=present is optional)] *********************************************************************
      ok: [appadmin@172.18.1.1]

      TASK [Start service httpd, if not started] ***********************************************************************************
      changed: [appadmin@172.18.1.1]

      TASK [Copy file with owner and permissions] **********************************************************************************
      ok: [appadmin@172.18.1.1]

      PLAY RECAP *******************************************************************************************************************
      appadmin@172.18.1.1        : ok=4    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
      ```
    - Once execution is completed you can verify if we are able to access index.html page or not using below command.
      ```bash
      curl http://172.18.1.1
      ```
      **expected output**


      ````html
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
          <meta name="viewport" content="width=device-width, initial-scale=1.0">
          <title>Learn with Aniket</title>
          <style>
              body {
                  font-family: Arial, sans-serif;
                  display: flex;
                  justify-content: center;
                  align-items: center;
                  height: 100vh;
                  margin: 0;
                  background-color: #f0f0f0;
              }
              h1 {
                  color: #333;
              }
          </style>
      </head>
      <body>
          <h1>Hello Ansible!!</h1>
      </body>
      </html>
      ```

- **2nd way: By following ansible roles**

  `Ansible roles allow for a cleaner, more organized way to handle playbooks by separating tasks, handlers, variables, and files into different directories. Here's how the previous playbook example can be restructured using roles.`

  - **Step 1: Create a Role Directory Structure**
    Roles follow a specific directory structure. For our Apache example, the structure would look like this:
    ```
    roles/
    └── apache/
        ├── tasks/
        │   └── main.yml   # Contains the playbook tasks
        ├── handlers/
        │   └── main.yml   # Contains actions triggered by tasks
        ├── templates/
        │   └── index.html  # HTML template
        └── vars/
            └── main.yml   # Variables used within the role
    ```
    To create a role use below mentioned command.
    ```bash
    ansible-galaxy role init apache2
    ```
    **expected output**

    ```markdown
    - Role apache2 was created successfully
    ``` 

  - **Step 2: Example Role Tasks (tasks/main.yml)**  
    `This file will contain the main tasks for installing Apache, starting the service, and copying the index.html file.`  
    ```yaml
    # file content of apache2/tasks/main.yml
    ---
    # roles/apache/tasks/main.yml
    ---
    - name: Install Apache HTTPD
      ansible.builtin.package:
        name: "{{ apache_package }}"
        state: present

    - name: Start Apache service
      ansible.builtin.command:
        cmd: sudo service "{{ apache_service }}" restart

    - name: Copy index.html to web root
      ansible.builtin.copy:
        src: "{{ html_file }}"
        dest: "{{ web_root }}/{{ html_file }}"
        owner: "{{ file_owner }}"
        group: "{{ file_group }}"
        mode: "{{ file_mode }}"
    ```
    - `{{ apache_package }}, {{ apache_service }}, and other placeholders are referencing the variables defined in vars/main.yml.`
    - `This approach makes it easy to update values like file paths, ownership, and permissions in one central place (the vars/main.yml file) without having to modify the task definitions directly.`
  
  - **Step 3: Example Handler (handlers/main.yml)**  
    `Handlers are tasks that are triggered by other tasks, such as restarting services when a file changes.`
    ```yaml
    # roles/apache/handlers/main.yml
    ---
    - name: Restart Apache
      ansible.builtin.service:
        name: apache2
        state: restarted
    ```

  - **Step 4: Example Variable (vars/main.yml)**  
    `Variables can be defined in this file. For instance, you might want to specify the package name or template file location.`
    ```yaml
    # roles/apache/vars/main.yml
    ---
    apache_package: apache2
    web_root: /var/www/html
    apache_service: apache2
    html_file: index.html
    file_owner: appadmin
    file_group: appadmin
    file_mode: '0644'
    ```

  - **Step 5: HTML Template (templates/index.html)**   
    `Move the index.html that we have created eariler or create new with below content.`
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Explore Ansible</title>
        <style>
            body {
                font-family: Arial, sans-serif;
                display: flex;
                justify-content: center;
                align-items: center;
                height: 100vh;
                margin: 0;
                background-color: #f0f0f0;
            }
            h1 {
                color: #333;
            }
        </style>
    </head>
    <body>
        <h1>Hello Ansible!!</h1>
    </body>
    </html>
    ```

  - **Step 6: Main Playbook File (apache2.yaml)**  
    `This is the main playbook that references the role. The playbook becomes much simpler as all logic is now handled by the role.`

    ```yaml
    # apache2.yaml
    ---
    - hosts: app
      become: true
      become_user: root
      become_method: sudo

      roles:
        - apache
    ```

  - **Step 7: Execute the Playbook**  
    `Finally, you can run the playbook just as before, using:`
    ```bash
    ansible-playbook -i inventory.ini apache2.yaml
    ```
  
  **By following this structure, the playbook is cleaner, more modular, and reusable, making it easier to manage large infrastructures.**
  ___
  | **Directory/File** | **Description** | **Example** |
  | --------- | ---------------------------- | --------------------- |
  | **defaults/** | Contains default variables for the role. These variables have the lowest precedence and can be overridden. | `yaml # defaults/main.yml apache_port: 80` |
  | **files/** | Stores static files that can be copied to the managed nodes using the copy or fetch modules. | `html <!-- files/index.html --> <html> <h1>Hello, Ansible!</h1> </html>` |
  | **handlers/** | Contains handlers, which are tasks triggered by other tasks. Typically used for actions like service restarts. | `yaml # handlers/main.yml - name: Restart Apache ansible.builtin.service: name: apache2 state: restarted` |
  | **meta/** | Contains metadata about the role, such as role dependencies. | `yaml # meta/main.yml dependencies: - { role: firewall, apache_ports: [80, 443] }` |
  | **README.md**	| Documentation for the role. It describes variables, usage instructions, and general information. | `markdown # Apache Role Installs and configures Apache web server.` |
  | **tasks/**	| Defines the main tasks the role performs, such as installing software or configuring services. | `yaml # tasks/main.yml - name: Install Apache ansible.builtin.package: name: apache2 state: present` |
  | **templates/** | Contains Jinja2 templates, which are dynamic files that can include variables and logic for customization. | `html <!-- templates/vhost.j2 --> <VirtualHost *:{{ apache_port }}> DocumentRoot {{ web_root }} </VirtualHost>` |
  | **tests/** | Contains test playbooks and inventory files to test the role locally or in CI pipelines. | `yaml # tests/test.yml - hosts: localhost roles: - apache` |
  | **vars/** | Stores variables that are specific to the role. Variables here have higher precedence than those in defaults. | `yaml # vars/main.yml web_root: /var/www/html apache_package: apache2` |

### **Conclusion**  
`By organizing playbooks into roles, you can separate different aspects of configuration management, making your automation tasks more modular and reusable. Roles allow you to manage complex setups with ease by grouping tasks, variables, files, and handlers logically. With roles, maintaining, scaling, and reusing your Ansible code becomes more efficient and manageable, improving the overall automation process in a production environment.`

___

