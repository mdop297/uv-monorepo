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

3. Creating and Linking svc1

```bash
uv init packages/svc1 --package --name uvws-svc1
uv run --package uvws-svc1 uvws-svc1  # Expected output: Hello from uvws-svc1!

# Make core a dependency of svc1
uv add --package uvws-svc1 ./packages/core

# Verify dependencies
cat ./packages/svc1/pyproject.toml
```

4. Using core in svc1
```bash
echo -e 'from uvws_core import hi\n\n__version__ = "0.0.0"\n\n\ndef main() -> None:\n    print("svc1 say: ", hi())' > packages/svc1/src/uvws_svc1/__init__.py

# Test the updated svc1 (by running the uvws-svc1 script)
uv run --package uvws_svc1 uvws-svc1  # Expected output: hi from core
```

5. Ensuring Build works
```bash
uv build --all-packages  # Builds uvws, core, and svc1
tar -tzf ./dist/uvws_svc1-0.1.0.tar.gz # inspect tgz package
unzip -l ./dist/uvws_svc1-0.1.0-py3-none-any.whl # inspect whl package
# inspect that svc1 package depends on uvws-core package
unzip -p ./dist/uvws_svc1-0.1.0-py3-none-any.whl 'uvws_svc1-0.1.0.dist-info/METADATA' | grep '^Requires-Dist'
rm -rf ./dist
```

6. Installing Dependencies
```bash
uv add python-semantic-release --dev
```

7. Downloading the Monorepo Parser
```bash
mkdir -p ./scripts/psr/custom_parser
curl https://raw.githubusercontent.com/asaf/uvws/refs/heads/main/scripts/psr/custom_parser/monorepo_parser.py -o ./scripts/psr/custom_parser/monorepo_parser.py
```