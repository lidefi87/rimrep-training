# CoTS Use Case Training Workshop
### 23 January 2024

## Goal

To use the GBR Data Management System (DMS) to extract and analyse data (point or time series data) from selected datasets provided by the DMS

## How to use DMS services and data

The GBR DMS provides a **metadata catalogue**, a **public AWS S3 repository of datasets** and a **data API**. The DMS also manages non-public datasets that could be accessed after authorisation.

<details>
<summary><b>The STAC metadata catalogue</b></summary>

The metadata catalogue is the discovery portal. The datasets are organised in items inside collections. A collection is a group of similar items (datasets) maintained by the same data provider or that referes to a similar type of data. For example, GBRMPA maintains a set of administrative regions (like GBR marine park limit) or natural features (GBR features); all theses datasets (items) are under the same collection (GBRMPA Administrative Regions).

At the collection level, you can search for datasets based on names or keywords. The search will return the collections that contain items related to your query. Selecting one collection you can search again by temporal/spatial exten and names.

</details>

<details>
<summary><b>The AWS S3 public repository</b></summary>

</details>

<details>
<summary><b>The data API</b><summary>

</details>

## Case Study examples

We will be working on the following use case examples: 

1. Extract eReef model time series data for a selected point (coordinates) or reef (reef name), using hydrological and biochemical products.
2. Extract and calculate average values for eReef model variables for an area defined by a shape file withing an specified time frame
3. Extract a time series of values from LTMP/MMP modelled output for a particular reef or a list of reefs

## Notebooks



## Datasets to be used

The files required for this workshop are: 

| File name | Description | Format | S3 URI                                                       |
| --- | --- | --- |--------------------------------------------------------------|
| GBR features | Great Barrier Reef (GBR) features | geoParquet | s3://rimrep-data-public/gbrmpa-complete-gbr-features/data.parquet/                                                             |


