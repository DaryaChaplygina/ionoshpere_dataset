# Ionospheric precursors to earthquake

This repository is a part of our research of anomalies in the ionosphere preceding earthquakes (ionospheric precursors to earthquakes). We processed a collection of ionosphere characteristics data, obtained from ground ionosondes, and applied two methods of precursors calculation. 

Data was collected over the period of 50 days before and 50 days after large earthquakes (magnitude ⩾ 5); only for ionosondes in earthquake preparation zone [[1]](#prep).

## Data sources

Data about the state of the ionosphere were obtained from the website of the [United States National Centers for Environmental Information](https://www.ngdc.noaa.gov/stp/iono/ionogram.html) (NCEI). 

The earthquakes information was obtained from the website of [United States Geological Survey](https://www.usgs.gov/natural-hazards/earthquake-hazards/earthquakes) (USGS).

## Data description
All data are stored in the ```.csv``` format with ```\t``` as column separator. 

### Earthquakes data

Folder ```USGS_dataset/``` contains the full list of earthquakes, during the period of which the data about the state of the ionosphere were obtained.

Earthquakes characteristics are described in the ```earthquakes_description``` table. There are also two supporting tables, ```sondes_in_eq_prep_zone``` and ```sondes_pairs```, that link and earthquake (_earthquake_id_) and ionosondes in the preparation zone of the earthquake (_sonde_). 

The last two tables contain attributes such as:
- (```eq_lat```, ```eq_lon```) and (```sonde_lat```, ```sonde_lon```) - coordinates of an earthquake epicenter and ionosonde respectively;
- ```prep_zone``` - earthquake preparation zone radius (km);
- ```eq_sonde_dist``` - distance from earthquake epicenter to ionosonde (km).

Ionosonde data in the ```sondes_pairs``` table are marked with indexes 1 and 2 for an ionosonde in the earthquake preparation zone (1) and an ionosonde outsize that zone, but close to the first one (2). 

### Ionosonde data
Ionosondes readings can be found in ```/NCEI_dataset/``` folder.

The table ```sondes_names``` contains general ionosondes information and has the following structure:

| sonde |	lat |	lon |	name |
|-------|-----|-----|------|
| ionosonde code name | latitude of ionosonde location | longitude of ionosonde location | region name of ionosonde location | 

There is ```{sonde}.csv``` file for each ionosonde in ```ionosondes_data/``` folder, where _sonde_ is ionosonde code name. Such file contains ionosonde readings (49 parameters describing the state of the ionosphere) and a time point, when these readings were observed. Each file has the same set of attributes, described below.

| sonde |	year | date |	h |	m | _parameter1_ | ... | _parameter49_ |
|-------|------|------|---|---|--------------|---|---------------|
| ionosonde code name | year | date in _YYYY-MM-DD_ format | hour | minute | 1st of 49 parameters | ... | 49th of 49 parameters | 

### Earthquake precursors

Features that may become an earthquake precursors are located in the ```/earthquake_precursors/``` folder. Functions to calculate such features from ionosonde readings could be found in ```/scripts/precursors_calculation.ipynb``` file.

```running_avg/``` folder contains ```{sonde}_runing_avg.csv``` files, where for every ionosonde sliding average of foF2 parameter is calculated over the last 15 days. As it was shown in _D. Davidenko & S. Pulinets_ [[2]](#mask) paper, foF2 deviates from its median values before an earthquake, so it can be considered as earthquake precursor.

For every time point, specified as (date, hour, minute), table contains:
- ```foF2``` -  foF2 current value;
- ```foF2_running_avg``` - sliding average of 15 previous foF2 values at the same moment in time;
- ```foF2_n_days``` - number of days (from 15 previous) when foF2 values was not missing .

Another feature considered as earthquake precursor could be found in ```correlation/``` folder. The article by  _S. Pulinets_ [[3]](#corr) demonstrates that if two ionosondes are close enough, but one of them located in the earthquake preparation zone and the other is outside that zone, then correlation of their readings (in particular, foF2) decreased.

File ```correlations.csv``` has the following features, calculated every day for nearby ionosondes pairs (sonde1, sonde2):
- ```foF2_corr``` - correlation of ionosondes' readings at current date; 	
- ```foF2_n_hours``` - number of hours per day when data from both ionosondes are known; 
- ```foF2_total_time_points``` - number of time points per day when data from both ionosondes are known.

## References

<a name="prep">[1]</a>  Zubkov S. I. Miachkin V. I. Dobrovolsky, I. P. Estimation of the size of earthquake
preparation zones. Pure and Applied Geophysics, 117(5):1025--1044, 1979.

<a name="mask">[2]</a>  Dmitry Davidenko and Sergey Pulinets. Deterministic variability of the ionosphere before strong (M ⩾  6) earthquakes in the regions of Greece and Italy based on many years of observation (in russian). Geomagnetism and Aeronomy, 59:529–544, January 2019.

<a name="corr">[3]</a> Sergey Pulinets. Ionospheric precursors of earthquakes; recent advances in theory and practical applications. Terrestrial Atmospheric And Oceanic Sciences, 15:445--467, September 2004.
