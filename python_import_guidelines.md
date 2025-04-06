# Python Import Guidelines Reference

## Import Categories

### 1. Standard Library Imports

The Python Standard Library includes modules that come bundled with a Python installation. These should be imported first.

**Common standard library modules:**
- `os`, `sys`: Operating system interfaces
- `datetime`, `time`: Date and time handling
- `math`, `random`: Mathematical functions and random number generation
- `json`, `csv`, `xml`: Data format handling
- `collections`, `array`: Data structures
- `tkinter`: GUI programming
- `unittest`, `logging`: Testing and logging
- `re`: Regular expressions
- `urllib`, `socket`: Networking
- `threading`, `multiprocessing`: Concurrent execution
- `sqlite3`: Database access
- `pathlib`: Object-oriented filesystem paths
- `argparse`: Command-line argument parsing
- `functools`, `itertools`: Functional programming tools

### 2. Related Third-Party Imports

Third-party libraries are external packages installed via pip, conda, or other package managers.

**Popular third-party libraries:**
- **Data Science & ML**:
  - `numpy`: Numerical computing
  - `pandas`: Data analysis and manipulation
  - `scipy`: Scientific computing
  - `sklearn` (scikit-learn): Machine learning
  - `tensorflow`, `pytorch`: Deep learning
  
- **Data Visualization**:
  - `matplotlib`: Plotting library
  - `seaborn`: Statistical data visualization
  - `plotly`: Interactive visualizations
  - `bokeh`: Interactive web plotting
  
- **Web Development**:
  - `django`, `flask`: Web frameworks
  - `requests`: HTTP requests
  - `beautifulsoup4`: Web scraping
  - `fastapi`: API development
  
- **Other Categories**:
  - `pillow`: Image processing
  - `nltk`, `spacy`: Natural language processing
  - `sqlalchemy`: SQL toolkit and ORM
  - `pytest`: Testing framework
  - `pydantic`: Data validation

### 3. Local Application/Library Specific Imports

These are modules you've created as part of your project.

Examples:
```python
from myapp.models import User
from .utils import helper_function
import config
from core.settings import CONSTANTS
```

## Import Order Best Practices

```python
# 1. Standard library imports
import os
import sys
from datetime import datetime

# 2. Third-party library imports
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

# 3. Local application/library specific imports
from myproject.utils import helper
import config
```

## Additional Guidelines

1. **Group and separate imports**: Use blank lines between import groups
2. **Alphabetize imports**: Within each section for better readability
3. **One import per line**: Avoid multiple imports on the same line (except for `from` imports)
4. **Avoid wildcard imports**: `from module import *` makes it unclear which names are present
5. **Use absolute imports**: They're more readable and less error-prone than relative imports
6. **Import specific functions/classes**: When you only need a few items from a large module
7. **Use consistent import style**: Choose a convention and stick to it throughout the project

## Import Styles

### Module import
```python
import module
module.function()
```

### Specific import
```python
from module import function, Class
function()
```

### Aliased import
```python
import numpy as np
np.array([1, 2, 3])
```

## Tools for Managing Imports

- **isort**: Automatically sorts imports according to PEP 8
- **flake8**: Checks import formatting along with other code style issues
- **black**: Code formatter that also handles import formatting
- **pylint**: Linter that checks import order and style

## References

- [PEP 8 - Style Guide for Python Code](https://pep8.org/#imports)
- [Python Import System Documentation](https://docs.python.org/3/reference/import.html)