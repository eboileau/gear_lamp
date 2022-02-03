Ansible Playbook install for the gEAR repository and LAMP stack
---------------------------------------------------------------

Ansible version 2.11.7
Python version 3.9.2 (host server)


```
python3 -m venv ~/.pyvenv/ansible
source ~/.pyvenv/ansible/bin/activate
pip install --upgrade pip
python -m pip install ansible
```

The inventory file defines the nodes in which the stack is configured.
Defaults are set in `config file = /etc/ansible/ansible.cfg`.

Basic setup instructions are taken from the [https://github.com/IGS/gEAR/blob/main/docs/setup.new_server.notes.md](gEAR setup page).
However, we decided to avoid installing Python 3.7.3 along with Python 3.9.2, so we are using the latest.

This playbook is partly adpated from the [https://github.com/ansible/ansible-examples](examples and best practices for building Ansible Playbooks).

The stack is deployed using the following command:

```
ansible-playbook site.yml --tags [tag] -v 2>&1 | tee output.log
```

Each role can be deployed in turn with the `--tags` option, *e.g.*

* setup-install: Install required libraries and python packages
* setup-config: Configure setup 
* web-install: Install Apache and PHP modules
* web-config: Configure web server
* db-install: Install MySQL
* db-config: Configure MySQL, create DB user, etc.
