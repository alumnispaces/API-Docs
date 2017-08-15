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
            "to": "tlannister@meereen.govv",
            "emailID": "log_66c55cd19as38276a60afde4119b2fd34c-3",
            "emailService": "Sendwithus",
            "sent": true,
            "_updated": "2016-11-29T19:07:16.221Z"
        },
        "external": [
            {
                "_updated": "2016-11-29T19:07:02.905Z",
                "key": "braintree",
                "value": "hdrqf3zg",
                "_id": "583dd1daggwqq13f8a2512d9"
            }
        ],
        "extra_fields": [
            {
                "value": "2007",
                "label": "Year of graduation",
                "field_type": "text",
                "id": "year_of_graduation",
                "_updated": "2017-08-10T16:08:39.574Z",
                "_id": "598c85071d18fa6948f21afa"
            },
            {
                "value": "english",
                "label": "Major",
                "field_type": "text",
                "id": "major",
                "_updated": "2017-08-10T16:08:39.574Z",
                "_id": "598c85071d18fa6948f21af9"
            },
            {
                "value": "1234556",
                "label": "Member ID",
                "field_type": "text",
                "id": "member_id",
                "_updated": "2017-08-10T16:08:39.574Z",
                "_id": "598c85071d18fa6948f21af8"
            }
        ]
    }
```

A `transaction` is a base object in the Alumni.Fund Vault API which describes a financial exchange within a 3rd party system.

![Buying stuff](https://files.readme.io/BFROEvGKSOm3iABjxNfg_transaction.jpg)

### What a transaction is not
Note that this object and its associated methods are for logging and recording, there are no methods to support the actual completion of a financial transaction.  POST actions add `Transaction` records, they do not initiate actual Transactions.

Transactions have the following attributes (**bold** indicates top level object element)


|  Response Parameter  | Definition | Type  | Example  |
|  ------------- | ------------- | ------------- | -------------  |
|  **_id** | Unique identifier for this transaction | string | "1234ABCD"  |
**_created** | Date this Transaction's record was first created | string as ISO8601 Date  |
**_updated** | Date this Transaction's record was last updated | string as ISO8601 Date |
|  **type** | Classification of what kind of transaction this is | string | Donation, Purchase, Ticket |
|  **value** | Currency value of the transaction. Only valid characters are numbers and decimal | number | 10.00 |
| **currency** | ISO 4217 currency code | string | USD,GBP |
| **method** | Payment method used | string | credit_card, Venmo, Check |
| **physical** | Was transaction completed in-person? | boolean | true/false |
| **transDate** | ISO 8601 date of transaction timestamp | string | 2015-07-05T22:16:18+00:00 |
| **person** | Unique ID of person created by or mapped to this transaction (see [/people/:id method](#get-a-person)) | string | 1234 |
| **external** | Object containing external data objects affiliated with this transaction, allowing us to map data between transaction systems | object |
| **disbursement** | Object containing data about payout of the funds associated with this transaction | object |   |
| disbursement.complete | Indicator for whether funds have been paid out to recipient for this transaction | boolean | true/false |
| disbursement.recipient | Client ID for intended recipient of funds | string | testnetwork_93 |
| **receipt** | Object containing information about customer notification | object |
| receipt.sent | Boolean indicator for receipt sent status | boolean | true/false |
| receipt.emailID | External mail service ID of receipt email. Mandrill ID, etc | string |
| receipt.to | Email address where receipt is to be sent | string | tester@gmail.com |
| receipt.service | Provider used to deliver receipt | string | Sendwithus |
| **source** | Object containing some basic info about how this transaction was obtained | object |
| source.channel | Medium through which customer was obtained | string | Facebook, email, event |
| source.interface | Tool the customer interfaced with to process payment | string | web, checkin, email |
| source.campaign | Campaign code used in channel | string | October_Watch_Party_email3 |
| source.id | ID code used in campaign | string | 12345 |
| **contentID** | Alumni Spaces Content ID affiliated with this transaction | string | testnetwork_93_18 |
| **asSiteID** | Alumni Spaces Group ID affiliated with this transaction | string | testnetwork_93 |
| **status** | Some payment methods are not completed right away and a transaction is not considered funded and complete until status = settled | string | pending/failed/settled |
| **note** | Array field of text notes that may be affiliated with transaction | Array |
| **items** | Array of objects describing what "product" was purchased in this flow. Alumni.Fund will show $5,$10,$20, etc by default but this may be customized by group or network. | Array |
| item.label | Group or network-provided text label for the product purchased.  If not present, there were no custom products defined and the Person selected an Alumni.Fund default of $5, $10, $20, etc. (as described in the `value` property)  | String | Membership |
| item.price | If custom product was purchased, it's sale price is saved here. Should be the same as the `value` property | Number | 85
| **extra_fields** | Array of objects describing additional fields added to the Alumni.Fund flow by the group or network. This is currently limited to 3 fields. | Array |
| extra_field.id | Programmatically-created "unique" identifier for this field. Simple strip/replace of the `item.label` to remove non-alphanumerics and replace spaces with underscores. Note: this will not have an underscore prefix. There is likely to be an auto-generated `_id` in this object as well | String | year_of_graduation |
| extra_field.label | Group or network-provided label for this field | String | Year of Graduation |
| extra_field.field_type | Field type. Currently, extra fields may be `checkbox`, `dropdown` or `text` | String | "text" |
| extra_field.value | Person-supplied value for this field. | String | "Summer 2007"



## Get a Transaction
You can fetch the record of a transaction by using a `GET` request accompanied by the transaction ID number.

> Fetch transaction ID `58d7d9187200bc2fbbff77f1`

```
GET https://vault.alumnispaces.com/transaction/58d7d9187200bc2fbbff77f1
```


### HTTP Request for Transaction

`GET https://vault.alumnispaces.com/transaction/{id}`

