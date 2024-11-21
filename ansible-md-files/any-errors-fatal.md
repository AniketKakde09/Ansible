## Aborting a Play on All Hosts (any_errors_fatal)

In Ansible, sometimes a failure on any host should halt the entire play to prevent continued execution across other hosts. The fail_fast directive allows you to accomplish this by aborting a play as soon as a specified number of hosts encounter a failure. This can be crucial in environments where failures need immediate attention, or continued execution may introduce inconsistencies.

- **How to Use**   
By setting any_errors_fatal: true, Ansible will monitor task failures, and if one or more hosts fail, it will immediately stop the play for all hosts.

**Example 1: YAML File Example without any_errors_fatal**
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

**Example 2: YAML File Example with any_errors_fatal Enabled**
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