The Hit Turizm API allows authorized partners to perform operations such as user login, flight search, basket creation, and booking management.

All API requests use **JSON** format and must include the correct headers.  
For endpoints requiring authorization, the request header must include a **Bearer Token** obtained from the login response.

---

## 1. 🔐 User Login

**Endpoint:**  
```
POST https://api/Account
```

**Description:**  
Authenticate the user and obtain an authorization token for subsequent requests.

**Headers:**
```
Content-Type: application/json
Accept: application/json
```

**Parameters:**

| Field | Type |
| --- | --- |
| tenancyName | string |
| usernameOrEmailAddress | string |
| password | string |

**Authorization:**
```
Bearer {token_value}
```

**Sample Request:**
```json
{
  "tenancyName": "default",
  "usernameOrEmailAddress": "user@example.com",
  "password": "********"
}
```

**Sample Response:**
```json
{
  "result": {
    "accessToken": "eyJhbGciOiJIUzI1NiIs...",
    "expireInSeconds": 3600,
    "userId": 1234
  },
  "success": true
}
```

---

## 2. 🧭 Provider Flight Search (NEW)

**Endpoint:**  
```
POST https://provider/flight/search
```

**Description:**  
Search for available charter or regular flights between two destinations.

**Headers:**
```
Content-Type: application/json
Accept: application/json
```

### **Request Body (JSON)**

```json
{
  "origin": "MOW",
  "destination": "AYT",
  "departureDate": "2025-10-30",
  "arrivalDate": "",
  "adultCount": 1,
  "childCount": 0,
  "infantCount": 0,
  "currencyId": "USD",
  "refId": 123
}
```

### **Request Parameters**

| Field | Type | Required | Description |
|-------|------|-----------|--------------|
| origin | string | ✅ Yes | IATA airport/city code (e.g. `MOW`) |
| destination | string | ✅ Yes | IATA airport/city code (e.g. `AYT`) |
| departureDate | string | ✅ Yes | Flight departure date (`YYYY-MM-DD`) |
| arrivalDate | string | ❌ No | Return date; leave empty for one-way |
| adultCount | integer | ✅ Yes | Number of adults |
| childCount | integer | ✅ Yes | Number of children |
| infantCount | integer | ✅ Yes | Number of infants |
| currencyId | string | ✅ Yes | ISO 4217 code (e.g. `USD`, `EUR`) |
| refId | integer | ✅ Yes | Agency reference ID (mandatory) |

### **Response Example**

```json
{
  "fsid": "ff0b6edf-efef-d4bc-a70b-3a1cfef9002d",
  "_request": {
    "departureDate": "2025-10-30T00:00:00",
    "arrivalDate": null,
    "fromAirportId": "MOW",
    "toAirportId": "AYT",
    "currencyId": "USD",
    "adultCount": 1,
    "childCount": 0,
    "infantCount": 0
  },
  "flights": {
    "0": [
      {
        "id": "1bec852e-5a87-ef17-2e5e-3a1c5fc59d28",
        "origin": "Moscow, Russia (VKO-Vnukovo Intl.)",
        "destination": "Antalya, Turkey (AYT-Antalya Intl.)",
        "departureDate": "2025-10-30T01:10:00",
        "arrivalDate": "2025-10-30T05:35:00",
        "airlineName": "AZURAIR",
        "flightNo": "ZF 1051",
        "iataCode": "ZF",
        "icaoCode": "KTK",
        "price": 147.10,
        "currencyId": "USD",
        "route": "VKO-AYT",
        "baggageWeight": "20",
        "baggageInfo": "kg",
        "isCharter": true,
        "handBaggage": "8"
      }
    ]
  },
  "errorMessage": null,
  "market": null,
  "isSuccess": false
}
```

### **Response Details (Expanded)**

