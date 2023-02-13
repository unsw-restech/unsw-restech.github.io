This repository of User Documentation is built with Sphinx and hosted on
GitHub.  You can read the [docs here](https://unsw-restech.github.io/).

To build, go into /source, and use the MakeFile.  `make html` will allow
a local preview, `make prep` to generate the PDF, and `make github`
generates publicly displayed HTML pages in /docs.

Pushing back to remote master will automatically generate the github.io
pages.

To generate the PDF, texlive-lualatex and xindy need to be installed on
your system.


## Lachlan's notes about requirement management

Requirements are listed in the Pipfile, and managed with
[Pipenv](https://pipenv-fork.readthedocs.io/en/latest/basics.html).  I
was hoping for more from Pipenv, but it seems to be a hot mess.

I have reverted to using plain pip, but note that chardet, idna and
docutils might not be updatable because Sphinx and Pillow have upper
version limits below their current versions.

I am currently running a regular venv and updating via `pip-check -uH`.
