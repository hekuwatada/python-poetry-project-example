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
