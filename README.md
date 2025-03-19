# Reten Documentation

This repository contains the technical documentation for all Reten services and components.

## Overview

The documentation is built using MkDocs with the Material theme, providing a modern and searchable interface for all Reten's technical documentation.

## Structure

```
reten-docs/
├── docs/                  # Documentation source files
│   ├── index.md          # Home page
│   ├── properties/       # Data entities documentation
│   └── events/          # Events documentation
├── src/                  # Source code for custom extensions
└── mkdocs.yml           # MkDocs configuration
```

## Local Development

1. Create and activate a virtual environment:
```bash
python -m venv .venv
source .venv/bin/activate  # On Unix or MacOS
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Start the documentation server:
```bash
mkdocs serve
```

4. View the documentation at http://127.0.0.1:8000

## Building for Production

To build the static site:
```bash
mkdocs build
```

The built site will be in the `site` directory.

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## Documentation Standards

- Use Markdown for all documentation files
- Include code examples where appropriate
- Keep the documentation up to date with the latest changes
- Follow the established structure for consistency

## Contact

For questions or suggestions about the documentation, please contact the Reten team. 