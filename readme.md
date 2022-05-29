

### Background

A million tons of ice was shipped from Norway each year during the golden age of ice trade to England, India, South America, China and Australia. The business that involved transporting natural ice for commercial purposes stated at 1806, flourished in the end of 19th-century and was replaced by plant ice until World War I (“Ice Trade,” 2022). A primary concern of the ice transportation is the melt loss. Luckily, there is a record to follow. In 1959, a three-ton block of ice from Mo i Rana by the Arctic Circle was trucked to Libreville by the Equator in an advertisement of insulation materials. The event left enough details and measurements: 2714 Kg ice remained with only 11% mass loss(“Ice Block Expedition of 1959,” 2022) in a 27 days long journey. Using the EAR5 climate reanalysis dataset (Muñoz Sabater, 2021), this study is able to simulate the 1959 ice block expedition by surface energy balance equations and heat transfer equations, which is a new perspective on this event and the topic of ice trade.

### Solutions

Steps:

1. Read the story line by line: [Ice block expedition of 1959 - Wikipedia](https://en.wikipedia.org/wiki/Ice_block_expedition_of_1959), [«Isblokkekspedisjonen» 1959 ~ Arkitektur  & Miljøteknologi (arkitekturnytt.no)](http://www.arkitekturnytt.no/2017/10/isblokkekspedisjonen-1959.html)
2. Generate .kml by google map according to the story. -> eu.kml
3. Parse kml files to files: date, time, lat, lon. -> eu_timeseries.csv
4. Download climate replay dataset from [ERA5-Land hourly data from 1950 to present (copernicus.eu)](https://cds.climate.copernicus.eu/cdsapp#!/dataset/reanalysis-era5-land?tab=overview), -> .NetCDF.
   1. Sub_region extraction: North 67, South -2, West 0, East 15.
   2. Variables: 2m dew temp, 2m temp, ski temp, surface sensible heat flux, surface latent heat flux, surface thermal radiation downwards, surface solar radiation downwards, 10m u-component of wind, 10m v-component of wind, surface pressure, total precipitation.
   3. Month: Feb, Mar. Days: all, Time: all.
5. Couple 3&4 to generate 1D weather condition dataset (just variables and timeseries). -> 1d_1959.csv
   1. turn accumulative value into hours discrete value. Interpolation needed some times.
6. Start modeling by energy balance equations parts -> 1D model
   1. turn wind from 10 m to 2 m, and calculate relative wind speed at 2 m.
   1. energy balance on the box surface. and heat transfer = melt energy -> mass loss
7. Heat equations parts. -> 2D or 3D model
   1. answering how the ice block is warming up, and ripe out.

8. Discussion.
   1. 1959 vs 2020, 2021
   2. how the covering and insulation prevent ice from melting.


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

This study reproduced the ice block expedition 1959 in multiple scenarios using surface energy balance models and heat equation model. The results include (1) how the ice would melt without the cover and insulation. (2) The influence of box material and insulation material. (3) how long do the warming up and ripe phase take. The study compared between simulation 1959, 2021. If there are only bare ice, the 2021 would be the first to melt away, and the 1959 would survive 2 more days. If there is covering and insulation, the 1959 lost even more. This is because the melting mechanism behaves differently on the ice surface and the cover surface.

There are some results that are very close the original records. The simulations shows that only about 3 cm thick, or 348.8 Kg to 352.3 Kg of the ice block melted. The heat equation simulation revealed temperature distribution of ice inside the box. The ice block takes 175 hours warming up from -6° to 0° in the expedition.

Transporting ice to the tropics in the 19th century was totally manageable business. Using a white-covered wooden box filled with double thickness is able to achieve the same insulation effect as glass wool, which is fully expected by simulations. We can use the ice block expedition of 1959 as a baseline. If we do an expedition again, the melt loss of transporting ice is not expected over than 1959. And, the simulations do not support blaming it to climate change because there is a covering upon ice.

More result in juypter notebook.



**Table 3**. *Comparison between original record and simulations**.*

| Event                   | Initial Mass [Kg] | Insulation        | Insulation  thickness[m] | Mass lost  Algiers [Kg] | Mass loss  Sahara [Kg/day] | Mass loss  In total [Kg] |
| ----------------------- | ----------------- | ----------------- | ------------------------ | ----------------------- | -------------------------- | ------------------------ |
| Original records        | 3050              | Mainly glass wool | -                        | About 4                 | About 15                   | 336                      |
| Aluminum_glasswool_1959 | 3050              | Glass wool        | 0.25                     | 26.5                    | 14.3                       | 352.3                    |
| Aluminum_glasswool_2021 | 3050              | Glass wool        | 0.25                     | 32.0                    | 13.2                       | 348.2                    |
| Aluminum_glasswool_2020 | 3050              | Glass wool        | 0.25                     | 23.0                    | 11.7                       | 322.8                    |

 
