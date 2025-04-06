# Python Environment Workshop Exercises

These exercises are designed to help you get some  practical experience working with different
types of Python environments. Enjoy!

## Exercise 1: Creating and Managing Virtual Environments

**Objective:** Create, activate, and manage a virtual environment for a simple project.

**Tasks:**

1. Create a new virtual environment:
   ```bash
   python -m venv my_project_env
   ```

2. Activate the environment:
   ```bash
   # Windows
   my_project_env\Scripts\activate
   
   # macOS/Linux
   source my_project_env/bin/activate
   ```

3. Install some packages:
   ```bash
   pip install requests matplotlib pandas
   ```

4. List installed packages:
   ```bash
   pip list
   ```

5. Create a requirements file:
   ```bash
   pip freeze > requirements.txt
   ```

6. Deactivate the environment:
   ```bash
   deactivate
   ```

7. Create a new environment and install from requirements:
   ```bash
   python -m venv my_project_env2
   # Activate it
   pip install -r requirements.txt
   ```

## Exercise 2: Managing Conda Environments

**Objective:** Create and manage Conda environments for data science work.

**Tasks:**

1. Create a new Conda environment:
   ```bash
   conda create --name data_science python=3.9
   ```

2. Activate the environment:
   ```bash
   conda activate data_science
   ```

3. Install data science packages:
   ```bash
   conda install numpy pandas scikit-learn matplotlib jupyter
   ```

4. Start a Jupyter notebook:
   ```bash
   jupyter notebook
   ```

5. Export the environment:
   ```bash
   conda env export > environment.yml
   ```

6. Deactivate and remove the environment:
   ```bash
   conda deactivate
   conda env remove --name data_science
   ```

7. Create a new environment from the exported file:
   ```bash
   conda env create -f environment.yml
   ```

## Exercise 3: Environment Variables and Configuration

**Objective:** Learn to manage configuration using environment variables.

**Tasks:**

1. Create a new virtual environment and activate it.

2. Create a file named `config.py`:
   ```python
   import os
   
   # Get configuration from environment variables
   DEBUG = os.environ.get('DEBUG', 'False').lower() == 'true'
   API_KEY = os.environ.get('API_KEY', 'default_key')
   DATABASE_URL = os.environ.get('DATABASE_URL', 'sqlite:///default.db')
   
   print(f"Debug mode: {DEBUG}")
   print(f"API Key: {API_KEY}")
   print(f"Database URL: {DATABASE_URL}")
   ```

3. Run the script with different environment variables:
   ```bash
   # Windows
   set DEBUG=True
   set API_KEY=my_secret_key
   python config.py
   
   # macOS/Linux
   DEBUG=True API_KEY=my_secret_key python config.py
   ```

4. Install python-dotenv:
   ```bash
   pip install python-dotenv
   ```

5. Create a `.env` file:
   ```
   DEBUG=True
   API_KEY=env_file_key
   DATABASE_URL=postgresql://user:pass@localhost/mydb
   ```

6. Modify `config.py` to use dotenv:
   ```python
   import os
   from dotenv import load_dotenv
   
   # Load environment variables from .env file
   load_dotenv()
   
   # Rest of the code stays the same
   DEBUG = os.environ.get('DEBUG', 'False').lower() == 'true'
   # ...
   ```

7. Run the script again and observe the difference.

## Exercise 4: Dependency Conflicts Resolution

**Objective:** Experience and resolve package dependency conflicts.

**Tasks:**

1. Create a new virtual environment and activate it.

2. Create a file named `requirements_conflict.txt`:
   ```
   # This will create a conflict
   pandas==1.3.0
   numpy==1.22.0  # pandas 1.3.0 requires numpy<1.22.0
   ```

3. Try to install these conflicting requirements:
   ```bash
   pip install -r requirements_conflict.txt
   ```

4. Observe the error message.

5. Resolve the conflict by creating a new requirements file:
   ```
   pandas==1.3.0
   # Let pip resolve the compatible numpy version
   ```

6. Test the installation again.

7. Verify the installed versions:
   ```bash
   pip list | grep -E "pandas|numpy"
   ```

## Exercise 5: Working with Docker Python Environments

**Objective:** Create and use a Docker container with a Python environment.

**Prerequisites:** Docker installed on the system.

**Tasks:**

1. Create a simple Python application (`app.py`):
   ```python
   from flask import Flask
   import os
   
   app = Flask(__name__)
   
   @app.route('/')
   def hello():
       return f"Hello from a Docker Python environment! Running Python in {os.environ.get('ENVIRONMENT', 'development')} mode."
   
   if __name__ == '__main__':
       app.run(host='0.0.0.0', port=5000)
   ```

2. Create a `requirements.txt` file:
   ```
   flask==2.2.3
   ```

3. Create a Dockerfile:
   ```dockerfile
   FROM python:3.9-slim
   
   WORKDIR /app
   
   COPY requirements.txt .
   RUN pip install --no-cache-dir -r requirements.txt
   
   COPY . .
   
   ENV ENVIRONMENT=production
   
   CMD ["python", "app.py"]
   ```

4. Build the Docker image:
   ```bash
   docker build -t python-flask-app .
   ```

5. Run the container:
   ```bash
   docker run -p 5000:5000 python-flask-app
   ```

6. Open a web browser and visit `http://localhost:5000`.

7. Modify the environment variable:
   ```bash
   docker run -p 5000:5000 -e ENVIRONMENT=staging python-flask-app
   ```

8. Refresh the browser to see the change.

## Exercise 6: Environment Management Challenge

**Objective:** Solve a complex environment setup problem.

**Scenario:** You need to create an environment for a project with the following requirements:
- Python 3.9
- TensorFlow 2.8 (which has specific dependency requirements)
- Pandas for data processing
- Flask for web API
- PostgreSQL database connection
- Environment variables for configuration
- Different settings for development and production

**Tasks:**

1. Create the appropriate environment (venv or conda).

2. Research compatible versions of all required packages.

3. Create a requirements.txt file with pinned versions.

4. Write a small Python script that shows all installed packages and their versions.

5. Create a configuration management solution using environment variables.

6. Document your approach and the commands you used.

7. Explain how you would deploy this environment to production.

## Extension Exercise: Environment Isolation Competition

**Objective:** Compare isolation levels between different environment types.

**Setup:** 

1. Divide students into teams, each using a different approach:
   - Team A: Global Python installation
   - Team B: Virtual environment (venv)
   - Team C: Conda environment
   - Team D: Docker container

2. Each team installs a complex package (like TensorFlow or Django) with all dependencies.

3. Each team must then identify:
   - Where the packages are installed
   - What system-level changes occurred
   - How to completely remove the environment
   - How to duplicate the environment on another machine

4. Teams present their findings and compare the isolation levels, ease of setup/teardown, and portability.