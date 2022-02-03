# Change Log

Changes to the DHART installation playbook (and to the gEAR setup) will be documented in this file.

The format is based on [Keep a Changelog](http://keepachangelog.com/).
This project currently does NOT adheres to [Semantic Versioning](http://semver.org/).

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

    

### Fixed
- Install of python3 package mod-wsgi requires Apache and its runtime libraries to have been compiled, move install to `web-install`.

### Removed
- Libraries: libreadline-gplv2-dev, libsqlite-dev
- pathlib from python3 dependencies ()
