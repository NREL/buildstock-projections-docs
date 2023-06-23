# Running the Workflow

... how to run the workflow

- checkout your branch of resstock...\
  
### FROM OLD README:
- Detailed documentation to help you start can be found [here](#documentation)
- `run_ABM` in the terminal to run the workflow and generate future buildstock.csv files (`run_ABM --help` for options)
    - Ensure your local checkout of resstock (the resstock project_directory) matches the initial buildstock version, currently v3.0.0 in resources/project_national.yml
    - Ensure the resources/options_lookup.tsv file is from the same version of resstock as well
- `results_viz` in the terminal to generate figures using the projected buildstock.csv files (`results_viz --help` for options)


## Command Line Arguments

```console
usage: run_abm [-h] [-z HORIZON] [-t TIMESTEP] [-b BUILDSTOCK_PATH]
               [--write_buildstocks] [--write_results_summary]
               [--input_filepath_population INPUT_FILEPATH_POPULATION]
               [--input_filepath_existing INPUT_FILEPATH_EXISTING]
               [--input_filepath_new_construction INPUT_FILEPATH_NEW_CONSTRUCTION]
               [-pd PROJECT_DIRECTORY] [--base_weight BASE_WEIGHT]
               [-s {resstock,comstock}] [-p] [-y Y]

options:
  -h, --help            show this help message and exit
  -z HORIZON, --horizon HORIZON
                        Projection horizon (year), starts at 2020
  -t TIMESTEP, --timestep TIMESTEP
                        Timestep (years).
  -b BUILDSTOCK_PATH, --buildstock_path BUILDSTOCK_PATH
                        Buildstock csv path.
  --write_buildstocks   Write buildstock csvs.
  --write_results_summary
                        Write results summary csv.
  --input_filepath_population INPUT_FILEPATH_POPULATION
                        Path to directory containing population file inputs.
  --input_filepath_existing INPUT_FILEPATH_EXISTING
                        Path to directory containing existing file inputs
  --input_filepath_new_construction INPUT_FILEPATH_NEW_CONSTRUCTION
                        Path to directory containing new construction file
                        inputs
  -pd PROJECT_DIRECTORY, --project_directory PROJECT_DIRECTORY
                        Path to the ResStock project directory, if applicable.
  --base_weight BASE_WEIGHT
                        Starting weight to apply to all rows in initial
                        buildstock
  -s {resstock,comstock}, --stock_type {resstock,comstock}
                        Stock type, resstock or comstock
  -p, --print_statements
                        Print outputs after each step
  -y Y                  Filepath to yml input.

```

## Development
- to build docs, install jupyter-book and run `jupyter-book build docs`
- to run tests...
- to run pre-commit hooks