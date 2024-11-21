## Ignoring unreachable host errors (ignore_unreachable)   

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
        echo "This is an example of handling unreachable hosts"
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
        echo "This is an example of handling unreachable hosts"
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