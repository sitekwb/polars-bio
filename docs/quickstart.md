[polars-bio](https://pypi.org/project/polars-bio/) is available on PyPI and can be installed with pip:
```shell
pip install polars-bio
```
To enable support for Pandas DataFrames, install the `pandas` extra:
```shell
pip install polars-bio[pandas]
```
For visualization features, which depend on `bioframe` and `matplotlib`, install the `viz` extra:
```shell
pip install polars-bio[viz]
```
There are binary versions for Linux (x86_64), MacOS (x86_64 and arm64) and Windows (x86_64).

## Installation on macOS

!!! tip "Recommended method for macOS"
    For most macOS users, the simplest installation method is using the pre-built wheel from PyPI:
    
    ```shell
    pip install polars-bio
    ```
    
    This works for both Intel (x86_64) and Apple Silicon (arm64) Macs.

### Prerequisites for macOS

Before installing polars-bio, ensure you have:

- **Python 3.9-3.13** (Python 3.12+ recommended)
- **pip** (usually comes with Python)

!!! warning "Python version compatibility"
    - Python 3.13+ is fully supported and recommended
    - Avoid Python from Xcode Command Line Tools as it may cause linking issues
    - Use Python from [Homebrew](https://brew.sh/) or [python.org](https://python.org) for best compatibility

### Step-by-step installation for macOS

=== "Option 1: Using Homebrew Python (Recommended)"
    
    1. **Install Homebrew** (if not already installed):
    ```shell
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
    ```
    
    2. **Install Python via Homebrew**:
    ```shell
    brew install python@3.13
    ```
    
    3. **Create virtual environment**:
    ```shell
    /opt/homebrew/bin/python3 -m venv polars-bio-env
    source polars-bio-env/bin/activate
    ```
    
    4. **Install polars-bio**:
    ```shell
    pip install polars-bio
    ```
    
    5. **Verify installation**:
    ```shell
    python -c "import polars_bio; print('polars-bio installed successfully!')"
    ```

=== "Option 2: Using system Python"
    
    1. **Create virtual environment**:
    ```shell
    python3 -m venv polars-bio-env
    source polars-bio-env/bin/activate
    ```
    
    2. **Upgrade pip**:
    ```shell
    pip install --upgrade pip
    ```
    
    3. **Install polars-bio**:
    ```shell
    pip install polars-bio
    ```

### Troubleshooting macOS Installation

??? failure "ImportError: No module named 'polars_bio.polars_bio'"
    This usually indicates a name collision with local development files. Solutions:
    
    - Run Python from a different directory:
    ```shell
    cd /tmp
    python -c "import polars_bio; print('Success!')"
    ```
    
    - Or ensure you're not in a development directory with conflicting `polars_bio/` folder

??? failure "Symbol not found / Linking errors"
    This typically occurs with Python from Xcode Command Line Tools. Solutions:
    
    - Install Python via Homebrew (recommended):
    ```shell
    brew install python@3.13
    ```
    
    - Or use Python from python.org
    
    - If issues persist, try building from source (see below)

??? failure "Architecture mismatch errors"
    Ensure you're using the correct Python for your Mac's architecture:
    
    - For Apple Silicon (M1/M2/M3): Use arm64 Python
    - For Intel Macs: Use x86_64 Python
    
    Check your architecture:
    ```shell
    python -c "import platform; print(platform.machine())"
    ```

### Building from source on macOS

In case of other platforms (or errors indicating incompatibilites between Python's ABI), it is fairly easy to build polars-bio from source with [poetry](https://python-poetry.org/) and [maturin](https://github.com/PyO3/maturin):
```shell
git clone https://github.com/biodatageeks/polars-bio.git
cd polars-bio
poetry env use 3.12
poetry update
RUSTFLAGS="-Ctarget-cpu=native" maturin build --release -m Cargo.toml
```
and you should see the following output:
```shell
Compiling polars_bio v0.10.3 (/Users/mwiewior/research/git/polars-bio)
Finished `release` profile [optimized] target(s) in 1m 25s
📦 Built wheel for abi3 Python ≥ 3.8 to /Users/mwiewior/research/git/polars-bio/target/wheels/polars_bio-0.10.3-cp38-abi3-macosx_11_0_arm64.whl
```
and finally install the package with pip:
```bash
pip install /Users/mwiewior/research/git/polars-bio/target/wheels/polars_bio-0.10.3-cp38-abi3-macosx_11_0_arm64.whl
```
!!! tip
    Required dependencies:

    * Python>=3.9<3.14 (3.12 is recommended),
    * [poetry](https://python-poetry.org/)
    * cmake,
    * Rust compiler
    * Cargo
    are required to build the package from source. [rustup](https://rustup.rs/) is the recommended way to install Rust.


```python
import polars_bio as pb
pb.read_vcf("gs://gcp-public-data--gnomad/release/4.1/genome_sv/gnomad.v4.1.sv.sites.vcf.gz", compression_type="bgz").limit(3).collect()
```

```shell
shape: (3, 8)
┌───────┬───────┬────────┬────────────────────────────────┬─────┬───────┬───────┬─────────────────────┐
│ chrom ┆ start ┆ end    ┆ id                             ┆ ref ┆ alt   ┆ qual  ┆ filter              │
│ ---   ┆ ---   ┆ ---    ┆ ---                            ┆ --- ┆ ---   ┆ ---   ┆ ---                 │
│ str   ┆ u32   ┆ u32    ┆ str                            ┆ str ┆ str   ┆ f64   ┆ str                 │
╞═══════╪═══════╪════════╪════════════════════════════════╪═════╪═══════╪═══════╪═════════════════════╡
│ chr1  ┆ 10000 ┆ 295666 ┆ gnomAD-SV_v3_DUP_chr1_01c2781c ┆ N   ┆ <DUP> ┆ 134.0 ┆ HIGH_NCR            │
│ chr1  ┆ 10434 ┆ 10434  ┆ gnomAD-SV_v3_BND_chr1_1a45f73a ┆ N   ┆ <BND> ┆ 260.0 ┆ HIGH_NCR;UNRESOLVED │
│ chr1  ┆ 10440 ┆ 10440  ┆ gnomAD-SV_v3_BND_chr1_3fa36917 ┆ N   ┆ <BND> ┆ 198.0 ┆ HIGH_NCR;UNRESOLVED │
└───────┴───────┴────────┴────────────────────────────────┴─────┴───────┴───────┴─────────────────────┘

```

If you see the above output, you have successfully installed **polars-bio** and can start using it. Please refer to the [Tutorial](
/polars-bio/notebooks/tutorial/) and [API documentation](/polars-bio/api/) for more details on how to use the library.