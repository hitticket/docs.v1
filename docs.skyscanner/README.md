# Hit Turizm API — Skyscanner Flight Integration

This document describes the flight availability endpoints required for Skyscanner integration. It includes only the Flight Search and Active Destination List services.

All requests and responses use JSON format.

---

## 1. Flight Search

**Endpoint**

```http
POST https://api.hit-turizm.com/api/services/app/flight/search
```

**Description**

Searches for available flights between an origin and a destination. The service supports one-way and return-trip searches, passenger counts by type, and pricing in the requested currency.

**Headers**

```http
Content-Type: application/json
Accept: application/json
```

### Request JSON Model

```json
{
  "origin": "string",
  "destination": "string",
  "departureDate": "string",
  "arrivalDate": "string | null",
  "adultCount": "integer",
  "childCount": "integer",
  "infantCount": "integer",
  "currencyId": "string",
  "refId": "integer"
}
```

### Request Parameters

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| origin | string | Yes | Origin airport or city IATA code. |
| destination | string | Yes | Destination airport or city IATA code. |
| departureDate | string | Yes | Departure date in `YYYY-MM-DD` format. |
| arrivalDate | string / null | No | Return date in `YYYY-MM-DD` format. Use `null` or an empty value for one-way searches. |
| adultCount | integer | Yes | Number of adult passengers. |
| childCount | integer | Yes | Number of child passengers. Use zero when there are no child passengers. |
| infantCount | integer | Yes | Number of infant passengers. Use zero when there are no infant passengers. |
| currencyId | string | Yes | Requested pricing currency. Supported values are `USD`, `EUR`, and `RUB`. |
| refId | integer | Yes | Partner or agency reference identifier used for tracking context. |

### Response JSON Model

```json
{
  "result": {
    "fsid": "string",
    "_request": {
      "departureDate": "string",
      "arrivalDate": "string | null",
      "fromAirportId": "string",
      "toAirportId": "string",
      "currencyId": "string",
      "adultCount": "integer",
      "childCount": "integer",
      "infantCount": "integer"
    },
    "flights": {
      "groupIndex": [
        {
          "id": "string",
          "origin": "string",
          "destination": "string",
          "departureDate": "string",
          "arrivalDate": "string",
          "flightDuration": "string | null",
          "departureHour": "string",
          "arrivalHour": "string",
          "flightHours": "string",
          "airlineName": "string",
          "flightNo": "string",
          "iataCode": "string",
          "icaoCode": "string",
          "airCraftFullName": "string | null",
          "price": "decimal",
          "currencyId": "string",
          "route": "string",
          "basePath": "string",
          "baggageWeight": "string",
          "baggageInfo": "string",
          "isCharter": "boolean",
          "handBaggage": "string"
        }
      ]
    },
    "errorMessage": "string | null",
    "market": "string | null",
    "isSuccess": "boolean"
  },
  "targetUrl": "string | null",
  "success": "boolean",
  "error": "object | null",
  "unAuthorizedRequest": "boolean",
  "__abp": "boolean"
}
```

### Response Wrapper

| Field | Type | Description |
| --- | --- | --- |
| result | object | Main flight search response payload. |
| targetUrl | string / null | Optional redirect target returned by the platform. |
| success | boolean | Indicates whether the API request was processed successfully at wrapper level. |
| error | object / null | Error details if the wrapper-level request fails. |
| unAuthorizedRequest | boolean | Indicates whether the request was unauthorized. |
| __abp | boolean | Framework-level response marker. |

### Result Object

| Field | Type | Description |
| --- | --- | --- |
| fsid | string | Unique flight search identifier. This value should be kept and used to associate selected flight options with the original search. |
| _request | object | Normalized request information processed by the service. |
| _request.departureDate | string | Processed departure date in ISO 8601 date-time format. |
| _request.arrivalDate | string / null | Processed return date in ISO 8601 date-time format, or `null` for one-way searches. |
| _request.fromAirportId | string | Processed origin IATA code. |
| _request.toAirportId | string | Processed destination IATA code. |
| _request.currencyId | string | Currency used for pricing in the response. |
| _request.adultCount | integer | Number of adult passengers used in the search. |
| _request.childCount | integer | Number of child passengers used in the search. |
| _request.infantCount | integer | Number of infant passengers used in the search. |
| flights | object | Collection of available flight option groups. Each property contains an array of flight options. |
| errorMessage | string / null | Search-level error message. `null` means no search-level error message was returned. |
| market | string / null | Optional market or distribution information returned by the service. |
| isSuccess | boolean | Search-level status flag returned by the flight service. |

### Flight Option Object

Each item inside `result.flights` represents an available flight option.

| Field | Type | Description |
| --- | --- | --- |
| id | string | Unique flight option identifier. This value is required when the selected flight is used in basket or booking flows. |
| origin | string | Human-readable origin airport information, including city, country, and airport code. |
| destination | string | Human-readable destination airport information, including city, country, and airport code. |
| departureDate | string | Scheduled departure date and time in ISO 8601 format. |
| arrivalDate | string | Scheduled arrival date and time in ISO 8601 format. |
| flightDuration | string / null | Flight duration if provided by the supplier. |
| departureHour | string | Local departure time. |
| arrivalHour | string | Local arrival time. |
| flightHours | string | Combined departure and arrival time range. |
| airlineName | string | Operating or marketing airline name. |
| flightNo | string | Flight number. |
| iataCode | string | Airline IATA code. |
| icaoCode | string | Airline ICAO code. |
| airCraftFullName | string / null | Aircraft name or type if provided. |
| price | decimal | Flight price in the requested currency. |
| currencyId | string | Currency code used for the price. |
| route | string | Route code built from origin and destination IATA codes. |
| basePath | string | Internal path that identifies the flight option in the Hit Turizm flight flow. |
| baggageWeight | string | Checked baggage allowance value. |
| baggageInfo | string | Unit or description for the checked baggage allowance. |
| isCharter | boolean | Indicates whether the flight is a charter flight. |
| handBaggage | string | Hand baggage allowance value, if available. |

---

## 2. Active Destination List

**Endpoint**

```http
GET https://api.hit-turizm.com/api/services/app/flight/GetRoutes
```

**Description**

Retrieves active origin and destination route pairs that are currently available for flight search. Partners should use this endpoint to identify searchable routes before calling the Flight Search endpoint.

**Headers**

```http
Accept: application/json
```

### Request Parameters

This endpoint does not require a request body or query parameters.

### Response JSON Model

```json
{
  "routes": [
    {
      "id": "integer",
      "orig": "string",
      "dest": "string",
      "dates": ["string"]
    }
  ]
}
```

### Response Structure

| Field | Type | Description |
| --- | --- | --- |
| routes | array | List of active route pairs available for search. |
| routes[n].id | integer | Route identifier returned by the service. |
| routes[n].orig | string | Origin airport or city IATA code. |
| routes[n].dest | string | Destination airport or city IATA code. |
| routes[n].dates | array of strings | Available departure dates for the route. Dates are returned in `DD.MM.YYYY` format. |

### Usage Notes

Only route pairs returned by the Active Destination List endpoint should be used as valid `origin` and `destination` combinations in Flight Search. If the `dates` array is populated for a route, partners should use those dates to guide and validate departure-date selection.
