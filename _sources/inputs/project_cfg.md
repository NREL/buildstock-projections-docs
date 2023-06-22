# Project Configuration
Arguments for the workflow can be set by passing inputs directly to the `run_abm` command or using a configuration .yml file, as described below. 

## Configuration file
We recommend using a .yml project file to define configuration inputs. [`project_national_test.yml`](https://github.com/NREL/buildstock-projections/tree/main/resources/project_national.yml) is an example file shown below with descriptions of the inputs

```yml
horizon: 2040
timestep: 5
print_statements: True
write_buildstocks: True
write_results_summary: True
input_filepath_population: './inputs/population'
comstock:
  buildstock_path: './com_buildstock_350k.csv.gz'
  input_filepath_new_construction: './inputs/new_construction_comstock'
  input_filepath_existing: './inputs/existing_comstock'
  geo_resolution: 'na'
resstock:
  buildstock_path: './res_buildstock_550k_v3.0.0.csv.gz'
  project_directory: '../resstock/project_national'
  input_filepath_new_construction: './inputs/new_construction_resstock'
  input_filepath_existing: './inputs/existing_resstock'
  geo_resolution: 'na'
```

`horizon` (*optional*) (int): The projection horizon across which the workflow runs specified as a year > 2020.  
&nbsp;&nbsp;&nbsp;&nbsp;**Units**: date (year), **Default**=2040  
`timestep` (*optional*) (int): The time interval in years at which the workflow runs.  
&nbsp;&nbsp;&nbsp;&nbsp;**Units**: year, **Default**=5  
`print_statements` (*optional*) (bool): print timestep-level details to the terminal.  
&nbsp;&nbsp;&nbsp;&nbsp;**Default**=True  
`write_buildstocks` (*optional*) (bool): write the buildstock.csvs for each timestep to buildstock-projections/outputs.  
&nbsp;&nbsp;&nbsp;&nbsp;**Default**=True  
`write_results_summary` (*optional*) (bool): write a csv file summarizing various outputs as detailed in the [outputs](../outputs) section.  
&nbsp;&nbsp;&nbsp;&nbsp;**Default**=True  
`input_filepath_population` (*optional*) (str): path to population input files  
&nbsp;&nbsp;&nbsp;&nbsp;**Default**='buildstockprojections/resources/inputs/population'  
`stock_type (resstock/comstock)`: configuration file must contain at least a `comstock` or a `resstock` block, described below. 
### ResStock Configuration
`buildstock_path` (str): filepath to the baseline ResStock buildstock.csv; can be an absolute or relative path.      
`project_directory` (str):  ResStock project directory path that contains `housing_characteristics/` and `options_lookup.tsv`; can be an absolute or relative path.   
`input_filepath_new_construction` (str): directory containing future year building options to be applied to new construction buildings, such as future energy codes; can be an absolute or relative path.   
`input_filepath_existing` (*optional*) (str): directory containing future year building options to be applied to existing buildings; can be an absolute or relative path. 
`geo_resolution` (*optional*) (str): **Not implemented**. Specify the geographic resolution at which modules are run. Default="PUMA"
### ComStock Configuation
`buildstock_path`: filepath to the baseline ComStock buildstock.csv; can be absolute or realtive.      
`input_filepath_new_construction` (str): directory containing future year building options to be applied to new construction buildings, such as future energy codes; can be an absolute or relative path.   
`input_filepath_existing` (*optional*) (str): directory containing future year building options to be applied to existing buildings; can be an absolute or relative path. 
`geo_resolution` (*optional*) (str): **Not implemented**. Specify the geographic resolution at which modules are run. Default="PUMA"

## Command Line
As an alternative to using a configuration file, the above inputs can be passed directly to the terminal command as arguments, which is described in more detail in [Running the Workflow](../usage)   
