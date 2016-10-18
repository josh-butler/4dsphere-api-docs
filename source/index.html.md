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

request = requests.get('http://api.4dsphere.com/airport-list')
```

```shell
curl "http://api.4dsphere.com/airport-list"
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

`GET http://api.4dsphere.com/airport-list`

<aside class="warning">
This endpoint is rate limited. A large number of requests over a short time span will return a 429 response "Too Many requests"
</aside>

## Get an Individual Airport in JSON format

```python
import requests

request = requests.get('http://api.4dsphere.com/nav-json/AH_0000001')
```

```shell
curl "http://api.4dsphere.com/nav-json/AH_0000001"
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

`GET http://api.4dsphere.com/nav-json/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the airport to retrieve



## Get an Individual Airport in AIXM 5.1 format

```python
import requests

request = requests.get('http://api.4dsphere.com/nav-aixm/AH_0000001')
```

```shell
curl "http://api.4dsphere.com/nav-aixm/AH_0000001"
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

`GET http://api.4dsphere.com/nav-aixm/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the airport to retrieve


# Search

## Get search results

```python
import requests

request = requests.get('http://api.4dsphere.com/nav-query/KDFW')
```

```shell
curl "http://api.4dsphere.com/nav-query/KDFW"
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

`GET http://api.4dsphere.com/nav-query/<PARAM>`

### URL Parameters

Parameter | Description
--------- | -----------
PARAM | The search query string. 
