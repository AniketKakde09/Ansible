## Use pause to Step Through Playbooks

In Ansible, debugging and troubleshooting can be challenging, especially when you're trying to understand how a playbook is executing. One effective way to gain control over the execution flow is by using the pause module. This module allows you to halt playbook execution temporarily, giving you time to review the current state, examine variables, or perform manual checks.

**How to Use the Pause Module**   
You can implement the pause module by adding a task that calls ansible.builtin.pause. This can be done with a specific message or a timeout period.

**Example 1: YAML file example using the pause module.**

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