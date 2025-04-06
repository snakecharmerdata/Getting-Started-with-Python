# Advanced Python Package Management

Effective package management is crucial for maintaining reliable Python environments. This guide covers advanced techniques that go beyond basic `pip install` commands.

## Dependency Resolution Strategies

### Pinning Dependencies

Explicitly specify exact versions to ensure reproducibility:

```
# requirements.txt
requests==2.28.1
numpy==1.23.5
pandas==1.5.2
```

### Version Ranges

Allow flexibility while maintaining compatibility:

```
# requirements.txt
requests>=2.28.0,<2.29.0
numpy>=1.23.0,<1.24.0
pandas>=1.5.0,<1.6.0
```

### Hashes for Security

Verify package integrity with hashes:

```
# Generate with pip hash
requests==2.28.1 --hash=sha256:7c5599b102feddaa661c826c56ab4fee28bfd17f5abbca41bf5c8c8f7e5b2c43
```

## Tools Beyond pip and conda

### Poetry

Poetry provides dependency resolution, package management, and virtual environment creation:

```bash
# Install Poetry
curl -sSL https://install.python-poetry.org | python3 -

# Create a new project
poetry new project-name

# Add dependencies
poetry add requests pandas

# Install dependencies
poetry install
```

**poetry.toml** example:
```toml
[tool.poetry]
name = "project-name"
version = "0.1.0"
description = "Project description"
authors = ["Your Name <your.email@example.com>"]

[tool.poetry.dependencies]
python = "^3.9"
requests = "^2.28.1"
pandas = "^1.5.2"

[tool.poetry.dev-dependencies]
pytest = "^7.2.0"
```

### Pipenv

Combines pip and virtualenv in one tool:

```bash
# Install Pipenv
pip install pipenv

# Install packages
pipenv install requests pandas

# Install dev packages
pipenv install --dev pytest

# Activate the environment
pipenv shell
```

**Pipfile** example:
```
[[source]]
url = "https://pypi.org/simple"
verify_ssl = true
name = "pypi"

[packages]
requests = "~=2.28.1"
pandas = "~=1.5.2"

[dev-packages]
pytest = "~=7.2.0"

[requires]
python_version = "3.9"
```

### pip-tools

Compile precise dependency versions:

```bash
# Install pip-tools
pip install pip-tools

# Create a requirements.in file
echo "requests\npandas" > requirements.in

# Compile to requirements.txt
pip-compile requirements.in

# Install from compiled requirements
pip-sync requirements.txt
```

## Handling Conflicting Dependencies

1. **Identify conflicts** using tools:
   ```bash
   pip check
   ```

2. **Use virtual environments** to isolate incompatible packages

3. **Try alternative package versions** that might be compatible

4. **Check for newer versions** of packages that might resolve conflicts

5. **Consider alternative packages** that provide similar functionality

## Working with Private Package Repositories

### Setting up Private PyPI Server

For organizations that need to host proprietary packages:

```bash
# Install pypiserver
pip install pypiserver

# Run a simple PyPI server
pypiserver -p 8080 ~/packages
```

### Using Private Repositories

```bash
# Install from private repository
pip install --index-url https://your-private-pypi.org/simple/ your-package

# Create a pip.conf file
# ~/.pip/pip.conf (Linux/macOS) or %APPDATA%\pip\pip.ini (Windows)
[global]
index-url = https://your-private-pypi.org/simple/
extra-index-url = https://pypi.org/simple/
```

## Vendoring Dependencies

For air-gapped environments or ensuring consistent builds:

1. **Download all packages**:
   ```bash
   pip download -r requirements.txt -d ./vendor
   ```

2. **Install from downloaded packages**:
   ```bash
   pip install --no-index --find-links=./vendor -r requirements.txt
   ```

## Environment Introspection

Commands to analyze your environment:

```bash
# List all installed packages
pip list

# Show details of a specific package
pip show requests

# Find outdated packages
pip list --outdated

# Export installed packages
pip freeze > requirements.txt

# Check for dependency conflicts
pip check
```

For conda:
```bash
# List all installed packages
conda list

# List packages needing update
conda update --all --dry-run

# View environment information
conda info

# View environment variables
conda env config vars list
```
</qodoArtifact>

## Containerization and Python Environments