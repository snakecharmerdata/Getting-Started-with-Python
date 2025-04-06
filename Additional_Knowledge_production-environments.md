# Production-Ready Python Environments

Setting up Python environments for production requires special considerations beyond development environments. 
This guide focuses on creating robust, secure, and maintainable Python environments for production deployments.

NOTE: "bash" just means that the below command script, should be copied to your Terminal and run from Terminal.

## Core Principles for Production Environments

1. **Reproducibility** - Exact environment recreation should be possible
2. **Stability** - Environments should be locked to specific versions
3. **Security** - Regular updates for security patches
4. **Monitoring** - Environment health should be observable
5. **Scalability** - Environment setup should work for multiple instances
6. **Documentation** - Comprehensive documentation of the environment

## Locking Dependencies for Production

### Pip

Generate a fully pinned requirements file:

```bash
pip freeze > requirements-lock.txt
```

For more control, consider using `pip-compile` from pip-tools:

```bash
# High-level dependencies in requirements.in
pip-compile requirements.in --output-file requirements-lock.txt

# Install exactly those versions
pip install -r requirements-lock.txt
```

### Conda

Lock environment with exact package versions:

```bash
conda env export > environment-lock.yml
```

For a more portable locked file:

```bash
conda env export --from-history > environment-lock.yml
```

## Security Considerations

### 1. Regular Security Updates

Schedule regular updates of dependencies to address security vulnerabilities:

```bash
# Update all packages
pip install --upgrade -r requirements.txt

# Update a specific package for security
pip install --upgrade requests
```

### 2. Vulnerability Scanning

Integrate security scanning into your workflow:

```bash
# Install safety
pip install safety

# Check for vulnerabilities
safety check -r requirements.txt
```

Or use more comprehensive tools:

```bash
# Install bandit for code scanning
pip install bandit

# Scan your code
bandit -r ./your_project
```

### 3. Minimizing the Attack Surface

- Include only necessary packages
- Use minimal base images in containers
- Remove development tools from production environments

## Deployment Strategies

### 1. Environment Creation at Deployment Time

Create the environment during the deployment process:

```bash
# In deployment script
python -m venv /opt/myapp/venv
/opt/myapp/venv/bin/pip install -r requirements-lock.txt
```

### 2. Pre-built Environments

Package your entire environment:

```bash
# Create a wheelhouse of all dependencies
pip wheel -r requirements-lock.txt -w ./wheelhouse

# In deployment
pip install --no-index --find-links=./wheelhouse -r requirements-lock.txt
```

### 3. Immutable Infrastructure with Containers

Build images that include your environment and application:

```dockerfile
FROM python:3.9-slim

COPY requirements-lock.txt /tmp/
RUN pip install --no-cache-dir -r /tmp/requirements-lock.txt

COPY . /app/
WORKDIR /app

CMD ["gunicorn", "app:app"]
```

## Environment Variables in Production

### 1. Configuration Management

Use environment variables for configuration:

```python
import os

DATABASE_URL = os.environ.get("DATABASE_URL", "sqlite:///default.db")
DEBUG = os.environ.get("DEBUG", "False").lower() == "true"
LOG_LEVEL = os.environ.get("LOG_LEVEL", "INFO")
```

### 2. Secrets Management

Never hardcode secrets:

```python
import os
from dotenv import load_dotenv

# Load .env file in development, use real env vars in production
if os.environ.get("ENVIRONMENT") != "production":
    load_dotenv()

API_KEY = os.environ["API_KEY"]
```

Consider dedicated secrets management services:
- AWS Secrets Manager
- HashiCorp Vault
- Azure Key Vault

## Monitoring and Observability

### 1. Application Monitoring

Integrate monitoring libraries:

```bash
pip install prometheus_client
```

```python
from prometheus_client import Counter, start_http_server

# Start metrics endpoint
start_http_server(8000)

# Create a metric
requests_total = Counter('requests_total', 'Total requests')

# Increment in your code
requests_total.inc()
```

### 2. Environment Health Checks

Create health check endpoints:

```python
@app.route('/health')
def health_check():
    # Check database connection
    try:
        db.session.execute('SELECT 1')
        db_healthy = True
    except Exception:
        db_healthy = False
    
    # Check cache connection
    try:
        redis_client.ping()
        cache_healthy = True
    except Exception:
        cache_healthy = False
    
    status = 200 if (db_healthy and cache_healthy) else 503
    return jsonify({
        'database': db_healthy,
        'cache': cache_healthy
    }), status
```

### 3. Dependency Tracking

Keep track of outdated or vulnerable packages:

```bash
# Check for outdated packages
pip list --outdated

# Schedule regular checks in your CI pipeline
```

## Scaling Considerations

### 1. Stateless Environments

Design environments to be stateless:
- Store configuration in environment variables
- Use external storage for persistent data
- Avoid local file system dependencies

### 2. Horizontal Scaling

Make environments work across multiple instances:
- Avoid hardcoded hostnames or IPs
- Use service discovery for dependencies
- Design for concurrent operations

### 3. Resource Management

Configure resource limits:

```python
# Set thread pool size based on environment
import os
from concurrent.futures import ThreadPoolExecutor

worker_count = int(os.environ.get("WORKER_COUNT", "4"))
executor = ThreadPoolExecutor(max_workers=worker_count)
```

## Documentation and Compliance

### 1. Environment Documentation

Document all aspects of your environment:
- Python version
- All dependencies and their versions
- System requirements
- Configuration options
- Deployment instructions

### 2. Change Management

Track changes to your environment:
- Keep a changelog
- Version your environment definitions
- Document upgrade paths

### 3. Compliance Considerations

For regulated industries:
- Document security measures
- Provide audit trails for changes
- Ensure license compliance
- Implement required access controls

## Disaster Recovery

### 1. Backup Strategies

Even for ephemeral environments, document recovery procedures:
- Environment recreation steps
- Configuration restoration
- Data recovery procedures

### 2. Rollback Procedures

Plan for environment failures:
- Keep previous working versions available
- Document rollback procedures
- Test recovery regularly