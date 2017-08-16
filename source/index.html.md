---
title: Alumni Spaces API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - json

toc_footers:
  - <a href='mailto:andrew@alumnispaces.com'>Email Andrew for help</a>


includes:
  - people
  - transactions

search: true
---

# Introduction

Alumni Spaces maintains two public APIs:

* **contentAPI** for accessing content created by groups
* **vaultAPI** for accessing transaction data from Alumni.Fund.  

At the moment, only vaultAPI is documented.  If you need access to contentAPI, please let us know.

The production URL for API access is `https://vault.alumnispaces.com`

For testing purposes, you may also use `https://vault-dev.alumnispaces.com`

## Authentication

To access any Alumni.Fund data, you'll first need to get two things from our team.

* HTTP Basic authentication username and password
* A valid API key - it is an alphanumeric string that determines your data access and "verb" permissions. We use this key to determine which institutions you have data right to and if you are allowed to GET, POST, PUT or DELETE. This API key is passed to our API using a custom header: `x-alumnispaces-key`

All requests require the presence of both authentication methods.

Any request that does not contain both authentication methods will be rejected.

<aside class="notice">
API Keys are limited by scope and verb permissions. Your key will be limited in scope to the records pertinent to your institution and, in most cases, will be limited to GET requests only, even though DELETE and PUT requests are documented here. If you feel that you need write access, please discuss with our team.
</aside>

# Alumni.Fund Vault

### Entities

* Alumni.Fund's Vault contains two entities: `Person` and `Transaction`.
* In the plural, they are referred to as `People` and `Transactions`.
* As you might imagine, a `Person` is the instigator of a `Transaction`.
* Each `Person` has a unique ID which is the only part of their identity that is stored with the `Transaction` record.  

At the moment, we do not de-duplicate or combine `Person` records, so `Tyrion Lannister` might have 5 different `Person` records to match the 5 transactions he's conducted. This is anticipated to change in the future. `Transactions` on the other hand, will always represent a single instance where funds are processed.

### Update Timestamps

> Example of `_updated` timestamps for object properties

```
"emails": [
  {
    "label": "work",
    "_updated": "2017-08-10T21:58:04.773Z",
    "address": "tlannister@meereen.gov"
  },
  {
    "label": "personal",
    "_updated": "2016-07-10T21:58:16.403Z",
    "address": "tyrionthetall@wmail.com"
  }
]
```

When a record is created for either a `Person` or a `Transaction`, we add a `_created` property to the record which contains as ISO8601 timestamp. If that record is ever updated, we add an `_updated` property with that timestamp. Almost every record will have an `_updated` value because it usually takes a few seconds for our code to communicate with the payment processing system or our own internal APIs.

> Example of `_updated` timestamps for non-object

```
"_contentID_updated": "2017-04-07T20:02:13.044Z",
"_asSiteID_updated": "2017-04-07T20:02:13.044Z",
"_status_updated": "2017-04-07T20:02:13.044Z",
"_taxed_updated": "2017-04-07T20:02:13.044Z",
```

Thinking ahead, it is reasonable to assume that we'll be updating `Person` records regularly as the Person perform more transactions, our data is integrated into more systems or we add additional data management features. So, we also maintain a per-property `_updated` value.

When a property is updated, you'll see a timestamp of when that update occurred. If the property is a nested object (like `emails`), we'll add the timestamp within that object. If the updated property it is not a nested object, we'll add the timestamp at the root of the record object using an underscore prefix and the property name. For example: `_status_updated: ` or `_disbursement_updated: `
