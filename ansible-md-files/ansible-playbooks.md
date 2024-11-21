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
    - `Check ignore_errors, ignore_unreachable, handlers, failed_when, changed_when, Aborting a play on all hosts, Aborting on the first error: any_errors_fatal, Handling errors with blocks`   
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

     - **Handlers**   
      Handlers are specialized tasks that are only triggered if another task explicitly notifies them. They are typically used to perform actions such as restarting services, clearing cache, or reloading configurations when changes occur.

       - **How to Use Handlers**   
       To use a handler, you define it at the playbook level within the handlers section, and then trigger it using the notify directive within a task.

         - Notify Directive: Tasks can use the notify directive to signal when a handler should run.
         - Handler Execution: Handlers only run after all tasks in a play have completed. If multiple tasks notify the same handler, it will only run once.

       - **Example 1: Playbook Without Handlers**
          ```yaml
            ---
            - hosts: web
              tasks:
                - name: "Example of handling Handlers - Task 1"
                  ansible.builtin.shell: |
                    echo "Task 1"

                - name: "Service Restart task"
                  ansible.builtin.shell: |
                    echo "task to restart service once task 1 is executed"

                - name: "Example of handling Handlers - Task 2"
                  ansible.builtin.shell: |
                    echo "Task 2"
            ```

         **OUTPUT**

         ```bash
          PLAY [web] *******************************************************************************************************************

          TASK [Gathering Facts] *******************************************************************************************************
          ok: [appadmin@172.18.1.3]

          TASK [Example of handling Handlers - Task 1] *********************************************************************************
          changed: [appadmin@172.18.1.3]

          TASK [Service Restart task] **************************************************************************************************
          changed: [appadmin@172.18.1.3]

          TASK [Example of handling Handlers - Task 2] *********************************************************************************
          changed: [appadmin@172.18.1.3]

          PLAY RECAP *******************************************************************************************************************
          appadmin@172.18.1.3        : ok=4    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
        ```

         - Every Time the Playbook Runs: The service restarts, even if the task 1 was not changed. This creates unnecessary restarts, which could disrupt the service.   

        - **Example 2: Playbook Using Handlers**

          ```yaml
          ---
          - hosts: web
            become: true
            tasks:
              - name: "Task 1 - Update packages"
                apt:
                  update_cache: yes

              - name: "Task 2 - Install apache2"
                apt:
                  name: apache2
                  state: present
                notify: Restart Service

            handlers:
              - name: "Restart Service"
                shell: systemctl restart apache2
          ```

          **OUTPUT**

          ```bash
              PLAY [web] *******************************************************************************************************************

              TASK [Gathering Facts] *******************************************************************************************************
              ok: [appadmin@172.18.1.3]

              TASK [Task 1 - Update packages] **********************************************************************************************
              ok: [appadmin@172.18.1.3]

              TASK [Task 2 - Install apache2] **********************************************************************************************
              ok: [appadmin@172.18.1.3]

              PLAY RECAP *******************************************************************************************************************
              appadmin@172.18.1.3        : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
            ```

            - The service restarts only if the configuration file changes, improving efficiency and reducing unnecessary service disruptions.
            - Handlers simplify playbook execution by handling these types of conditional tasks elegantly.

     - **Defining Failure in Ansible**   
      In Ansible, failure occurs when a task does not achieve the intended outcome, either by returning a non-zero exit code or encountering an error in execution. By default, any task failure stops playbook execution on that host, but Ansible provides control over failure handling for more resilient automation.

       - **How to Define and Manage Failure**  
       Ansible provides mechanisms for handling task failures in ways that allow playbook flexibility:
         - **ignore_errors:** Allows a task to fail without stopping the playbook.
         - **failed_when:** Defines custom failure conditions, enabling tasks to continue based on specific criteria.
         - **any_errors_fatal:** Stops playbook execution on all hosts if a task fails on any host.

        - **Example 1: Playbook Without Failure Handling**
          ```yaml
            ---
            - hosts: web
              tasks:
                - name: "Install Package - Task 1"
                  ansible.builtin.shell: |
                    apt-get install non-existent-package
            ```

          **OUTPUT**
          ```bash
            PLAY [web] *******************************************************************************************************************

            TASK [Gathering Facts] *******************************************************************************************************
            ok: [appadmin@172.18.1.3]

            TASK [Install Package - Task 1] **********************************************************************************************
            fatal: [appadmin@172.18.1.3]: FAILED! => {"changed": true, "cmd": "apt-get install non-existent-package\n", "delta": "0:00:01.638607", "end": "2024-10-25 05:59:47.389864", "msg": "non-zero return code", "rc": 100, "start": "2024-10-25 05:59:45.751257", "stderr": "E: Unable to locate package non-existent-package", "stderr_lines": ["E: Unable to locate package non-existent-package"], "stdout": "Reading package lists...\nBuilding dependency tree...\nReading state information...", "stdout_lines": ["Reading package lists...", "Building dependency tree...", "Reading state information..."]}

            PLAY RECAP *******************************************************************************************************************
            appadmin@172.18.1.3        : ok=1    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0
          ```
            **Result:** The playbook halts when the package installation fails, which may not be desirable in all cases, especially if the failure is non-critical.

        - **Example 2: Playbook Using ignore_errors**
          ```yaml
          ---
          - hosts: web
            become: true
            tasks:
              - name: "Install Package - Task 1"
                ansible.builtin.shell: |
                  apt-get install non-existent-package
                register: install_package
                ignore_errors: true

              - name: "Ouput Of Install Package"
                debug:
                  var: install_package
          ```

          **OUTPUT**

          ```bash
          PLAY [web] *******************************************************************************************************************

          TASK [Gathering Facts] *******************************************************************************************************
          ok: [appadmin@172.18.1.3]

          TASK [Install Package - Task 1] **********************************************************************************************
          fatal: [appadmin@172.18.1.3]: FAILED! => {"changed": true, "cmd": "apt-get install non-existent-package\n", "delta": "0:00:01.736469", "end": "2024-10-25 06:04:05.707614", "msg": "non-zero return code", "rc": 100, "start": "2024-10-25 06:04:03.971145", "stderr": "E: Unable to locate package non-existent-package", "stderr_lines": ["E: Unable to locate package non-existent-package"], "stdout": "Reading package lists...\nBuilding dependency tree...\nReading state information...", "stdout_lines": ["Reading package lists...", "Building dependency tree...", "Reading state information..."]}
          ...ignoring

          TASK [Ouput Of Install Package] **********************************************************************************************
          ok: [appadmin@172.18.1.3] => {
              "install_package": {
                  "changed": true,
                  "cmd": "apt-get install non-existent-package\n",
                  "delta": "0:00:01.736469",
                  "end": "2024-10-25 06:04:05.707614",
                  "failed": true,
                  "msg": "non-zero return code",
                  "rc": 100,
                  "start": "2024-10-25 06:04:03.971145",
                  "stderr": "E: Unable to locate package non-existent-package",
                  "stderr_lines": [
                      "E: Unable to locate package non-existent-package"
                  ],
                  "stdout": "Reading package lists...\nBuilding dependency tree...\nReading state information...",
                  "stdout_lines": [
                      "Reading package lists...",
                      "Building dependency tree...",
                      "Reading state information..."
                  ]
              }
          }

          PLAY RECAP *******************************************************************************************************************
          appadmin@172.18.1.3        : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=1
          ```

          **Result:** By setting ignore_errors: true, the playbook ignores the failure and proceeds to print the logs of Install package task, ensuring essential tasks continue.

        - **Example 3: Playbook Using failed_when**

          ```yaml
          ---
          - hosts: web
            become: true
            tasks:
              - name: "Check If Java is installed or not"
                ansible.builtin.shell: |
                  echo "Check if java is installed or not on host"
                  if java -version >/dev/null 2>&1; then
                    echo "Java is installed"
                  else
                    echo "Java is not installed"
                  fi
                register: check_java
                failed_when: "'Java is not installed' in stdout_lines"

              - name: "Output Of Check If Java is installed or not"
                debug:
                  var: check_java
           ```

           **OUTPUT**

           ```bash
          PLAY [web] *******************************************************************************************************************

          TASK [Gathering Facts] *******************************************************************************************************
          ok: [appadmin@172.18.1.3]

          TASK [Check If Java is installed or not] *************************************************************************************
          fatal: [appadmin@172.18.1.3]: FAILED! => {"changed": true, "cmd": "echo \"Check if java is installed or not on host\"\nif java -version >/dev/null 2>&1; then\n  echo \"Java is installed\"\nelse\n  echo \"Java is not installed\"\nfi\n", "delta": "0:00:00.003978", "end": "2024-10-25 06:47:52.253265", "failed_when_result": "The conditional check ''Java is not installed' in stdout_lines' failed. The error was: error while evaluating conditional ('Java is not installed' in stdout_lines): 'stdout_lines' is undefined", "msg": "", "rc": 0, "start": "2024-10-25 06:47:52.249287", "stderr": "", "stderr_lines": [], "stdout": "Check if java is installed or not on host\nJava is not installed", "stdout_lines": ["Check if java is installed or not on host", "Java is not installed"]}

          PLAY RECAP *******************************************************************************************************************
          appadmin@172.18.1.3        : ok=1    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0
          ```

          **Result:** Here we are checking if java is installed or not, we are storing output of task "Check If Java is installed or not" in check_java. If check_java.stdout_lines contains "Java is not installed" then task be marked as failed.

          >**Note** Multiple conditions can be used in below mentioned format

          ```yaml
          #example of AND
          failed_when:
            - result.rc == 0
            - '"No such" not in result.stderr'
          ```
          This task will fail if both conditions are true

          ```yaml   
          #example of OR
          failed_when: >
            ("No such file or directory" in ret.stdout) or
            (ret.stderr != '') or
            (ret.rc == 10)
          ```
          If you have too many conditions to fit neatly into one line, you can split it into a multi-line YAML value with >

        - **Defining “changed”**   
         In Ansible, a task is marked as "changed" when it modifies the target system. This could mean changing a configuration, installing a new package, or adding a file. Tracking what’s "changed" is useful because it helps us decide whether to trigger other tasks, like restarting a service, only when necessary.

          - **How to Define and Control "Changed" Status**

            - **changed_when:** Customizes when a task is marked as "changed." This is useful for tasks that may or may not actually change something.
            - **check_mode:** Runs the task in a "dry run" mode, meaning it won’t change anything but will show if it would have changed something.
          
        - **Example 1: Playbook Without "Changed" Control**
          ```yaml
          ---
          - hosts: web
            tasks:
              - name: "Check if java exists"
                ansible.builtin.shell: |
                  if java -version >/dev/null 2>&1; then
                    echo "Java is installed"
                  else
                    echo "Java is not installed"
                  fi
                register: java_check

              - name: "Install java on system"
                ansible.builtin.shell: |
                  echo "sudo apt install java"
          ```
          **OUTPUT**
          ```bash
          PLAY [web] *******************************************************************************************************************

          TASK [Gathering Facts] *******************************************************************************************************
          ok: [appadmin@172.18.1.3]

          TASK [Check if java exists] **************************************************************************************************
          changed: [appadmin@172.18.1.3]

          TASK [Install java on system] ************************************************************************************************
          changed: [appadmin@172.18.1.3]

          PLAY RECAP *******************************************************************************************************************
          appadmin@172.18.1.3        : ok=3    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
          ```

       - **Example 2: Playbook Using changed_when**
          ```yaml
          ---
          - hosts: web
            tasks:
              - name: "Check if java exists"
                ansible.builtin.shell: |
                  if java -version >/dev/null 2>&1; then
                    echo "Java is installed"
                  else
                    echo "Java is not installed"
                  fi
                register: java_check
                changed_when: "'Java is not installed' in java_check.stdout_lines"
          ```
          **OUTPUT**
          ```bash
          PLAY [web] *******************************************************************************************************************

          TASK [Gathering Facts] *******************************************************************************************************
          ok: [appadmin@172.18.1.3]

          TASK [Check if java exists] **************************************************************************************************
          ok: [appadmin@172.18.1.3]

          PLAY RECAP *******************************************************************************************************************
          appadmin@172.18.1.3        : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
          ```
          **Result :** This Ansible playbook checks if Java is installed on the web host by running a shell command. It registers the output as java_check and considers a change only if the output indicates "Java is not installed."

        - **Example 3: Playbook Using check_mode**

          ```yaml
          ---
          - hosts: web
            tasks:
              - name: "Check if java exists"
                ansible.builtin.shell: |
                  if java -version >/dev/null 2>&1; then
                    echo "Java is installed"
                  else
                    echo "Java is not installed"
                  fi
                check_mode: true
          ```

          **OUTPUT**

          ```bash
          PLAY [web] *******************************************************************************************************************

          TASK [Gathering Facts] *******************************************************************************************************
          ok: [appadmin@172.18.1.3]

          TASK [Check if java exists] **************************************************************************************************
          skipping: [appadmin@172.18.1.3]

          PLAY RECAP *******************************************************************************************************************
          appadmin@172.18.1.3        : ok=1    changed=0    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
          ```

          **Result :** In this playbook, the check_mode: true option is used, which runs tasks in "check mode" to simulate changes without making actual modifications. As a result, the Check if java exists task is skipped, showing that no changes would be made if the playbook were executed normally.


        - **Aborting a Play on All Hosts (any_errors_fatal)**
        In Ansible, sometimes a failure on any host should halt the entire play to prevent continued execution across other hosts. The fail_fast directive allows you to accomplish this by aborting a play as soon as a specified number of hosts encounter a failure. This can be crucial in environments where failures need immediate attention, or continued execution may introduce inconsistencies.

          - **How to Use**   
          By setting any_errors_fatal: true, Ansible will monitor task failures, and if one or more hosts fail, it will immediately stop the play for all hosts.

          - **Example 1: YAML File Example without any_errors_fatal**
            ```yaml
              ---
              - hosts: web
                tasks:
                  - name: "Execute Shell"
                    ansible.builtin.shell: |
                      echo "Example of yaml file without using fail_fast."
                      exit 1
                    register: shelloutput

                  - name: "Print Output of shell"
                    debug:
                        var: shelloutput
              ```

            **OUTPUT**

            ```bash
            PLAY [web] *******************************************************************************************************************

            TASK [Gathering Facts] *******************************************************************************************************
            ok: [appadmin@172.18.1.3]
            ok: [appadmin@172.18.1.4]

            TASK [Execute Shell] **************************************************************************************************
            fatal: [appadmin@172.18.1.3]: FAILED! => {"changed": true, "cmd": "echo \"Example of yaml file without using fail_fast.\"\nexit 1\n", "delta": "0:00:00.004331", "end": "2024-10-26 10:52:31.230925", "msg": "non-zero return code", "rc": 1, "start": "2024-10-26 10:52:31.226594", "stderr": "", "stderr_lines": [], "stdout": "Example of yaml file without using fail_fast.", "stdout_lines": ["Example of yaml file without using fail_fast."]}
            fatal: [appadmin@172.18.1.4]: FAILED! => {"changed": true, "cmd": "echo \"Example of yaml file without using fail_fast.\"\nexit 1\n", "delta": "0:00:00.005215", "end": "2024-10-26 10:52:31.242259", "msg": "non-zero return code", "rc": 1, "start": "2024-10-26 10:52:31.237044", "stderr": "", "stderr_lines": [], "stdout": "Example of yaml file without using fail_fast.", "stdout_lines": ["Example of yaml file without using fail_fast."]}

            PLAY RECAP *******************************************************************************************************************
            appadmin@172.18.1.3        : ok=1    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0
            appadmin@172.18.1.4        : ok=1    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0
            ```

          - **Example 2: YAML File Example with any_errors_fatal Enabled**
            ```yaml
            ---
            - hosts: web
              any_errors_fatal: true
              tasks:
                - name: Execute Shell
                  ansible.builtin.shell: |
                    echo "Example of yaml file with error handling."
                    exit 1
                  register: shelloutput

                - name: Print output of shell
                  debug:
                    var: shelloutput
            ```

            **OUTPUT**

            ```bash
            PLAY [web] *******************************************************************************************************************

            TASK [Gathering Facts] *******************************************************************************************************
            ok: [appadmin@172.18.1.4]
            ok: [appadmin@172.18.1.3]

            TASK [Execute Shell] *********************************************************************************************************
            fatal: [appadmin@172.18.1.4]: FAILED! => {"changed": true, "cmd": "echo \"Example of yaml file with error handling.\"\nexit 1\n", "delta": "0:00:00.005813", "end": "2024-10-26 11:06:55.360730", "msg": "non-zero return code", "rc": 1, "start": "2024-10-26 11:06:55.354917", "stderr": "", "stderr_lines": [], "stdout": "Example of yaml file with error handling.", "stdout_lines": ["Example of yaml file with error handling."]}
            fatal: [appadmin@172.18.1.3]: FAILED! => {"changed": true, "cmd": "echo \"Example of yaml file with error handling.\"\nexit 1\n", "delta": "0:00:00.004574", "end": "2024-10-26 11:06:55.359492", "msg": "non-zero return code", "rc": 1, "start": "2024-10-26 11:06:55.354918", "stderr": "", "stderr_lines": [], "stdout": "Example of yaml file with error handling.", "stdout_lines": ["Example of yaml file with error handling."]}

            NO MORE HOSTS LEFT ***********************************************************************************************************

            PLAY RECAP *******************************************************************************************************************
            appadmin@172.18.1.3        : ok=1    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0
            appadmin@172.18.1.4        : ok=1    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0
            ```

            **Result :** any_errors_fatal: true stops further tasks if any host fails, but only after the current task finishes on all hosts in parallel.

          - **Handling Errors with Blocks**   
            In Ansible, managing errors effectively is crucial for creating robust and reliable automation scripts. One of the ways to handle errors is through the use of blocks. Blocks allow you to group tasks and apply error handling in a more organized way, making your playbooks easier to read and maintain.

            - **How to Use Blocks**

              To define a block, you use the block keyword followed by a list of tasks. You can also define rescue and always sections to specify what to do when the block fails or what should always run regardless of the block's success.

            - **Example 1: YAML file example without using blocks.**
              ```yaml
              ---
              - hosts: web
                tasks:
                  - name: "Run a command that might fail"
                    ansible.builtin.shell: |
                      echo "Running a command that fails."
                      exit 1
                    register: command_output

                  - name: "Print Output of Command"
                    debug:
                      var: command_output
              ```

              **OUTPUT**

              ```bash
              PLAY [web] *******************************************************************************************************************

              TASK [Gathering Facts] *******************************************************************************************************
              ok: [appadmin@172.18.1.4]
              ok: [appadmin@172.18.1.3]

              TASK [Run a command that might fail] *****************************************************************************************
              fatal: [appadmin@172.18.1.4]: FAILED! => {"changed": true, "cmd": "echo \"Running a command that fails.\"\nexit 1\n", "delta": "0:00:00.005808", "end": "2024-10-26 11:25:01.082384", "msg": "non-zero return code", "rc": 1, "start": "2024-10-26 11:25:01.076576", "stderr": "", "stderr_lines": [], "stdout": "Running a command that fails.", "stdout_lines": ["Running a command that fails."]}
              fatal: [appadmin@172.18.1.3]: FAILED! => {"changed": true, "cmd": "echo \"Running a command that fails.\"\nexit 1\n", "delta": "0:00:00.003787", "end": "2024-10-26 11:25:01.087448", "msg": "non-zero return code", "rc": 1, "start": "2024-10-26 11:25:01.083661", "stderr": "", "stderr_lines": [], "stdout": "Running a command that fails.", "stdout_lines": ["Running a command that fails."]}

              PLAY RECAP *******************************************************************************************************************
              appadmin@172.18.1.3        : ok=1    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0
              appadmin@172.18.1.4        : ok=1    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0
              ```
              In this example, when the command fails, the playbook stops executing, resulting in an error.

            - **Example 2: YAML file example using blocks for error handling.**

              ```yaml
              ---
              - hosts: web
                tasks:
                  - block: 
                      - name: "Run a command that might fail"
                        ansible.builtin.shell: |
                          echo "Running a command that fails."
                          exit 1
                        register: command_output

                      - name: "Print Output of Command"
                        debug:
                          var: command_output
                    rescue:
                      - name: "Executing Rescue section"
                        ansible.builtin.shell: |
                          echo "Handle error"
                        
                    always:
                      - name: "This task always runs"
                        debug:
                          msg: "This will run regardless of success or failure."
              ```

              **OUTPUT**

              ```bash
              PLAY [web] *******************************************************************************************************************

              TASK [Gathering Facts] *******************************************************************************************************
              ok: [appadmin@172.18.1.4]
              ok: [appadmin@172.18.1.3]

              TASK [Run a command that might fail] *****************************************************************************************
              fatal: [appadmin@172.18.1.3]: FAILED! => {"changed": true, "cmd": "echo \"Running a command that fails.\"\nexit 1\n", "delta": "0:00:00.005671", "end": "2024-10-26 11:32:36.045758", "msg": "non-zero return code", "rc": 1, "start": "2024-10-26 11:32:36.040087", "stderr": "", "stderr_lines": [], "stdout": "Running a command that fails.", "stdout_lines": ["Running a command that fails."]}
              fatal: [appadmin@172.18.1.4]: FAILED! => {"changed": true, "cmd": "echo \"Running a command that fails.\"\nexit 1\n", "delta": "0:00:00.004208", "end": "2024-10-26 11:32:36.045421", "msg": "non-zero return code", "rc": 1, "start": "2024-10-26 11:32:36.041213", "stderr": "", "stderr_lines": [], "stdout": "Running a command that fails.", "stdout_lines": ["Running a command that fails."]}

              TASK [Executing Rescue section] **********************************************************************************************
              changed: [appadmin@172.18.1.4]
              changed: [appadmin@172.18.1.3]

              TASK [This task always runs] *************************************************************************************************
              ok: [appadmin@172.18.1.3] => {
                  "msg": "This will run regardless of success or failure."
              }
              ok: [appadmin@172.18.1.4] => {
                  "msg": "This will run regardless of success or failure."
              }

              PLAY RECAP *******************************************************************************************************************
              appadmin@172.18.1.3        : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=1    ignored=0
              appadmin@172.18.1.4        : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=1    ignored=0
              ```
              
    - `Use pause to Step Through Playbooks`

      In Ansible, debugging and troubleshooting can be challenging, especially when you're trying to understand how a playbook is executing. One effective way to gain control over the execution flow is by using the pause module. This module allows you to halt playbook execution temporarily, giving you time to review the current state, examine variables, or perform manual checks.

      - **How to Use the Pause Module**   
        You can implement the pause module by adding a task that calls ansible.builtin.pause. This can be done with a specific message or a timeout period.

      - **Example 1: YAML file example using the pause module.**

        ```yaml
        ---
        - hosts: web
          tasks:
            - name: "Run a command"
              ansible.builtin.shell: |
                echo "Running a command that will complete quickly."
                sleep 2
              register: command_output

            - name: "Pause for user input"
              ansible.builtin.pause:
                prompt: "Press Enter to continue after reviewing the command output."

            - name: "Print Output of Command"
              debug:
                var: command_output
        ```
        **OUTPUT**

        ```bash
        PLAY [web] *******************************************************************************************************************

        TASK [Gathering Facts] *******************************************************************************************************
        ok: [appadmin@172.18.1.4]
        ok: [appadmin@172.18.1.3]

        TASK [Run a command] *********************************************************************************************************
        changed: [appadmin@172.18.1.4]
        changed: [appadmin@172.18.1.3]

        TASK [Pause for user input] **************************************************************************************************
        [Pause for user input]
        Press Enter to continue after reviewing the command output.:
        ^Mok: [appadmin@172.18.1.3]

        TASK [Print Output of Command] ***********************************************************************************************
        ok: [appadmin@172.18.1.3] => {
            "command_output": {
                "changed": true,
                "cmd": "echo \"Running a command that will complete quickly.\"\nsleep 2\n",
                "delta": "0:00:02.005826",
                "end": "2024-10-26 12:08:57.556930",
                "failed": false,
                "msg": "",
                "rc": 0,
                "start": "2024-10-26 12:08:55.551104",
                "stderr": "",
                "stderr_lines": [],
                "stdout": "Running a command that will complete quickly.",
                "stdout_lines": [
                    "Running a command that will complete quickly."
                ]
            }
        }
        ok: [appadmin@172.18.1.4] => {
            "command_output": {
                "changed": true,
                "cmd": "echo \"Running a command that will complete quickly.\"\nsleep 2\n",
                "delta": "0:00:02.006245",
                "end": "2024-10-26 12:08:57.546617",
                "failed": false,
                "msg": "",
                "rc": 0,
                "start": "2024-10-26 12:08:55.540372",
                "stderr": "",
                "stderr_lines": [],
                "stdout": "Running a command that will complete quickly.",
                "stdout_lines": [
                    "Running a command that will complete quickly."
                ]
            }
        }

        PLAY RECAP *******************************************************************************************************************
        appadmin@172.18.1.3        : ok=4    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
        appadmin@172.18.1.4        : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
        ```

    - `Use ansible-playbook --check (Dry Run)`

    - `Use ansible.builtin.fail to Force a Task to Fail`

    - `Run the Playbook on a Subset of Hosts`

    - `Ansible Debug Mode (ANSIBLE_DEBUG=true)`

    - `Run Playbook in Step Mode`
