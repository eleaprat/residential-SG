# Optimal Operation of a Building with Electricity-Heat Networks and Seasonal Storage

The code (in Julia) is available in *operation_building.ipynb*. It includes many plots for the data used and the results.

We use the data from SIRTA and the tertiary building Drahi-X at Ecole Polytechnique, Paris, France. The data consists of hourly measurements between July of 2016 and July of 2022. From this, the data used are separated in different files, per component and time period, as detailed below. We also detail the dimensionning of the different components. We only use data from 2020 to 2022 here.

## Electricity Demand
We use the hourly consumption for zone 2 of the building Drahi-X. For some periods, the data is missing. We identify the hours in which the consumption is equal to 0 and replace it with the consumption of one week before at the same time.
The data are available in *data_D_DE_2020.csv* for 2020 and *data_D_DE.csv* for 2021 and the beginning of 2022.

## Solar PV
We use the data for the hourly production of one panel of 250W.
The data are available in *data_P_PV_2020.csv* for 2020 and *data_P_PV.csv* for 2021 and the beginning of 2022.
We calculate that 114 panels are necessary to cover the electrical demand for 2021. Based on this, we decide to assume that 80 panels are installed, which seems reasonable in terms of space required.

## Power Grid
We use the hourly spot electricity price for the zone DK2 of Denmark, which are publicly available on the entsoe Transparency Platform.
The data are available in *Day-ahead Prices_DK2_2020.csv* for 2020 and *Day-ahead Prices_DK2_2021_2022.csv* for 2021 and the beginning of 2022.
When buying electricity, a fixed transport fee of 0.20€/kWh is added.

## Electricity Storage
To evaluate the size of the electrical storage system, we look at the maximum difference between electrical load and PV output. This is used to set the maximum discharge to 10 kW and the maximum charge to 16 kW. We set the capacity of the storage system to 49 kWh which can correspond to the characteristics of a battery, for which the round trip efficiency should be 95% and the rate of self-discharge 1\% per hour. This corresponds to a charge efficiency and a discharge efficiency of 97% each.

## Heat Demand and Air Conditioning
Regarding the demand for heating, we use the total consumption of electricity for heating of the building. We assume that the consumption of electricity by the heaters corresponds to the heat demand in winter and to the heat production resulting from the use of the AC in summer, more precisely from the 1st of June to the 30th of September. This is an approximation, in practice we expect AC and heating to overlap for a part of the year. However we can observe that the consumption pattern changes significantly between the 1st of June and the 30th of September.
For the heat demand, the data are available in *data_D_DH_2020.csv* for 2020 and *data_D_DH.csv* for 2021 and the beginning of 2022.
For the production of heat from using air conditioning, the data are available in *data_P_AC_2020.csv* for 2020 and *data_P_AC.csv* for 2021 and the beginning of 2022.

## Solar Thermal
We use the irradiance data measured by SIRTA, a building close to Drahi-X over the year 2021. 
These data are available in *data_P_ST_2020.csv* for 2020 and *data_P_ST.csv* for 2021 and the beginning of 2022.
We then multiply it by an efficiency of 0.9 to get the production of 1 panel. We consider 12 panels, to cover the yearly consumption of heat.

## Thermal Storage
To evaluate the size of the thermal storage system, we look at the maximum difference between heat load and solar thermal output. This is used to set the maximum discharge to 9.18 kW and the maximum charge to 10.2 kW. We set the capacity of the storage system to 4640 kWh, which is sufficient to ensure that all the heat produced by the solar panels and the AC in the summer can be stored until the winter.
The round trip efficiency is set to 60%, which corresponds to a charge efficiency and a discharge efficiency of 78% each.
The rate of self-discharge is 0.007% per hour, such that the system loses 26% by self-discharge in 6 months.
We first impose that the storage system is empty at the beginning of the year and run the optimization for 2021 and until the beginning of 2022. We retrieve the final level at the end of the year, which is equal to 3000 kWh. Assuming that the behaviour follows an annual cyclical pattern, we further set that the initial level at the beginning of the year and the final level at the end of the year are equal to 3000 kWh. 

## Heat Pump
The capacity for heat production is 15 kW and the coefficient of performance is 4.
