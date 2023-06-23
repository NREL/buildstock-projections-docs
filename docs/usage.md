# Running the Workflow
BuildStock-Projections is run locally using a command line argument. Prior to running, make sure to follow the [installation instructions](./installation) and checkout a compatible version of the resstock repo (if running for residential buildings).

## Command Line Arguments
We recommend setting up a [project configuration file](./inputs/project_cfg) first, and running `run_abm -y <path_to_my_file.yml>` to run a workflow. Alternatively, the arguments specified in a configuration file can also be passed directly to the `run_abm` command, described in `run_abm --help` output:


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