| Field | Type | Description |
|-------|------|-------------|
| **fsid** | `string` | Unique identifier for the search request (used for tracking or re-fetching results). |
| **_request** | `object` | Echo of the normalized request parameters processed by the API. |
| **_request.departureDate / arrivalDate** | `string` | Departure and return dates (ISO 8601). |
| **_request.fromAirportId / toAirportId** | `string` | IATA codes for departure and arrival airports. |
| **_request.currencyId** | `string` | ISO 4217 currency code used in the request. |
| **_request.adultCount / childCount / infantCount** | `integer` | Passenger distribution per category. |
| **flights** | `object` | Collection of flight segments grouped by index (`"0"`, `"1"`, etc.). Each contains an array of available flight options. |
| **flights[n].id** | `string` | Unique flight ID for booking or basket usage. |
| **flights[n].origin / destination** | `string` | Full name of the airports including country and IATA code. |
| **flights[n].departureDate / arrivalDate** | `string` | Departure and arrival times (ISO 8601). |
| **flights[n].airlineName** | `string` | Airline name. |
| **flights[n].flightNo** | `string` | Flight number (e.g. `ZF 1051`). |
| **flights[n].iataCode / icaoCode** | `string` | Airline codes per IATA/ICAO. |
| **flights[n].route** | `string` | Flight route code (e.g. `VKO-AYT`). |
| **flights[n].price** | `decimal` | Flight price per passenger in the given currency. |
| **flights[n].currencyId** | `string` | Currency used for the price. |
| **flights[n].baggageWeight / baggageInfo** | `string` | Checked baggage weight and unit (e.g., `20`, `kg`). |
| **flights[n].handBaggage** | `string` | Hand baggage allowance (e.g., `8 kg`). |
| **flights[n].isCharter** | `boolean` | `true` if the flight is charter, otherwise `false`. |
| **errorMessage** | `string/null` | Null when successful; contains error info when failed. |
| **market** | `string/null` | Reserved field for market or distribution info (optional). |
| **isSuccess** | `boolean` | Indicates success status of the request. |

---

## 3. 🌍 Active Destination List

**Endpoint:**  
```
GET https://api/services/app/flight/GetRoutes
```

**Description:**  
Retrieve all active route pairs available for search.

**Sample Response:**
```json
{
  "routes": [
    { "id": 0, "orig": "ADB", "dest": "DME", "dates": ["12.10.2025"] },
    { "id": 0, "orig": "DME", "dest": "ADB", "dates": ["05.10.2025"] },
    { "id": 0, "orig": "AER", "dest": "AYT", "dates": ["01.04.2023"] }
  ]
}
```

---

## 4. 🧺 Basket

**Endpoint:**  
```
POST https://api/services/app/booking/AddBasketAsync
```

**Parameters:**

| Field | Type |
| --- | --- |
| searchId | string |
| flightId | string |
| returnFlightId | string |

**Sample Request:**
```json
{
  "searchId": "xxxxx",
  "flightId": "xxxxx",
  "returnFlightId": "e1f732c4-40f2-fe21-8597-39e63da0633f"
}
```

**Sample Response:**
```json
{
  "result": {
    "basketId": "fe2c9b90-1290-441e-823d-391d0e8a8b19",
    "createdAt": "2025-10-16T12:30:00Z"
  },
  "success": true
}
```

---

## 5. 🧾 Booking

**Endpoint:**  
```
POST https://api/services/app/booking/CreateFlightBooking
```

**Parameters**

| Field | Type |
| --- | --- |
| basketId | string |
| orderInput | json |

**OrderInput Structure**

| Key | Type | Description |
| --- | --- | --- |
| passengers | array | Passenger details |
| customer | object | Customer contact info |
| payment | object | Payment details |

**Sample Request:**
```json
{
  "basketId": "f28ee7c2-52fb-faae-2460-39e772499d23",
  "orderInput": {
    "passengers": [
      {
        "name": "Test",
        "surname": "Tester",
        "gender": 0,
        "birthday": "1990-07-15",
        "passengerType": 1,
        "passportNumber": "123456789",
        "passportSerial": "DF",
        "passportExpireDate": "2026-01-01",
        "nationality": "RU"
      }
    ],
    "customer": {
      "name": "Test",
      "surname": "Tester",
      "gsm": "37644444444444",
      "email": "admin@flyantalya.com",
      "title": ""
    },
    "payment": {
      "paymentType": 0,
      "creditCard": {
        "cardOwner": "Test kullanici",
        "cardNumber": "1111-1111-1111-1111",
        "cardType": 1,
        "expiryMonth": 6,
        "expiryYear": 2026,
        "cvc": "000"
      }
    }
  }
}
```

**Sample Response:**
```json
{
  "result": {
    "orderNo": "EX61234",
    "totalPrice": 20286.88,
    "currencyId": "USD",
    "customer": {
      "name": "Test",
      "surname": "Tester",
      "email": "admin@flyantalya.com"
    },
    "flightOrders": [
      {
        "pnr": "54777",
        "flightNo": "ZF 1051",
        "origin": "MOW",
        "destination": "AYT",
        "departureDate": "2025-10-30T01:10:00",
        "arrivalDate": "2025-10-30T05:35:00"
      }
    ]
  },
  "success": true
}
```

---
