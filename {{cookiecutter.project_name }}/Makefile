USER = {{ cookiecutter.user }}
PW = {{ cookiecutter.pw }}
HOST = {{ cookiecutter.host }}
PROJECT_ROOT = {{ cookiecutter.project_root }}
VENV_PATH = {{ cookiecutter.venv_path }}
PYTHON_PATH = $(VENV_PATH)/bin/python

create_venv:
	sshpass -p $(PW) ssh $(USER)@$(HOST) "python3 -m venv $(VENV_PATH)"

install_packages:
	remote_pip.sh -h $(HOST) -c ./src/config

setup_env: create_venv install_packages
	echo "Done creating the environment!"

copy_scripts:
	sshpass -p $(PW) rsync -av -e ssh --exclude='config/config.cfg' ./src $(USER)@$(HOST):$(PROJECT_ROOT) && echo success!

run: copy_scripts
	sshpass -p $(PW) ssh $(USER)@$(HOST) "$(PYTHON_PATH) $(PROJECT_ROOT)/src/main.py"