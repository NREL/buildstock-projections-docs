# Installation

## Minimum Installation Steps
This library is designed to run locally on your computer. Follow these steps to install:
- Clone the [buildstock-projections repo](https://github.com/NREL/buildstock-projections)
- Create & activate your favorite venv in the repo (I prefer pyenv for [mac/linux](https://github.com/pyenv/pyenv#installation) or [windows](https://github.com/pyenv-win/pyenv-win#installation), and the [virtualenv plugin](https://github.com/pyenv/pyenv-virtualenv))
- Run `python -m pip install . --user`
- If running for ResStock, clone the [resstock repo](https://github.com/NREL/resstock)

### ResStock
If running projections for ResStock, a local ResStock checkout is required to enable sampling of housing characteristics. Any version of ResStock should be compatible with BuildStock-Projections so long as the initial buildstock.csv aligns with the checked out version.

## Development
- Developers should install the library in editable mode: `python -m pip install -e .[dev] --user` 
- Activate pre-commit (only once, after making a new venv) with `pre-commit install`
  - Runs automatically on your staged changes before every commit
  - Settings and documentation links for pre-commit and ruff are in .pre-commit-config.yaml and pyproject.toml
  - To check all files, run `pre-commit run --all-files`
- Documentation will not build until there is a push to `main`, to view your updates to documentation as a Jupyter Book, install `jupyter-book` and run `jupyter-book build docs` from the parent directory.
- Pytest is used for unit testing, run `pytest .` to run them.