### URL Parameters for Transaction

Parameter | Description
--------- | -----------
ID | The ID of the Transaction to retrieve

## Create a Transaction

> Response when creating a Transaction

```
	{
	  "_id": "58d7d9187200bc2fbbff77f1"
	}
```
Creating a transaction simply requires making a POST to the /transaction endpoint with JSON-ready data passed as parameters. When successfully created, a `Transaction` ID is returned.

## Update a Transaction

Updating a transaction simply requires making a PUT request to the /transaction endpoint with JSON-ready data passed as parameters. When successfully updated, the full, updated `Transaction` record is returned.

<aside class="notice">
When POSTing or PUTing, make sure you have set the `Content-Type` header to `application/json`
</aside>

## Querying Transactions

> Example request combining Order and Grouping parameters (records truncated for readability)

```
GET https://vault.alumnispaces.com/transaction/query?order=DESC&order_by=value&group_by=asSiteID
```

```
{
  "bigstate_19": [
      {
        "_id": "596453247e9213434132811",
        "_created": "2017-07-11T02:26:15.715Z",
        "value": 900
      },
      {
        "_id": "593252247e92233434132231",
        "_created": "2017-07-11T14:55:09.769Z",
        "value": 800
      },
      {
        "_id": "131252247e9223343098021",
        "_created": "2016-06-12T14:12:08.121Z",
        "value": 500
      },
  ],
  "bigstate_37": [
      {
        "_id": "324237e92134341332523",
        "_created": "2013-09-04T02:26:15.715Z",
        "value": 100
      },
      {
        "_id": "080252247e9223343877",
        "_created": "2015-11-25T14:55:09.769Z",
        "value": 75
      },
      {
        "_id": "566247e92233430780082",
        "_created": "2016-06-12T14:12:08.121Z",
        "value": 50
      },
  ],
}
```



The Vault supports a number of query parameters that can make it easier to fetch and handle data in meaningful ways. All queries are a part of the `/transaction/query/` endpoint and depend on additional query parameters.

### Return Grouping and Ordering

You may request that the `Transaction` records be returned in specific groups, orders or ask us to do some math before sending it back to you.

