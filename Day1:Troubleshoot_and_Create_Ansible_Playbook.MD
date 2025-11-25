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
stapp02 ansible_host=172.238.16.204 ansible_user=steve ansible_ssh_common_args='-o StrictHostKeyChecking=no'thor@jumphost ~/ansible$
```
