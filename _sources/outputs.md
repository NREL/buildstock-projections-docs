# Outputs and Vizualizations
Output files from the workflow can be specified at run time, while visualizations can be generated with a post-processing script using the output files. Output files are always saved to the directory `./outputs`.
## Output Files
Several types of outputs can be retrieved from the workflow:
- **buildstock.csvs** (*optional*): a buildstock csv for each projection year; optional output that is specied during [project configuration](./inputs/project_cfg)
- **results_summary** (*optional*): a summary of the buildstocks aggregated at each year; optional output that is specied during [project configuration](./inputs/project_cfg)
- **upgrade reports**: Summaries of building options applied to existing and/or new construction buildings, describes the actual adoption rate and percentage of total stock to which an option is applied. One file is output for each year and existing/new construction distinction. 

### Results Summary
If specified, results_summary.csv files can be output for ResStock and ComStock that aggregates a select set of outputs by year. The following oclumns are included:
- *Year*: projection year	
- *Population*: total population, aligns with [population inputs](./inputs/population)
- *Demolished*: count of units or buildings demolished in that year	
- *Vacancy rate*: final vacancy rate that meets the population constraints (ResStock only)
- *Desired Vacancy Change*: desired vacancy rate output from the vacancy module (ResStock only)
- *Final Vacancy Change*: final count units that change from occupied to vacant (ResStock only)	
- *New Construction Occupied*: number of new units occupied (ResStock only)
- *Vacant*: number of vacant units (ResStock only) 	
- *Total buildings*: total number of housing units or buildings	
- *Cumulative demolished*: sum of units or buildings demolished from the first year
- *age_\<yr1\>_\<yr2\>*: number of units or buildings that belong to the age bin (yr1, yr2)  

### Upgrade Reports
Upgrade reports are designed to help users understand how building option speicifications for new construction and existing buildings are applied. These files are particularly important to review for options applied with adoption rates < 1 and/or there are extensive housing characteristics filters applied. 

These reports are similar to the building option specification files, but have additional columns and are organized differently. Firstly, there is one file for each year and existing/new construction distinction, aggregating what could be numerous option files (i.e., packages) into one file that shows the options applied in that year. Columns added to these reports that are not in the input files include "Package", "Percent of stock applied", "Desired adoption rate", and "Actual adoption rate", as shown in the example table below. 

| Package        | Parameter                | Option        | Parameter Category    | Percent of stock applied  | Desired adoption rate | Actual adoption rate  | Housing Characteristics 1 | Housing Characteristics 2 | 
| -----------    | -----------              | -----------   | -----------           | -----------               | -----------           | -----------           | -----------               | -----------   |
| iecc_2018      | Insulation Floor         | Ceiling R-13  | Foundation            | 3.48E-05                  | 0.05                  | 0.05714               | ASHRAE IECC Climate Zone 2004\|\|['1A', '2A', '2B']      | Geometry Foundation Type\|\|['Vented Crawlspace', 'Unheated Basement', 'Ambient']            |
| iecc_2018      | Insulation Floor         | Ceiling R-19  | Foundation            | 0.0189                    | 0.05                  | 0.05002               | ASHRAE IECC Climate Zone 2004\|\|['3A', '3B', '3C', '4A', '4B']      | Geometry Foundation Type\|\|['Vented Crawlspace', 'Unheated Basement', 'Ambient']|
| ...            | ...                      | ...               | ...               | ...                       | ...                   | ...                   | ...                       | ...         |
| CFR_2021       | HVAC Cooling Efficiency  | AC, SEER 14   | HVAC Cooling          | 0.4498                    | 1.0                   | 1.0                   | HVAC Cooling Type\|\|['Central AC']           | HVAC Has Shared System\|\|['None', 'Heating Only']         |

The desired adoption rate reflects the inputted value, while the actual indicates what percentage of the building subset the option was actually applied to. These numbers differ because options are applied to a discrete number of rows, and perfectly meeting the adoption rate is generally not possible. The percent of stock applied refers to the percentage of the entire building stock that the option is applied, if there are no housing characteristics filters applied, than this number will be the same as the actual adoption rate.



## Vizualization
Results visualization can be run as a separate post-processing step to help users understand the outputs of our workflow. In order to run visualizations, either a results summary or the buildstocks must be output during the main buildstock-projections run. Use the `results_viz` command in the terminal to generate figures (`results_viz --help` for options)
- **Building counts**: output by occupancy status and projected year
- **Age counts**: output by building age bin and projected year
- **Housing parameters**: with a user-specified list of parameters, outputs one plot for each parameter showing the fraction homes with each option across all projection years