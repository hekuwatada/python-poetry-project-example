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

### Step 4-1: Install black - formatter for consistent coding style

[Black](https://black.readthedocs.io/en/stable/the_black_code_style/index.html) is a formatter based on an opinionated style guide for Python code [PEP 8](https://peps.python.org/pep-0008/) to increase consistency and readability.

To install it, run:
```
poetry add --group dev black
```

This will:
- Add `black` as a dev dependency to `pyproject.toml`
- Install `black`in the project's virtual environment
- Update `poetry.lock` with the latest version of `black`

The updated `pyproject.toml` will look like:

```
[tool.poetry.group.dev.dependencies]
black = "^22.10.0"
```
#### How to format files
In the top directory, run:

```
black .
```

This will auto-reformat any file that violates Black's style guide.

#### Why is formatter/coding style guide required?
To increase readability of our Python code, it is important to maintain consistency and do so automatically. One easy way to achieve this is to select a particular styling guide, albeit opinionated, and enforce it in the codebase by formatting the code by a tool.
