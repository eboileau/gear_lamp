# Change Log

Changes to the DHART installation playbook (and to the gEAR setup) will be documented in this file.

The format is based on [Keep a Changelog](http://keepachangelog.com/).
This project currently does NOT adheres to [Semantic Versioning](http://semver.org/).


## [Unreleased] - 2022-05

### Changed
- Updated version of scanpy and immediate dependencies to solve h5ad reading errors in the portal.

## [Unreleased] - 2022-04

### Changed
- gear.conf is now the only VH configuration enabled, and the python-home is set to the virtual environment. 
- Perform install of python packages in a virtual environment, however install selected packages via apt to avoid errors (see below). Added python3-virtualenv, python3-tk, and python3-opencv to the list. Note that this requires changing the WSGI scripts, and also the custom Apache config, see e.g. https://modwsgi.readthedocs.io/en/master/user-guides/virtual-environments.html#daemon-mode-multiple-applications.

### Added 
- VH config for gear to avoid messing with existing default configs.
- Set file permission for upload.

### Removed
- mod-wsgi==4.9.0 and use default Debian libapache2-mod-wsgi-py3. Remove configuration `LoadModule wsgi_module`, and Python-related lines.
- Removed all SSL-related stuff. This will be handled separately afterwards.
- Password for MySQL.

## [Unreleased] - started 2022-01-01

### Changed
- Use system Python 3.9.2 instead of Python 3.7.3 
- Install of base python3 packages via Debian package manager avoids installing missing libraries/compilers on a fresh system (due to e.g. numpy being compiled): python3-numpy, python3-scipy, python3-sklearn, python3-pandas. As a results we cannot satisfy pinned versions of numpy==1.18.5, scipy==1.4.1, scikit-learn==0.23.1, and pandas==1.0.4.
- Upgraded selected python3 packages, due to version conflict, or Python 3.9 support issues/missing versions: numba and dependencies (e.g. llvmlite), opencv-python, Flask, and dash. 
- Upgraded and pinned un-pinned python3 packages.
- Libraries: libapache2-mod-wsgi-py3

### Added
- Install of python3: venv, pip, and setuptools via Debian package manager.
- Libraries: build-essential, git.
- List of python3 packages for reference: environment.txt.
- python3-mysqldb required to create application DB in ansible

### Fixed
- Install of python3 package mod-wsgi requires Apache and its runtime libraries to have been compiled, move install to `web-install`. Upgraded to 4.9.0.
- Reinstall Flask-RESTful==0.3.9 and update requirements due to wrong import see e.g. https://github.com/flask-restful/flask-restful/pull/913
- DocumentRoot /var/www/html changed to DocumentRoot /var/www in both /etc/apache2/sites-available/000-default.conf and /etc/apache2/sites-available/default-ssl.conf (TODO: ADD TO PLAYBOOK).
- MySQL layout table: changed INSERT INTO layout VALUES to avoid `Column count doesn't match value count at row 1` (TODO: ADD TO PLAYBOOK).

### Removed
- Libraries: libreadline-gplv2-dev, libsqlite-dev
- pathlib from python3 dependencies ()

