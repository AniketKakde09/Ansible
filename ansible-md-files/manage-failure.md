## How to Define and Manage Failure
Ansible provides mechanisms for handling task failures in ways that allow playbook flexibility:
- **ignore_errors:** Allows a task to fail without stopping the playbook.
- **failed_when:** Defines custom failure conditions, enabling tasks to continue based on specific criteria.
- **any_errors_fatal:** Stops playbook execution on all hosts if a task fails on any host.

**Example 1: Playbook Without Failure Handling**
```yaml
---
- hosts: web

  tasks:
    - name: "Install Package - Task 1"
      ansible.builtin.shell: |
        apt-get install -y non-existent-package
      ignore_errors: yes  # To continue play even if package installation fails
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

**Example 2: Playbook Using ignore_errors**
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

    - name: "Output of Install Package"
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

**Example 3: Playbook Using failed_when**

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
# example of AND
failed_when:
  - result.rc == 0
  - '"No such" not in result.stderr'
```
This task will fail if both conditions are true

```yaml   
# example of OR
failed_when: >
  ("No such file or directory" in ret.stdout) or
  (ret.stderr != '') or
  (ret.rc == 10)
```
If you have too many conditions to fit neatly into one line, you can split it into a multi-line YAML value with >