# Intro

[![DOI](https://zenodo.org/badge/580744536.svg)](https://doi.org/10.5281/zenodo.13941723)

These scripts can be used to convert FLUXNET2015 CSV files into netCDF files. The meteorological data files are prepared to drive the LPJ-GUESS model. We use the carbon and water fluxes from FLUXNET2015 as reference data for model testing and evaluation. Thanks to Ben and Konni for their contributions. Note that we only convert daily values; sub-daily data conversion is not implemented.

What these scripts do:

Convert data format from FLUXNET2015 (CSV) to CF-compliant (not always) netCDF4 files.

Convert units to match LPJ-GUESS inputs.

Create one file per site and variable (one time series per file) for reference data.

Reference data has different time periods for each site.

Observations with quality flags lower than 0.75 were excluded from reference data.

Create one file per variable for all sites for driving (climatic) data.

Evapotranspiration is calculated from Temperature and Latent Heat flux (Allen et al., 1998)

Driving variables cover the period from 1989-12-31 to 2019-12-31 (ERA downscaled data - Warm Winter Dataset).

Create some Precipitation (pr) and VPD (vpd) datasets with modified values (for testing purposes).

## Use

Currently we have the folowing [sites](./driver/FLUXNET2015.grd) configured.

Everything is set to convert those sites. You just need to download the appropriate data from [FLUXNET 2015 repository](https://fluxnet.org/login/?redirect_to=/data/download-data/) and unzip the files for each site in a child directory. All folders with data for each FLUXNET site must be inside this child directory. You will need to edit the script [site_data.py](./site_data.py) in line 12. The variable ```fluxnet_data``` must point to this folder, all sites included will be processed. You can use the [clean_csv.py script](./clean_csvs.py) to remove unused subfolders in the FLUXNET zipped archives (after unzip it).

With that, run the main script:

``$ python fluxnet2netcdf.py``

This will populate two subfolders in the parent directory:

Daily Reference data (NEE, GPP, Reco and ET) are stored in a new folder named ref.

Daily driver variables (tas, wind, rsds, pr, ps, hurs and VPD) are stored [here](./driver/).

Metadata about variables used here can be found in the [nc_write.py script](./nc_write.py). They are hardcoded in two dictionaries: FLUXNET_FULLSET_VARS and OBS_VARS. These names refer respectively to the daily meteorological data downscaled with ERA5 - gapfilled with the MDS method; and to the variables related with the carbon and water fluxes (Pastorello et al., 2020).

There are some details ommited in this readme. Feel free to reach me, I can help you. If you are interested in modify this code feel free to do pull requests or wathever.

FLUXNET data must be downloaded from the [FLUXNET 2015 repository](https://fluxnet.org/login/?redirect_to=/data/download-data/) (Registration required). The dataset for which the main script is configured to can be found here: <https://www.icos-cp.eu/data-products/2G60-ZHAK>

## Cite

Darela Filho, J. P. and Gregor, K.: Fluxnet_to_Netcdf: Convert Fluxnet2015 Csv Files to Netcdf Format, Zenodo [code], https://doi.org/https://doi.org/10.5281/zenodo.13941723, 2024.

## References

Allen, R. G. et al.: Crop Evapotranspiration - Guidelines for Computing Crop Water Requirements, Fao Irrigation and Drainage Paper 56, Food and Agriculture Organization of the United Nations, [https://www.fao.org/4/x0490e/x0490e00.htm#Contents1998](https://www.fao.org/4/x0490e/x0490e00.htm#Contents1998), 1998.

ICOS: Warm Winter 2020 Ecosystem Eddy Covariance Flux Product for 73 Stations in Fluxnet-Archive Format—Release 2022-1 [dataset], [https://doi.org/https://doi.org/10.18160/2G60-ZHAK](https://doi.org/https://doi.org/10.18160/2G60-ZHAK), 2022.

Pastorello, G. et al.: The Fluxnet2015 Dataset and the Oneflux Processing Pipeline for Eddy Covariance Data, Sci Data, 7, 225, [https://doi.org/10.1038/s41597-020-0534-3](https://doi.org/10.1038/s41597-020-0534-3), 2020.
