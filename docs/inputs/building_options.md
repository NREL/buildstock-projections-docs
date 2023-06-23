# Future Building Options
At each timestep, building stock inputs can be updated for the existing stock and/or the set of new construction buildings with the use of a common .csv file convention. We provide example files for new construction (ResStock and ComStock) [here](https://github.com/NREL/buildstock-projections/tree/main/resources/inputs), while more complex test files for [existing](https://github.com/NREL/buildstock-projections/tree/main/tests/test_files/project_CA/existing) and [new construction](https://github.com/NREL/buildstock-projections/tree/main/tests/test_files/project_CA/new_construction) ResStock buildings can be found in the [tests](https://github.com/NREL/buildstock-projections/tree/main/tests/test_files/project_CA) directory. The goal of these inputs are to change specific options for future building stock states.

## Package Apply File
Both new construction and existing inputs may include a package_apply file as a way to help organize inputs and specify in which years packages get applied. The following columns are used to specify how packages are applied, all other column headers will be ignored:

`Year`: year in which to apply the package  
`Package Apply`:  building option specification file to apply  
`Housing Characteristics <n>`: *(optional)*: characteristics by which to filter the building stock before applying the package. Any number of Housing Characteristics columns can be included (1..n) and the filters follow the convention `Parameter||Option 1|Option 2|...|Option n`.


The example below demonstrates how this might be applied to set future code levels by state for new construction homes; here there are four different code packages that get applied in 2025 differently ddepending on the state.

| Year        | Package Apply    | Housing Characteristics 1   | 
| ----------- | ----------- | -----------       | 
| 2025          | iecc_2021     | State\|\|CA\|WA\|VT           | 
| 2025   | iecc_2018    | State\|\|OR\|NE\|PA\|MD\|DC\|DE\|NY\|MA\|NH           | 
| 2025   | iecc_2018    | State\|\|HI\|TX\|ME         | 
| 2025   | iecc_2009    | State\|\|AK\|AZ\|AR\|TN\|CO\|KS\|MO\|MS\|ND\|SD\|WY\|MT\|ID\|NV\|UT\|NM\|OK\|MN\|IA\|LA\|WI\|IL\|MI\|IN\|OH\|KY\|AL\|GA\|FL\|SC\|NC\|VA\|WV\|NJ\|CT\|RI         | 
| ...         | ...         | ...               | 

## Building Option Specification Files
At least one building option specification file, or "package", is required to describe what and how options are updated. The following columns are used to specify this, all other column headers will be ignored:

`Parameter`: parameter to be updated change   
`Option`:  specific option to which to change to.
`Parameter Category`: a unique descriptor that groups together rows, all common parameters must have the same category, but multiple parameters can have the same category.
`Adoption Rate`: The fraction of buildings that have that option applied (dependent on the Housing Characteristics columns).
`Housing Characeteristics <n>` *(optional)*: characteristics by which to filter the building stock before applying the package options. Any number of Housing Characteristics columns can be included (1..n) and the filters follow the convention `Parameter||Option 1|Option 2|...|Option n`.

The example below demonstrates how a package may be used to define code minimum and above-code options for new construction buildings in ResStock. This file will apply ceiling insulation options based on the climate zone of a building, aligning with the 2009 IECC requirements. In this scenario, 75% of climate zone 4A-5B (w/ vented attics) have the R-38 code minimum insulation applied, while 25% of those climate zones receive an above-code R-49 insulation. Each row also specifies that these options only applied to homes with vented attics as the `Insulation Ceiling` option would be invalid for other home attic types. 

| Parameter         | Option        | Parameter Category    | Adoption Rate | Housing Characteristics 1                                 |Housing Characteristics 2  |
| -----------       | -----------   | -----------           | -----------   | -----------                                               |-----------                |
| Insulation Ceiling| R-30          | Roof/Ceiling          | 1.0           | ASHRAE IECC Climate Zone 2004\|\|1A\|2A\|2B\|3A\|3B\|3C   | Geometry Attic Type\|\|Vented Attic |
| Insulation Ceiling| R-38          | Roof/Ceiling          | 0.75          | ASHRAE IECC Climate Zone 2004\|\|4A\|4B\|4C\|5A\|5B       | Geometry Attic Type\|\|Vented Attic |
| Insulation Ceiling| R-49          | Roof/Ceiling          | 0.25          | ASHRAE IECC Climate Zone 2004\|\|4A\|4B\|4C\|5A\|5B       | Geometry Attic Type\|\|Vented Attic |
| Insulation Ceiling| R-49          | Roof/Ceiling          | 1.0           | ASHRAE IECC Climate Zone 2004\|\|6A\|6B\|7A\|7B           | Geometry Attic Type\|\|Vented Attic |
| ...               | ...           | ...                   | ...           | ...                                                       | ...           | 

## Limitations
The options specified in packages are applied at each timestep, so a timestep greater than 1 may lead to higher efficiency technologies applied earlier than desired. For example, if the timestep is 5 years (sampling at 2020, 2025, 2030, etc) and a new efficiency characteristic is specified for 2027 and applied in 2030, it will apply to the full 5 years represented in 2030 (2026, 2027, 2028, 2029, 2030), instead of just 2027 on.

