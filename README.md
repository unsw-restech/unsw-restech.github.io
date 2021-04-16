This repository of User Documentation is built with Sphinx and hosted on github. You can read the [docs here](https://unsw-restech.github.io/)

Requirements are listed in the Pipfile, and managed with [Pipenv](https://pipenv-fork.readthedocs.io/en/latest/basics.html).
I was hoping for more from Pipenv, but it seems to be a hot mess. 

I have reverted to using plain pip, but note that chardet, idna and docutils might not be updatable because Sphinx and Pillow
have upper version limits below their current versions.

I am currently running a regular venv and updating via `pip-check -uH`
