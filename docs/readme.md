# BuildStock Projections [![Jupyter Book Badge](https://jupyterbook.org/badge.svg)](https://NREL.github.io/buildstock-projections-docs/)
\***_Warning_\*: As of June 2023 this library is in beta and is not appropriate for production use.**

This package is used to generate inputs to [ResStock](https://resstock.nrel.gov/) and [ComStock](https://www.nrel.gov/buildings/comstock.html) for use in simulation of future building stock scenarios. Building stock projections consider the influence of demolition, vacancy, new construction, future code, and existing stock technology turnover at the Public Use Microdata Area (PUMA) geographic resolution.

The buildstock-projections tool receives numerous inputs to define baseline housing characteristics, future populations, and technology adoption, then outputs residential and/or commercial buildstock.csv input files corresponding with future years at pre-configured time intervals. These files can be used to simulate future building energy usage when run with [BuildStockBatch](https://github.com/NREL/buildstockbatch) and future weather files (not provided in this repo).

The target users of this tool are researchers with experience running ResStock and/or ComStock. If not already familiar, we recommend reading the respective documentation on these tools prior to running buildstock-projections.

More information on methodology, installation, and running can be found in the [documentation](https://NREL.github.io/buildstock-projections-docs).
