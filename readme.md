

### Background

A million tons of ice was shipped from Norway each year during the golden age of ice trade to England, India, South America, China and Australia. The business that involved transporting natural ice for commercial purposes stated at 1806, flourished in the end of 19th-century and was replaced by plant ice until World War I (“Ice Trade,” 2022). A primary concern of the ice transportation is the melt loss. Luckily, there is a record to follow. In 1959, a three-ton block of ice from Mo i Rana by the Arctic Circle was trucked to Libreville by the Equator in an advertisement of insulation materials. The event left enough details and measurements: 2714 Kg ice remained with only 11% mass loss(“Ice Block Expedition of 1959,” 2022) in a 27 days long journey. Using the EAR5 climate reanalysis dataset (Muñoz Sabater, 2021), this study is able to simulate the 1959 ice block expedition by surface energy balance equations and heat transfer equations, which is a new perspective on this event and the topic of ice trade.

### Solutions

Steps:

1. Read the story: [Ice block expedition of 1959 - Wikipedia](https://en.wikipedia.org/wiki/Ice_block_expedition_of_1959)
2. Generate kml by google map according to the story. -> eu.kml
3. Parse kml files to files: date, time, lat, lon. -> eu_timeseries.csv
4. Download climate replay dataset from [ERA5-Land hourly data from 1950 to present (copernicus.eu)](https://cds.climate.copernicus.eu/cdsapp#!/dataset/reanalysis-era5-land?tab=overview), -> .NetCDF.
   1. Sub_region extraction: North 67, South -2, West 0, East 15.
   2. Variables: 2m dew temp, 2m temp, ski temp, surface sensible heat flux, surface latent heat flux, surface thermal radiation downwards, surface solar radiation downwards, 10m u-component of wind, 10m v-component of wind, surface pressure, total precipitation.
   3. Month: Feb, Mar. Days: all, Time: all.
5. Couple 3&4 to generate 1d weather condition dataset (just variables and timeseries). -> 1d_1959.csv
   1. turn accumulative value into hours discrete value. interpolation needs some times.
6. Start modeling by energy balance equations parts -> 1D model
   1. turn wind from 10 to 2, and calculate relative wind speed at 2m.
7. Heat equations parts. -> 2D or 3D model
8. Discussion.

Three script were used in this project:

- parse-kml: step 3
- read_grib: step 5
- main: step 6&7

The requirements:
- numpy, pandas
- [process kml] geopandas, shapely
- [heat equations] numba, scipy


### Settings:



**Table 1**. *Characteristics of materials**.*

| **Type**   | **Properties**     | **density**  **[Kg/m3]** | **heat capacity**  **[J/(KgK)]** | **albedo** | **emissivity** | **H****eat**   **conductivity**  **[W/(mK)]** | **Diffusivity**  **[mm2/s]** | **Thickness**  **[m]** |
| ---------- | ------------------ | ------------------------ | -------------------------------- | ---------- | -------------- | --------------------------------------------- | ---------------------------- | ---------------------- |
| -          | Ice                | 917                      | 2108                             | 0.5        | 0.97           | 2.3                                           | 1.19                         | -                      |
| -          | Water              | 1000                     | 4182                             | -          | 0.97           | -                                             | -                            | -                      |
| Box        | Aluminum           | 2700                     | 890                              | 0.61       | 0.25           | 237                                           | -                            | 0.02                   |
| Box        | Galvanized steel   | 7800                     | 470                              | 0.61       | 0.04           | 52                                            | -                            | 0.02                   |
| Box        | Pine wood          | 510                      | 2301                             | 0.15       | 0.90           | 0.11                                          | -                            | 0.02                   |
| Insulation | Glass wool         | 20                       | 840                              | -          | -              | 0.04                                          | 1.79                         | 0.25                   |
| Insulation | Clay-Sawdust (10%) | 1648                     | 838                              | -          | -              | 0.63                                          | 0.45                         | 0.25                   |
| Insulation | Sawdust            | 210                      | 900                              | -          | -              | 0.08                                          | 0.42                         | 0.25                   |



### Results



**Table 3**. *Comparison between original record and simulations**.*

| Event                   | Initial Mass [Kg] | Insulation        | Insulation  thickness[m] | Mass lost  Algiers [Kg] | Mass loss  Sahara [Kg/day] | Mass loss  In total [Kg] |
| ----------------------- | ----------------- | ----------------- | ------------------------ | ----------------------- | -------------------------- | ------------------------ |
| Original records        | 3050              | Mainly glass wool | -                        | About 4                 | About 15                   | 336                      |
| Aluminum_glasswool_1959 | 3050              | Glass wool        | 0.25                     | 26.5                    | 14.3                       | 352.3                    |
| Aluminum_glasswool_2021 | 3050              | Glass wool        | 0.25                     | 32.0                    | 13.2                       | 348.2                    |
| Aluminum_glasswool_2020 | 3050              | Glass wool        | 0.25                     | 23.0                    | 11.7                       | 322.8                    |

 
