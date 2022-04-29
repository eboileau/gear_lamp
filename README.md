Ansible Playbook install for the gEAR repository (DHART) and LAMP stack
-----------------------------------------------------------------------

Ansible version 2.11.7 (Python 3.7.3)
Python 3.9.2 (host server)

```bash
python3 -m venv ~/.pyvenv/ansible
source ~/.pyvenv/ansible/bin/activate
pip install --upgrade pip
python -m pip install ansible
```

The inventory file defines the nodes in which the stack is configured.
Defaults are set in `config file = /etc/ansible/ansible.cfg`.

Basic setup instructions are taken from the [https://github.com/IGS/gEAR/blob/main/docs/setup.new_server.notes.md](gEAR setup page).
However, we made significant changes, including upgrade to Python 3.9.2, and install of pinned version of Python packages in a virtual environment.
Selected packages were upgraded, and the installation/configuration was simplified. See [CHANGELOG.md](CHANGELOG).

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


To perform a dry-run, use

```
ansible-playbook site.yml --tags [tag] --check -v 2>&1 | tee output.log
```

**Note:** Before finalising the installation (before `db-config`), we need to manually checkout the **DHART** branch (and all upstream changes). There is no
plan to include this into the playbook.



### Install notes - 27.04.2022 (eboileau@gear)

- Installing Debian packages with apt randomly fails with `502 Connection closed [IP: 10.250.140.16 3142]`. The only solution so far is
to run the install until it works...

- TODO: Fix *Force reinstall selected Python packages* in `setup_pip.yaml` (upgrade?)

- Install diffxpy: for now, we did not do anything. We should probably follow the same fix by `sudo pip3 install git+https://github.com/adkinsrs/diffxpy.git@b2ebeb0fb7c6c215d51264cd258edf9d013ff021`.

- What is the order of precedence of Apache config? Setting `DocumentRoot /var/www` in custom config is not enough, we have to modify it in the 
`000-default.conf`, and restart the server. TODO: Fix this.




