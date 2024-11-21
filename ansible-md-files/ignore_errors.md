## Ignoring failed commands (ignore_errors)

By default, Ansible stops the execution of a playbook when a task fails on a host. This behavior can interrupt workflows when certain failures are not critical and can be safely ignored. To allow the execution of tasks to continue even if a particular task fails, we use the ignore_errors directive.

**How to Use**   
When you set ignore_errors: yes in a task, it tells Ansible to proceed with the rest of the playbook, even if that specific task fails.

**Example 1: YAML file example without using ignore_errors.**
```yaml
---
- hosts: web
  tasks:
    - name: Example of ignore_errors in ansible.
      ansible.builtin.shell: |
        echo "This is example of ignore_errors, in the below commmand we are setting exit code to 1 so that this module can fail"
        exit 1
      register: ShellModuleOutput
    - name: Print Output of Module :- Example of ignore_errors in ansible.
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
        echo "This is an example of ignore_errors, in the below command we are setting exit code to 1 so that this module can fail"
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