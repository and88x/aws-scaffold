[![Python application test with Github Actions](https://github.com/and88x/aws-scaffold/actions/workflows/aws_pythonapp.yml/badge.svg)](https://github.com/and88x/aws-scaffold/actions/workflows/aws_pythonapp.yml)

# Steps to run this project

* Create a Github Repo (if not created)
* Create a new cloud9 environment of AWS 
* Create ssh-keys in the AWS Shell
* Upload ssh-keys to Github
* Create aws-scaffolding for project (if not created)

## Files

  - Makefile

```bash
install:
	pip install --upgrade pip &&\
		pip install -r requirements.txt

test:
	python -m pytest -vv test_hello.py

format:
	black *.py


lint:
	pylint --disable=R,C hello.py

all: install lint test
```

  - requirements.txt
  
The requirements.txt should include:

```bash
pylint
pytest
black
click
pytest-cov
```


* Create a python virtual environment and source it if not created

```bash
python3 -m venv ~/.myrepo
source ~/.myrepo/bin/activate
```

* Create initial `hello.py` and `test_hello.py`

hello.py
```python
def toyou(x):
    return f"hi {x}"

def add(x):
    return x + 1

def subtract(x):
    return x - 1
    
result = add(1)    
print(f"This is the sum : 1 + 1, {result}")
```

test_hello.py
```python
from hello import toyou, add, subtract


def setup_function(function):
    print(f" Running Setup: {function.__name__}")
    function.x = 10


def teardown_function(function):
    print(f" Running Teardown: {function.__name__}")
    del function.x


### Run to see failed test
#def test_hello_add():
#    assert add(test_hello_add.x) == 12

def test_hello_add():
    assert add(8) == 9
```

* Run `make all` which will install, lint and test code.

* Setup Github Actions in `aws_pythonapp.yml`

```yaml
name: Python application test with Github Actions

on: [push]

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2
    - name: Set up Python 3.5
      uses: actions/setup-python@v1
      with:
        python-version: 3.5
    - name: Install dependencies
      run: |
        make install
    - name: Lint with pylint
      run: |
        make lint
    - name: Test with pytest
      run: |
        make test
```

* Commit changes and push to Github

* Verify Github Actions Test Software

* Run project in AWS Shell

* Push container to AWS Registery
