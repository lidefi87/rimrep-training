<<<<<<< HEAD
# Crown of Thorns Starfish (CoTS) Use Case Training Workshop

### 23 January 2024

## Goal

To use the [GBR Data Management System (DMS)](https://stac.reefdata.io/browser/?.language=en) to extract and analyse data (point or time series data) from selected datasets hosted in the DMS. Note that you will need to have a GBR DMS account to access data.

## How to use DMS services and data

The GBR DMS provides three ways to search for public data: a **metadata catalogue**, a **public AWS S3 repository of datasets**, and a **data API**. Click on the titles below to find more information about each of these methods.

Also note that while the majority of datasets in the DMS are publicly available, the DMS also manages non-public datasets that could be accessed after authorisation from the original data provider. This is the reason why DMS users need an account to access this system.

<details>

<summary><b>The STAC metadata catalogue</b></summary>

The metadata catalogue is the discovery portal. The datasets are organised as *items* inside **collections**. A **collection** is a group of similar *items* (datasets) either maintained by the same data provider (e.g., GBRMPA), or it can also refer to a similar type of data. For example, GBRMPA maintains a set of administrative regions (e.g., GBR marine protected area boundaries) and another for natural features (e.g., reefs inside the boundaries of the GBR). Both of these datasets (*items*) are included under the same **collection** ([GBRMPA Administrative Spatial Regions](https://stac.reefdata.io/browser/collections/gbrmpa-admin-regions)).

In the DMS, you can search for datasets by their name or using keywords. This search will return any collections that contain items related to your query. You can further search filter results by selecting one of the collection and searching by temporal/spatial extent and names. This will return a set of items (within the chosen collection) that fits your query.

</details>

<details>

<summary><b>The AWS S3 public repository</b></summary>

All DMS datasets are stored in a Amazon AWS S3 public repository. If you know the location of the collection (the S3 URI) you can use this address to connect directly with the dataset. The S3 URI is provided in the [STAC](<https://stac.reefdata.io/browser/>) Item page for every collection.

![](images/clipboard-3883780758.png)

</details>

<details>

<summary><b>The data API</b></summary>

The data API services are provided through [pygeoapi](https://pygeoapi.io) a server implementation of a set of [OGC API standards](https://ogcapi.ogc.org). This service allows you to extract data from every collection using simple filters like time and space. Note that given that not all datasets in the DMS are publicly available, DMS users will need to an **Access Token** to access the DMS API services.

If you need access to the API services, contact the DMS team by emailing [info-dms\@utas.edu.au](mailto:info-dms@utas.edu.au). We will create a unique set of `CLIENT_ID` and `CLIENT_SECRET` that you can use to create an **Access Token** following the instructions in the next section of this document. Note that the `CLIENT_ID` and `CLIENT_SECRET` should be considered private information, just like a password. This is because these identifiers are linked to a user profile that defines the private datasets you are granted permission to access.

### Using the data API: How to get an access token

You can generate an access token using command line commands or inside your code. Note that the access token is only valid for one hour, so it is possible that you need to request a new token for each new API call.

For human users, you need to login into the DMS system accessing the [login dashboard](https://dashboard.reefdata.io). Then, go to the same page again and copy the access token. You are ready to paste the token value into a variable, preferably, an environment variable.

![](images/clipboard-2491887232.png)

If you are planning to use a Machine-to-Machine workflow, there are several ways you can request an access token. The DMS admin needs to create a "machine client" for you and provide the `client_id` and `client_secret` for this non-human user. Please do not share the provided secrets and store them in a secure way.

<details>

<summary><b>R</b></summary>

Included in this folder, you will find a file called `useful_functions.R`. This file includes a collection of functions that you may find useful when accessing data in the DMS. One of the functions we are including is `dms_token`, which requests an access token using your DMS user credentials.

In the chunk below, we assume that you have stored your user credentials as environment variables in `R`. If you have not, we have instructions on how to set environment variables under the [Notebooks](#notebooks) section further down in this document.

``` r
#Loading DMS custom made functions
source("R_notebooks/useful_functions.R")

#Get access token
token <- dms_token(Sys.getenv("CLIENT_ID"), Sys.getenv("CLIENT_SECRET"))
```

In the code above, the access token will be stored in `token` variable.

</details>

<details>

<summary><b>Command Line Interface</b></summary>

It is recommended to store the `CLIENT_ID` and `CLIENT_SECRET` as environment variables. Assuming that you have already defined these environment variables, you can request an access token using the following command:

``` bash
ACCESS_TOKEN=$(curl --location --request POST "https://keycloak.reefdata.io/realms/rimrep-production/protocol/openid-connect/token" -s \
  --header "Content-Type: application/x-www-form-urlencoded" \
  --data-urlencode "client_id=$CLIENT_ID" \
  --data-urlencode "client_secret=$CLIENT_SECRET" \
  --data-urlencode "grant_type=client_credentials" | jq -r '.["access_token"]')
```

</details>

<details>

<summary><b>Python</b></summary>

Assuming that `CLIENT_ID` and `CLIENT_SECRET` are already defined as environment variables, you can use the code below to get the access token:

``` python
import requests
import os

client_id = os.environ["CLIENT_ID"]
client_secret = os.environ["CLIENT_SECRET"]

# Get the access token
url = "https://keycloak.reefdata.io/realms/rimrep-production/protocol/openid-connect/token"
headers = {"Content-Type": "application/x-www-form-urlencoded"}
data = {
    "client_id": client_id,
    "client_secret": client_secret,
    "grant_type": "client_credentials",
}
response = requests.post(url, headers=headers, data=data)

assert response.status_code == 200, response.text

access_token = response.json().get("access_token")
```

</details>

</details>

## Use case examples

We will be working on the following use case examples:

1.  Extract eReef model time series data for a selected point (coordinates) or reef (reef name), using hydrological and biochemical products.\
2.  Extract and calculate average values for eReef model variables for an area defined by a shapefile within a specified time frame.\
3.  Extract a time series from LTMP/MMP model output for a particular reef or list of reefs.

## Notebooks {#notebooks}

Example notebooks for this workshop were developed in `R` because it is the most widely used programming language within the CoTS team. However, the DMS can also be accessed using `Python`, you can see some examples in [this repository](https://github.com/aodn/rimrep-examples/tree/main/Python_based_scripts). Before running these `R` notebooks, make sure you have installed all libraries used in this workshop.

To keep the DMS secure and to allow for different access levels to non-public datasets via the API, we provide DMS users with unique credentials. To access the API, users must generate an access token using their credentials. These token last one hour and while current, users are able to access any public datasets in the DMS, as well as any non-public dataset for which they have been granted permission. User credentials should be treated similar to passwords and they should not be shared.

To ensure you do not accidentally share your user credentials within a script, we recommend that you store this information as environment variables in `R`. You can create these environment variables as follows:

``` r
#Create or update the environment variable 
Sys.setenv(CLIENT_ID = "paste_CLIENT_ID_here")
Sys.setenv(CLIENT_SECRET = "paste_CLIENT_SECRET_here")

#Check environment variable has been corrected created/updated
Sys.getenv(c("CLIENT_ID", "CLIENT_SECRET"))
```

Note that the credentials must be given within quotation marks, for example: `"client_id123"`. If you provide the token as `client_id123`, that is without quotation marks (`""`), you will get an error.

Setting environment variables in this way will only save them for the duration of your `R` session. If you would like to store them permanently, you can use `usethis::edit_r_environ()` function. This will open a file called `.Renviron` in a new tab in your RStudio session. Copy and paste your credentials as shown below.

```         
CLIENT_ID = "paste_CLIENT_ID_here"
CLIENT_SECRET = "paste_CLIENT_SECRET_here"
```

Save your changes, close the tab, and restart your `R` session for changes to take effect.

## Datasets to be used

In this workshop, we will use the following datasets:

| Dataset Title                                                                                  | STAC Metadata URL                                                                                             | s3 URI                                                                               | Pygeoapi URL                                                                                 | Security       |
|---------------|---------------|---------------|---------------|---------------|
| AIMS - eReefs Aggregations of Biogeochemistry Model Outputs (Baseline Annual Sediments)        | <https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-biogeochem-baseline-annual-sed>        | s3://gbr-dms-data-public/aims-ereefs-biogeochem-baseline-annual-sed/data.zarr        | <https://pygeoapi.reefdata.io/collections/aims-ereefs-biogeochem-baseline-annual-sed>        | Public         |
| AIMS - eReefs Aggregations of Biogeochemistry Model Outputs (Baseline Annual)                  | <https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-biogeochem-baseline-annual>            | s3://gbr-dms-data-public/aims-ereefs-biogeochem-baseline-annual/data.zarr            | <https://pygeoapi.reefdata.io/collections/aims-ereefs-biogeochem-baseline-annual>            | Public         |
| AIMS - eReefs Aggregations of Biogeochemistry Model Outputs (Baseline Daily-Monthly Sediments) | <https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-biogeochem-baseline-daily-monthly-sed> | s3://gbr-dms-data-public/aims-ereefs-biogeochem-baseline-daily-monthly-sed/data.zarr | <https://pygeoapi.reefdata.io/collections/aims-ereefs-biogeochem-baseline-daily-monthly-sed> | Public         |
| AIMS - eReefs Aggregations of Biogeochemistry Model Outputs (Baseline Daily-Monthly)           | <https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-biogeochem-baseline-daily-monthly>     | s3://gbr-dms-data-public/aims-ereefs-biogeochem-baseline-daily-monthly/data.zarr     | <https://pygeoapi.reefdata.io/collections/aims-ereefs-biogeochem-baseline-daily-monthly>     | Public         |
| AIMS - eReefs Aggregations of Biogeochemistry Model Outputs (Baseline Monthly Sediments)       | <https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-biogeochem-baseline-monthly-sed>       | s3://gbr-dms-data-public/aims-ereefs-biogeochem-baseline-monthly-sed/data.zarr       | <https://pygeoapi.reefdata.io/collections/aims-ereefs-biogeochem-baseline-monthly-sed>       | Public         |
| AIMS - eReefs Aggregations of Biogeochemistry Model Outputs (Baseline Monthly)                 | <https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-biogeochem-baseline-monthly>           | s3://gbr-dms-data-public/aims-ereefs-biogeochem-baseline-monthly/data.zarr           | <https://pygeoapi.reefdata.io/collections/aims-ereefs-biogeochem-baseline-monthly>           | Public         |
| AIMS - eReefs Aggregations of Hydrodynamic Model Outputs (1km Annual)                          | <https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-agg-hydrodynamic-1km-annual>           | s3://gbr-dms-data-public/aims-ereefs-agg-hydrodynamic-1km-annual/data.zarr           | <https://pygeoapi.reefdata.io/collections/aims-ereefs-agg-hydrodynamic-1km-annual>           | Public         |
| AIMS - eReefs Aggregations of Hydrodynamic Model Outputs (1km Daily)                           | <https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-agg-hydrodynamic-1km-daily>            | s3://gbr-dms-data-public/aims-ereefs-agg-hydrodynamic-1km-daily/data.zarr            | <https://pygeoapi.reefdata.io/collections/aims-ereefs-agg-hydrodynamic-1km-daily>            | Public         |
| AIMS - eReefs Aggregations of Hydrodynamic Model Outputs (1km Monthly)                         | <https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-agg-hydrodynamic-1km-monthly>          | s3://gbr-dms-data-public/aims-ereefs-agg-hydrodynamic-1km-monthly/data.zarr          | <https://pygeoapi.reefdata.io/collections/aims-ereefs-agg-hydrodynamic-1km-monthly>          | Public         |
| AIMS - eReefs Aggregations of Hydrodynamic Model Outputs (4km Annual)                          | <https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-agg-hydrodynamic-4km-annual>           | s3://gbr-dms-data-public/aims-ereefs-agg-hydrodynamic-4km-annual/data.zarr           | <https://pygeoapi.reefdata.io/collections/aims-ereefs-agg-hydrodynamic-4km-annual>           | Public         |
| AIMS - eReefs Aggregations of Hydrodynamic Model Outputs (4km Daily)                           | <https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-agg-hydrodynamic-4km-daily>            | s3://gbr-dms-data-public/aims-ereefs-agg-hydrodynamic-4km-daily/data.zarr            | <https://pygeoapi.reefdata.io/collections/aims-ereefs-agg-hydrodynamic-4km-daily>            | Public         |
| AIMS - eReefs Aggregations of Hydrodynamic Model Outputs (4km Monthly)                         | <https://stac.reefdata.io/browser/collections/ereefs/items/aims-ereefs-agg-hydrodynamic-4km-monthly>          | s3://gbr-dms-data-public/aims-ereefs-agg-hydrodynamic-4km-monthly/data.zarr          | <https://pygeoapi.reefdata.io/collections/aims-ereefs-agg-hydrodynamic-4km-monthly>          | Public         |
| AIMS - LTMP Manta Tow Surveys                                                                  | <https://stac.reefdata.io/browser/collections/aims-ltmp/items/aims-ltmp-manta-tows>                           | s3://gbr-dms-data-limited-access/aims-ltmp-manta-tows/data.parquet                   | <https://pygeoapi.reefdata.io/collections/aims-ltmp-manta-tows>                              | Limited Access |
| BOM - AUSWAVE 10-day Forecast                                                                  | <https://stac.reefdata.io/browser/collections/bom-auswave/items/bom-auswave-10d>                              | s3://gbr-dms-data-public/bom-auswave-10d/data.zarr                                   | <https://pygeoapi.reefdata.io/collections/bom-auswave-10d>                                   | Public         |
| BOM - AUSWAVE 3-day Forecast                                                                   | <https://stac.reefdata.io/browser/collections/bom-auswave/items/bom-auswave-3d>                               | s3://gbr-dms-data-public/bom-auswave-3d/data.zarr                                    | <https://pygeoapi.reefdata.io/collections/bom-auswave-3d>                                    | Public         |
| GBRMPA - Benthic Map                                                                           | <https://stac.reefdata.io/browser/collections/gbrmpa-admin-regions/items/gbrmpa-benthic-map>                  | s3://gbr-dms-data-public/gbrmpa-benthic-map/data.zarr                                | <https://pygeoapi.reefdata.io/collections/gbrmpa-benthic-map>                                | Public         |
| GBRMPA - Complete GBR Features                                                                 | <https://stac.reefdata.io/browser/collections/gbrmpa-admin-regions/items/gbrmpa-complete-gbr-features>        | s3://gbr-dms-data-public/gbrmpa-complete-gbr-features/data.parquet                   | <https://pygeoapi.reefdata.io/collections/gbrmpa-complete-gbr-features>                      | Public         |
| GBRMPA - Geomorphic Map                                                                        | <https://stac.reefdata.io/browser/collections/gbrmpa-admin-regions/items/gbrmpa-geomorphic-map>               | s3://gbr-dms-data-public/gbrmpa-geomorphic-map/data.zarr                             | <https://pygeoapi.reefdata.io/collections/gbrmpa-geomorphic-map>                             | Public         |
| Geoscience Australia - High-resolution depth model for the GBR (30m)                           | <https://stac.reefdata.io/browser/collections/ga-gbr-hr-depth-model/items/ga-bathymetry-gbr-30m>              | s3://gbr-dms-data-public/ga-bathymetry-gbr-30m/data.zarr                             | <https://pygeoapi.reefdata.io/collections/ga-bathymetry-gbr-30m>                             | Public         |
| IMOS - Satellite Remote Sensing Aqua Ocean Colour Chlorophyll-a (4km)                          | <https://stac.reefdata.io/browser/collections/imos-satellite-remote-sensing/items/imos-srs-aqua-oc-chla-4km>  | s3://gbr-dms-data-public/imos-srs-aqua-oc-chla-4km/data.zarr                         | <https://pygeoapi.reefdata.io/collections/imos-srs-aqua-oc-chla-4km>                         | Public         |
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

The metadata catalogue is the discovery portal. The datasets are organised as *items* inside **collections**. A **collection** is a group of similar *items* (datasets) either maintained by the same data provider (e.g., GBRMPA), or it can also refer to a similar type of data. For example, GBRMPA maintains a set of administrative regions (e.g., GBR marine protected area boundaries) and another for natural features (e.g., reefs inside the boundaries of the GBR). Both of these datasets (*items*) are included under the same **collection** ([GBRMPA Administrative Spatial Regions](https://stac.reefdata.io/browser/collections/gbrmpa-admin-regions)).  
  
In the DMS, you can search for datasets by their name or using keywords. This search will return any collections that contain items related to your query. You can further search filter results by selecting one of the collection and searching by temporal/spatial extent and names. This will return a set of items (within the chosen collection) that fits your query.  
  
</details>

<details>
<summary><b>The AWS S3 public repository</b></summary>
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aliquam tincidunt ligula eu ligula fermentum aliquet. Donec gravida urna et sapien dictum tristique. Pellentesque sed nunc ut dolor dignissim iaculis. Sed quam dui, gravida nec eros eget, tincidunt aliquet arcu. Vestibulum sollicitudin neque at sem accumsan porta. Etiam ipsum quam, vehicula quis laoreet vitae, pellentesque quis erat. Morbi tincidunt tincidunt nisl eget sagittis. Vivamus pulvinar elit in enim hendrerit, eget varius metus tincidunt. Sed leo neque, feugiat ac diam a, mollis elementum libero. Nulla vitae ex ac purus consequat blandit. In dui libero, condimentum sed commodo at, interdum vitae erat. Nullam consequat magna in fermentum semper. Quisque tortor urna, imperdiet sit amet luctus nec, iaculis at mauris.

</details>

<details>
<summary><b>The data API</b></summary>
  
The data API services are provided through [pygeoapi](https://pygeoapi.io) a server implementation of a set of [OGC API standards](https://ogcapi.ogc.org). This service allows you to extract data from every collection using simple filters like time and space. Note that given that not all datasets in the DMS are publicly available, DMS users will need to an **Access Token** to access the DMS API services.  
  
If you need access to the API services, contact the DMS team by emailing [info-dms@utas.edu.au](mailto:info-dms@utas.edu.au). We will create a unique set of `CLIENT_ID` and `CLIENT_SECRET` that you can use to create an **Access Token** following the instructions in the next section of this document. Note that the `CLIENT_ID` and `CLIENT_SECRET` should be considered private information, just like a password. This is because these identifiers are linked to a user profile that defines the private datasets you are granted permission to access.  
  
### Using the data API: How to get an access token

You can generate an access token using command line commands or inside your code. Note that the access token is only valid for one hour, so it is possible that you need to request a new token for each new API call.
  
<details>
<summary><b>R</b></summary>
  
Included in this folder, you will find a file called `useful_functions.R`. This file includes a collection of functions that you may find useful when accessing data in the DMS. One of the functions we are including is `dms_token`, which requests an access token using your DMS user credentials.
  
In the chunk below, we assume that you have stored your user credentials as environmental variables in `R`. If you have not, we have instructions on how to set environmental variables under the [Notebooks](#notebooks) section further down in this document.  
  
```R
#Loading DMS custom made functions
source("R_notebooks/useful_functions.R")

#Get access token
token <- dms_token(Sys.getenv("CLIENT_ID"), Sys.getenv("CLIENT_SECRET"))

```
  
In the code above, the access token will be stored in `token` variable.  
  
</details>
  
<details>
<summary><b>Command Line Interface</b></summary>
  
It is recommended to store the `CLIENT_ID` and `CLIENT_SECRET` as environmental variables. Assuming that you have already defined these environmental variables, you can request an access token using the following command: 

```bash
ACCESS_TOKEN=$(curl --location --request POST "https://keycloak.reefdata.io/realms/rimrep-production/protocol/openid-connect/token" -s \
  --header "Content-Type: application/x-www-form-urlencoded" \
  --data-urlencode "client_id=$CLIENT_ID" \
  --data-urlencode "client_secret=$CLIENT_SECRET" \
  --data-urlencode "grant_type=client_credentials" | jq -r '.["access_token"]')
```
</details>
  
<details>
<summary><b>Python</b></summary>
  
Assuming that `CLIENT_ID` and `CLIENT_SECRET` are already defined as environment variables, you can use the code below to get the access token: 

```python
import requests
import os

client_id = os.environ["CLIENT_ID"]
client_secret = os.environ["CLIENT_SECRET"]

# Get the access token
url = "https://keycloak.reefdata.io/realms/rimrep-production/protocol/openid-connect/token"
headers = {"Content-Type": "application/x-www-form-urlencoded"}
data = {
    "client_id": client_id,
    "client_secret": client_secret,
    "grant_type": "client_credentials",
}
response = requests.post(url, headers=headers, data=data)

assert response.status_code == 200, response.text

access_token = response.json().get("access_token")

```
</details>

</details>


## Use case examples

We will be working on the following use case examples:  

1. [Extract time series from eReef model outputs using a shapefile for a reef from the GBR features dataset](https://github.com/aodn/rimrep-training/blob/main/CoTS_training_Jan2024/R_notebooks/01_eReef_data_extraction.Rmd).  
2. [Extract and calculate average values for eReef model variables for an area defined by a bounding box within a specified time frame](https://github.com/aodn/rimrep-training/blob/main/CoTS_training_Jan2024/R_notebooks/02_eReef_maps_timeseries.Rmd).  
3. [Extract a time series from LTMP/MMP model output for a particular reef or list of reefs](https://github.com/aodn/rimrep-training/blob/main/CoTS_training_Jan2024/R_notebooks/03_LTMP_data_extraction.Rmd).  

## Notebooks

Example notebooks for this workshop were developed in `R` because it is the most widely used programming language within the CoTS team. However, the DMS can also be accessed using `Python`, you can see some examples in [this repository](https://github.com/aodn/rimrep-examples/tree/main/Python_based_scripts). Before running these `R` notebooks, make sure you have installed all libraries used in this workshop.  
  
To keep the DMS secure and to allow for different access levels to non-public datasets via the API, we provide DMS users with unique credentials. To access the API, users must generate an access token using their credentials. These token last one hour and while current, users are able to access any public datasets in the DMS, as well as any non-public dataset for which they have been granted permission. User credentials should be treated similar to passwords and they should not be shared.  
  
To ensure you do not accidentally share your user credentials within a script, we recommend that you store this information as environmental variables in `R`. You can create these environmental variables as follows:  
  
```R
#Create or update the environmental variable 
Sys.setenv("CLIENT_ID" = "paste_CLIENT_ID_here")
Sys.setenv("CLIENT_SECRET" = "paste_CLIENT_SECRET_here")

#Check environmental variable has been corrected created/updated
Sys.getenv(c("CLIENT_ID", "CLIENT_SECRET"))
```
    
Note that the credentials must be given within quotation marks, for example: `"client_id123"`. If you provide the token as `client_id123`, that is without quotation marks (`""`), you will get an error.   
  
Setting environmental variables in this way will only save them for the duration of your `R` session. If you would like to store them permanently, you can use `usethis::edit_r_environ()` function. This will open a file called `.Renviron` in a new tab in your RStudio session. Copy and paste your credentials as shown below.  
  
```
"CLIENT_ID" = "paste_CLIENT_ID_here"
"CLIENT_SECRET" = "paste_CLIENT_SECRET_here"
```
  
Save your changes, close the tab, and restart your `R` session for changes to take effect.  
  
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
| BOM - AUSWAVE 10-day Forecast                                                                  | https://stac.reefdata.io/browser/collections/bom-auswave/items/bom-auswave-10d                              | s3://gbr-dms-data-public/bom-auswave-10d/data.zarr                                   | https://pygeoapi.reefdata.io/collections/bom-auswave-10d                                   | Public         |
| BOM - AUSWAVE 3-day Forecast                                                                   | https://stac.reefdata.io/browser/collections/bom-auswave/items/bom-auswave-3d                               | s3://gbr-dms-data-public/bom-auswave-3d/data.zarr                                    | https://pygeoapi.reefdata.io/collections/bom-auswave-3d                                    | Public         |
| GBRMPA - Benthic Map                                                                           | https://stac.reefdata.io/browser/collections/gbrmpa-admin-regions/items/gbrmpa-benthic-map                  | s3://gbr-dms-data-public/gbrmpa-benthic-map/data.zarr                                | https://pygeoapi.reefdata.io/collections/gbrmpa-benthic-map                                | Public         |
| GBRMPA - Complete GBR Features                                                                 | https://stac.reefdata.io/browser/collections/gbrmpa-admin-regions/items/gbrmpa-complete-gbr-features        | s3://gbr-dms-data-public/gbrmpa-complete-gbr-features/data.parquet                   | https://pygeoapi.reefdata.io/collections/gbrmpa-complete-gbr-features                      | Public         |
| GBRMPA - Geomorphic Map                                                                        | https://stac.reefdata.io/browser/collections/gbrmpa-admin-regions/items/gbrmpa-geomorphic-map               | s3://gbr-dms-data-public/gbrmpa-geomorphic-map/data.zarr                             | https://pygeoapi.reefdata.io/collections/gbrmpa-geomorphic-map                             | Public         |
| Geoscience Australia - High-resolution depth model for the GBR (30m)                           | https://stac.reefdata.io/browser/collections/ga-gbr-hr-depth-model/items/ga-bathymetry-gbr-30m              | s3://gbr-dms-data-public/ga-bathymetry-gbr-30m/data.zarr                             | https://pygeoapi.reefdata.io/collections/ga-bathymetry-gbr-30m                             | Public         |
| IMOS - Satellite Remote Sensing Aqua Ocean Colour Chlorophyll-a (4km)                          | https://stac.reefdata.io/browser/collections/imos-satellite-remote-sensing/items/imos-srs-aqua-oc-chla-4km  | s3://gbr-dms-data-public/imos-srs-aqua-oc-chla-4km/data.zarr                         | https://pygeoapi.reefdata.io/collections/imos-srs-aqua-oc-chla-4km                         | Public         |


>>>>>>> 596b55d9312a0438f80223751a8c3e1c964f3e59
