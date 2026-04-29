# LiveNeuro

LiveNeuro is an interactive brain visualization package for vector source
time series. It uses Plotly and Dash to combine source activity time courses
with anatomical brain projections.

This README orients contributors to the repository. User-facing installation
and usage documentation lives in the
[Sphinx documentation](https://liveneuro.readthedocs.io/).

## Repository Layout

- `src/liveneuro/`: package source code
- `tests/`: pytest suite covering imports, plotting behavior, integration paths,
  and basic performance checks
- `docs/`: Sphinx documentation for users
- `.github/workflows/`: CI checks for linting and tests
- `pyproject.toml`: packaging metadata, runtime dependencies, dev extras, and
  pytest configuration

## Contributor Setup

Create an environment with the scientific stack and Eelbrain available, 
then install LiveNeuro in editable mode with developer tools:

```bash
git clone https://github.com/liang-bo96/LiveNeuro.git
cd LiveNeuro
pip install -e ".[dev]"
```

If you are working from a checkout without installing editable, set
`PYTHONPATH=src` before running tests or scripts.

## Checks

Run the main test suite:

```bash
pytest
```

Build the user documentation when changing files under `docs/`:

```bash
cd docs
pip install -r requirements.txt
make html
```

CI also runs linting, package build, and package metadata checks.
