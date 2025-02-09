---
title: Python Environments
icon: material/language-python
hide:
  - toc
---
# Python Environments

Miniconda is a minimal implementation of [Anaconda](https://docs.anaconda.com/miniconda/) that includes only `conda`, Python, and a minimal set of dependencies. Conda provides simplified virtual environment management with individual Python versions isolated to the specified environment. 

Installers for macOS, Linux, and Windows available at: [https://docs.anaconda.com/miniconda/install/](https://docs.anaconda.com/miniconda/install/). 

!!! warning "Caution"

    The graphical installer for macOS installs to `/opt/miniconda3`. Use the terminal installer to ensure installation to your home directory.

## Commands

`conda info --envs`

:   List all available environments.

`conda create -n ENVNAME python=3.12.8`

:   Create a new environment with a specified version of Python.

`conda activate ENVNAME`

:   Activate (switch to) the specified environment.

`conda install PKGNAME=VERSION`

:   Install the specified package and version in the current environment.

`conda list --show-channel-urls`

:   List installed packages in the current environment.

`conda update --all -n ENVNAME`

:   Update all packages in the specified environment.

See the [Conda User Guide Cheatsheet](https://docs.conda.io/projects/conda/en/latest/user-guide/cheatsheet.html) for additional information. 