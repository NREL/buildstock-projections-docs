# Overview and Methodology
The buildstock-projections library calculates future building counts and tenure status while also providing a mechanism for users to define future code adoption and existing building technology adoption. This is accomplished through the modules described below. There are four steps that get repeated with each timestep, where operations are performed on the previous years buildstock. Modules are applied sequentially as:

1. Population Projections (prior to first timestep)
2. Demolition (at each timestep)
3. Vacancy (at each timestep)
4. New Construction (at each timestep)
5. Building Options (at each timestep)
   - New construction options
   - Existing building options
6. Results Vizualization (after final timestep)

## Population Projections [<sup>🔗</sup>](https://github.com/NREL/buildstock-projections/tree/main/buildstockprojections/population_estimate.py)
The population module imports population projection inputs and applies a regression to infer missing data.This module runs prior to iterating through future projection years and the outputs inform other modules. Inputs for the population module include baseline population data by PUMA, and population data for at least one future year. A 2nd order polynomial regression is applied to the input data to infer PUMA populations for timesteps in which there is no data. An example for one PUMA is shown below, where the population inputs are provided for 2020, 2030, and 2050.

:::{figure-md} population-fig
<img src="./inputs/population_projection_example.png" class="bg-primary mb-1" width="500px" height="500px">

Demonstration of the population regression for a single PUMA. Known datapoints include 2020, 2030, and 2050.
:::

Documentation regarding input files for population can be found [here](./inputs/population)


## Demolition [<sup>🔗</sup>](https://github.com/NREL/buildstock-projections/tree/main/buildstockprojections/demolition.py)
The demolition module calculates expected demolition and updates the building stock to represent demolition for each PUMA and year. The current demolition model is only implemented for ResStock and assumes flat demolition rates by building vintage with a random scaling factor. To apply rates, the total count of residential buildings by PUMA and Age Bin is calculated and reduced by the specific rate for the PUMA and age bin combination, as shown in the example below. This new count is then proportionately distributed to the building rows based on their PUMA and Age Bin.

| PUMA        | Age Bin     | Demolition Rate   | Initial Count | New Count     |
| ----------- | ----------- | -----------       | -----------   | -----------   |
| AL, 00100   | (0, 9)      | 0.001             | 8,744         | 8,735.26      |
| AL, 00100   | (10, 19)    | 0.0014            | 9,200         | 9,187.12      |
| ...         | ...         | ...               | ...           | ...           | 
| WY, 00500   | 100+        | 0.0021            | 7,500         | 7,484.25      |

This is a simplified approach to modeling demolition; future work will include implementing demolition projections for commercial buildings and building a more advanced model for projection residential demolition.  

## Vacancy [<sup>🔗</sup>](https://github.com/NREL/buildstock-projections/tree/main/buildstockprojections/vacancy.py)
Vacancy is only applicable to residential buildings, and follows a similar approach as demolition. Vacancy rates by PUMA are currently assumed to be the same for each future year run, defaulting to the vacancy rates calculated from the initial buildstock. The application of these rates differs from demolition in that the total count of buildings does not change, instead, just the ratio of occupied to vacant units changes. For a given PUMA, if the desired vacancy rate is higher than the existing, than the weights of occupied units is increased and the weights of vacant units is decreased, the opposite occurs when the desired vacancy rate is less than the existing.

Future work may include implementation of a more advanced vacancy model that introduces other external variables such as population change.


## New Construction [<sup>🔗</sup>](https://github.com/NREL/buildstock-projections/tree/main/buildstockprojections/new_construction.py)
The new construction module calculates the number of new units or buildings that should be added in a timestep, then samples to generate new buildstock samples. Additionally, the building options module is called to update the new construction and existing options, and the vacant units are scaled to ensure that the desired population projection is met. The steps for new construction differ for ResStock and ComStock:

**ResStock**
1. **Calculate New Construction Required**: The population change from the previous year is calculated and divided by the occupant density (people/unit) to get a desired number of new units for each PUMA. Constraints are applied to this operation to set minimum construction rates, maximum construction rates, and bounds on the year to year change in construction rates.
2. **Sample New Construction**: The desired number of units is divided by the base sample weight to estimate the number of new rows needed to meet the population. This number is provided to the sampler along with constraints to return only newer vintages.
3. **Correct Vacancy for Population**: Because of the constraints applied and the variability in sampled occupant density, the population change is not always met after sampling. This difference is addressed by adjusting the vacancy of the existing building stock in each PUMA - if sampled population is too low, vacant units are scaled down and occupied scaled up, if sampled population is too high, occupied units are scaled down and vacant units are scaled up. 
4. **Apply Building Options**: Characteristics of the sampled units are updated based on user inputs, as described in the next section.

**ComStock**
1. **Calculate New Construction Required**: The population change from the previous year is calculated and divided by the floor area density (people/ft$^2$) to get a desired number of new buildings for each PUMA and building type. This assumes that the initial year density is desired in future years, and new buildings are only added to meet the change in population.
2. **Sample New Construction**: Constraints limiting the PUMA, building type, and floor area are applied to the building stock and one building is sampled for each unique combination of those inputs. If no building exists, the floor area constraint is relaxed to allow for any size building to be sampled.
3. **Apply Building Options**: Characteristics of the sampled buildings are updated based on user inputs, as described in the next section.
   
## Building Options (New Construction & Existing)[<sup>🔗</sup>](https://github.com/NREL/buildstock-projections/tree/main/buildstockprojections/stock.py)
In each projection year, users can define a set up options that are applied to new construction and options applied to the existing set of buildings. Options applied to new construction can often be thought of as future energy code and equipment standards, while options applied to the existing stock may represent technology turnover or energy efficiency and equipment upgrades. Both the new and existing scenarios use the same format for inputs, and may be applied to subsets of the stock by using an adoption rate or housing characteristic filters, as further described in the [Projected Building Inputs](./inputs/building_options) section. 

## Results Visualization and Outputs [<sup>🔗</sup>](https://github.com/NREL/buildstock-projections/tree/main/buildstockprojections/results_viz.py)
**Outputs** - Several types of outputs can be retrieved from the workflow:
- **buildstock.csvs** (*optional*): a buildstock csv for each projection year
- **results_summary** (*optional*): a summary of the buildstocks aggregated at each year
- **upgrade reports**: Summaries of building options applied to existing and/or new construction buildings, describes the actual adoption rate and percentage of total stock to which an option is applied. One file is output for each year and existing/new construction distinction. 

**Results Visualization**  
Results visualization can be run as a separate post-processing step to help users understand the outputs of our workflow. In order to run visualizations, either a results summary or the buildstocks must be output during the main buildstock-projections run.  
- **Building counts**: output by occupancy status and projected year
- **Age counts**: output by building age bin and projected year
- **Housing parameters**: with a user-specified list of parameters, outputs one plot for each parameter showing the fraction homes with each option across all projection years


# Limitations
There exists limitations in the current model to be addressed in the future:
- No demolition model implemented for ComStock
- Only a simple demolition model for ResStock
- Only a simple vacancy model for ResStock
- ResStock sampling: the ResStock sampler can be called, however both PUMA and vintage cannot be applied to constrain the sampler, and therefore only PUMA is downselected and any vintage may be sampled (instead of just new buildings)
- ComStock sampling: the ComStock sampler cannot be called, and therefore sampling is performed only with the existing building stock
- Technology turnover can be modeled using an adoption rate, but cannot not depend on existing age of technologies
