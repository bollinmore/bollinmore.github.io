## Install local package

> pip install -e path/to/package

The **-e** keyword in pip facilitates the installation of packages from a local folder, yet it carries certain constraints. Notably, the package being installed must contain a **pyproject.toml** file, a prerequisite for its successful installation. Moreover, the usage of this feature necessitates an upgrade of pip to version **21.3** or beyond. More comprehensive details can be found in the discussion on How to install a package using pip in editable mode with pyproject.toml?."

### pyproject.toml
```toml
[project]
name = "web"
version = "0.9.0"
description = "Investment Dashboard"
dependencies = [
    "flask",
]

[build-system]
requires = ["flit_core<4"]
build-backend = "flit_core.buildapi"
```
