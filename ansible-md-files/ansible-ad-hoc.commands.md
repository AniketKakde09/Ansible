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