# Hit-Ticket Web Api

Link: https://api

Our system is based on sending the necessary parameters in links in json format.

For login transactions, you must first get a successful response to the login request, and then you can perform the operation that requires authorization.

The important thing is that these operations are done in the same session. API requests that require user input are required to write token information in the request header section.

## 1. User Login

Link: https://api/Account (Type : POST)

Parameters: Tenancy Name, User Name and User Password (You can also obtain this information from Hit Software.)

| Field | Type |
| --- | --- |
| tenancyName | string |
| usernameOrEmailAddress | string |
| password | string |

Authorization:Bearer {token\_value}

Sample Request:

POST JSON TYPE: 
```
{
    "tenancyName": "default",
    "usernameOrEmailAddress": "**",
    "password": "**"
}
```

## 2. Flight Search

Link: https://api/services/app/flight/Search (Type : POST)

Parameters: Query (This information is valid for the fields "Departure shortcode / Arrival shortcode / Departure date / Arrival date (for round trip) / Adult passenger / Child passenger / Baby passenger number / Short code / Reference". The query information will be compared to these fields and the appropriate records will be fetched.) The sample response for directing the web site is direct forwarding with the url information in the "basePath" field. Url information ("fsid / flightId") fields are created with.

| Field | Type |
| --- | --- |
| origin | string |
| destination | string |
| departureDate | string |
| arrivalDate | string |
| adultCount | int |
| childCount | int |
| infantCount | int |
| currencyId | string |
| refId | int |

Sample Request:

POST JSON TYPE :

One way

```
{
    "origin": "AYT",
    "destination": "MOW",
    "departureDate": "2016-12-12",
    "arrivalDate": "",
    "adultCount": 1,
    "childCount": 0,
    "infantCount": 0,
    "currencyId": "TRY",
    "refId": 0
}
```

Round Trip

```
{
    "origin": "AYT",
    "destination": "MOW",
    "departureDate": "2016-12-12",
    "arrivalDate": "2016-12-18",
    "adultCount": 1,
    "childCount": 0,
    "infantCount": 0,
    "currencyId": "TRY",
    "refId": 0
}
```

## 3.Active Destination List

Link: https://api/services/app/flight/GetRoutes (Type : GET)

## 4. Basket

Link: https://api/services/app/booking/AddBasketAsync (Type : POST)

Parameters

| Field | Type |
| --- | --- |
| searchId | string |
| flightId | string |
| returnFlightId | string |

Sample Request:

POST JSON TYPE : 
```
{
    "searchId": "**",
    "flightId": "**",
    "returnFlightId": "e1f732c4-40f2-fe21-8597-39e63da0633f"
}
```

## 5. Booking

Link : https://api/services/app/booking/CreateFlightBooking (Type : POST)

Parameters

| Field | Type |
| --- | --- |
| basketId | string |
| orderInput | json |

OrderInput parameters

| passengers | json array |
| --- | --- |
| customer | Json |
| payment | json |

Passenger Parameters

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

PassengerType

Adult = 1, Child = 2, Infant = 3

Gender

Male = 0,Female = 1

Customer Parameters

| Field | Type |
| --- | --- |
| name | string |
| surname | string |
| gsm | string |
| email | string |
| title | string |

Payment Parameters

| Field | Type |
| --- | --- |
| paymentType | int |
| creditCard | json |

PaymentType

CreditCard = 0, Account = 1

CreditCard Parameters

| Field | Type |
| --- | --- |
| cardOwner | string |
| cardNumber | string |
| cardType | int |
| expiryMonth | int |
| expiryYear | int |
| cvc | string |

CardType

Master = 0,Visa = 1,Amex = 2

Sample Request :

POST JSON TYPE :
```
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
        "passportExpireDate": "2020-01-01"
      },
      {
        "name": "Best",
        "surname": "Tester",
        "gender": 1,
        "birthday": "1990-07-13",
        "passengerType": 1,
        "passportNumber": "1564897",
        "passportSerial": "DF",
        "passportExpireDate": "2020-01-01"
      },
      {
        "name": "Nest",
        "surname": "Tester",
        "gender": 0,
        "birthday": "2008-07-20",
        "passengerType": 2,
        "passportNumber": "695845",
        "passportSerial": "DF",
        "passportExpireDate": "2020-01-01"
      },
      {
        "name": "Rest",
        "surname": "Tester",
        "gender": 1,
        "birthday": "2017-07-18",
        "passengerType": 3,
        "passportNumber": "74584565",
        "passportSerial": "DF",
        "passportExpireDate": "2020-01-01"
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
        "expiryYear": 2016,
        "cvc": "000"
      }
    }
  }
}
```
