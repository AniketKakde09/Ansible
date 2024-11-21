## Handling Errors with Blocks   
In Ansible, managing errors effectively is crucial for creating robust and reliable automation scripts. One of the ways to handle errors is through the use of blocks. Blocks allow you to group tasks and apply error handling in a more organized way, making your playbooks easier to read and maintain.

- **How to Use Blocks**   
To define a block, you use the block keyword followed by a list of tasks. You can also define rescue and always sections to specify what to do when the block fails or what should always run regardless of the block's success.

**Example 1: YAML file example without using blocks.**
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

**Example 2: YAML file example using blocks for error handling.**
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