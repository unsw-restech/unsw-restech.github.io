title: Python

It is common for Python scripts to begin with:

```bash
#!/usr/bin/python
```

If you are using a Python module, you should instead use:

```bash
#!/usr/bin/env python
```

or, preferably:

```bash
#!/usr/bin/env python3
```

<h2> Conda and Anaconda </h2>

We receive many questions about installing Conda and Anaconda. These tools are not designed for multi-user environments, but they can be installed in your own home or scratch directories.

Installing Conda in your scratch directory is usually preferable, as Conda environments can grow quite large��especially if you maintain multiple environments.

Begin by downloading your preferred Conda distribution. The two main options are:

Anaconda (https://www.anaconda.com/
)

Miniconda (https://docs.anaconda.com/free/miniconda/
)

The instructions below use Miniconda. Once installed, Anaconda follows the same workflow.

Example installation and setup:

```bash 
Miniconda3-latest-Linux-x86_64.sh
conda activate
conda create --name my_conda
conda activate my_conda
conda install python
```
NOTE:
If you are using Conda for GPU-enabled software, ensure installation occurs on a GPU node during an interactive session.

<h2> Python Virtual Environments </h2>

Many Python packages can be installed using pip install. When this is possible, we strongly recommend using Python virtual environments (venv), especially if you are developing your own software or need packages that are not centrally installed.

<h3> Background </h3>

A virtual environment (or venv) is an isolated Python installation in your own directory. It allows you to install and manage packages without affecting system-wide Python installations or other users.

<h3> Setting up the default environment </h3>

Official documentation for virtual environments is available in the Python documentation:
https://docs.python.org/3/library/venv.html

To create a virtual environment, run:
```bash
python3 -m venv /path/to/new/virtual/environment
```

This directory contains the Python runtime and libraries and should not be used to store your Python scripts.

We recommend creating a dedicated directory for all virtual environments:
```bash
mkdir /home/z1234567/environments
```

<h3> Setting up the virtual environment </h3>

Katana provides multiple Python modules. If you have no specific requirements, load the most recent version available.

Example:
```bash
module load python/3.10.8
which python3
python3 -m venv /home/z1234567/environments/my_env
```
NOTE:
The which command shows the location of a command on your system. It is optional, but useful for verification.

Activate the virtual environment:
```bash
source /home/z1234567/environments/my_env/bin/activate
```
Once activated, the shell prompt changes and Python now runs from the virtual environment. For example:
```bash
which python3
/home/z1234567/environments/my_env/bin/python3
```

<h3> pip3 - the Python package manager ("the Package Installer for Python") </h3>

pip3 is used to list, install, and manage Python packages.

To view installed packages:
```bash
pip3 list

If a newer version of pip is available, upgrade it:

pip3 install --upgrade pip
pip3 install --upgrade setuptools
```

<h3> Installing software </h3>

To install a package into your virtual environment:
```bash
pip3 install numpy
```
Verify installed packages:
```bash
pip3 list
```

<h3> Exiting the venv, and returning later </h3>

To leave a virtual environment:
```bash
deactivate
```
You can create multiple environments side by side:

python3 -m venv /home/z1234567/environments/my_new_env
source /home/z1234567/environments/my_new_env/bin/activate

Installing a package such as SciPy will automatically install its dependencies (for example, NumPy):
```bash
pip install scipy

You may also install specific versions:

pip install scipy==1.2.3
```

<h2> Special Cases </h2>

If you want a virtual environment to use software already installed on Katana via environment modules, follow this workflow:

Load the required module

Create the virtual environment using the --system-site-packages flag

Install the required Python packages

Example using samtools with Jupyter:
```bash
module add samtools/1.15.1
python3 -m venv /home/z1234567/environments/my_samtools_env --system-site-packages
source /home/z1234567/environments/my_samtools_env/bin/activate
pip3 install pysam
```

You can now create a Jupyter kernel that uses this virtual environment.