# macOS Installation Guide for polars-bio

This guide provides detailed instructions for installing polars-bio on macOS, including troubleshooting common issues.

## Quick Start

For most macOS users, the simplest installation is:

```bash
pip install polars-bio
```

## Detailed Installation Steps

### Prerequisites

- **macOS 11.0+** (Big Sur or later)
- **Python 3.9-3.13** (Python 3.12+ recommended)
- **pip** package manager

### Recommended Installation Method

1. **Install Homebrew** (if not already installed):
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

2. **Install Python via Homebrew**:
```bash
brew install python@3.13
```

3. **Create a virtual environment**:
```bash
# Create environment
/opt/homebrew/bin/python3 -m venv polars-bio-env

# Activate environment
source polars-bio-env/bin/activate
```

4. **Install polars-bio**:
```bash
pip install --upgrade pip
pip install polars-bio
```

5. **Verify installation**:
```bash
python -c "import polars_bio; print('✅ polars-bio installed successfully!')"
```

### Testing Your Installation

Create a test script to verify everything works:

```python
#!/usr/bin/env python3
"""Test polars-bio installation"""

import polars as pl
import polars_bio as pb

# Test basic import
print("🧬 Testing polars-bio installation...")
print(f"✅ polars-bio version: {getattr(pb, '__version__', 'unknown')}")
print(f"✅ polars version: {pl.__version__}")

# Test basic functionality
df = pl.DataFrame({
    "chromosome": ["chr1", "chr2", "chr3"],
    "start": [100, 200, 300],
    "end": [150, 250, 350],
    "name": ["gene1", "gene2", "gene3"]
})

print("\n📊 Sample genomic data:")
print(df)

# Check available operations
operations = [attr for attr in dir(pb) if not attr.startswith('_')]
print(f"\n🔧 Available operations: {len(operations)}")
print("✅ Installation test completed successfully!")
```

Save this as `test_polars_bio.py` and run:
```bash
python test_polars_bio.py
```

## Architecture-Specific Notes

### Apple Silicon (M1/M2/M3) Macs

- Use native arm64 Python for best performance
- Homebrew automatically installs the correct architecture
- Verify with: `python -c "import platform; print(platform.machine())"`
  - Should output: `arm64`

### Intel Macs

- Use x86_64 Python
- Verify with: `python -c "import platform; print(platform.machine())"`
  - Should output: `x86_64`

## Common Issues and Solutions

### Issue 1: ModuleNotFoundError

**Error**: `ModuleNotFoundError: No module named 'polars_bio.polars_bio'`

**Cause**: Name collision with local development files

**Solutions**:
```bash
# Option 1: Run from different directory
cd /tmp
python -c "import polars_bio; print('Success!')"

# Option 2: Clean install
pip uninstall polars-bio -y
pip install polars-bio
```

### Issue 2: Symbol/Linking Errors

**Error**: Various linking or symbol not found errors

**Cause**: Incompatible Python installation (usually from Xcode)

**Solutions**:
```bash
# Install Homebrew Python
brew install python@3.13

# Create new environment with Homebrew Python
/opt/homebrew/bin/python3 -m venv new-env
source new-env/bin/activate
pip install polars-bio
```

### Issue 3: Architecture Mismatch

**Error**: Architecture-related errors

**Solutions**:
```bash
# Check your architecture
uname -m

# Ensure Python matches
python -c "import platform; print(platform.machine())"

# Reinstall with correct architecture
pip uninstall polars-bio -y
pip install polars-bio --force-reinstall
```

## Advanced Installation Options

### Installing with Optional Dependencies

```bash
# For pandas integration
pip install polars-bio[pandas]

# For visualization features
pip install polars-bio[viz]

# For all optional features
pip install polars-bio[pandas,viz]
```

### Development Installation

If you need to build from source:

```bash
# Install dependencies
brew install rust

# Clone and build
git clone https://github.com/biodatageeks/polars-bio.git
cd polars-bio

# Create environment
python3 -m venv .venv
source .venv/bin/activate

# Install Poetry
pip install poetry

# Install dependencies
poetry install

# Build (if needed)
RUSTFLAGS="-Ctarget-cpu=native" maturin develop --release
```

## Performance Tips for macOS

1. **Use Homebrew Python** for best compatibility
2. **Enable native CPU optimizations** when building from source
3. **Use virtual environments** to avoid conflicts
4. **Keep dependencies updated** for optimal performance

## Getting Help

If you encounter issues not covered here:

1. Check the [FAQ](faq.md)
2. Search [GitHub Issues](https://github.com/biodatageeks/polars-bio/issues)
3. Join the [Discord community](https://discord.gg/bpxQ4Yxhk5)
4. Review the [documentation](https://biodatageeks.org/polars-bio/)

## Next Steps

After successful installation:

- Read the [Tutorial](notebooks/tutorial.ipynb)
- Explore [Features](features.md)
- Check [Performance benchmarks](performance.md)
- Review [API documentation](api.md)
