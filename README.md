# OzoneWatch: Real-time Ozone Design Values

R shiny application developed by Ben Wells, US EPA

Latest Update: November 3, 2026

https://www.epa.gov/outdoor-air-quality-data/ozone-watch

## Description

Ozone Watch is designed to help users keep track of ozone concentrations reported during the current ozone season and understand the possible implications for attainment of the National Ambient Air Quality Standards (NAAQS) for ozone during the current 3-year design value period. The information in Ozone Watch is based on ozone concentration data reported to the EPA's Air Quality System (AQS) database, combined with real-time ozone concentration data reported to the EPA's AirNow database on days where the data have not yet been submitted to AQS.

The ozone concentration data for the current ozone season are refreshed each morning, beginning on April 1st and continuing throughout the year. Ozone concentration data for prior years are retrieved from AQS and refreshed on a monthly basis. This preliminary data is intended for informational use only. Data and figures obtained from Ozone Watch should NOT be used for regulatory or other official purposes.

## Repository contents

This repository contains the source code and other files for the Ozone Watch R shiny app. In addition to the app source code, the scripts needed to generate the data files used by the app. These scripts are contained in the 'code_dev' directory. They require SQL access to the AQS database and are intended for developers, but are not needed to run the app. This repository also contains a static set of data files in the 'data' directory that are needed for the app to run locally. Finally, the 'xlsx' directory contains a spreadsheet template used in generating a spreadsheet version of the data. It is largely obsolete except for calculating design values for the 1-hour ozone NAAQS which are not provided through the app.

1) `global.R` - loads R packages and global data files needed to run the app. Developers will need to make changes to this file for the app to function.
2) `server.R` and `ui.R` - scripts that provide the R shiny server-side functions and build the user interface for the app.
3) `deploy.R` - deployment script for Posit Connect deployments to the staging server only.
4) `OzoneWatch_update_hist.Rmd` - source code for the monthly process to refresh the historical AQS ozone concentration data files. Can only be run by users with direct SQL access to the AQS database. Provided for reference only, not needed to run the app locally.
5) `OzoneWatch_update_curr.Rmd` - source code for the daily process to refresh the AQS and AirNow ozone concentration data files for the current ozone season. Can only be run by users with direct SQL access to the AQS database. Provided for reference only, not needed to run the app locally.
6) `code/areamaps.r` - source code for the `Design Value Maps` application in Ozone Watch.
7) `code/dvtables.r` - source code for the `Design Value Tables` application in Ozone Watch.
8) `code/dvtrends.r` - source code for the `Design Value Trends` application in Ozone Watch.
9) `code/max4plot.r` - source code for the `Cumulative 4th Max Plots` application in Ozone Watch.
10) `code/rmapfuns.r` - helper functions used in the `Design Value Maps` application in Ozone Watch.
11) `code/tileplot.r` - source code for the `Daily AQI Tile Plots` application in Ozone Watch.
12) `code_dev/dvs_step1.r` and `code_dev/dvs_step2.r` - workhorse functions used to process ozone design values from hourly concentration data.
13) `code_dev/get_aqs_data.r`, `code_dev/get_monitors.r`, and `code_dev/get_rawdata.r` - helper functions used to retrieve AQS data.
14) `code_dev/get_naa_shape.r` - code used to generate the nonattainment area layer for the `Design Value Maps` application from EPA's public GIS layers.
15) `code_dev/update_curr_data.r` - source code to generate the merged current year AQS and AirNow data files. Intended as a local development environment version of `OzoneWatch_update_curr.Rmd`.
16) `code_dev/update_hist_data.r` - source code to generate the historical AQS data files. Intended as a local development environment version of `OzoneWatch_update_curr.Rmd`.
17) `data/daily_20261103.Rdata` and `data/sitedv_20261103.Rdata` - outputs from `code_dev/update_curr_data.r`.
18) `data/hist_1997_20261103.Rdata`, `data/hist_2008_20261103.Rdata`, and `data/hist_2015_20261103.Rdata` - outputs from `code_dev/update_hist_data.r`.
19) `data/map_data_20m.Rdata` and `data/map_data_500k.Rdata` - contains county, CBSA, and CSA layers for the `Design Value Maps` application in two different resolutions.
20) `data/O3_naa_1997.Rdata`, `data/O3_naa_2008.Rdata` and `data/O3_naa_2015.Rdata` - contains nonattainment area layers for the `Design Value Maps` application.
21) `xlsx/Ozone Watch template.xlsx` - template Excel file used in creating spreadsheet outputs.
22) `xlsx/Ozone_Watch_20261103.xlsx` and `sitedv_20261103.xlsx` - example spreadsheet outputs provided for reference.
