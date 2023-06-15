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

## Population Projections [🔗](https://github.com/NREL/buildstock-projections/tree/main/buildstockprojections/population_estimate.py)
The population module imports population projection inputs and applies a regression to infer missing data.This module runs prior to iterating through future projection years and the outputs inform other modules. Inputs for the population module include baseline population data by PUMA, and population data for at least one future year. A 2nd order polynomial regression is applied to the input data to infer PUMA populations for timesteps in which there is no data. An example for one PUMA is shown below, where the population inputs are provided for 2020, 2030, and 2050.

<img src="./inputs/population_projection_example.png" width="60%" height="60%">  
  
Documentation regarding input files for population can be found [here](./inputs/population)


## Demolition [🔗](https://github.com/NREL/buildstock-projections/tree/main/buildstockprojections/demolition.py)
The demolition module calculates expected demolition and updates the building stock to represent demolition for each PUMA and year. The current demolition model is only implemented for ResStock and assumes flat demolition rates by building vintage with a random scaling factor. To apply rates, the total count of residential buildings by PUMA and Age Bin is calculated and reduced by the specific rate for the PUMA and age bin combination, as shown in the example below. This new count is then proportionately distributed to the building rows based on their PUMA and Age Bin.

| PUMA        | Age Bin     | Demolition Rate   | Initial Count | New Count     |
| ----------- | ----------- | -----------       | -----------   | -----------   |
| AL, 00100   | (0, 9)      | 0.001             | 8,744         | 8,735.26      |
| AL, 00100   | (10, 19)    | 0.0014            | 9,200         | 9,187.12      |
| ...         | ...         | ...               | ...           | ...           | 
| WY, 00500   | 100+        | 0.0021            | 7,500         | 7,484.25      |

This is a very simplified approach to modeling demolition; future work will include implementing demolition projections for commercial buildings and building a more advanced model for projection residential demolition.  

## Vacancy [🔗](https://github.com/NREL/buildstock-projections/tree/main/buildstockprojections/vacancy.py)
Vacancy is only applicable to residential buildings, and follows a similar approach as demolition. Vacancy rates by PUMA are currently assumed to be the same for each future year run, defaulting to the vacancy rates calculated from the initial buildstock. The application of these rates differs from demolition in that the total count of buildings does not change, instead, just the ratio of occupied to vacant units changes. For a given PUMA, if the desired vacancy rate is higher than the existing, than the weights of occupied units is increased and the weights of vacant units is decreased, the opposite occurs when the desired vacancy rate is less than the existing.

Future work may include implementation of a more advanced vacancy model that introduces other external variables such as population change.


## New Construction [🔗](https://github.com/NREL/buildstock-projections/tree/main/buildstockprojections/new_construction.py)
The new construction module calculates the number of new units or buildings that should be added in a timestep, then samples to generate new buildstock samples. Additionally, the building options module is called to update the new construction and existing options, and the vacant units are scaled to ensure that the desired population projection is met. The steps for new construction differ for ResStock and ComStock:

**ResStock**
- **New Construction Required**: The population change from the previous year is calculated and divided by the occupant density to get a desired number of new units for each PUMA. Constraints are applied to this operation to apply a minimum construction rates, maximum construction rates, and bounds on the year to year change in construction rates.
- **New Construction Sampling**: The desired number of units is divided by the base sample weight to estimate the number of new rows needed to meet the population. This
- **Vacancy Correction**: Because of the constraints applied and the , the population change is not always met after sampling 

**ComStock**
- **New Construction Required**: The population change from the previous year is calculated and divided by the occupant density to get a desired number of new units for each PUMA.
- **New Construction Sampling**: The desired number of units is divided by the base sample weight to
- **Vacancy Correction**: Not applicable to commercial 

## Building Options (New Construction & Existing)[🔗](https://github.com/NREL/buildstock-projections/tree/main/buildstockprojections/stock.py)


## Results Visualization [🔗](https://github.com/NREL/buildstock-projections/tree/main/buildstockprojections/results_viz.py)

...


# Limitations
Gaps in the current model:
- Not demolition model for ComStock
- Simple demo model for ResStock
- Simple vacancy model for RessTock