Parameter | Description | Example
--------- | ----------- | --------
`order` | Change the order of the returned data. Must be used with `order_by` | `ASC` (default), `DESC`
`order_by` | The field that `order` should be based upon. Will work with any first-level field based on alphabetic or numerical order. (Date fields will inherently work in alpha/numeric ordering since they are in ISO format) | `value`, `transDate`, `_updated`
`group_by` | Group records based on a shared property value. The `group_by` value will become the parent to the transaction records contain within it (see example JSON)  Supports `person`, `status`, `taxed`, `contentID`,or `asSiteID` | `asSiteID` will return records within parent objects such as `littlestate_39` and `oklahoma_123`


<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />


### Return Aggregate Math

> Example of aggregation math

```
This request says to aggregate by asSiteID and return the sum of the "value" fields within those asSiteIDs
GET https://vault.alumnispaces.com/transaction/query?aggregate_by=asSiteID&aggregate_sum=value
```

```
[
    {
        "_id": "bigstate_39",
        "_count": 6,
        "value_sum": 185
    },
    {
        "_id": "bigstate_38",
        "_count": 1,
        "value_sum": 20
    },
    {
        "_id": "bigstate_21",
        "_count": 6,
        "value_sum": 120
    },
    {
        "_id": "bigstate_18",
        "_count": 1,
        "value_sum": 10
    }
]
```

You may also want to quickly determine "What is the total value of all transactions per group?" or "What is the most recent transaction date?". To help with this, Vault provides aggregate math parameters.

Aggregation parameters work with `order` and `order_by` query parameters but will not work when combined with [Return Filtering](#return-filtering) parameters.

| Parameter | Description | Example |
| ----------------- | ----------- | --------
| `aggregate_by` | Requesting aggregate data will not return any `Transaction` records. Instead, you'll get grouped math summaries of the data within those records. This parameter is very similar to `group_by`  and accepts the same options as `group_by`. Requires `aggregate_{method}` parameter. | (See code example) |
| `aggregate_{method}`&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | This parameter uses both the key and value to determine your request. {method} may be `sum`, `avg`, `min` or `max` while the parameter's value should be a `Transaction` property. | `aggregate_sum=value`, `aggregate_min=transDate` |

### Aggregation Request Examples

|  | |
| ------- | ----- |
| When was the most recent transaction for each group? | `/transaction/query?aggregate_by=asSiteID&aggregate_max=transDate` |
| What is the average payment value for a particular event? | `/transaction/query?aggregate_by=contentID&aggregate_avg=value` |
| What is the average payment value for a particular event? | `/transaction/query?aggregate_by=contentID&aggregate_avg=value` |
| What is the total value of all transactions still pending settlement? | `/transaction/query?aggregate_by=status&aggregate_sum=value` |

<br />
<br />
<br />
<br />
<br />

### Return Filtering

You may also request records based on field value or transaction data (`transDate`).  Returned data will be in the same format as expected from a vanilla `/transaction` or `group_by` request.

These filter options may be combined with `order`, `order_by` and `group_by` parameters to customize return format.



### Date Ranges

Parameter  | Description | Example
------- | ----- | -----
`transDate_start` | Only return transactions that occurred after this date. Format YYY-MM-DD. | `transDate_start=2017-06-01`
`transDate_end` | Only return transactions that occurred before this date. Format YYY-MM-DD. | `transDate_end=2017-08-10`

You may use `transDate_start` and `transDate_end` to create a range. Ranges are only inclusive of the start date: requesting June 1 - August 10 means you're asking for data from 12:01AM on June 1 through 11:59PM on August 9.

### Field Filter

You may also add a parameter to limit by any field value by simply using `{field}: {value}`

Examples:

|  | |
------- | -----
Get all transactions for specific group | `/transaction/query?contentID=bigstate_38_268` |
Get all transactions where `disbursement.complete` is true. Also demonstrates targeting nested fields in queries | `/transaction/query?disbursement.complete=true`
Get all transactions for specific `Person` ID | `/transaction/query?person=58e7f3252192134341749143347c3`
