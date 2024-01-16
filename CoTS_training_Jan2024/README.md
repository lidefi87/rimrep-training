<<<<<<< HEAD:CoTS-training-Jan2024/README.md
# CoTS Use Case Training Workshop
### 23 January 2024

## Goal

To use the GBR Data Management System (DMS) to extract and analyse data (point or time series data) from selected datasets provided by the DMS

## How to use DMS services and data

The GBR DMS provides a **metadata catalogue**, a **public AWS S3 repository of datasets** and a **data API**. The DMS also manages non-public datasets that could be accessed after authorisation.

<details>
<summary><b>The STAC metadata catalogue</b></summary>

The metadata catalogue is the discovery portal. The datasets are organised in items inside collections. A collection is a group of similar items (datasets) maintained by the same data provider or refers to a similar type of data. For example, GBRMPA maintains a set of administrative regions (like GBR marine park limit) or natural features (GBR features); all these datasets (items) are under the same collection (GBRMPA Administrative Regions).

At the collection level, you can search for datasets based on names or keywords. The search will return the collections that contain items related to your query. Selecting one collection you can search again by temporal/spatial extent and names.It will return a set of item that fit your query.

</details>

<details>
<summary><b>The AWS S3 public repository</b></summary>
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aliquam tincidunt ligula eu ligula fermentum aliquet. Donec gravida urna et sapien dictum tristique. Pellentesque sed nunc ut dolor dignissim iaculis. Sed quam dui, gravida nec eros eget, tincidunt aliquet arcu. Vestibulum sollicitudin neque at sem accumsan porta. Etiam ipsum quam, vehicula quis laoreet vitae, pellentesque quis erat. Morbi tincidunt tincidunt nisl eget sagittis. Vivamus pulvinar elit in enim hendrerit, eget varius metus tincidunt. Sed leo neque, feugiat ac diam a, mollis elementum libero. Nulla vitae ex ac purus consequat blandit. In dui libero, condimentum sed commodo at, interdum vitae erat. Nullam consequat magna in fermentum semper. Quisque tortor urna, imperdiet sit amet luctus nec, iaculis at mauris.

</details>

<details>
<summary><b>The data API</b></summary>
the DMS API services are provided by our `pygeoapi` service. This is a python implementation of the new OGC standards. With it, you can make API calls to request portions of the data, slicing it by time and/or space.

To access the API services, a client needs to be created by the DMS. This client will have an unique set of credentials (secrets) that will be used to request an access token. You need to pass the token along with any API call. For security reasons, the token will have only one hour of validity, so probably you will like to request a new token for every new api call. This token is also valid for making STAC fast-api call (metadata). To request the token you need to provide the Client secrets.

Using command line: 

```
CLIENT_ID=<provided by the DMS>
CLIENT_SECRET=<provided by the DMS>
ACCESS_TOKEN=$(curl --location --request POST "https://keycloak.reefdata.io/realms/rimrep-production/protocol/openid-connect/token" -s \
  --header "Content-Type: application/x-www-form-urlencoded" \
  --data-urlencode "client_id=$CLIENT_ID" \
  --data-urlencode "client_secret=$CLIENT_SECRET" \
  --data-urlencode "grant_type=client_credentials" | jq -r '.["access_token"]')
```



</details>



## Case Study examples

We will be working on the following use case examples: 

1. Extract eReef model time series data for a selected point (coordinates) or reef (reef name), using hydrological and biochemical products.
2. Extract and calculate average values for eReef model variables for an area defined by a shape file withing an specified time frame
3. Extract a time series of values from LTMP/MMP modelled output for a particular reef or a list of reefs

## Notebooks



## Datasets to be used

The files required for this workshop are: 

