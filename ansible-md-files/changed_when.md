## Defining "changed"
In Ansible, a task is marked as "changed" when it modifies the target system. This could mean changing a configuration, installing a new package, or adding a file. Tracking what’s "changed" is useful because it helps us decide whether to trigger other tasks, like restarting a service, only when necessary.

- **How to Define and Control "Changed" Status**

    - **changed_when:** Customizes when a task is marked as "changed." This is useful for tasks that may or may not actually change something.

    - **check_mode:** Runs the task in a "dry run" mode, meaning it won’t change anything but will show if it would have changed something.
          
**Example 1: Playbook Without "Changed" Control**
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

**Example 2: Playbook Using changed_when**
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

**Example 3: Playbook Using check_mode**
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