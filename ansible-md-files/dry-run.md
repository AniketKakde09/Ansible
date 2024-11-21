## Use ansible-playbook --check (Dry Run)

The --check flag performs a dry run, allowing you to preview what changes would be made without actually making any modifications to the target systems. This is useful for testing, validation, and troubleshooting.

The --check option tells Ansible to simulate the execution of tasks without making any real changes to the target systems. It performs the same steps as a normal run but only reports what would be done.

- **How to Use ansible-playbook --check**  
    Using the --check flag is straightforward. Simply add it when running the ansible-playbook command:

    ```bash
    ansible-playbook your_playbook.yml --check
    ```

    This will simulate the playbook execution and show you the potential changes that would occur.   

**Example 1: YAML file with using --check**

```yaml
---
- hosts: web
  tasks:

    - name: Create a file
      ansible.builtin.file:
        path: /tmp/example.txt
        state: touch

    - name: Ensure the file contains some text
      ansible.builtin.copy:
        dest: /tmp/example.txt
        content: "This is a dry run test for Ansible."

    - name: Check if file is created or not
      ansible.builtin.shell: |
        if [ -f "/tmp/example.txt" ]; then
          echo "File :- /tmp/example.txt is created"
        else
          echo "File :- /tmp/example.txt is missing"
        fi
      register: file_check

    - name: logs :- Check if file is created or not
      debug:
        var: file_check
```

**Command**   
Below command is used to execute playbook.
```bash
ansible-playbook -i inventory.ini example.yaml --check
```

**OUTPUT**

```markdown

PLAY [web] *******************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************
ok: [appadmin@172.18.1.4]
ok: [appadmin@172.18.1.3]

TASK [Create a file] *********************************************************************************************************
changed: [appadmin@172.18.1.4]
changed: [appadmin@172.18.1.3]

TASK [Ensure the file contains some text] ************************************************************************************
changed: [appadmin@172.18.1.3]
changed: [appadmin@172.18.1.4]

TASK [Check if file is created or not] ***************************************************************************************
skipping: [appadmin@172.18.1.3]
skipping: [appadmin@172.18.1.4]

TASK [logs :- Check if file is created or not] *******************************************************************************
ok: [appadmin@172.18.1.3] => {
    "file_check": {
        "changed": false,
        "cmd": "if [[ -f \"/tmp/example.txt\" ]]; then\n  echo \"File :- /tmp/example.txt is created\"\nelse\n  echo \"File :- /tmp/example.txt is missing\"\nfi\n",
        "delta": null,
        "end": null,
        "failed": false,
        "msg": "Command would have run if not in check mode",
        "rc": 0,
        "skipped": true,
        "start": null,
        "stderr": "",
        "stderr_lines": [],
        "stdout": "",
        "stdout_lines": []
    }
}
ok: [appadmin@172.18.1.4] => {
    "file_check": {
        "changed": false,
        "cmd": "if [[ -f \"/tmp/example.txt\" ]]; then\n  echo \"File :- /tmp/example.txt is created\"\nelse\n  echo \"File :- /tmp/example.txt is missing\"\nfi\n",
        "delta": null,
        "end": null,
        "failed": false,
        "msg": "Command would have run if not in check mode",
        "rc": 0,
        "skipped": true,
        "start": null,
        "stderr": "",
        "stderr_lines": [],
        "stdout": "",
        "stdout_lines": []
    }
}

PLAY RECAP *******************************************************************************************************************
appadmin@172.18.1.3        : ok=4    changed=2    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
appadmin@172.18.1.4        : ok=4    changed=2    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
```

- What happens during the dry run:   
    - Dry run mode (--check) simulates the tasks and shows what changes would be made, but it does not actually apply any changes to the system.   
    - In this case, it will show that it would create a file at /tmp/example.txt and copy the content into it, but no changes will be actually made.

**Example 2: YAML file without using --check**

```yaml
---
- hosts: web
  tasks:

    - name: Create a file
      ansible.builtin.file:
        path: /tmp/example.txt
        state: touch

    - name: Ensure the file contains some text
      ansible.builtin.copy:
        dest: /tmp/example.txt
        content: "This is a dry run test for Ansible."

    - name: Check if file is created or not
      ansible.builtin.shell: |
        if [ -f "/tmp/example.txt" ]; then
          echo "File :- /tmp/example.txt is created"
        else
          echo "File :- /tmp/example.txt is missing"
        fi
      register: file_check

    - name: logs :- Check if file is created or not
      debug:
        var: file_check
```

**Command**   
Below command is used to execute playbook.
```bash
ansible-playbook -i inventory.ini example.yaml --check
```

```markdown

PLAY [web] *******************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************
ok: [appadmin@172.18.1.4]
ok: [appadmin@172.18.1.3]

TASK [Create a file] *********************************************************************************************************
changed: [appadmin@172.18.1.4]
changed: [appadmin@172.18.1.3]

TASK [Ensure the file contains some text] ************************************************************************************
ok: [appadmin@172.18.1.3]
ok: [appadmin@172.18.1.4]

TASK [Check if file is created or not] ***************************************************************************************
changed: [appadmin@172.18.1.3]
changed: [appadmin@172.18.1.4]

TASK [logs :- Check if file is created or not] *******************************************************************************
ok: [appadmin@172.18.1.3] => {
    "file_check": {
        "changed": true,
        "cmd": "if [ -f \"/tmp/example.txt\" ]; then\n  echo \"File :- /tmp/example.txt is created\"\nelse\n  echo \"File :- /tmp/example.txt is missing\"\nfi\n",
        "delta": "0:00:00.004041",
        "end": "2024-11-21 09:38:03.574193",
        "failed": false,
        "msg": "",
        "rc": 0,
        "start": "2024-11-21 09:38:03.570152",
        "stderr": "",
        "stderr_lines": [],
        "stdout": "File :- /tmp/example.txt is created",
        "stdout_lines": [
            "File :- /tmp/example.txt is created"
        ]
    }
}
ok: [appadmin@172.18.1.4] => {
    "file_check": {
        "changed": true,
        "cmd": "if [ -f \"/tmp/example.txt\" ]; then\n  echo \"File :- /tmp/example.txt is created\"\nelse\n  echo \"File :- /tmp/example.txt is missing\"\nfi\n",
        "delta": "0:00:00.004084",
        "end": "2024-11-21 09:38:03.576919",
        "failed": false,
        "msg": "",
        "rc": 0,
        "start": "2024-11-21 09:38:03.572835",
        "stderr": "",
        "stderr_lines": [],
        "stdout": "File :- /tmp/example.txt is created",
        "stdout_lines": [
            "File :- /tmp/example.txt is created"
        ]
    }
}

PLAY RECAP *******************************************************************************************************************
appadmin@172.18.1.3        : ok=5    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
appadmin@172.18.1.4        : ok=5    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

**Conclusion**  
--check is a must-have tool in the toolbox of any Ansible user. It will let you do safe testing and validation of your playbooks, ensuring that they execute as expected BEFORE any changes are made to your systems. If you debug a new playbook or test and validate them before deploying, then an action of --check will bring you peace of mind, guarding against any errors in mission-critical environments.