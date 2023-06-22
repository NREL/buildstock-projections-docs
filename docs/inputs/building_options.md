# Projected Building Inputs
At each timestep, building stock inputs can be updated for the existing stock and/or the set of new construction buildings with the use of a common .csv file convention. We provide example files for new construction (ResStock and ComStock) [here](https://github.com/NREL/buildstock-projections/tree/main/resources/inputs), while more complex test files for [existing](https://github.com/NREL/buildstock-projections/tree/main/tests/test_files/project_CA/existing) and [new construction](https://github.com/NREL/buildstock-projections/tree/main/tests/test_files/project_CA/new_construction) ResStock buildings can be found in the [tests](https://github.com/NREL/buildstock-projections/tree/main/tests/test_files/project_CA) directory.

## Package Apply File
Both new construction and existing inputs may include a package_apply.csv file.

## Building Option Files
FIXME

### Old readme - [Building inputs files](resources/inputs/new_construction_resstock/)
- These input files describe how existing and new construction building characteristics get updated at each future timestep
- These are applied at each timestep, so a timestep greater than 1 may lead to the new higher efficiency building characteristics being applied earlier than expected
    - e.g. if the timestep is 5 (sampling at 2020, 2025, 2030, etc) and a new efficiency characteristic is added in 2027, when the 2030 sample occurs all buildings between 2025 & 2030 will have the new higher efficiency
- Examples are found in [resources/inputs/new_construction_*/](resources/inputs/) to define future building changes in buildstock parameters
    - ComStock input files are not yet defined. Development is left to the user
- _**Should be altered**_ to match your estimates of future building codes and technology adoption rates
- Must be compatible with the current buildstock version
