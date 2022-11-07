# How to set up a Python project with poetry

This repo is a skeleton Python project with `poetry` to demonstrate how to set up a Python project with minimum dev dependencies to assist Python good practices.

## Pre-requisites

- `poetry`

## Step 1: Create a new project with a package

```
poetry new python-poetry-project-example
```

This will generate a project with below directory structure:

```
.
├── README.md
├── pyproject.toml
├── python_poetry_project_example
│   └── __init__.py
└── tests
    └── __init__.py
```

### Key points
- `pyproject.toml` - This is the project configuration file where dependencies are defined
- `pyton_poetry_project_example` - Source directory/Top level package to structure module namespaces
- `tests` - Test directory
- `__init__.py` - Required to make Python treat the directory containing this file as a package

## Step 2: Create a Python virtual environment for the project

Since Python dependencies can only be installed against a single version of Python, which may be different per project or different to the version installed on your local machine, it is best to create a virtual environment where the specific version of Python required for the project and dependencies can be installed. To do this, run:

```
poetry shell
```

This will:
- Create and activate a virtual environment under poetry's directory for your user
- Start a sub-shell with the Python in the virtual environment

Run the following to ensure that the Python is now in the virtual environment:
```
which python
```

The output path to Python will look like:

```
<poetry's cache dir>/virtualenvs/python-poetry-project-example-Tn9RUSPg-py3.10/bin/python
```

## Step 3: Install dependencies for the first time

Run:
```
poetry install
```

This will create `poetry.lock` file.

## Step 4: Install dev dependencies

We are now ready to install dependencies for development only, typically called "dev dependencies" They are not required to run Python scripts/modules in production and therfore should be managed explicitly for development.

To add a tool to the project as a dev dependency, run:
```
poetry add --group dev <tool>
```

This will:
- Add the tool as a dev dependency to `pyproject.toml`
- Install the tool in the project's virtual environment
- Update `poetry.lock` with the latest version of the tool

The updated `pyproject.toml` will look like:

```
[tool.poetry.group.dev.dependencies]
some_tool = "^12.34.0"
```

### Step 4-1: Install black - formatter for consistent coding style

[Black](https://black.readthedocs.io/en/stable/the_black_code_style/index.html) is a formatter based on an opinionated style guide for Python code [PEP 8](https://peps.python.org/pep-0008/) to increase consistency and readability.

To install it, run:
```
poetry add --group dev black
```

#### **How to format files with black**
In the top directory, run:

```
black .
```

This will auto-reformat any file that violates Black's style guide.

#### **Why is formatter/coding style guide required?**
To increase readability of our Python code, it is important to maintain consistency and do so automatically. One easy way to achieve this is to select a particular styling guide, albeit opinionated, and enforce it in the codebase by formatting the code by a tool.

### Step 4-2: Install flake8 - Linter for reporting issues with coding style

[flake8](https://flake8.pycqa.org/en/latest/) is a linter for enforcing Python coding style. Note that it will not re-foramt violations in the code, for which `black` can be used.

1. To install `flake8`, run:
```
poetry add --group dev flake8
```
2. Configure `flake8` to be `black` compatible in `.flake8` file ([link](https://black.readthedocs.io/en/stable/guides/using_black_with_other_tools.html#id2))


#### **How to check issues with coding style with flake8**

In the top directory, run:
```
flake8 .
```

When there are issues, output will look like below and `flake8` will return non-zero (ie with errors):
```
% flake8 .
./python_poetry_project_example/dev_dependencies_examples/bad_code.py:4:4: F821 undefined name 'foo'
bad_code.py:10:1: E741 ambiguous variable name 'l'
./python_poetry_project_example/dev_dependencies_examples/bad_code.py:14:5: F841 local variable 'a' is assigned to but never used

# check the exit code of flake8 (previously run command)
% echo $?
1
```

#### **Why is linter required?**

A linter is to validate if the code is following a style guide and report issues if any. `flake8` is to catch issues that are not covered by `black`.

### Step 4-3: Install mypy - Static type checker for finding bugs

[`mypy`](https://mypy.readthedocs.io/en/stable/) is a static type check for Python based on [Python's type annotations/hints](https://docs.python.org/3/library/typing.html) added by developers in the code so that integrity of intended types can be verified before running the code.

To install `mypy`, run:
```
poetry add --group dev mypy
```

#### **How to check types in the code with mypy**

```
mypy .
```

When `mypy` finds below bad code
```
foo: str = 1
```

It will output an error like below and exit with non-zero exit code:
```
% mypy .
bad_code.py:59: error: Incompatible types in assignment (expression has type "int", variable has type "str")

% echo $?
1
```

#### **Why is static type checker required?**

Since Python is not a language that enforeces static types, in order to "type" integrity of the code statically (ie before runtime) and increase readability, developers can add type annotations and validate them with a type checker, such as `mypy`.

For example, below code should ideally be flagged as an issue before runtime:

**Bad code - unintended argument type pased**
```
def add_integers(int_a, int_b):
    return int_a + int_b

add_integers("40", 2)
```

However, the issue in the code will not be reported and will result in `TypeError: can only concatenate str (not "int") to str` at runtime, which will be a production bug.

**Better code - intended argument types hinted**
```
def add_integers(int_a: int, int_b: int) -> int:
    return int_a + int_b

add_integers("40", 2)
```

With the type annotations added to the code, the intent of the function to add two integers is clearer. However, type annotations are only for readability in Python and will be treated as "comments" by Python interpreter, therefore the issue will not be flagged before running the code.

**Best solution - type annotations + type cheker**

We can combine type annotations in the code added by developer and running a statick type checker such as `mypy` to flag potential production bugs in advance. For example, `mypy` will report the code above with type annotations as below error and therfore can stop the code from being PR reviewed or getting deployed to production:

```
bad_code.py:71: error: Argument 1 to "add_integers" has incompatible type "str"; expected "int"
```