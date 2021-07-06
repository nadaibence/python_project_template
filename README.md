Project template how-to
===

Prerequisites
---

1. Install cookiecutter
```bash
pip install cookiecutter
```

Create new project
---

1. Create local project using the template 
```bash
cookiecutter gh:nadaibence/python_project_template
```

2. Fill out the required information, example:
```bash
project_name [<Replace with project name>]: RandomProject
user [<Replace with your username>]: nadaibence
host [<Replace with target host>]: myhost.com
project_root [<Replace with desired project location on host>]: /tmp/remote_project_folder
venv_path [<Replace with the desired path of the virtualenv>]: /tmp/remote_project_venv
pyinstallers [<Replace with a tmp folder on the remote host>]: /tmp/remote_project_folder
```

3. Edit the **src/config.requirements.txt**, add python packages needed to be installed on the remote server
```text
pandas==1.3.0
requests==2.7.0
```

4. Create the virtual environment
```bash
make setup_env
```

5. Edit the **src/main.py** file
```python
print('Hello World!')
```

6. Run the script on the remote server
```bash
make run
```

7. Clean everything once you finished everything
```bash
make cleanup
```