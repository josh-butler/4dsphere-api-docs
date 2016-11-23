---
title: API Reference

language_tabs:
  - shell
  - python

toc_footers:
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the 4D Sphere API! You can use our API to access aviation API endpoints, which can get information on airports, waypoints and navigation aids in our database.

We have provided example API requests in Shell and Python languages. You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.


# Authentication

Authentication is not required to issue GET requests. All POST, PUT and PATCH requests require authentication and are currently restricted to 4D Sphere developers only.


# Airports

## Get All Airport IDs

```python
import requests

request = requests.get('https://api.4dsphere.com/airport-list')
```

```shell
curl "https://api.4dsphere.com/airport-list"
```

> The above command returns JSON structured like this:

```json
{
    "airport_list": [
        "AH_0000001",
        "AH_0000002",
        "AH_0000003", 
        ..,
     ]
}
```

This endpoint retrieves all airport IDs in the database.

### HTTP Request

`GET https://api.4dsphere.com/airport-list`

<aside class="warning">
This endpoint is rate limited. A large number of requests over a short time span will return a 429 response "Too Many requests"
</aside>

## Get an Individual Airport in JSON format

```python
import requests

request = requests.get('https://api.4dsphere.com/nav-json/AH_0000001')
```

```shell
curl "https://api.4dsphere.com/nav-json/AH_0000001"
```

> The above command returns JSON structured like this:

```json
{
  "ARP": {
    "ElevatedPoint": {
      "pos": "-176.645919 51.878025",
      "@gml:id": "AIRPORT_HELIPORT_ELEVATED_POINT_ARP_0000001",
      "@srsName": "urn:ogc:def:crs:OGC:1.3:CRS83"
    }
  },
  "fieldElevation": {
    "#text": "19.5",
    "@uom": "FT"
  },
  "locationIndicatorICAO": "PADK",
  "City": {
    "name": "ADAK ISLAND",
    "@gml:id": "AIRPORT_HELIPORT_CITY_PROPERTY_0000001"
  }
  ...
  ...
  ...
}
```

This endpoint retrieves a specific airport, returning data in JSON format.


### HTTP Request

`GET https://api.4dsphere.com/nav-json/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the airport to retrieve



## Get an Individual Airport in AIXM 5.1 format

```python
import requests

request = requests.get('https://api.4dsphere.com/nav-aixm/AH_0000001')
```

```shell
curl "https://api.4dsphere.com/nav-aixm/AH_0000001"
```

> The above command returns AIXM structured like this:

```xml
<aixm:AirportHeliport 
  xmlns:aixm="http://www.aixm.aero/schema/5.1" 
  xmlns:gml="http://www.opengis.net/gml/3.2" 
  xmlns:apt="http://www.faa.gov/aixm5.1/apt" 
  xmlns:xlink="http://www.w3.org/1999/xlink" gml:id="AH_0000001">
  <aixm:timeSlice>
    <aixm:AirportHeliportTimeSlice gml:id="AIRPORT_HELIPORT_TS_0000001">
      <gml:validTime>
        <gml:TimePeriod gml:id="AIRPORT_HELIPORT_TIME_PERIOD_0000001">
          <gml:beginPosition>2016-02-04T00:00:00.000-05:00</gml:beginPosition>
          <gml:endPosition indeterminatePosition="unknown"/>
        </gml:TimePeriod>
      </gml:validTime>
      <aixm:interpretation>BASELINE</aixm:interpretation>
  ...
  ...
  ...    
</aixm:AirportHeliport>
```

This endpoint retrieves a specific airport, returning data in AIXM format.

### HTTP Request

`GET https://api.4dsphere.com/nav-aixm/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the airport to retrieve


# GIS

## Gedodetic Inverse

```python
import requests

data = {
    "points": {
        "type": "MultiPoint",
        "coordinates": [
            [55.32432, -37.34324], [59.00342, -30.23432]
        ]
    }
}

request = r = requests.post('https://api.4dsphere.com/geo/inverse', data=data)
```

```shell
curl -H "Content-Type: application/json" -X POST -d '{"points": {"type": MultiPoint, coordinates: ...}' https://api.4dsphere.com/geo/inverse
```

> The above command returns JSON structured like this:

```json
{
  "status": 200,
  "data": {
    "distance": {
      "units": "meters",
      "value": 615329.0807114205
    },
    "azimuth": {
      "units": "degrees",
      "value": 319.21485614603466
    }
  }
}
```

This endpoint calculates the geodetic distance and azimuth between two points. CRS = WGS-84


### HTTP Request

`POST https://api.4dsphere.com/geo/inverse`

### URL Parameters

