# People

> Example Person Record

```
{
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
      "address": "tlannister@meereen.gov"
    },
    {
      "label": "personal",
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
  "gender": "male",
  "birthday": "01.01.1971",
  "twitter": {
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
| **id** | Unique identifier for this person | string | 
**name** | Object containing Person's name | object | 
first | Person's first name | string | 
last | Person's last name | string | 
**creationDate** | Date this Person's record was first created | string as ISO8601 Date  | 
**updateDate** | Date this Person's record was last updated | string as ISO8601 Date | 
**address** | Array of objects containing address records | array|object | 
address.label | Text label for this address record | string | "Home", "Office","Summer home"
address.validStart | Beginning of date range for which this is/was a valid address | string as ISO8601 Date
address.validEnd | End of date range for which this is/was a valid address | string as ISO8601 Date
address.addr1 | First line of address | string | 181 York St
address.addr2 | Second line of address | string | Suite 5B
address.city | Town/City name | string | Brooklyn
address.region | Region/State name | string | New York
address.country | Country name | string as ISO 3166-1 Alpha-2 abbreviation | US
address.postal | postal/zipcode | string | 11201
**facebook** | Object containing identifying info for facebook | object
facebook.identityToken | Token saved if user has authenticated with the Alumni Spaces Facebook App. **Note:**  Not returned to public apps. | string
facebook.displayName | Name user has chosen to display on Facebook | string
facebook.ID | Unique numerical id for facebook profile | number
**twitter** | Object containing twitter profile data | object
twitter.username | Twitter id without @ symbol | string | acafourek
**linkedin** | Object containing LinkedIn profile data | object
linkedin.username | Unique username on LinkedIn | string | andrew.cafourek
**email**  | Array of objects containing email data | array
email.label | Text label for email field | string | Personal
email.address | Email address | string | andrew@gmail.com
**phone** | Array of objects containing phone numbers | array
phone.label | Text label for email field | string | Cell
phone.number | Phone number, stripped of non numeric characters | string (intentionally left as string instead of number)
**externalIDs** | Array of objects containing ID values for 3rd party services. Objects follow standard key/value format but could have custom attributes added. | Array
external.:serviceid: | Contains string with ID for external service. Service is known by the key, standardization of 3rd party services will be available later. | object | skype:"acafourek"
**organization** | Array of objects representing employment, volunteer or involvement history | array
organization.name | Text name of company/organization | string | Alumni Spaces
organization.orgType | Category of affiliation with this organization. Could be multiple values | array | Employer,Volunteer
organization.startDate | ISO 8601 date of beginning of affiliation | string
organization.endDate | ISO 8601 date of end of affiliation | string
organization.title | Applicable title to describe this person's role in the organization | string | Chief Technical Officer
organization.prevTitles | Titles that have previously been assigned to the organization.title value | array | Jr. Javascript Developer, Sr. Technical Manager, Director of Engineering
organization.linkedinID | LinkedIn ID of this organization | string
**urls** | Array of objects in the familiar label/value format | Array
url.label | Textual label for this url field | string
url.value | Valid url | string
**groups** | Alumni Spaces groups this person is affiliated with | Object
groups.:groupid: | Key is Group ID in the Alumni Spaces system, Value is readable name | String | mizzou_34:"New York Mizzou Alumns"
**gender** | Person's gender | String
**birthday** | ISO 8601 Date of Person's birthday | String


## Get a Person

You can fetch the record of an individual by using a `GET` request accompanied by the user's id number.

### HTTP Request

`GET https://vault.alumnispaces.com/person/{id}`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the Person to retrieve

## Create a Person

Creating a person simply requires making a POST to the /person endpoint with JSON-formatted profile data. When successfully created, an ID is returned.

<aside class="notice">
When POSTing, make sure you have set the `Content-Type` header to `application/json`
</aside>

> Response when creating a Person

```
	{
	  "_id": "5653b26dd75eafac08185b3e"
	}
```
