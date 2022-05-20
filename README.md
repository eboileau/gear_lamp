# Ansible Playbook install for the gEAR repository (DHART) and LAMP stack

---

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

Basic setup instructions are taken from the [gEAR setup page](https://github.com/IGS/gEAR/blob/main/docs/setup.new_server.notes.md).
However, we made significant changes, including upgrade to Python 3.9.2, and install of pinned version of Python packages in a virtual environment.
Selected packages were upgraded, and the installation/configuration was simplified. See [CHANGELOG](CHANGELOG.md).

This playbook is partly adpated from the [examples and best practices for building Ansible Playbooks](https://github.com/ansible/ansible-examples).

## Quickstart

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
plan to include this into the playbook! Default DB-related variables (`group_vars/all`) must match the settings _e.g_ in `gear.ini`.

**Note:** When running CGI scripts through the web server, `PATH` does not see the virtual environment. Solutions such as [How to use Python virtual environments with mod_wsgi](https://modwsgi.readthedocs.io/en/master/user-guides/virtual-environments.html) do not seem to work for CGI scripts. So far, the solution is to specify the full path to the interpreter (`#!/usr/local/envs/dhart/bin/python`), so that the shell can find the local installation when it attempts to execute CGI programs. Hence default variables (`group_vars/all`) such as `python_venv_name` _must_ match the shebang in all CGI scripts. I think this also holds for the `www/p` script.


### Install notes - eboileau@gear

- Installing Debian packages with apt randomly fails with `502 Connection closed [IP: 10.250.140.16 3142]`. The only solution so far is
to run the install until it works. This is _not_ reproducible.

- After finalising the installation, install `diffxpy` (follow the same fix): `pip install git+https://github.com/adkinsrs/diffxpy.git@b2ebeb0fb7c6c215d51264cd258edf9d013ff021` in the dhart env (we need to activate the environment as www-data user, otherwise
the package will NOT be installed in the virtual environment).

- Permissions need to be writeable for user. We did `cd dhart/gEAR/www/` and `sudo chmod 777 datasets analyses/* uploads/files/`. This has now
been added to the playbook.
