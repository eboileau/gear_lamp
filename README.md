# Ansible Playbook install for the gEAR repository (DHART) and LAMP stack

---

```bash
python3 -m venv ~/.venv/ansible
source ~/.venv/ansible/bin/activate
pip install --upgrade pip
pip install ansible

ansible --version
# ansible [core 2.16.2]
#   python version = 3.11.2
#   jinja version = 3.1.3
#   libyaml = True
```

The inventory file defines the nodes in which the stack is configured. Check [Ansible (quick setup)](#ansible).

Basic setup instructions were taken from the [gEAR setup page](https://github.com/IGS/gEAR/blob/main/docs/setup.new_server.notes.md).
This playbook is partly adpated from the [examples and best practices for building Ansible Playbooks](https://github.com/ansible/ansible-examples).

<a id="quickstart"></a>

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

**Note:** This playbook installs the **DHART** branch, and whatever latest commits were made, for instance values in `www/site_domain_prefs.json` are used _as is_. Default DB-related variables (`group_vars/all`) must match the settings in `gear.ini`, see also `create_schema.sql`.

**Note:** When running CGI scripts through the web server, `PATH` does not see the virtual environment. Solutions such as [How to use Python virtual environments with mod_wsgi](https://modwsgi.readthedocs.io/en/master/user-guides/virtual-environments.html) do not seem to work for CGI scripts. So far, the solution is to specify the full path to the interpreter (`#!/usr/local/envs/dhart/bin/python`), so that the shell can find the local installation when it attempts to execute CGI programs. Hence default variables (`group_vars/all`) such as `python_venv_name` _must_ match the shebang in all CGI scripts. I think this also holds for the `gEAR/www/p` script. For practical purposes, this has also been done for the `gEAR/bin` scripts (in particular to run `db-config`). If installing the **DHART** branch, then there is nothing to do.

**Note:** Running `db-config` will create the DB, download the annotations, and load them to the respective DB tables. This is currently done using scripts 
under `gEAR/bin` (another way would be to use MySQL dumps, but we would need an appropriate _loader_, _i.e._ we don't want to install the whole Ensembl data, or redefine tables). Some default variables are specified in `group_vars/all`, such as release number, _etc._, but URLs are hard coded in `dump_db.yaml`. The list of organisms (`db/configure/vars/main.yaml`) is fixed, and must match the one defined in the DB (`create_schema.sql`)! Currently, re-running `dump_db.yaml` is NOT harmless, this will (re-)load annotations each time to the DB (or results in an error for loading gene ontologies).

<a id="install"></a>

## Install

For recent changes to the installation, see [CHANGELOG](CHANGELOG.md). For installation notes, see [INSTALL](INSTALL.md)

<a id="ansible"></a>

## Ansible (quick setup)

Defaults are set in `config file = /etc/ansible/ansible.cfg`. A minimal configuration file looks like:

```
[defaults]
inventory = /etc/ansible/hosts/hosts.ini, /home/eboileau/.ansible_local_inventory.ini
remote_user = eboileau

[ssh_connection]
pipelining = True

[privilege_escalation]
become = True
```

where `/etc/ansible/hosts/hosts.ini` is *e.g.* 

```
gear ansible_host=gear_IP
dhart ansible_host=dhart_IP
```

and `/home/eboileau/.ansible_local_inventory.ini` is

```
[all:vars]
ansible_become_password=your_remote_password
```

To list available hosts, use `ansible all --list-hosts`, or list all `ansible-inventory --list`.
For more information on how to use Ansible, consult the [Ansible documentation](https://docs.ansible.com/ansible/latest/index.html).
