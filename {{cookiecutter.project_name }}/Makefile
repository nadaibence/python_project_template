USER = {{ cookiecutter.user }}
HOST = {{ cookiecutter.host }}
PROJECT_ROOT = {{ cookiecutter.project_root }}
VENV_PATH = {{ cookiecutter.venv_path }}
PYTHON_PATH = $(VENV_PATH)/bin/python
PYINSTALLERS = {{ cookiecutter.pyinstallers }}

create_venv:
	@ssh $(USER)@$(HOST) "python3 -m venv $(VENV_PATH)"

install_packages:
	@mkdir ./python_packages
	@pip download --python-version 3.6 --platform=manylinux1_x86_64 --only-binary=:all: -r ./src/config/requirements.txt -d './python_packages'
	@tar cvfz python_packages.tgz python_packages && echo "#### Downloaded packages and zipped them!"
	@rsync -av -e ssh python_packages.tgz $(USER)@$(HOST):$(PYINSTALLERS) && echo "#### Files copied! ####"
	@ssh $(USER)@$(HOST) "mkdir $(PYINSTALLERS)/python_packages && tar xvfz $(PYINSTALLERS)/python_packages.tgz -C $(PYINSTALLERS)"
	@ssh $(USER)@$(HOST) "$(PIP_PATH) install $(PYINSTALLERS)/python_packages/* -f ./ --no-index --no-deps"
	@ssh $(USER)@$(HOST) "rm $(PYINSTALLERS)/python_packages.tgz && rm -r $(PYINSTALLERS)/python_packages"
	@rm python_packages.tgz && rm -r ./python_packages

setup_env: create_venv install_packages
	@echo "#### Done creating the environment! ####"

copy_scripts:
	@rsync -av -e ssh ./src $(USER)@$(HOST):$(PROJECT_ROOT) && echo "#### Files copied! ####"

run: copy_scripts
	@ssh $(USER)@$(HOST) "$(PYTHON_PATH) $(PROJECT_ROOT)/src/main.py"

delete_venv:
	@ssh $(USER)@$(HOST) "rm -r $(VENV_PATH)"

delete_src:
	@ssh $(USER)@$(HOST) "rm -r $(PROJECT_ROOT)"

cleanup: delete_venv delete_src
	@echo "#### Done deleting venv and project! ####"