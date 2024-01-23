## Install notes

### Install notes - eboileau@dhart

**2024 (Debian 12, Python 3.11)**

- Run `--tags setup-install` only.
- `MulticoreTSNE` installed from master, see [Error in builds using cmake](https://github.com/DmitryUlyanov/Multicore-TSNE/issues/81).

**prior to 2024 (Debian 11, Python 3.9)**

- Make sure the setfacl tool (provided by the acl package) is installed on the remote host. `dpkg --list | grep acl` should show libraries AND command line utilities. We had to install `apt-get install acl`. We did not add this to the playbook. Also make sure `wget` is installed.
- Skip `setup-config` tag.
- For `TASK [web/configure : Clone source code]`, we get `[WARNING]: Unable to use /var/www/.ansible/tmp as temporary directory, failing back to system: [Errno 13] Permission denied: '/var/www/.ansible'`. This is because we performs this task as Apache user (www-data), but /var/www is owned by root.
- After finalising the installation, install `diffxpy` (follow the same fix for now): `pip install git+https://github.com/adkinsrs/diffxpy.git@b2ebeb0fb7c6c215d51264cd258edf9d013ff021` in the dhart env (we need to activate the environment as www-data user, otherwise
the package will NOT be installed in the virtual environment). `pip show diffxpy` should show _e.g._ `/usr/local/envs/dhart/lib/python3.9/site-packages`.


### Install notes - eboileau@gear2

**2024 (Debian 12, Python 3.11)**

- No installation

**prior to 2024 (Debian 11, Python 3.9)**

- Installing Debian packages with apt randomly fails with `502 Connection closed [IP: 10.250.140.16 3142]`. This is _not_ systematically reproducible. This is a specific issue with our firewall/proxy set-up. The only solution so far is to run the install until it works. 
- After finalising the installation, install `diffxpy` (follow the same fix for now): `pip install git+https://github.com/adkinsrs/diffxpy.git@b2ebeb0fb7c6c215d51264cd258edf9d013ff021` in the dhart env (we need to activate the environment as www-data user, otherwise
the package will NOT be installed in the virtual environment). `pip show diffxpy` should show _e.g._ `/usr/local/envs/dhart/lib/python3.9/site-packages`.
- We skipped the `setup-config` tag in the latest install...
- For `TASK [web/configure : Clone source code]`, we get `[WARNING]: Unable to use /var/www/.ansible/tmp as temporary directory, failing back to system: [Errno 13] Permission denied: '/var/www/.ansible'`. This is because we performs this task as Apache user (www-data), but /var/www is owned by root.


