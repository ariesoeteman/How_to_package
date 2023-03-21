# test_package

How to create a packaged repo with a shared environment and githooks

#### 1) Installing requirements
  - Conda: check "https://docs.conda.io/projects/conda/en/latest/user-guide/install/index.html" to find instructions for your OS
  
  - Poetry 
  ```
  curl -sSL https://install.python-poetry.org | python3 -
  ```

#### 2) Setup a conda environment

This is to create a shared environment that includes non-python packages, and maybe the python version itself

  - create a file named environment.yml (or something_else.yml)
  
  - include your desired modules; for instance:
  """
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
  """
  
  Here, conda-forge is the channel where most conda packages are stored, mamba is a package manager, and  poetry-dynamic-versioning is a nice resolver for dependency conflicts.

#### 2) 
