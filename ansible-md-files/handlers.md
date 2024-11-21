## Handlers   
Handlers are specialized tasks that are only triggered if another task explicitly notifies them. They are typically used to perform actions such as restarting services, clearing cache, or reloading configurations when changes occur.

- **How to Use Handlers**   
To use a handler, you define it at the playbook level within the handlers section, and then trigger it using the notify directive within a task.

    - **Notify Directive:** Tasks can use the notify directive to signal when a handler should run.
    - **Handler Execution:** Handlers only run after all tasks in a play have completed. If multiple tasks notify the same handler, it will only run once.

**Example 1: Playbook Without Handlers**
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

**Example 2: Playbook Using Handlers**

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