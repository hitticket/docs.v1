
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

---

## 2. 🧭 Provider Flight Search (NEW)

**Endpoint:**  
```
POST https://api.hit-turizm.com/provider/flight/search
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

### **Notes**
- `refId` is **mandatory** for all requests.  
- Leave `arrivalDate` empty for **one-way** flights.  
- `currencyId` must follow **ISO 4217** (e.g., `USD`, `EUR`, `TRY`).  
- `_request` field in the response reflects normalized request data.

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

### Passenger Parameters

| Field | Type |
| --- | --- |
| name | string |
| surname | string |
| gender | int |
| birthday | string |
| passengerType | int |
| passportNumber | string |
| passportSerial | string |
| passportExpireDate | string |
| nationality | string |

**PassengerType**
```
Adult = 1, Child = 2, Infant = 3
```

**Gender**
```
Male = 0, Female = 1
```

**Nationality**
```
Use short country code (e.g., "RU")
```

---

### Customer Parameters

| Field | Type |
| --- | --- |
| name | string |
| surname | string |
| gsm | string |
| email | string |
| title | string |

---

### Payment Parameters

| Field | Type |
| --- | --- |
| paymentType | int |
| creditCard | json |

**PaymentType**
```
CreditCard = 0, Account = 1
```

**CreditCard Parameters**

| Field | Type |
| --- | --- |
| cardOwner | string |
| cardNumber | string |
| cardType | int |
| expiryMonth | int |
| expiryYear | int |
| cvc | string |

**CardType**
```
Master = 0, Visa = 1, Amex = 2
```

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

---

© 2025 Hit Turizm API Documentation
