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

## Requirements and credentials needed

The app requires the R packages `data.table`, `DT`, `pins`, `plyr`, `reshape2`, `sf` and `shiny`. `deploy.R`
additionally requires `rsconnect` and Git.

For local use, copy `.Renviron.example` to `.Renviron` in the repository root and fill in the settings required for your workflow. Restart R after creating or editing `.Renviron` so the values are loaded.

The data refresh reports, `OzoneWatch_update_curr.Rmd` and `OzoneWatch_update_hist.Rmd`, use `AQS_userid` and `AQS_passwd` for direct AQS database access. If the report runs on Posit Connect, configure them in the data refresh report environment, separately from the Shiny app.

## How to refresh the supporting data

`OzoneWatch_update_curr.Rmd` and `OzoneWatch_update_hist.Rmd` are provided for reference only, they are not needed to run the app locally. These data refresh processes requires direct AQS database access and publishes the supporting data to the OzoneWatch_curr_data, OzoneWatch_hist1997, OzoneWatch_hist2008, and OzoneWatch_hist2015 pins. This process is separate from running or publishing the Shiny app.

The bundled files in the `data` directory are a static snapshot; they are not updated automatically when the app runs. Local and staging results may differ from public results if the snapshot and the public pin are based on different data files. If a newer snapshot is needed for testing, the R scripts `code_dev/update_curr_data.r` and `code_dev/update_hist_data.r` can be used to refresh the data locally. Additionally, the R script `code_dev/get_naa_shape.r` can be used to make changes to the nonattainment layers in `data/O3_naa_2015.Rdata`, `data/O3_naa_2008.Rdata`, and `data/O3_naa_1997.Rdata` as needed, as well as write these layers to the `Ozone_Watch_map_data` pin on the Posit Connect staging server.

## How to get the application running locally

1) Clone this repository to your local machine.
2) Set AQS_userid and AQS_passwd in your local .Renviron file.
3) In global.R, set server.type <- "local" to use the bundled data.
4) In RStudio, open the repository folder, then run the app from ui.R, or run shiny::runApp() from the R console.
5) Restore server.type <- "remote" before committing or pushing changes.

## Using the application

Ozone Watch consists of five applications, which are listed in the first drop-down menu on the left. Additional information and instructions specific to each application will be shown beneath this paragraph the first time it is selected in the drop-down menu. Users can return to this screen at any time by clicking the 'View Intro' button that will appear after content is generated.

## Testing and deploying to staging

### Option 1: Deploy to staging from RStudio

1) Check out the feature or review branch you want to test.
2) In global.R, temporarily set server.type <- "local".
3) Run the app locally and confirm it uses the bundled data.
4) Restore server.type <- "remote" in global.R.
5) Use RStudio's Publish button and select the staging Connect server and account.

### Option 2: Deploy to staging with deploy.R
This script is for staging only. It always packages the app with server.type <- "remote" so it uses the pins stored on the remote server, regardless of the current branch or the tracked setting in global.R. Configure the staging server URL and, if needed, staging credentials in your local .Renviron file (or in the deployment environment):

CONNECT_STAGING_SERVER=https://your-staging-connect-server
CONNECT_STAGING_ACCOUNT=your_account
CONNECT_STAGING_API_KEY=your_api_key

CONNECT_STAGING_SERVER is required; there is no fallback to a generic or public-server setting. CONNECT_STAGING_ACCOUNT defaults to dmap, and the API key can be omitted if rsconnect is already configured for that staging server and account. Set the URL to the staging Connect server. The script creates a branch-specific app name prefixed with ee_dvcalc_staging_. It can be run from any branch from the repository root, directly in the RStudio console.

## Public Deployment

deploy.R does not deploy to the public server. Public deployment must use a separate, explicitly reviewed workflow; confirm the target, app name, data source, and hosted credentials with the developer before publishing public content. Public deployments need AQS_API_USER, AQS_API_KEY, app_owner, and access to the supporting pin.

## Contact

Ben Wells, US EPA (Wells.Benjamin@epa.gov)