| Dataset Title                                                                                  | STAC Metadata URL                                                                                           | s3 URI                                                                               | Pygeoapi URL                                                                               | Security       |
|------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|----------------|
| AIMS - eReefs Aggregations of Biogeochemistry Model Outputs (Baseline Annual Sediments)        | https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-biogeochem-baseline-annual-sed        | s3://gbr-dms-data-public/aims-ereefs-biogeochem-baseline-annual-sed/data.zarr        | https://pygeoapi.reefdata.io/collections/aims-ereefs-biogeochem-baseline-annual-sed        | Public         |
| AIMS - eReefs Aggregations of Biogeochemistry Model Outputs (Baseline Annual)                  | https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-biogeochem-baseline-annual            | s3://gbr-dms-data-public/aims-ereefs-biogeochem-baseline-annual/data.zarr            | https://pygeoapi.reefdata.io/collections/aims-ereefs-biogeochem-baseline-annual            | Public         |
| AIMS - eReefs Aggregations of Biogeochemistry Model Outputs (Baseline Daily-Monthly Sediments) | https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-biogeochem-baseline-daily-monthly-sed | s3://gbr-dms-data-public/aims-ereefs-biogeochem-baseline-daily-monthly-sed/data.zarr | https://pygeoapi.reefdata.io/collections/aims-ereefs-biogeochem-baseline-daily-monthly-sed | Public         |
| AIMS - eReefs Aggregations of Biogeochemistry Model Outputs (Baseline Daily-Monthly)           | https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-biogeochem-baseline-daily-monthly     | s3://gbr-dms-data-public/aims-ereefs-biogeochem-baseline-daily-monthly/data.zarr     | https://pygeoapi.reefdata.io/collections/aims-ereefs-biogeochem-baseline-daily-monthly     | Public         |
| AIMS - eReefs Aggregations of Biogeochemistry Model Outputs (Baseline Monthly Sediments)       | https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-biogeochem-baseline-monthly-sed       | s3://gbr-dms-data-public/aims-ereefs-biogeochem-baseline-monthly-sed/data.zarr       | https://pygeoapi.reefdata.io/collections/aims-ereefs-biogeochem-baseline-monthly-sed       | Public         |
| AIMS - eReefs Aggregations of Biogeochemistry Model Outputs (Baseline Monthly)                 | https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-biogeochem-baseline-monthly           | s3://gbr-dms-data-public/aims-ereefs-biogeochem-baseline-monthly/data.zarr           | https://pygeoapi.reefdata.io/collections/aims-ereefs-biogeochem-baseline-monthly           | Public         |
| AIMS - eReefs Aggregations of Hydrodynamic Model Outputs (1km Annual)                          | https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-agg-hydrodynamic-1km-annual           | s3://gbr-dms-data-public/aims-ereefs-agg-hydrodynamic-1km-annual/data.zarr           | https://pygeoapi.reefdata.io/collections/aims-ereefs-agg-hydrodynamic-1km-annual           | Public         |
| AIMS - eReefs Aggregations of Hydrodynamic Model Outputs (1km Daily)                           | https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-agg-hydrodynamic-1km-daily            | s3://gbr-dms-data-public/aims-ereefs-agg-hydrodynamic-1km-daily/data.zarr            | https://pygeoapi.reefdata.io/collections/aims-ereefs-agg-hydrodynamic-1km-daily            | Public         |
| AIMS - eReefs Aggregations of Hydrodynamic Model Outputs (1km Monthly)                         | https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-agg-hydrodynamic-1km-monthly          | s3://gbr-dms-data-public/aims-ereefs-agg-hydrodynamic-1km-monthly/data.zarr          | https://pygeoapi.reefdata.io/collections/aims-ereefs-agg-hydrodynamic-1km-monthly          | Public         |
| AIMS - eReefs Aggregations of Hydrodynamic Model Outputs (4km Annual)                          | https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-agg-hydrodynamic-4km-annual           | s3://gbr-dms-data-public/aims-ereefs-agg-hydrodynamic-4km-annual/data.zarr           | https://pygeoapi.reefdata.io/collections/aims-ereefs-agg-hydrodynamic-4km-annual           | Public         |
| AIMS - eReefs Aggregations of Hydrodynamic Model Outputs (4km Daily)                           | https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-agg-hydrodynamic-4km-daily            | s3://gbr-dms-data-public/aims-ereefs-agg-hydrodynamic-4km-daily/data.zarr            | https://pygeoapi.reefdata.io/collections/aims-ereefs-agg-hydrodynamic-4km-daily            | Public         |
| AIMS - eReefs Aggregations of Hydrodynamic Model Outputs (4km Monthly)                         | https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-agg-hydrodynamic-4km-monthly          | s3://gbr-dms-data-public/aims-ereefs-agg-hydrodynamic-4km-monthly/data.zarr          | https://pygeoapi.reefdata.io/collections/aims-ereefs-agg-hydrodynamic-4km-monthly          | Public         |
| AIMS - LTMP Manta Tow Surveys                                                                  | https://stac.reefdata.io/browser/collections/aims-ltmp/items/aims-ltmp-manta-tows                           | s3://gbr-dms-data-limited-access/aims-ltmp-manta-tows/data.parquet                   | https://pygeoapi.reefdata.io/collections/aims-ltmp-manta-tows                              | Limited Access |
| BOM - AUSWAVE 10-day Forecast                                                                  | https://stac.reefdata.io/browser/collections/bom-auswave/items/bom-auswave-10d                              | s3://gbr-dms-data-public/bom-auswave-10d/data.zarr                                   | https://pygeoapi.reefdata.io/collections/bom-auswave-10d                                   | Public         |
| BOM - AUSWAVE 3-day Forecast                                                                   | https://stac.reefdata.io/browser/collections/bom-auswave/items/bom-auswave-3d                               | s3://gbr-dms-data-public/bom-auswave-3d/data.zarr                                    | https://pygeoapi.reefdata.io/collections/bom-auswave-3d                                    | Public         |
| GBRMPA - Benthic Map                                                                           | https://stac.reefdata.io/browser/collections/gbrmpa-admin-regions/items/gbrmpa-benthic-map                  | s3://gbr-dms-data-public/gbrmpa-benthic-map/data.zarr                                | https://pygeoapi.reefdata.io/collections/gbrmpa-benthic-map                                | Public         |
| GBRMPA - Complete GBR Features                                                                 | https://stac.reefdata.io/browser/collections/gbrmpa-admin-regions/items/gbrmpa-complete-gbr-features        | s3://gbr-dms-data-public/gbrmpa-complete-gbr-features/data.parquet                   | https://pygeoapi.reefdata.io/collections/gbrmpa-complete-gbr-features                      | Public         |
| GBRMPA - Geomorphic Map                                                                        | https://stac.reefdata.io/browser/collections/gbrmpa-admin-regions/items/gbrmpa-geomorphic-map               | s3://gbr-dms-data-public/gbrmpa-geomorphic-map/data.zarr                             | https://pygeoapi.reefdata.io/collections/gbrmpa-geomorphic-map                             | Public         |
| Geoscience Australia - High-resolution depth model for the GBR (30m)                           | https://stac.reefdata.io/browser/collections/ga-gbr-hr-depth-model/items/ga-bathymetry-gbr-30m              | s3://gbr-dms-data-public/ga-bathymetry-gbr-30m/data.zarr                             | https://pygeoapi.reefdata.io/collections/ga-bathymetry-gbr-30m                             | Public         |
| IMOS - Satellite Remote Sensing Aqua Ocean Colour Chlorophyll-a (4km)                          | https://stac.reefdata.io/browser/collections/imos-satellite-remote-sensing/items/imos-srs-aqua-oc-chla-4km  | s3://gbr-dms-data-public/imos-srs-aqua-oc-chla-4km/data.zarr                         | https://pygeoapi.reefdata.io/collections/imos-srs-aqua-oc-chla-4km                         | Public         |


