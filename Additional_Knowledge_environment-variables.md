# Environment Variables in Python Environments

Environment variables play a crucial role in Python applications, especially when dealing with different environments. They allow for configuration without code changes.

## What Are Environment Variables?

Environment variables are dynamic-named values that can affect the way running processes behave on a computer. In Python development, they're commonly used for:

- Storing sensitive information (API keys, database credentials)
- Configuring application behavior for different environments (development, testing, production)
- Setting paths and locations for files and services
- Controlling logging behavior

## Environment Variables and Python Environments

Each Python environment (global, virtual, or conda) inherits environment variables from the system, but you can also:

1. Set environment-specific variables
2. Use different values for the same variable in different environments
3. Isolate configuration between projects

## Setting Environment Variables

### Temporary (Session-Only)

**Windows (Command Prompt):**
```
set VARIABLE_NAME=value
```

**Windows (PowerShell):**
```
$env:VARIABLE_NAME = "value"
```

**macOS/Linux:**
```
export VARIABLE_NAME=value
```

### Permanent Environment Variables

**For Virtual Environments (venv):**

Create an activation hook:
1. Navigate to your venv's activation directory:
   ```
   # Windows
   cd .venv\Scripts
   
   # macOS/Linux
   cd .venv/bin
   ```

2. Create a file named `activate.d/environment_variables.sh` (for bash) with:
   ```bash
   #!/bin/bash
   export VARIABLE_NAME=value
   ```

**For Conda Environments:**

Use conda's environment variables feature:
```
conda env config vars set VARIABLE_NAME=value -n environment_name
```

Then reactivate the environment:
```
conda activate environment_name
```

## Using Environment Variables in Python

```python
import os

# Get an environment variable
api_key = os.environ.get('API_KEY', 'default_value')

# Set an environment variable
os.environ['DEBUG'] = 'True'

# Check if an environment variable exists
if 'DATABASE_URL' in os.environ:
    # Use the database URL
    db_url = os.environ['DATABASE_URL']
else:
    # Use a default
    db_url = 'sqlite:///default.db'
```

## Environment Variable Files (.env)

For managing multiple environment variables, `.env` files are common:

1. Create a `.env` file:
   ```
   DEBUG=True
   API_KEY=your_secret_key
   DATABASE_URL=postgresql://user:password@localhost/dbname
   ```

2. Install python-dotenv:
   ```
   pip install python-dotenv
   ```

3. Load in your Python code:
   ```python
   from dotenv import load_dotenv
   
   # Load variables from .env
   load_dotenv()
   
   # Now use them
   import os
   debug = os.environ.get("DEBUG")
   ```

## Environment Variables Best Practices

1. **Never commit sensitive values** to version control
2. Use **different .env files** for different environments (dev, test, prod)
3. Include a **.env.example** file in version control with the structure but not real values
4. Consider using a **secrets management** solution for production environments
5. Document all environment variables your application uses
6. Set **sensible defaults** when getting environment variables
7. Use **lowercase** for environment variable references in code (for consistency)