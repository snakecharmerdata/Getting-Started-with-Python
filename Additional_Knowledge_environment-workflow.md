# Python Environment Management Workflow

The following diagram illustrates a typical workflow for managing Python environments:

```mermaid
flowchart TD
    A[New Project] --> B{Choose Environment Type}
    B -->|Data Science| C[Create Conda Environment]
    B -->|Web/App Development| D[Create Virtual Environment]
    B -->|Quick Script| E[Use Global Python]
    
    C --> F[Install Dependencies]
    D --> F
    E --> F
    
    F --> G[Export Environment]
    G --> H[Development Work]
    
    H --> I{Environment Issues?}
    I -->|Yes| J[Troubleshoot]
    I -->|No| H
    
    J --> K{Fixable?}
    K -->|Yes| L[Fix Issue]
    K -->|No| M[Recreate Environment]
    
    L --> H
    M --> F
    
    H --> N{Project Complete?}
    N -->|No| H
    N -->|Yes| O[Archive Environment File]
```

This workflow shows the decision points and processes involved in managing Python environments throughout a project lifecycle.