=======
# Crown of Thorns Starfish (CoTS) Use Case Training Workshop
### 23 January 2024

## Goal

To use the [GBR Data Management System (DMS)](https://stac.reefdata.io/browser/?.language=en) to extract and analyse data (point or time series data) from selected datasets hosted in the DMS. Note that you will need to have a GBR DMS account to access data.  
  
## How to use DMS services and data

The GBR DMS provides three ways to search for public data: a **metadata catalogue**, a **public AWS S3 repository of datasets**, and a **data API**. Click on the titles below to find more information about each of these methods.  
  
Also note that while the majority of datasets in the DMS are publicly available, the DMS also manages non-public datasets that could be accessed after authorisation from the original data provider. This is the reason why DMS users need an account to access this system.     
  
<details>
<summary><b>The STAC metadata catalogue</b></summary>

The metadata catalogue is the discovery portal. The datasets are organised as *items* inside **collections**. A **collection** is a group of similar *items* (datasets) either maintained by the same data provider (e.g., GBRMPA), or it can also refer to a similar type of data. For example, GBRMPA maintains a set of administrative regions (e.g., GBR marine protected area boundaries) and another for natural features (e.g., reefs inside the boundaries of the GBR). Both of these datasets (*items*) are included under the same *collection** ([GBRMPA Administrative Spatial Regions](https://stac.reefdata.io/browser/collections/gbrmpa-admin-regions)).  
  
In the DMS, you can search for datasets by their name or using keywords. This search will return any collections that contain items related to your query. You can further search filter results by selecting one of the collection and searching by temporal/spatial extent and names. This will return a set of items (within the chosen collection) that fits your query.  
  
</details>

<details>
<summary><b>The AWS S3 public repository</b></summary>
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aliquam tincidunt ligula eu ligula fermentum aliquet. Donec gravida urna et sapien dictum tristique. Pellentesque sed nunc ut dolor dignissim iaculis. Sed quam dui, gravida nec eros eget, tincidunt aliquet arcu. Vestibulum sollicitudin neque at sem accumsan porta. Etiam ipsum quam, vehicula quis laoreet vitae, pellentesque quis erat. Morbi tincidunt tincidunt nisl eget sagittis. Vivamus pulvinar elit in enim hendrerit, eget varius metus tincidunt. Sed leo neque, feugiat ac diam a, mollis elementum libero. Nulla vitae ex ac purus consequat blandit. In dui libero, condimentum sed commodo at, interdum vitae erat. Nullam consequat magna in fermentum semper. Quisque tortor urna, imperdiet sit amet luctus nec, iaculis at mauris.

</details>

<details>
<summary><b>The data API</b></summary>
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aliquam tincidunt ligula eu ligula fermentum aliquet. Donec gravida urna et sapien dictum tristique. Pellentesque sed nunc ut dolor dignissim iaculis. Sed quam dui, gravida nec eros eget, tincidunt aliquet arcu. Vestibulum sollicitudin neque at sem accumsan porta. Etiam ipsum quam, vehicula quis laoreet vitae, pellentesque quis erat. Morbi tincidunt tincidunt nisl eget sagittis. Vivamus pulvinar elit in enim hendrerit, eget varius metus tincidunt. Sed leo neque, feugiat ac diam a, mollis elementum libero. Nulla vitae ex ac purus consequat blandit. In dui libero, condimentum sed commodo at, interdum vitae erat. Nullam consequat magna in fermentum semper. Quisque tortor urna, imperdiet sit amet luctus nec, iaculis at mauris.

</details>



## Case Study examples

We will be working on the following use case examples:  

1. Extract eReef model time series data for a selected point (coordinates) or reef (reef name), using hydrological and biochemical products.  
2. Extract and calculate average values for eReef model variables for an area defined by a shapefile within a specified time frame.  
3. Extract a time series from LTMP/MMP model output for a particular reef or list of reefs.  

## Notebooks

Example notebooks for this workshop were developed in `R` because it is the most widely used programming language within the CoTS team. However, the DMS can also be accessed using `Python`, you can see some examples in [this repository](https://github.com/aodn/rimrep-examples/tree/main/Python_based_scripts). Before running these `R` notebooks, make sure you have installed all libraries used in this workshop.  
  
To keep the DMS secure, we provide DMS users with tokens that last xxx hours. While tokens are current, users are able to access any public datasets in the DMS, as well as any non-public dataset for which they have been granted permission. Tokens should be treated similar to passwords and they should not be shared.  
  
To ensure you do not accidentally share a token within a script, we recommend that you create an environmental variable in `R` called `RIMREP_DMS_TOKEN` to store your token. You can create this environmental variable as follows:  
  
```R
#Create or update the environmental variable 
Sys.setenv("RIMREP_DMS_TOKEN" = "paste_DMS_token_here")
#Check environmental variable has been corrected created/updated
Sys.getenv("RIMREP_DMS_TOKEN")
```
  
**Note:** The DMS token must be given within quotation marks, for example: `"example_token123"`. If you provide the token as `example_token123`, that is without quotation marks (`""`), you will get an error.   
  
In the example notebooks, we will show you how to use this environmental variable to access gridded data in `zarr` format.  
  
## Datasets to be used

In this workshop, we will use the following datasets: 

| Dataset Title                                                                                  | STAC Metadata URL                                                                                           | s3 URI                                                                               | Pygeoapi URL                                                                               | Security       |
|------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|----------------|
| AIMS - eReefs Aggregations of Biogeochemistry Model Outputs (Baseline Annual Sediments)        | https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-biogeochem-baseline-annual-sed        | s3://gbr-dms-data-public/aims-ereefs-biogeochem-baseline-annual-sed/data.zarr        | https://pygeoapi.reefdata.io/collections/aims-ereefs-biogeochem-baseline-annual-sed        | Public         |
| AIMS - eReefs Aggregations of Biogeochemistry Model Outputs (Baseline Annual)                  | https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-biogeochem-baseline-annual            | s3://gbr-dms-data-public/aims-ereefs-biogeochem-baseline-annual/data.zarr            | https://pygeoapi.reefdata.io/collections/aims-ereefs-biogeochem-baseline-annual            | Public         |
| AIMS - eReefs Aggregations of Biogeochemistry Model Outputs (Baseline Daily-Monthly Sediments) | https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-biogeochem-baseline-daily-monthly-sed | s3://gbr-dms-data-public/aims-ereefs-biogeochem-baseline-daily-monthly-sed/data.zarr | https://pygeoapi.reefdata.io/collections/aims-ereefs-biogeochem-baseline-daily-monthly-sed | Public         |
| AIMS - eReefs Aggregations of Biogeochemistry Model Outputs (Baseline Daily-Monthly)           | https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-biogeochem-baseline-daily-monthly     | s3://gbr-dms-data-public/aims-ereefs-biogeochem-baseline-daily-monthly/data.zarr     | https://pygeoapi.reefdata.io/collections/aims-ereefs-biogeochem-baseline-daily-monthly     | Public         |
| AIMS - eReefs Aggregations of Biogeochemistry Model Outputs (Baseline Monthly Sediments)       | https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-biogeochem-baseline-monthly-sed       | s3://gbr-dms-data-public/aims-ereefs-biogeochem-baseline-monthly-sed/data.zarr       | https://pygeoapi.reefdata.io/collections/aims-ereefs-biogeochem-baseline-monthly-sed       | Public         |
| AIMS - eReefs Aggregations of Biogeochemistry Model Outputs (Baseline Monthly)                 | https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-biogeochem-baseline-monthly           | s3://gbr-dms-data-public/aims-ereefs-biogeochem-baseline-monthly/data.zarr           | https://pygeoapi.reefdata.io/collections/aims-ereefs-biogeochem-baseline-monthly           | Public         |
| AIMS - eReefs Aggregations of Hydrodynamic Model Outputs (1km Annual)                          | https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-agg-hydrodynamic-1km-annual           | s3://gbr-dms-data-public/aims-ereefs-agg-hydrodynamic-1km-annual/data.zarr           | https://pygeoapi.reefdata.io/collections/aims-ereefs-agg-hydrodynamic-1km-annual           | Public         |
| AIMS - eReefs Aggregations of Hydrodynamic Model Outputs (1km Daily)                           | https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-agg-hydrodynamic-1km-daily            | s3://gbr-dms-data-public/aims-ereefs-agg-hydrodynamic-1km-daily/data.zarr            | https://pygeoapi.reefdata.io/collections/aims-ereefs-agg-hydrodynamic-1km-daily            | Public         |
| AIMS - eReefs Aggregations of Hydrodynamic Model Outputs (1km Monthly)                         | https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-agg-hydrodynamic-1km-monthly          | s3://gbr-dms-data-public/aims-ereefs-agg-hydrodynamic-1km-monthly/data.zarr          | https://pygeoapi.reefdata.io/collections/aims-ereefs-agg-hydrodynamic-1km-monthly          | Public         |
| AIMS - eReefs Aggregations of Hydrodynamic Model Outputs (4km Annual)                          | https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-agg-hydrodynamic-4km-annual           | s3://gbr-dms-data-public/aims-ereefs-agg-hydrodynamic-4km-annual/data.zarr           | https://pygeoapi.reefdata.io/collections/aims-ereefs-agg-hydrodynamic-4km-annual           | Public         |
| AIMS - eReefs Aggregations of Hydrodynamic Model Outputs (4km Daily)                           | https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-agg-hydrodynamic-4km-daily            | s3://gbr-dms-data-public/aims-ereefs-agg-hydrodynamic-4km-daily/data.zarr            | https://pygeoapi.reefdata.io/collections/aims-ereefs-agg-hydrodynamic-4km-daily            | Public         |
| AIMS - eReefs Aggregations of Hydrodynamic Model Outputs (4km Monthly)                         | https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-agg-hydrodynamic-4km-monthly          | s3://gbr-dms-data-public/aims-ereefs-agg-hydrodynamic-4km-monthly/data.zarr          | https://pygeoapi.reefdata.io/collections/aims-ereefs-agg-hydrodynamic-4km-monthly          | Public         |
| AIMS - LTMP Manta Tow Surveys                                                                  | https://stac.reefdata.io/browser/collections/aims-ltmp/items/aims-ltmp-manta-tows                           | s3://gbr-dms-data-limited-access/aims-ltmp-manta-tows/data.parquet                   | https://pygeoapi.reefdata.io/collections/aims-ltmp-manta-tows                              | Limited Access |
| BOM - AUSWAVE 10-day Forecast                                                                  | https://stac.reefdata.io/browser/collections/bom-auswave/items/bom-auswave-10d                              | s3://gbr-dms-data-public/bom-auswave-10d/data.zarr                                   | https://pygeoapi.reefdata.io/collections/bom-auswave-10d                                   | Public         |
| BOM - AUSWAVE 3-day Forecast                                                                   | https://stac.reefdata.io/browser/collections/bom-auswave/items/bom-auswave-3d                               | s3://gbr-dms-data-public/bom-auswave-3d/data.zarr                                    | https://pygeoapi.reefdata.io/collections/bom-auswave-3d                                    | Public         |
| GBRMPA - Benthic Map                                                                           | https://stac.reefdata.io/browser/collections/gbrmpa-admin-regions/items/gbrmpa-benthic-map                  | s3://gbr-dms-data-public/gbrmpa-benthic-map/data.zarr                                | https://pygeoapi.reefdata.io/collections/gbrmpa-benthic-map                                | Public         |
| GBRMPA - Complete GBR Features                                                                 | https://stac.reefdata.io/browser/collections/gbrmpa-admin-regions/items/gbrmpa-complete-gbr-features        | s3://gbr-dms-data-public/gbrmpa-complete-gbr-features/data.parquet                   | https://pygeoapi.reefdata.io/collections/gbrmpa-complete-gbr-features                      | Public         |
| GBRMPA - Geomorphic Map                                                                        | https://stac.reefdata.io/browser/collections/gbrmpa-admin-regions/items/gbrmpa-geomorphic-map               | s3://gbr-dms-data-public/gbrmpa-geomorphic-map/data.zarr                             | https://pygeoapi.reefdata.io/collections/gbrmpa-geomorphic-map                             | Public         |
| Geoscience Australia - High-resolution depth model for the GBR (30m)                           | https://stac.reefdata.io/browser/collections/ga-gbr-hr-depth-model/items/ga-bathymetry-gbr-30m              | s3://gbr-dms-data-public/ga-bathymetry-gbr-30m/data.zarr                             | https://pygeoapi.reefdata.io/collections/ga-bathymetry-gbr-30m                             | Public         |
| IMOS - Satellite Remote Sensing Aqua Ocean Colour Chlorophyll-a (4km)                          | https://stac.reefdata.io/browser/collections/imos-satellite-remote-sensing/items/imos-srs-aqua-oc-chla-4km  | s3://gbr-dms-data-public/imos-srs-aqua-oc-chla-4km/data.zarr                         | https://pygeoapi.reefdata.io/collections/imos-srs-aqua-oc-chla-4km                         | Public         |


>>>>>>> b67ddaca374a14be3920650da8c43730a2b6559e:CoTS_training_Jan2024/README.md
