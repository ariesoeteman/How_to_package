# test_package

How to create a packaged repo with a shared environment and githooks

#### 0) Installing requirements
  - Conda: check "https://docs.conda.io/projects/conda/en/latest/user-guide/install/index.html" to find instructions for your OS
  
  - Poetry 
  ```
  curl -sSL https://install.python-poetry.org | python3 -
  ```


#### 1) Setup a git repository

Click 'new repository' in the github client, this gives you a remote url, for instance:
```[
https://github.com/your-username/your-repository.git
```

On your local terminal, go to your desired foler and run
```
git init
git clone 'your url'
```

Then move inside your repo (i.e. with cd)

#### 2) Setup a conda environment

This is to create a shared environment that includes non-python packages, and maybe the python version itself

  - create a file named environment.yml (or something_else.yml)
  
  - include your desired modules; for instance:
  ```yaml
  name: my_environment
  channels:
    - conda-forge
    - nodefaults
dependencies:
    - mamba
    - pre-commit
    - pip
    - pip:
      - poetry-dynamic-versioning
    - python=3.10.*
  ```  
  Here, conda-forge is the channel where most conda packages are stored, mamba is a package manager, and  poetry-dynamic-versioning is a nice resolver for dependency conflicts. pre-commit is a package for git-hooks, which are introduced later.


Now create your environment using:
```
conda env create -f environment.yml
```

And activate it using:
```
conda activate my_environment
```


#### 3) Setup a Poetry package
You define your package using a .toml file, i.e.
'''
pyproject.toml
'''
This is an optimal place to store all python dependencies for your project.
I have included an example in this repository. You can fill in a name and version number for your package.

The header [tool.poetry.dependencies] contains the things your project/package needs to run.
The header [tool.poetry.group.dev.dependencies] contains packages that are needed when developing the project. Examples are 'black' (for uniform formatting) and 'pytest' for testing your code.



Having written the .toml file, run installation from the folder containing this file:
```
poetry install
```

This should add a '''poetry.lock''' file that locks in your package.

### 4) Setup git hooks

Git hooks can be used to trigger actions upon commiting to git, such as reformatting and tasting.
The pre-commit package has been installed in the conda environment, but we still need to specify which actions to perform.
This you can do in a '.pre-commit-config.yaml' file, with content structured as follows:

```yaml
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v3.2.0
    hooks:
    - id: trailing-whitespace
    - id: end-of-file-fixer
    - id: check-yaml
    - id: check-added-large-files

  - repo: https://github.com/python-poetry/poetry
    rev: 1.3.1
    hooks:
      - id: poetry-check
      - id: poetry-lock
        args: ["--no-update"]
      - id: poetry-export

  - repo: https://github.com/psf/black
    rev: 22.12.0
    hooks:
      - id: black

  - repo: https://github.com/pycqa/isort
    rev: 5.10.1
    hooks:
      - id: isort
```

With this file saved, you can run:

```
pre-commit install
```

This should add the specified actions to `.git/hooks/pre-commit`.

#### 5) That's it

Try uploading your repo using
```
git add *
git commit -m "made a repo"
git push
```


### 6) Now other people can work in the same environment as follows:

Download your repo using 'git clone'

activate your conda environment using:
```
conda env create -f environment.yml
conda activate my_environment
```

Install your poetry package and git hooks using:

```
poetry install
pre-commit install
```

Note that you can always update your .toml, and run 'poetry update' to lock in your changes.

Enjoy your project

