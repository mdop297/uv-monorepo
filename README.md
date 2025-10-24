1. Initializing the Workspace

```bash
mkdir uvws && cd uvws
uv init --package  # Initialize the root project

# Test the root project
uv run uvws uvws  # Expected output: Hello from uvws!

# Initialize the core package
uv init packages/core --package --name uvws-core
# run the core script (see pyproject.toml project.scripts])
uv run --package uvws-core uvws-core  # Expected output: Hello from uvws-core!
# can run via python (this works because uv creates pth file per package)
source ./.venv/bin/activate
python -c "from uvws_core import main; main()"
```

2. Adding Functionality to `core`
```bash
echo -e '\n__version__ = "0.0.0"\n\ndef hi() -> str:\n    return "hi from core"' >> ./packages/core/src/uvws_core/__init__.py

# Run the method
uv run --package uvws-core python -c "import uvws_core; print(uvws_core.hi())"
```
3. 