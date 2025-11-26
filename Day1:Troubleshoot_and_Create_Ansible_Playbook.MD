An Ansible playbook needs completion on the jump host, where a team member left off. Below are the details: 

The inventory file /home/thor/ansible/inventory requires adjustments. The playbook must run on App Server 1 in Stratos DC. Update the inventory accordingly. 

Create a playbook /home/thor/ansible/playbook.yml. Include a task to create an empty file /tmp/file.txt on App Server 1. 

Note: Validation will run the playbook using the command ansible-playbook -i inventory playbook.yml. Ensure the playbook works without any additional arguments

## Solution:

```ini
thor@jumphost ~$ ls 
ansible 
thor@jumphost ~$ cd ansible/ 
thor@jumphost ~/ansible$ ls 
inventory 
thor@jumphost ~/ansible$ cat inventory 
stapp02 ansible_host=172.238.16.204 ansible_user=steve ansible_ssh_common_args='-o StrictHostKeyChecking=no'thor@jumphost
thor@jumphost ~$ vi inventory
```
### Replace with this:
```ini
stapp01 ansible_host=172.16.238.10 ansible_user=tony ansible_ssh_pass='Ir0nM@n' ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```
### then create playbook.yml with below contents
```ini
thor@jumphost ~$ vi playbook.yml
```
```ini
---
- name: Create file on App Server 1
  hosts: stapp01
  gather_facts: false

  tasks:
    - name: Create empty file /tmp/file.txt
      file:
        path: /tmp/file.txt
        state: touch
```

## Test and run:
```ini
thor@jumphost ~$ ansible -i inventory stapp01 -m ping
```
### Expected output:
```ini
stapp01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

### If unreachable, check the inventory SSH credentials.

## Then:
```ini
thor@jumphost ~$ ansible-playbook -i inventory playbook.yml
```

### Expected output:
```ini
PLAY [Create file on App Server 1] ******************************

TASK [Create empty file /tmp/file.txt] **************************
ok: [stapp01]

PLAY RECAP ********************************************************
stapp01                    : ok=1    changed=1    unreachable=0    failed=0
```

## Verification

### SSH into stapp01 and check the file:
```ini
ssh tony@172.16.238.10
ls -l /tmp/file.txt
```

### Output should show:
```ini
-rw-r--r-- 1 tony tony 0 Nov 25 12:00 /tmp/file.txt
```
