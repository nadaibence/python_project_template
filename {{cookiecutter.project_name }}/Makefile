USER = {{ cookiecutter.user }}
PW = {{ cookiecutter.pw }}
HOST = {{ cookiecutter.host }}
PROJECT_ROOT = {{ cookiecutter.project_root }}
VENV_PATH = {{ cookiecutter.venv_path }}
PYTHON_PATH = $(VENV_PATH)/bin/python

create_venv:
	@sshpass -p $(PW) ssh $(USER)@$(HOST) "python3 -m venv $(VENV_PATH)"

install_packages:
	remote_pip.sh -h $(HOST) -c ./src/config

setup_env: create_venv install_packages
	@echo "#### Done creating the environment! ####"

copy_scripts:
	@sshpass -p $(PW) rsync -av -e ssh --exclude='config/config.cfg' ./src $(USER)@$(HOST):$(PROJECT_ROOT) && echo "#### Files copied! ####"

run: copy_scripts
	@sshpass -p $(PW) ssh $(USER)@$(HOST) "$(PYTHON_PATH) $(PROJECT_ROOT)/src/main.py"

delete_venv:
	@sshpass -p $(PW) ssh $(USER)@$(HOST) "rm -r $(VENV_PATH)"

delete_src:
	@sshpass -p $(PW) ssh $(USER)@$(HOST) "rm -r $(PROJECT_ROOT)"

cleanup: delete_venv delete_src
	@echo "#### Done deleting venv and project! ####"