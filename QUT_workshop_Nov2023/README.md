# QUT training workshop
## 24 Nov 2023

## Goal

To use the GBR Data Management System (DMS) to explore and analyse temperature data from the Great Barrier Reef (GBR)

## Problem

NOAA Coral Reef Watch program provides satellite-derived sea surface temperature (SST) data for the GBR. With a cloudless 
5km-pixel resolution and daily coverage, this dataset constitutes a valuable resource for monitoring the GBR. 
However, the satellite-derived SST data calibration is only sometimes accurate, especially in the shallow areas of the reefs. 
To improve the accuracy of the SST data, it is possible to calibrate the satellite-derived SST data with in-situ temperature loggers. 
The Australian Institute of Marine Science (AIMS) has deployed temperature loggers in the GBR. The AIMS temperature loggers are located 
in the shallow areas of the reefs, where the satellite-derived SST data is less accurate.
We want to produce a "calibration factor" for the satellite-derived SST data using the AIMS temperature loggers. 
This calibration factor will be used to improve the accuracy of the satellite-derived SST data.
We can also explore the calibration factor that allows us to estimate the water temperature at the reefs' deeper areas (6-12m). 
For that, we will use data from the logger deployed at the reef slopes and compare it with the satellite-derived SST data.
Also, it would be interesting to check if the calibration factors are the same for all the reefs in the GBR. 
It is well known that very few calibration data exist for internal areas of the GBR, as the commonly used drifters are 
not deployed in these areas as they get stuck in the reefs. Therefore, the calibration factors may differ for the GBR's internal areas.


## Files 

The files required for this workshop are: 

| File name | Description | Type | S3 URI                                                       |
| --- | --- | --- |--------------------------------------------------------------|
| NOAA CRW SST | NOAA Coral Reef Watch Sea Surface Temperature (SST) | Zarr | s3://rimrep-data-public/noaa-crw-chs-sst/data.zarr/          |
| AIMS Temperature loggers | Australian Institute of Marine Science (AIMS) Temperature loggers | geoParquet | s3://rimrep-data-public/091-aims-sst/test-50-64-spatialpart/ | 
| GBR features | Great Barrier Reef (GBR) features | geoParquet | s3://rimrep-data-public/gbrmpa-complete-gbr-features/data.parquet/                                                             |


