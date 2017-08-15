# People

> Example Person Record

```
{
  "_id": "598cd8a53fw352252b6ffqc",
  "_created": "2017-08-10T22:05:25.561Z",
  "_updated": "2017-08-10T22:05:35.253Z",
  "contentIDs": [
    "westeros_20_135",
    "westeros_35_123"
  ],
  "asSiteIDs": [
    "westeros_20",
    "westeros_35"
  ],
  "name": {
    "first": "Tyrion",
    "last": "Lannister",
    "_updated": "2015-09-01T17:28:23.781Z"
  },
  "addresses": [
    {
      "label": "Dorne Vacation Home",
      "company": "Lannister Family",
      "validStart": "05.08.1982",
      "validEnd": "11.11.2009",
      "addr1": "412 S. Sandsnake Dr.",
      "addr2": "Penthouse Suite",
      "city": "Sunspear",
      "region": "Dorne",
      "country": "Dorne",
      "postal": "1"
    }
  ],
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
  ],
  "organizations": [
    {
      "name": "The Iron Throne",
      "orgType": "Government Body",
      "startDate": "01.01.1995",
      "endDate": "01.01.1997",
      "title": "Hand of the King",
      "prevTitles": [ "Brother-in-law", "Nuncle" ]
    }
  ],
  "externalIDs" : [
    {
      "_updated": "2017-08-10T22:05:25.561Z",
      "key": "braintree",
      "value": null,
      "_id": "5953432579ger6b624g3d"
    },
    {
      "_updated": "2017-07-11T12:35:15.561Z",
      "key": "yourExternalDB",
      "value": "lannister_t",
      "_id": "09998122j"
    }
  ],
  "gender": "male",
  "birthday": "01.01.1971",
  "Twitter": {
    "username": "the_pImp"
  },
  "phones": [
    {
      "label": "pager",
      "number": "1234"
    },
    {
      "label": "mobile",
      "number": "5678"
    }
  ],
  "urls": [
    {
      "label": "Wikipedia",
      "value": "https://en.wikipedia.org/wiki/Tyrion_Lannister"
    }
  ]
}
```

A `person` is a core object in the Alumni.Fund Vault API. A person is exactly that: a warm-blooded human for home we have some amount of data. A person is created when one of the following things occurs:

* They perform a transaction through our system, either a purchase or donation
* (future) They RSVP or check in at an event managed by the Alumni Spaces platform
* (future) A member of a local group or national alumni association manually enters some information about this person

![A person](https://files.readme.io/wOOjyUC9T3a0AWqZyJtO_people.jpeg)


People have the following attributes (**bold** indicates top level object element)

| Response Parameter  | Definition | Type  | Example |
| ------------- | ------------- | ------------- | ------------- |
**_id** | Unique identifier for this person | string |
**_created** | Date this Person's record was first created | string as ISO8601 Date  |
**_updated** | Date this Person's record was last updated | string as ISO8601 Date |
**name** | Object containing Person's name | object |
name.first | Person's first name | string |
name.last | Person's last name | string |
**addresses** | Array of objects containing address records | Array|object |
address.label | Text label for this address record | string | "Home", "Office","Summer home"
address.validStart | Beginning of date range for which this is/was a valid address | string as ISO8601 Date
address.validEnd | End of date range for which this is/was a valid address | string as ISO8601 Date
address.addr1 | First line of address | string | 68 Jay St
address.addr2 | Second line of address | string | Suite 503
address.city | Town/City name | string | Brooklyn
address.region | Region/State name | string | New York
address.country | Country name | string as ISO 3166-1 Alpha-2 abbreviation | US
address.postal | postal/zipcode | string | 11201
**facebook** | Object containing identifying info for facebook | object
facebook.identityToken | Token saved if user has authenticated with the Alumni Spaces Facebook App.  Internal use only, not returnable. | string
facebook.displayName | Name user has chosen to display on Facebook | string
facebook.ID | Unique numerical id for Facebook profile | number
**twitter** | Object containing Twitter profile data | object
twitter.username | Twitter id without @ symbol | string | acafourek
**linkedin** | Object containing LinkedIn profile data | object
linkedin.username | Unique username on LinkedIn | string | andrew.cafourek
**emails**  | Array of objects containing email data | Array
email.label | Text label for email field | string | Personal
email.address | Email address | string | andrew@gmail.com
**phones** | Array of objects containing phone numbers | Array
phone.label | Text label for email field | string | Cell
phone.number | Phone number, stripped of non numeric characters | string (intentionally left as string instead of number)
**externalIDs** | Array of objects containing ID values for 3rd party services. Objects follow standard key/value format but could have custom attributes added. | Array
external.:serviceid: | Contains string with ID for external service. Service is known by the key, standardization of 3rd party services will be available later. | object | skype:"acafourek"
**organizations** | Array of objects representing employment, volunteer or involvement history | Array
organization.name | Text name of company/organization | string | Alumni Spaces
organization.orgType | Category of affiliation with this organization. Could be multiple values | Array | Employer,Volunteer
organization.startDate | ISO 8601 date of beginning of affiliation | string
organization.endDate | ISO 8601 date of end of affiliation | string
organization.title | Applicable title to describe this person's role in the organization | string | Chief Technical Officer
organization.prevTitles | Titles that have previously been assigned to the organization.title value | Array | Jr. Javascript Developer, Sr. Technical Manager, Director of Engineering
organization.linkedinID | LinkedIn ID of this organization | string
**urls** | Array of objects in the familiar label/value format | Array
url.label | Textual label for this url field | string
url.value | Valid url | string
**groups** | Alumni Spaces groups this person is affiliated with | Object
groups.:groupid: | Key is Group ID in the Alumni Spaces system, Value is readable name | String | mizzou_34:"New York Mizzou Alumns"
**gender** | Person's identified gender | String
**birthday** | ISO 8601 Date of Person's birthday | String
**contentIDs** | Array of strings- if a Person creates a Transaction via Alumni.Fund, the contentID affiliated with that Alumni.Fund flow is saved here. | Array
**asSiteIDs** | Array of strings- if a Person creates a Transaction via Alumni.Fund, the recipient group's unique ID is added to this Array | Array

## Get a Person

You can fetch the record of an individual by using a `GET` request accompanied by the Person's id number.

### HTTP Request for Person

`GET https://vault.alumnispaces.com/person/{id}`

### URL Parameters for Person

Parameter | Description
--------- | -----------
ID | The ID of the Person to retrieve

## Create a Person

> Response when creating a Person

```
	{
	  "_id": "5653b26dd75eafac08185b3e"
	}
```

Creating a person simply requires making a POST to the /person endpoint with JSON-formatted profile data. When successfully created, an ID is returned.

<aside class="notice">
When POSTing, make sure you have set the `Content-Type` header to `application/json`
</aside>
