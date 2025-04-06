# Python Environment Troubleshooting Guide

## Common Virtual Environment Issues

### "Command Not Found" After Activating Environment

**Problem:** After activating your environment, commands like `python` or `pip` aren't recognized.

**Solutions:**
- Check that your environment was created correctly
- Ensure you're using the correct activation command for your OS
- Try using absolute paths to the commands inside your environment folder

### Package Installation Failures

**Problem:** `pip install` commands fail with error messages.

**Solutions:**
- Check your internet connection
- Ensure pip is updated: `pip install --upgrade pip`
- Look for specific error messages about incompatible versions
- Try installing from a different source: `pip install package --index-url https://pypi.org/simple/`

### Environment Not Showing in VS Code

**Problem:** Your virtual environment doesn't appear in the Python interpreter list in VS Code.

**Solutions:**
- Ensure the environment is in your workspace folder or a known location
- Run the "Python: Select Interpreter" command from the command palette
- Reload VS Code
- Check VS Code settings.json for python.venvPath settings

## Conda Environment Issues

### Conda Environment Not Activating

**Problem:** `conda activate envname` doesn't work or gives errors.

**Solutions:**
- Initialize conda for your shell: `conda init <shell_name>`
- Check if the environment exists: `conda env list`
- Try creating the environment again

### Conflicts Between Pip and Conda

**Problem:** Installing packages with pip in a conda environment causes conflicts.

**Solutions:**
- Install packages with conda when possible
- If using pip, install in this order: conda packages first, then pip packages
- Consider creating a conda-forge only environment: `conda create -n myenv -c conda-forge python=3.9`

### Package Not Available in Conda

**Problem:** You can't find a package in conda repositories.

**Solutions:**
- Check conda-forge: `conda install -c conda-forge package_name`
- Use pip as a fallback: `pip install package_name`
- Create a custom conda recipe if needed for reproducibility

## General Environment Problems

### Multiple Python Versions Confusion

**Problem:** Commands use unexpected Python versions.

**Solutions:**
- Use explicit paths to the Python you want
- Check your PATH environment variable
- Use `python --version` or `which python` (Unix) / `where python` (Windows) to check which Python is being used

### ImportError: No Module Named X

**Problem:** Python can't find installed modules.

**Solutions:**
- Verify you're running Python from the correct environment
- Check if the package is installed: `pip list | grep package_name`
- Reinstall the package
- Check for path issues: `python -c "import sys; print(sys.path)"`

### Performance Issues with Conda

**Problem:** Conda operations are very slow.

**Solutions:**
- Use Mamba as a drop-in replacement: `conda install -c conda-forge mamba`
- Then use mamba instead of conda: `mamba install package`
- Reduce the number of channels in your .condarc
- Clean conda caches: `conda clean -a`

## Fixing Broken Environments

Sometimes it's better to recreate an environment than to fix it:

1. Export your working packages: `pip freeze > requirements.txt` or `conda env export > environment.yml`
2. Remove the broken environment: 
   - For venv: Delete the environment folder
   - For conda: `conda env remove -n environment_name`
3. Create a fresh environment and reinstall packages from your export file

## Diagnosing Path and System Issues

If you suspect PATH or system configuration issues:

1. Check environment variables:
   ```
   # Windows (PowerShell)
   $env:PATH
   
   # macOS/Linux
   echo $PATH
   ```

2. Inspect Python executable location:
   ```
   # Windows
   where python
   
   # macOS/Linux
   which python
   ```

3. Check for multiple Python installations:
   ```
   # Windows
   where python
   
   # macOS/Linux
   which -a python
   ```