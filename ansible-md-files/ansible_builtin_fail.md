## Use `ansible.builtin.fail` to Force a Task to Fail

In Ansible, tasks are generally designed to either succeed or fail based on the conditions you define. However, there might be situations where you want to explicitly force a task to fail, regardless of the task's state. This is where the ansible.builtin.fail module comes into play. It allows you to manually fail a task and control the flow of your playbook based on specific conditions.

- **How to Use ansible.builtin.fail**   
To use the ansible.builtin.fail module, simply include it as a task in your playbook. You can specify a custom error message that will be displayed when the task fails.

**Example 1: Forcing Failure Based on a Condition**

```yaml
---
---
- hosts: web
  tasks:
    - name: Check if service is present or not
      ansible.builtin.package_facts:

    - name: check if nginx is present or not
      ansible.builtin.fail:
        msg: "Nginx is not installed"
      when: '"nginx" not in ansible_facts.packages'
```

**OUTPUT**

```markdown

PLAY [web] *******************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************
ok: [appadmin@172.18.1.4]
ok: [appadmin@172.18.1.3]

TASK [Check if service is present or not] ************************************************************************************
ok: [appadmin@172.18.1.4]
ok: [appadmin@172.18.1.3]

TASK [check if nginx is present or not] **************************************************************************************
skipping: [appadmin@172.18.1.3]
fatal: [appadmin@172.18.1.4]: FAILED! => {"changed": false, "msg": "Nginx is not installed"}

PLAY RECAP *******************************************************************************************************************
appadmin@172.18.1.3        : ok=2    changed=0    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
appadmin@172.18.1.4        : ok=2    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0
```

- The Ansible playbook ran successfully on both hosts.
    - On appadmin@172.18.1.3, the NGINX check was skipped, due to nignx package was present.
    - On appadmin@172.18.1.4, the NGINX check failed, indicating that NGINX was not installed on the system.

**Conclusion**   
The ansible.builtin.fail module is an invaluable tool for ensuring the correct execution of playbooks. It allows you to intentionally stop the execution of a playbook when certain conditions aren't met, ensuring that your automation behaves as expected. By using this module with customized error messages, you can prevent unwanted actions and provide helpful feedback for troubleshooting and error resolution.



