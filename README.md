
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

Example Response
```
{
    "routes": [
        {
            "id": 0,
            "orig": "ADB",
            "dest": "DME",
            "dates": [
                "12.10.2025"
            ]
        },
        {
            "id": 0,
            "orig": "DME",
            "dest": "ADB",
            "dates": [
                "05.10.2025"
            ]
        },
        {
            "id": 0,
            "orig": "AER",
            "dest": "AYT",
            "dates": [
                "01.04.2023"
            ]
        },
        {
            "id": 0,
            "orig": "AYT",
            "dest": "AER",
            "dates": [
                "29.03.2023",
                "31.03.2023",
                "01.04.2023",
                "04.04.2023",
                "05.04.2023"
            ]
        }
    ]
}
```
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

Booking Example Response

```
{
    "result": {
        "referrerId": null,
        "emailMessageId": null,
        "emailMessage": null,
        "totalPrice": 20286.88,
        "currency": null,
        "currencyId": "RUB",
        "orderNo": "EX6**",
        "customer": {
            "name": "ANNA",
            "surname": "FILEVA",
            "gsm": "***",
            "email": "***",
            "title": null,
            "fullName": "ANNA FILEVA",
            "hotelId": null,
            "regionId": null,
            "agencyId": null,
            "tcNumber": null,
            "isDeleted": false,
            "deleterUserId": null,
            "deletionTime": null,
            "lastModificationTime": null,
            "lastModifierUserId": null,
            "creationTime": "2023-03-28T16:02:00",
            "creatorUserId": null,
            "id": "1192b6d0-d2d7-52e9-2d84-3a0a3a7c6268"
        },
        "payments": [
            {
                "orderId": "45e7f1a6-5650-d3e8-348a-3a0a3a7c64d9",
                "paymentType": 8,
                "cardId": "7680294b-3759-9544-1939-3a0a3a7c6268",
                "creditCard": {
                    "cardNumber": "",
                    "expiryYear": 0,
                    "expiryMonth": 0,
                    "cardOwner": "",
                    "cardType": 0,
                    "cvc": "",
                    "ownerId": null,
                    "ownerType": 0,
                    "isDeleted": false,
                    "deleterUserId": null,
                    "deletionTime": null,
                    "lastModificationTime": null,
                    "lastModifierUserId": null,
                    "creationTime": "2023-03-28T16:02:00",
                    "creatorUserId": null,
                    "id": "7680294b-3759-9544-1939-3a0a3a7c6268"
                },
                "bankTransferId": null,
                "bankTransfer": null,
                "paymentTotal": 20286.88,
                "currencyId": "RUB",
                "currency": {
                    "name": "Pубль",
                    "symbol": "rub",
                    "isDeleted": false,
                    "deleterUserId": null,
                    "deletionTime": null,
                    "lastModificationTime": null,
                    "lastModifierUserId": null,
                    "creationTime": "2015-08-28T16:03:00",
                    "creatorUserId": null,
                    "id": "RUB"
                },
                "isTestPayment": false,
                "creationTime": "2023-03-28T16:02:00",
                "paymentStatus": 1,
                "id": "de43dfa8-2256-85ac-b678-3a0a3a7c6268"
            }
        ],
        "flightOrders": [
            {
                "pnr": "54777",
                "orderId": "45e7f1a6-5650-d3e8-348a-3a0a3a7c64d9",
                "order": null,
                "orderLineId": "7b0b56e8-dc7c-9733-7853-3a0a3a7c64d9",
                "flightId": "c2c6d80d-8c54-6882-22eb-3a0a3a69e06c",
                "flight": {
                    "fromAirportId": "KIV",
                    "fromAirport": {
                        "name": "Chisinau, Moldova (KIV-Chisinau Intl.)",
                        "country": null,
                        "city": null,
                        "parentId": null,
                        "cityId": 601844,
                        "countryId": "MD",
                        "countryName": "Moldova",
                        "nameWithCode": "Chisinau, Moldova (KIV-Chisinau Intl.) (KIV)",
                        "latitude": "46.935212",
                        "longitude": "28.934121",
                        "expediaId": 6000333,
                        "airportIsActive": true,
                        "isCity": false,
                        "isDeleted": false,
                        "deleterUserId": null,
                        "deletionTime": null,
                        "lastModificationTime": null,
                        "lastModifierUserId": null,
                        "creationTime": "2015-11-04T11:46:00",
                        "creatorUserId": null,
                        "id": "KIV"
                    },
                    "fromAirportName": "Chisinau, Moldova (KIV-Chisinau Intl.)",
                    "toAirportId": "AYT",
                    "toAirport": {
                        "name": "Antalya, Turkey (AYT-Antalya Intl.)",
                        "country": null,
                        "city": null,
                        "parentId": null,
                        "cityId": 481,
                        "countryId": "TR",
                        "countryName": "Turkey",
                        "nameWithCode": "Antalya, Turkey (AYT-Antalya Intl.) (AYT)",
                        "latitude": "50.026049",
                        "longitude": "50.026049",
                        "expediaId": 1111111,
                        "airportIsActive": true,
                        "isCity": false,
                        "isDeleted": false,
                        "deleterUserId": null,
                        "deletionTime": null,
                        "lastModificationTime": "2017-02-03T09:26:00",
                        "lastModifierUserId": 10013,
                        "creationTime": "2015-08-28T15:57:00",
                        "creatorUserId": null,
                        "id": "AYT"
                    },
                    "toAirportName": "Antalya, Turkey (AYT-Antalya Intl.)",
                    "departureDate": "2023-10-01T06:30:00+03:00",
                    "arrivalDate": "2023-10-01T08:30:00",
                    "departureTerminalId": null,
                    "departureTerminal": null,
                    "arrivalTerminalId": null,
                    "arrivalTerminal": null,
                    "flightDuration": "2",
                    "totalDuration": null,
                    "iataCode": "9U",
                    "icaoCode": "MLD",
                    "takeOffTime": null,
                    "landingTime": null,
                    "departureHour": "06:30",
                    "arrivalHour": "08:30",
                    "flightHours": "06:30-08:30",
                    "airlineName": "AIR MOLDOVA",
                    "flightNo": "9U 755",
                    "flightCode": null,
                    "airCraftId": "00000000-0000-0000-0000-000000000000",
                    "airCraft": null,
                    "airCraftFullName": null,
                    "airLineCompanyId": "f537590c-d083-1568-411e-39df23dbb9ed",
                    "airlineCompany": {
                        "name": "AIR MOLDOVA",
                        "type": 0,
                        "companyType": 0,
                        "logoImageUrl": "/Content/images/airlines/9u@2x.png",
                        "accountNumber": null,
                        "accountRating": null,
                        "addressType": null,
                        "addressCityId": null,
                        "addressCountryId": null,
                        "addressFax": null,
                        "addressLatitude": null,
                        "addressLongitude": null,
                        "doNotBulkEmail": null,
                        "doNotBulkPostalMail": null,
                        "doNotEmail": null,
                        "doNotFax": null,
                        "doNotPhone": null,
                        "doNotPostalMail": null,
                        "doNotSendMailMerge": null,
                        "emailAddress1": null,
                        "emailAddress2": null,
                        "emailAddress3": null,
                        "fax": null,
                        "ftpSiteUrl": null,
                        "isPrivate": null,
                        "parentId": null,
                        "primaryContactId": null,
                        "secondaryContactId": null,
                        "reservationContactId": null,
                        "accountingContactId": null,
                        "revenue": null,
                        "status": 0,
                        "phone1": null,
                        "phone2": null,
                        "phone3": null,
                        "territoryId": null,
                        "webSiteUrl": null,
                        "contacts": [],
                        "sendConfirmationEmailTo": 0,
                        "confirmationContactId": null,
                        "confirmationContact": null,
                        "confirmationMailLanguageId": null,
                        "companyEmailSettings": [],
                        "companyContacts": [],
                        "shortCode": "9U",
                        "icaoCode": "MLD",
                        "id": "f537590c-d083-1568-411e-39df23dbb9ed"
                    },
                    "airlineLogoUrl": null,
                    "packageProviderId": "b24970bc-5da4-4d73-91ec-f8d226e5cfe5",
                    "packageProvider": null,
                    "serviceProviderFlightId": "c2c6d80d-8c54-6882-22eb-3a0a3a69e06c",
                    "serviceName": null,
                    "serviceKeyObject": "14382",
                    "hasStops": false,
                    "stopCount": 0,
                    "flightIsActive": false,
                    "minPrice": 20286.882,
                    "rtMinPrice": 0.0,
                    "currencyId": "RUB",
                    "currency": null,
                    "baggageLimit": 0,
                    "numberOfSeats": 0,
                    "changeCount": 0,
                    "passengerCount": 0,
                    "totalQuota": 0,
                    "totalConfirmed": 0,
                    "route": "KIV-AYT",
                    "flightPrices": [
                        {
                            "flightId": "00000000-0000-0000-0000-000000000000",
                            "flightClassId": "ECO",
                            "flightClass": null,
                            "flightSegmentId": "O",
                            "flightSegment": null,
                            "totalQuota": 0,
                            "flightPriceGenerals": [
                                {
                                    "flightPriceId": "d6c54ef2-421e-d7bf-03c7-3a0a3aa143e6",
                                    "pricePassengerTypeId": 1,
                                    "pricePassengerType": null,
                                    "endUserOwPrice": 20286.882,
                                    "endUserRtPrice": 20286.882,
                                    "agencyOwPrice": 20286.882,
                                    "agencyRtPrice": 20286.882,
                                    "minAge": 0,
                                    "maxAge": 0,
                                    "salesChannelId": "INTERNET",
                                    "salesChannel": null,
                                    "paxCount": 2,
                                    "flightPriceSpecials": null,
                                    "isGroup": false,
                                    "flightClassId": "ECO",
                                    "flightSegmentId": "0",
                                    "currencyId": null,
                                    "baggage": 0,
                                    "isDeleted": false,
                                    "deleterUserId": null,
                                    "deletionTime": null,
                                    "lastModificationTime": null,
                                    "lastModifierUserId": null,
                                    "creationTime": "2023-03-28T16:42:11.1749102+03:00",
                                    "creatorUserId": null,
                                    "id": "98b90284-a81a-455b-4b76-3a0a3aa143e6"
                                },
                                {
                                    "flightPriceId": "d6c54ef2-421e-d7bf-03c7-3a0a3aa143e6",
                                    "pricePassengerTypeId": 2,
                                    "pricePassengerType": null,
                                    "endUserOwPrice": 20286.882,
                                    "endUserRtPrice": 20286.882,
                                    "agencyOwPrice": 20286.882,
                                    "agencyRtPrice": 0.0,
                                    "minAge": 0,
                                    "maxAge": 0,
                                    "salesChannelId": "INTERNET",
                                    "salesChannel": null,
                                    "paxCount": 0,
                                    "flightPriceSpecials": null,
                                    "isGroup": false,
                                    "flightClassId": "ECO",
                                    "flightSegmentId": "0",
                                    "currencyId": null,
                                    "baggage": 0,
                                    "isDeleted": false,
                                    "deleterUserId": null,
                                    "deletionTime": null,
                                    "lastModificationTime": null,
                                    "lastModifierUserId": null,
                                    "creationTime": "2023-03-28T16:42:11.1749102+03:00",
                                    "creatorUserId": null,
                                    "id": "63041692-ec9a-cb64-793f-3a0a3aa143e6"
                                },
                                {
                                    "flightPriceId": "d6c54ef2-421e-d7bf-03c7-3a0a3aa143e6",
                                    "pricePassengerTypeId": 3,
                                    "pricePassengerType": null,
                                    "endUserOwPrice": 20286.882,
                                    "endUserRtPrice": 20286.882,
                                    "agencyOwPrice": 20286.882,
                                    "agencyRtPrice": 0.0,
                                    "minAge": 0,
                                    "maxAge": 0,
                                    "salesChannelId": "INTERNET",
                                    "salesChannel": null,
                                    "paxCount": 1,
                                    "flightPriceSpecials": null,
                                    "isGroup": false,
                                    "flightClassId": "ECO",
                                    "flightSegmentId": "0",
                                    "currencyId": null,
                                    "baggage": 0,
                                    "isDeleted": false,
                                    "deleterUserId": null,
                                    "deletionTime": null,
                                    "lastModificationTime": null,
                                    "lastModifierUserId": null,
                                    "creationTime": "2023-03-28T16:42:11.1749102+03:00",
                                    "creatorUserId": null,
                                    "id": "6331be4e-40f2-c626-4750-3a0a3aa143e6"
                                }
                            ],
                            "flightQuotas": [
                                {
                                    "flightPriceId": "d6c54ef2-421e-d7bf-03c7-3a0a3aa143e6",
                                    "quota": 10,
                                    "remainingQuota": 0,
                                    "isGroup": false,
                                    "flightClassId": null,
                                    "flightSegmentId": null,
                                    "isDeleted": false,
                                    "deleterUserId": null,
                                    "deletionTime": null,
                                    "lastModificationTime": null,
                                    "lastModifierUserId": null,
                                    "creationTime": "2023-03-28T16:42:11.1749102+03:00",
                                    "creatorUserId": null,
                                    "id": "db08d85a-da9b-061e-b121-3a0a3aa143e6"
                                }
                            ],
                            "isDeleted": false,
                            "deleterUserId": null,
                            "deletionTime": null,
                            "lastModificationTime": null,
                            "lastModifierUserId": null,
                            "creationTime": "2023-03-28T16:42:11.1749102+03:00",
                            "creatorUserId": null,
                            "id": "d6c54ef2-421e-d7bf-03c7-3a0a3aa143e6"
                        }
                    ],
                    "flightPriceRules": [],
                    "flightUrl": null,
                    "priceGenerals": [
                        {
                            "flightPriceId": "d6c54ef2-421e-d7bf-03c7-3a0a3aa143e6",
                            "pricePassengerTypeId": 1,
                            "pricePassengerType": null,
                            "endUserOwPrice": 20286.882,
                            "endUserRtPrice": 20286.882,
                            "agencyOwPrice": 20286.882,
                            "agencyRtPrice": 20286.882,
                            "minAge": 0,
                            "maxAge": 0,
                            "salesChannelId": "INTERNET",
                            "salesChannel": null,
                            "paxCount": 2,
                            "flightPriceSpecials": null,
                            "isGroup": false,
                            "flightClassId": "ECO",
                            "flightSegmentId": "0",
                            "currencyId": null,
                            "baggage": 0,
                            "isDeleted": false,
                            "deleterUserId": null,
                            "deletionTime": null,
                            "lastModificationTime": null,
                            "lastModifierUserId": null,
                            "creationTime": "2023-03-28T16:42:11.1749102+03:00",
                            "creatorUserId": null,
                            "id": "98b90284-a81a-455b-4b76-3a0a3aa143e6"
                        },
                        {
                            "flightPriceId": "d6c54ef2-421e-d7bf-03c7-3a0a3aa143e6",
                            "pricePassengerTypeId": 2,
                            "pricePassengerType": null,
                            "endUserOwPrice": 20286.882,
                            "endUserRtPrice": 20286.882,
                            "agencyOwPrice": 20286.882,
                            "agencyRtPrice": 0.0,
                            "minAge": 0,
                            "maxAge": 0,
                            "salesChannelId": "INTERNET",
                            "salesChannel": null,
                            "paxCount": 0,
                            "flightPriceSpecials": null,
                            "isGroup": false,
                            "flightClassId": "ECO",
                            "flightSegmentId": "0",
                            "currencyId": null,
                            "baggage": 0,
                            "isDeleted": false,
                            "deleterUserId": null,
                            "deletionTime": null,
                            "lastModificationTime": null,
                            "lastModifierUserId": null,
                            "creationTime": "2023-03-28T16:42:11.1749102+03:00",
                            "creatorUserId": null,
                            "id": "63041692-ec9a-cb64-793f-3a0a3aa143e6"
                        },
                        {
                            "flightPriceId": "d6c54ef2-421e-d7bf-03c7-3a0a3aa143e6",
                            "pricePassengerTypeId": 3,
                            "pricePassengerType": null,
                            "endUserOwPrice": 20286.882,
                            "endUserRtPrice": 20286.882,
                            "agencyOwPrice": 20286.882,
                            "agencyRtPrice": 0.0,
                            "minAge": 0,
                            "maxAge": 0,
                            "salesChannelId": "INTERNET",
                            "salesChannel": null,
                            "paxCount": 1,
                            "flightPriceSpecials": null,
                            "isGroup": false,
                            "flightClassId": "ECO",
                            "flightSegmentId": "0",
                            "currencyId": null,
                            "baggage": 0,
                            "isDeleted": false,
                            "deleterUserId": null,
                            "deletionTime": null,
                            "lastModificationTime": null,
                            "lastModifierUserId": null,
                            "creationTime": "2023-03-28T16:42:11.1749102+03:00",
                            "creatorUserId": null,
                            "id": "6331be4e-40f2-c626-4750-3a0a3aa143e6"
                        }
                    ],
                    "isMatched": false,
                    "flightWeight": null,
                    "baggage": "",
                    "id": "c2c6d80d-8c54-6882-22eb-3a0a3a69e06c"
                },
                "flightNo": "9U 755",
                "agencyId": "f829617c-f467-3b56-9365-39faf0c6a29e",
                "agency": null,
                "customerId": null,
                "orderStatus": 4,
                "passengerCount": 3,
                "adultCount": 2,
                "childCount": 0,
                "infantCount": 1,
                "totalPrice": 60860.64,
                "totalDiscount": 0.00,
                "currencyId": "RUB",
                "currency": null,
                "agencyNote": "METASEARCH",
                "airlineNote": "",
                "providerNote": "",
                "passengers": [
                    {
                        "flightOrderId": "fe7736ad-f52d-d54f-146e-3a0a3a7c64d9",
                        "flight": null,
                        "passengerId": "2b9679c5-ac14-5f11-afda-3a0a3a7c6268",
                        "passengers": [],
                        "passengerIsConfirmed": false,
                        "confirmationNo": null,
                        "confirmationDate": "0001-01-01T00:00:00",
                        "confirmedByUserId": null,
                        "confirmedByUser": null,
                        "tourOperatorId": "00000000-0000-0000-0000-000000000000",
                        "tourOperator": null,
                        "confirmationPrice": 0.0,
                        "confirmationCurrencyId": null,
                        "currency": null,
                        "pnl": null,
                        "flightNo": null,
                        "pnr": null,
                        "flightProviderId": null,
                        "flightProvider": null,
                        "flightProviderQuotaId": null,
                        "flightProviderQuota": null,
                        "flightClassId": "ECO",
                        "flightSegmentId": "0",
                        "passengerStatus": 1,
                        "price": 20286.88,
                        "isDeleted": false,
                        "deleterUserId": null,
                        "deletionTime": null,
                        "lastModificationTime": "2023-03-28T16:05:00",
                        "lastModifierUserId": null,
                        "creationTime": "2023-03-28T16:02:00",
                        "creatorUserId": null,
                        "id": "7695ecc1-6ec4-881d-3cef-3a0a3a7c6268"
                    },
                    {
                        "flightOrderId": "fe7736ad-f52d-d54f-146e-3a0a3a7c64d9",
                        "flight": null,
                        "passengerId": "d1e403e9-899c-a050-07f6-3a0a3a7c6268",
                        "passengers": [],
                        "passengerIsConfirmed": false,
                        "confirmationNo": null,
                        "confirmationDate": "0001-01-01T00:00:00",
                        "confirmedByUserId": null,
                        "confirmedByUser": null,
                        "tourOperatorId": "00000000-0000-0000-0000-000000000000",
                        "tourOperator": null,
                        "confirmationPrice": 0.0,
                        "confirmationCurrencyId": null,
                        "currency": null,
                        "pnl": null,
                        "flightNo": null,
                        "pnr": null,
                        "flightProviderId": null,
                        "flightProvider": null,
                        "flightProviderQuotaId": null,
                        "flightProviderQuota": null,
                        "flightClassId": "ECO",
                        "flightSegmentId": "0",
                        "passengerStatus": 1,
                        "price": 20286.88,
                        "isDeleted": false,
                        "deleterUserId": null,
                        "deletionTime": null,
                        "lastModificationTime": "2023-03-28T16:05:00",
                        "lastModifierUserId": null,
                        "creationTime": "2023-03-28T16:02:00",
                        "creatorUserId": null,
                        "id": "4e0ad7d0-ecb1-63e9-7ec2-3a0a3a7c6268"
                    },
                    {
                        "flightOrderId": "fe7736ad-f52d-d54f-146e-3a0a3a7c64d9",
                        "flight": null,
                        "passengerId": "16144de5-803e-c808-0ec6-3a0a3a7c6268",
                        "passengers": [],
                        "passengerIsConfirmed": false,
                        "confirmationNo": null,
                        "confirmationDate": "0001-01-01T00:00:00",
                        "confirmedByUserId": null,
                        "confirmedByUser": null,
                        "tourOperatorId": "00000000-0000-0000-0000-000000000000",
                        "tourOperator": null,
                        "confirmationPrice": 0.0,
                        "confirmationCurrencyId": null,
                        "currency": null,
                        "pnl": null,
                        "flightNo": null,
                        "pnr": null,
                        "flightProviderId": null,
                        "flightProvider": null,
                        "flightProviderQuotaId": null,
                        "flightProviderQuota": null,
                        "flightClassId": "ECO",
                        "flightSegmentId": "0",
                        "passengerStatus": 1,
                        "price": 20286.88,
                        "isDeleted": false,
                        "deleterUserId": null,
                        "deletionTime": null,
                        "lastModificationTime": "2023-03-28T16:05:00",
                        "lastModifierUserId": null,
                        "creationTime": "2023-03-28T16:02:00",
                        "creatorUserId": null,
                        "id": "004c194c-a347-892b-ede5-3a0a3a7c6268"
                    }
                ],
                "baggages": [],
                "flightInfo": "01.10.2023 KIV - AYT 06:30",
                "direction": 0,
                "creationTime": "2023-03-28T16:02:00",
                "lastModificationTime": "2023-03-28T16:05:00",
                "relatedOrderId": null,
                "price": 0.0,
                "isGroup": false,
                "isAuxService": true,
                "newFlight": null,
                "id": "fe7736ad-f52d-d54f-146e-3a0a3a7c64d9"
            }
        ],
        "tourOrders": [],
        "ticketOrders": [],
        "agency": {
            "name": "Hit-Ticket",
            "contact": "Hit-Ticket",
            "country": "NULL",
            "city": "NULL",
            "district": "NULL",
            "web": "NULL",
            "email": "NULL",
            "phone": "+90242****",
            "fax": "NULL",
            "invoiceAttachment": "Hit-Ticket",
            "address": "Antalya",
            "taxOffice": "NULL",
            "taxNo": "NULL",
            "logoUrl": "http://hit-ticket.com/content/images/hit-ticket.com.jpg",
            "parentId": null,
            "tenantId": null,
            "allowUserCount": 0,
            "creditLimit": 0,
            "defaultCurrency": null,
            "mikroTransferId": 0,
            "isDeleted": false,
            "deleterUserId": null,
            "deletionTime": null,
            "lastModificationTime": null,
            "lastModifierUserId": null,
            "creationTime": "2021-02-26T16:49:00",
            "creatorUserId": ***,
            "id": "f829617c-f467-3b56-9365-39faf0c6a29e"
        },
        "creationTime": "2023-03-28T16:02:00",
        "orderStatus": 4,
        "orderNotes": [],
        "paymentLogs": [],
        "operationStatus": 0,
        "operatorName": null,
        "isConfirmed": false,
        "cardStatusCode": "OK",
        "payment": null,
        "transferOrderNo": null,
        "id": "45e7f1a6-5650-d3e8-348a-3a0a3a7c64d9"
    },
    "targetUrl": null,
    "success": true,
    "error": null,
    "unAuthorizedRequest": false,
    "__abp": true
}
```
