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

```
poetry install
```

This will create `poetry.lock` file.