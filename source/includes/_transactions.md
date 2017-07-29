# Transactions

> Example Transaction Record

```
{
        "_id": "583dd1d6daaab713f8a7a4d8",
        "_created": "2016-11-29T19:07:02.905Z",
        "_updated": "2016-11-29T19:07:16.221Z",
        "_method_updated": "2016-11-29T19:07:02.905Z",
        "_contentID_updated": "2016-11-29T19:07:02.905Z",
        "_asSiteID_updated": "2016-11-29T19:07:02.905Z",
        "_status_updated": "2016-11-29T19:07:02.905Z",
        "_taxed_updated": "2016-11-29T19:07:02.905Z",
        "_currency_updated": "2016-11-29T19:07:02.905Z",
        "_value_updated": "2016-11-29T19:07:02.905Z",
        "value": 25,
        "currency": "USD",
        "transDate": "2016-11-29T19:07:02.000Z",
        "person": "583dd1d6dagbe323522",
        "taxed": true,
        "status": "disbursed",
        "asSiteID": "oklahoma_23",
        "contentID": "oklahoma_23_223",
        "method": "credit_card",
        "__v": 0,
        "note": [
            "Notes go in this space!"
        ],
        "items": [
            {
                "_updated": "2016-11-29T19:07:02.905Z",
                "label": "Lifetime Membership",
                "price": 25,
                "_id": "583dd1d6daaab713f8a7a4da"
            }
        ],
        "receipt": {
            "to": "cafourek@gmail.com",
            "emailID": "log_66c55cd19a8276a60afde4119b2fd34c-3",
            "emailService": "Sendwithus",
            "sent": true,
            "_updated": "2016-11-29T19:07:16.221Z"
        },
        "external": [
            {
                "_updated": "2016-11-29T19:07:02.905Z",
                "key": "braintree",
                "value": "hdrqf3zg",
                "_id": "583dd1d6daaab713f8a7a4d9"
            }
        ]
    }
```

A `transaction` is a base object in the Alumni.Fund Vault API which describes the attributes of a particular exchange of financial information that has transpired in a 3rd party system.

![Buying stuff](https://files.readme.io/BFROEvGKSOm3iABjxNfg_transaction.jpg)

### What a transaction is not
Note that this object and its associated methods are for logging and recording, there are no methods to support the actual completion of a financial transaction.  All records are inbound via direct calls or created by webhook. 

Transactions have the following attributes (**bold** indicates top level object element)


|  Response Parameter  | Definition | Type  | Example  | 
|  ------------- | ------------- | ------------- | -------------  | 
|  **id** | Unique identifier for this transaction | string | "1234ABCD"  | 
|  **type** | Classification of what kind of transaction this is | string | Donation, Purchase, Ticket | 
|  **value** | Currency value of the transaction. Only valid characters are numbers and decimal | number | 10.00 | 
| **currency** | ISO 4217 currency code | string | USD,GBP | 
| **channel** | Channel through which payment was made | string | Braintree,Venmo,Square | 
| **method** | Payment method used | string | Credit card, Venmo, Check | 
| **physical** | Was transaction completed in-person? | boolean | true/false | 
| **transDate** | ISO 8601 date of transaction timestamp | string | 2015-07-05T22:16:18+00:00 | 
| **person** | Unique ID of person created by or mapped to this transaction (see /people/:id method) | string | 1234 | 
| **external** | Object containing external IDs affiliated with this transaction. Keys are TBD based on future integrations | object | 
| external.braintree | Braintree transaction ID | string | 1234 | 
| external.shopify | Shopify transaction ID | string |   | 
| **createdDate** | ISO 8601 timestamp for when record was created | string |   | 
| **modifiedDate** | ISO 8601 timestamp for when record was last altered. Should be updated with each write operation | string |   | 
| **disbursement** | Object containing data about payout of the funds associated with this transaction | object |   | 
| disbursement.complete | Indicator for whether funds have been paid out to recipient for this transaction | boolean | true/false | 
| disbursement.recipient | Client ID for intended recipient of funds | string | testnetwork_93 | 
| **taxed** | Boolean indicator where false indicates tax exempt status | boolean | true/false | 
| **receipt** | Object containing information about customer notification | object | 
| receipt.sent | Boolean indicator for receipt sent status | boolean | true/false | 
| receipt.emailID | External mail service ID of receipt email. Mandrill ID, etc | string | 
| receipt.to | Email address where receipt is to be sent | string | tester@gmail.com | 
| **source** | Object containing some basic info about how this transaction was obtained | object | 
| source.channel | Medium through which customer was obtained | string | Facebook, email, event | 
| source.interface | Tool the customer interfaced with to process payment | string | web, checkin, email | 
| source.campaign | Campaign code used in channel | string | October_Watch_Party_email3 | 
| source.id | ID code used in campaign | string | 12345 | 
| **contentID** | Alumni Spaces Content ID affiliated with this transaction | string | testnetwork_93_18 | 
| **status** | Some payment methods are not completed right away and a transaction is not considered funded and complete until status = settled | string | pending/failed/settled | 
| **note** | Array field of text notes that may be affiliated with transaction | array | 



## Get a Transaction

You can fetch the record of a transaction by using a `GET` request accompanied by the transaction ID number.

> Fetch transaction ID `58d7d9187200bc2fbbff77f1`

```
GET https://vault.alumnispaces.com/transaction/58d7d9187200bc2fbbff77f1
```

> Returns

```
{
    "_id": "58d7d9187200bc2fbbff77f1",
    "_created": "2017-03-26T15:07:04.808Z",
    "_updated": "2017-03-26T15:07:22.366Z",
    "_method_updated": "2017-03-26T15:07:04.808Z",
    "_contentID_updated": "2017-03-26T15:07:04.808Z",
    "_asSiteID_updated": "2017-03-26T15:07:04.808Z",
    "_status_updated": "2017-03-26T15:07:04.808Z",
    "_taxed_updated": "2017-03-26T15:07:04.808Z",
    "_currency_updated": "2017-03-26T15:07:04.808Z",
    "_value_updated": "2017-03-26T15:07:04.808Z",
    "value": 5,
    "currency": "USD",
    "transDate": "2017-03-26T15:07:04.000Z",
    "person": "58d7d9187200bc2fbbff77ef",
    "taxed": true,
    "status": "pending",
    "asSiteID": "littlestate_30",
    "contentID": "littlestate_30_25",
    "method": "credit_card",
    "__v": 0,
    "note": [
        "I have a comment!"
    ],
    "items": [
        {
            "_updated": "2017-03-26T15:07:04.808Z",
            "_id": "58d7d9187200bc2fbbff77f3"
        }
    ],
    "receipt": {
        "to": "cafourek@gmail.com",
        "emailID": "log_ce4626ff55eb95830039930b67206b6f-3",
        "emailService": "Sendwithus",
        "sent": true,
        "_updated": "2017-03-26T15:07:22.366Z"
    },
    "external": [
        {
            "_updated": "2017-03-26T15:07:04.808Z",
            "key": "braintree",
            "value": "4pth679m",
            "_id": "58d7d9187200bc2fbbff77f2"
        }
    ]
}
```


### HTTP Request

`GET https://vault.alumnispaces.com/transaction/{id}`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the Transaction to retrieve

## Create a Transaction

Creating a transaction simply requires making a POST to the /transaction endpoint with JSON-formatted data. When successfully created, an ID is returned.

<aside class="notice">
When POSTing, make sure you have set the `Content-Type` header to `application/json`
</aside>

> Response when creating a Transaction

```
	{
	  "_id": "58d7d9187200bc2fbbff77f1"
	}
```
