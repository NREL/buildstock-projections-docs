# BuildStock Files
A buildstock.csv file is required to initialize the ResStock and/or ComStock building stocks. 
- This library contains example buildstocks for [ResStock]([resources/res_buildstock_550k_v3.0.0.csv.gz](https://github.com/NREL/buildstock-projections/blob/main/resources/res_buildstock_550k_v3.0.0.csv.gz)) v3.0.0 and [ComStock]([resources/com_buildstock_350k.csv.gz](https://github.com/NREL/buildstock-projections/blob/main/resources/com_buildstock_350k.csv.gz)) are in the [resources](https://github.com/NREL/buildstock-projections/tree/main/resources) folder. 
- If you want to build your own buildstock files you may use [this cli](https://github.com/NREL/resstock/blob/develop/resources/run_sampling.rb) from ResStock. Ensure the version of ResStock you use to generate that file matches the options_lookup file and the building inputs files.
- To confirm the buildstock files generated from this library are compatible with ResStock, check out ResStock `develop` branch, move into the `test` dir, and call `ruby check_buildstock.rb -h` for details on further commands the check needs to operate correctly
    - Check_buildstock cannot read compressed files, so be sure to unzip the outputs first

A custom `options_lookup.tsv` located in the [resources](https://github.com/NREL/buildstock-projections/tree/main/resources) directory is also needed for ResStock. To pass the validation checks when running ResStock, the file must be compatible with the inputted buildstock.csv and all future vintages must be added, for example:

| Parameter Name        | Option Name  |
| ----------- | ----------- |
| Vintage  | 2010s (*existing*)     |
| Vintage | 2025 (*new*)        |
| ...         | ...         |
| Vintage | 2050 (*new*)        |
