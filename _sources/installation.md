# Installation

## Minimum Installation Steps
This library is designed to run locally on your computer, follow these steps to install:
- Clone the [buildstock-projections repo](https://github.com/NREL/buildstock-projections)
- Create & activate your favorite venv in the repo (I prefer pyenv for [mac/linux](https://github.com/pyenv/pyenv#installation) or [windows](https://github.com/pyenv-win/pyenv-win#installation), and the [virtualenv plugin](https://github.com/pyenv/pyenv-virtualenv))
- run `python -m pip install -e . --user`
- If running for ResStock, clone the [resstock repo](https://github.com/NREL/resstock)

### ResStock
If running projections for ResStock, a local ResStock checkout is required to enable sampling of housing characteristics. Any version of ResStock should be compatible with BuildStock-Projections so long as the initial buildstock.csv aligns with the checked out version.