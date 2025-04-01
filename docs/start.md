# Getting Started
-----------------------------
##### Base Endpoint URLs:
- DEV: `https://scoring-dev.nbcsports.io`
- PROD: `https://scoring.nbcsports.io`

#### Leagues:
Below is a list of sports and league abbreviations that are supported by the API.

- Football
    - NFL (National Football League)
    - CFB (College Football - FBS Only)
- Basketball
    - NBA (National Basketball Association)
    - CBK (Men's College Basketball - Division 1 Only)
    - WCBK (Women's College Baskettball - Division 1 Only)
- Baseball
    - MLB (Major League Baseball)
- Hockey
    - NHL (National Hockey League)
- Soccer
    - EPL (Premier League)
- Motor
    - NASCAR (National Association for Stock Car Auto Racing)
    - Truck (NASCAR Craftsman Truck Series)
    - Xfinity (NASCAR Xfinity Series)
- Golf
    - PGA (Professional Golfers' Association of America)
    - LPGA (Ladies Professional Golf Association)
- Horse Racing
    - HRAC (Horse Racing)
- Tennis
    - ATP (Association of Tennis Professionals)
    - WTA (Womens Tennis Association)
- Cycling
    - TDF (Tour de France)

# Response Wrapper
----------------------
Each response has a wrapper around the core data of the endpoint. This wrapper is useful for pagination and iterating through large datasets.
Below is a sample response wrapper:

```
{
    "href": "http://scoring-dev.nbcsports.io/v1/Stats/sports?Offset=0&Limit=100",
    "ref": [
        "collection"
    ],
    "method": "GET",
    "offset": 0,
    "limit": 100,
    "size": 20,
    "first": {
        "href": "http://scoring-dev.nbcsports.io/v1/Stats/sports?offset=0&limit=100",
        "ref": [
            "collection"
        ],
        "method": "GET"
    },
    "values": [],
    "self": {
        "href": "http://scoring-dev.nbcsports.io/v1/Stats/sports?Offset=0&Limit=100",
        "ref": [
            "collection"
        ],
        "method": "GET"
    }
}
```

# Last Update
------------------------------
Most endpoints are equipped with the ability to return data based on a timestamp supplied via the query parameters of the request. Passing in this timestamp will return data that has been modified or created ***after*** the supplied time. 

Timestamps should be in milliseconds.

The API offers two different unversioned endpoints to collect last update timestamps for each league and data type combination. Regular polling of these endpoints reduces the need to regularly poll the wider set of endpoints. This allows clients to call specific endpoints, only when updates have actually been made to data that the client has not already ingested.

Each endpoint will be appropriately marked as last update enabled in this documentation.

# Pagination
------------------------------
Most endpoints are equipped with the ability to paginate response data. The *`offset`* and *`limit`* query parameters are used in requests to control the amount of data returned.

- *`Offset`*: Specifies the number of records to skip before starting to return rows. For example, an offset of 10 will skip the first 10 records.
- *`Limit`*: Specifies the maximum number of records to return. For instance, a limit of 5 will return at most 5 records.

Together, they help in retrieving a specific subset of data. For example, if you have a total of 100 records and want to fetch records 11 to 20, you would use an offset of 10 and a limit of 10.
This is useful in breaking up large, expensive requests, into smaller, more efficient requests and subsequent responses.

Each endpoint will be appropriately marked as pagination enabled in this documentation.

# Last Update Endpoints
--------------------
#### ***Get Last Updates***
This endpoint will return last update timestamps for each league / endpoint type. It is highly recommended to use some sort of combination of optional query parameters to maximize the efficiency of this endpoint. If no query parameters are present, then a response will be generated that contains last updates for every possible league / endpoint combination. This is not efficient!

- Path: ***`/stats/lastupdate`***
- Request Type: `GET`
- Pagination Enabled
- Required Query Parameters:
    - None, not recommended.
- Optional Query Parameters:
    - `league`
        - Corresponds to any of the league abbreviations listed in the [Getting Started](#getting-started) section.
    - `seasonId` OR `seasonYear`
        - Corresponds to either the ID of a given season or the calendar year at the start of a season (i.e. 2024).
    - `dataType`
        - Corresponds to the type of data returned by a given endpoint (i.e. Venue).

### ***Get Static Table Last Updates***
This endpoint will return last update timestamps for each league / static table endpoint type. It is highly recommended to use some sort of combination of optional query parameters to maximize the efficiency of this endpoint. If no query parameters are present, then a response will be generated that contains last updates for every possible league / static table endpoint combination. This is not efficient!

- Path: ***`/stats/tables/lastupdate`***
- Request Type: `GET`
- Pagination Enabled
- Required Query Parameters:
    - None, not recommended.
- Optional Query Parameters:
    - `league`
        - Corresponds to any of the league abbreviations listed in the [Getting Started](#getting-started) section.
    - `seasonId` OR `seasonYear`
        - Corresponds to either the ID of a given season or the calendar year at the start of a season (i.e. 2024).
    - `tableType`
        - Enum value (ID or string name accepted) that corresponds to the desired table type. These can be chained together in the query parameters to retrieve multiple desired table types for a given event. If the parameter is not used, all table types will be returned. These table type values can be found using the dictionary endpoint.