Parameter | Description
--------- | -----------
coordintates | The latitude and Longitude of the initial and target points. 


## Gedodetic Direct

```python
import requests

{
  "vector": {
  "point": {
    "type": "Point",
    "coordinates": [37, 144]
  },
  "azimuth": 300,
  "distance": 20000
}
}
request = r = requests.post('https://api.4dsphere.com/geo/direct', data=data)
```

```shell
curl -H "Content-Type: application/json" -X POST -d '{"vector": {"type": point, coordinates: ...}' https://api.4dsphere.com/geo/direct
```

> The above command returns JSON structured like this:

```json
{
  "status": 200,
  "data": {
    "type": "FeatureCollection",
    "features": [
      {
        "geometry": {
          "type": "Point",
          "coordinates": [
            36.995805286077506,
            143.77537743852463
          ]
        },
        "type": "Feature",
        "properties": {
          "CRS": "WGS84",
          "coordinateString": "36 59m 44.899s N, 143 46m 31.3588s E"
        }
      }
    ]
  }
}
```

This endpoint calculates the geodetic distance and azimuth between two points. CRS = WGS-84


### HTTP Request

`POST https://api.4dsphere.com/geo/inverse`

### URL Parameters

Parameter | Description
--------- | -----------
point | The latitude and Longitude of the initial point. 
azimuth | Angle in degrees measured from true north (0.0T).
distance | Distance in meters from the intial point.


## Runway Boundary Polygon

```python
import requests

data = {
  "runwayData": {
    "width": 200,
    "points": {
      "type": "MultiPoint",
      "coordinates": [
         [26.526485, -81.770019], [26.54584, -81.740287]
      ]
    }
  }
}

request = r = requests.post('https://api.4dsphere.com/geo/runway-boundary', data=data)
```

```shell
curl -H "Content-Type: application/json" -X POST -d '{"runwayData": {"width": 200, points: ...}' https://api.4dsphere.com/geo/runway-boundary
```

> The above command returns JSON object structured like this:

```json
{
  "status": 200,
  "data": {
    "type": "FeatureCollection",
    "features": [
      {
        "geometry": {
          "type": "Polygon",
          "coordinates": [
            [
              [
                26.525753898923156,
                -81.7694306574336
              ],
              [
                26.527216098574705,
                -81.77060735002091
              ],
              [
                26.546571096588522,
                -81.7408754487494
              ],
              [
                26.54510890090725,
                -81.73969855871265
              ],
              [
                26.525753898923156,
                -81.7694306574336
              ]
            ]
          ]
        },
        "type": "Feature",
        "properties": {
          "CRS": "WGS84",
          "coordinateStrings": [
            [
              "26 31m 32.714s N",
              " 81 46m 9.95037s W"
            ],
            [
              "26 31m 37.978s N",
              " 81 46m 14.1865s W"
            ],
            [
              "26 32m 47.6559s N",
              " 81 44m 27.1516s W"
            ],
            [
              "26 32m 42.392s N",
              " 81 44m 22.9148s W"
            ],
            [
              "26 31m 32.714s N",
              " 81 46m 9.95037s W"
            ]
          ]
        }
      }
    ]
  }
}
```

This endpoint returns a GeoJSON polygon that represents and runway boundary.

### HTTP Request

`POST https://api.4dsphere.com/geo/runway-boundary`

### URL Parameters

Parameter | Description
--------- | -----------
width | Runway width in feet. 
points | WGS-84 GeoJSON Multipoint object containing the runway start and end point.


<aside class="warning">
This endpoint is rate limited. A large number of requests over a short time span will return a 429 response "Too Many requests"
</aside>


# Search

## Get search results

```python
import requests

request = requests.get('https://api.4dsphere.com/nav-query/KDFW')
```

```shell
curl "https://api.4dsphere.com/nav-query/KDFW"
```

> The above command returns JSON structured like this:

```json
  [
    { "AirportHeliportExtension": {
        "stateName": "TEXAS"}, "City": {"name": "DALLAS-FORT WORTH"},
        "name": "DALLAS/FORT WORTH INTL",
        "type": "AH",
        "designator": "DFW",
        "locationIndicatorICAO": "KDFW",
        "root_gml_id": "AH_0016003"
    },
    ..,
    ..,
    ..,
  ]
```

This endpoint retrieves airport whose attributes (ICAO, IATA, Name or City) match the provided query string.

<aside class="notice">A minimum of search 3 characters must be provided in the request.</aside>

### HTTP Request

`GET https://api.4dsphere.com/nav-query/<PARAM>`

### URL Parameters

Parameter | Description
--------- | -----------
PARAM | The search query string. 
