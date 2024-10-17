
# Supercharge.info API - Data Access

## Metadata

- **Document Version**: 1.0
- **Last Updated**: October 2024
- **Associated Software Release**: Supercharge.info API v2.0
- **Document Author**: The Supercharge.info Team
- **Contact Information**: support@supercharge.info

## Changelog

- **v1.0** (October 2024): Initial version, including details for `/allSites`, `/changesByDate`, `allSites` dataset, `allChanges` dataset, and example queries.

## Introduction
This document provides a comprehensive guide for retrieving data using the *supercharge.info* API. It focuses on read-only endpoints designed for research and analysis purposes.

## Table of Contents
1. [Endpoints Overview](#endpoints-overview)
2. [Endpoints](#endpoints)
3. [Datasets](#datasets)
4. [Example Queries](#example-queries)

## Endpoints Overview
The following endpoints provide read-only access to data about supercharging stations and network changes:

- `/allSites`: Retrieve detailed information about all supercharging stations.
- `/changesByDate`: Get aggregated data about changes by date.

## Endpoints

### Endpoint: `/allSites`

**Description**: Retrieve a list of all supercharging sites with detailed metadata.

**Method**: `GET`

**Authentication Required**: `NO`

**Parameters**:
| Parameter   | Type   | Required | Description                                               |
|-------------|--------|----------|-----------------------------------------------------------|
| `status`    | String | NO       | Filter by site status (e.g., `OPEN`, `CLOSED`).            |
| `regionId`  | Integer| NO       | Filter by region.                                          |

**Response**:
```json
{
  "id": 12345,
  "name": "Supercharger - Los Angeles",
  "locationId": "los-angeles-ca-123",
  "status": "OPEN",
  "address": {
    "street": "123 Tesla Rd",
    "city": "Los Angeles",
    "state": "CA",
    "zip": "90001",
    "country": "USA"
  },
  "gps": {
    "latitude": 34.0522,
    "longitude": -118.2437
  },
  "stallCount": 12,
  "powerKilowatt": 150
}
```

### Endpoint: `/changesByDate`

**Description**: Retrieve the number of supercharging stations updated on each date, broken down by their status.

**Method**: `GET`

**Authentication Required**: `NO`

**Parameters**: None.

**Response**:
```json
{
  "Date": "2024-10-14",
  "OPEN": 10,
  "CLOSED": 2,
  "CONSTRUCTION": 4
}
```

## Datasets

### Dataset: `changesByDate`

This dataset tracks the number of supercharging stations updated on a given date, broken down by their status.

| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `Date`      | Date      | The date when the change was logged. |
| `{STATUS}`  | Integer   | Number of stations that were updated with the given status. `{STATUS}` can be one of the following: `CLOSED_PERM`, `CLOSED_TEMP`, `VOTING`, `PLAN`, `PERMIT`, `CONSTRUCTION`, `EXPANDING`, `OPEN`. |

---

### Dataset: `allSites`

This dataset provides detailed information about each supercharging station, including location, operational status, and station capabilities.

| Column Name      | Data Type  | Description                                                |
|------------------|------------|------------------------------------------------------------|
| `id`             | Integer    | Unique identifier for each supercharging station entry.     |
| `locationId`     | String     | Unique location identifier for the supercharging station.   |
| `name`           | String     | Name or description of the supercharging station.           |
| `status`         | String     | Current operational status of the station (`OPEN`, `CLOSED`).|
| `address`        | Object     | Nested object containing detailed address information.      |
| - `street`       | String     | Street address of the supercharging station.                |
| - `city`         | String     | City where the supercharging station is located.            |
| - `state`        | String     | State or province where the supercharging station is located.|
| - `zip`          | String     | Postal code for the supercharging station location.         |
| - `countryId`    | Integer    | Numeric identifier for the country where the station is located.|
| - `country`      | String     | Country where the supercharging station is located.         |
| - `regionId`     | Integer    | Numeric identifier for the region where the station is located.|
| - `region`       | String     | Region where the supercharging station is located (e.g., North America, Europe). |
| `gps`            | Object     | Nested object containing GPS coordinates.                   |
| - `latitude`     | Float      | Latitude of the supercharging station.                      |
| - `longitude`    | Float      | Longitude of the supercharging station.                     |
| `dateOpened`     | Date       | Date (if any) when the supercharging station was opened.     |
| `hours`          | String     | Charging availability hours. Blank means the site is available 24/7. |
| `stallCount`     | Integer    | Number of charging stalls available at the station.         |
| `counted`        | Boolean    | Indicates whether the station’s stall count is included in Tesla’s official network count. |
| `elevationMeters`| Integer    | Elevation of the station in meters above sea level.         |
| `powerKilowatt`  | Integer    | Maximum power output of the chargers at the station, in kW. |
| `solarCanopy`    | Boolean    | Indicates whether the station is equipped with a solar canopy.|
| `battery`        | Boolean    | Indicates if there is battery storage available at the station.|
| `otherEVs`       | Boolean    | Indicates if chargers are compatible with electric vehicles other than Tesla.|
| `stalls`         | Object     | Nested object containing details about the types of charging stalls available. |
| - `urban`        | Integer    | Number of urban Tesla supercharger stalls.                  |
| - `v2`           | Integer    | Number of version 2 Tesla supercharger stalls.              |
| - `v3`           | Integer    | Number of version 3 Tesla supercharger stalls.              |
| - `v4`           | Integer    | Number of version 4 Tesla supercharger stalls.              |
| - `accessible`   | Integer    | Number of accessible stalls designated for disabled access. |
| `plugs`          | Object     | Nested object containing details about the types of charging plugs available. |
| - `tpc`          | Integer    | Number of Tesla proprietary connector plugs.                |
| - `ccs1`         | Integer    | Number of Combined Charging System version 1 plugs.         |
| - `ccs2`         | Integer    | Number of Combined Charging System version 2 plugs.         |
| - `type2`        | Integer    | Number of Type 2 connector plugs (common in Europe).        |

---

### Dataset: `allChanges`

This dataset logs every change made to supercharging stations, providing detailed records of the modification history.

| Column Name    | Data Type | Description                                      |
|----------------|-----------|--------------------------------------------------|
| `id`           | Integer   | Unique identifier for the change record.         |
| `siteId`       | Integer   | Identifier linking to a specific supercharging site. |
| `date`         | Date      | The date when the change was recorded.           |
| `changeType`   | String    | Type of change made (e.g., `ADD`, `UPDATE`).     |
| `siteStatus`   | String    | Current status of the site after the change.     |
| `prevStatus`   | String    | Previous status of the site before the change.   |
| `stallCount`   | Integer   | Number of charging stalls after the change.      |
| `prevCount`    | Integer   | Number of charging stalls before the change, if known. |
| `powerKilowatt`| Integer   | Power capacity of the site in kilowatts.         |
| `notify`       | Boolean   | Indicates whether notifications for the change were sent out. |

---

## Example Queries

### Example: Retrieve All Open Sites

**Request**:
```
GET /allSites?status=OPEN
```

**Response**:
```json
[
  {
    "id": 12345,
    "name": "Supercharger - Los Angeles",
    "status": "OPEN",
    "stallCount": 12,
    "gps": {
      "latitude": 34.0522,
      "longitude": -118.2437
    }
  }
]
```
