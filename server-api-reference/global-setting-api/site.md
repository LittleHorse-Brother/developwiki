# Site

You need the `Manage Site` permission to manage site.

* `GET /api/v3/globalSettings/site` - [Get profile of a site](broken-reference)
* `PUT /api/v3/globalSettings/site` - [Update profile of a site](broken-reference)

## Site Related Object JSON format

### Site Object

Each Comm100 account is treated as a Site and has a unique Site Id.

| Name              | Type    | Include | Read-only | Mandatory |        Default        | Description                                                                                                                                                                      |
| ----------------- | ------- | :-----: | :-------: | :-------: | :-------------------: | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `id`              | integer |         |    yes    |     no    |                       | Site identifier.                                                                                                                                                                 |
| `dateTimeFormat`  | string  |         |     no    |     no    | 'MM-dd-yyyy HH:mm:ss' | Date & Time format of site, value options include : MM-dd-yyy HH:mm:ss, MM/dd/yyyy HH:mm:ss, dd-MM-yyyy HH:mm:ss, dd/MM/yyyy HH:mm:ss, yyyy-MM-dd HH:mm:ss, yyyy/MM/dd HH:mm:ss. |
| `timeZone`        | string  |         |     no    |    yes    |                       | Time zone of site. Value include all [Time Zone Option](broken-reference) identifers.                                                                                            |
| `company`         | string  |         |     no    |    yes    |                       | Company name.                                                                                                                                                                    |
| `companySize`     | string  |         |     no    |     no    |                       | The number of staff of the company, value options include: 1-20, 21-50, 51-100, 101-180, 181-310, 311-600, Above 600.                                                            |
| `website`         | string  |         |     no    |    yes    |                       | Company website.                                                                                                                                                                 |
| `registeredEmail` | string  |         |    yes    |     no    |                       | Email used for site registration.                                                                                                                                                |
| `phone`           | string  |         |     no    |     no    |                       | Company phone number.                                                                                                                                                            |
| `fax`             | string  |         |     no    |     no    |                       | Company fax number.                                                                                                                                                              |
| `mailingAddress`  | string  |         |     no    |     no    |                       | The mailing address of the company.                                                                                                                                              |
| `city`            | string  |         |     no    |     no    |                       | City where the company located.                                                                                                                                                  |
| `stateOrProvince` | string  |         |     no    |     no    |                       | State/Province where the company located.                                                                                                                                        |
| `countryOrRegion` | string  |         |     no    |     no    |                       | Country/Region where the company located.                                                                                                                                        |
| `postalOrZipCode` | string  |         |     no    |     no    |                       | Postal/Zip Code where the company located.                                                                                                                                       |
| `firstName`       | string  |         |     no    |    yes    |                       | First Name of site registrant.                                                                                                                                                   |
| `lastName`        | string  |         |     no    |    yes    |                       | Last Name of site registrant.                                                                                                                                                    |

## Site Endpoints

### Get profile of a site

`GET /api/v3/globalSettings/site`

#### Parameters

```
No parameters
```

#### Response

The response is [Site](broken-reference) Object, just include base informations.

#### Example

Using curl

```
curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/site
```

Response

```json
HTTP/1.1 200 OK
Content-Type:  application/json
{
    "id": 10000,
    "dateTimeFormat":"yyyy-MM-dd hh:mm:ss",
    "timeZone":"canadaCentralStandardTime",
    "company":"BMW",
    "companySize": "Above 600",
    "website":"www.bmw.com",
    "registeredEmail":"bmw@gmail.com",
    "phone":"88654987",
    "fax":"58469215",
    "mailingAddress":"mail@bmw.com",
    "city":"Berlin",
    "stateOrProvince":"",
    "countryOrRegion":"Germany",
    "postalOrZipCode":"10115–14199",
    "firstName":"Jasn",
    "lastName":"Statham",
}
```

### Update profile of a site

`PUT /api/v3/globalSettings/site`

#### Parameters

Request body

The request body contains data with the [Site](broken-reference) structure.

#### Response

The response is the [Site](broken-reference) Object.

#### Example

Using curl

```
curl -H "Content-Type: application/json" -d '{
    "dateTimeFormat"："yyyy-MM-dd hh:mm:ss"，
    "timeZone"："canadaCentralStandardTime"，
    "company"："BMW"，
    "companySize": "Above 600",
    "website"："www.bwm.com"，
    "registeredEmail"："bmw@gmail.com"，
    "phone"："88654987"，
    "fax"："58469215"，
    "mailingAddress"："mail@bmw.com"，
    "city"："Berlin"，
    "stateOrProvince"：""，
    "countryOrRegion"："Germany"，
    "postalOrZipCode"："10115–14199"，
    "firstName"："Jasn"，
    "lastName"："Statham"，
    ...,
}' -X PUT https://api1.comm100.io/api/v3/globalSettings/site
```

Response

```json
HTTP/1.1 200 OK
Content-Type:  application/json
{
    "id": 10000,
    "dateTimeFormat": "yyyy-MM-dd hh:mm:ss",
    "timeZone": "canadaCentralStandardTime",
    "company": "BMW",
    "companySize": "Above 600",
    "website": "www.bwm.com",
    "registeredEmail": "bmw@gmail.com",
    "phone": "88654987",
    "fax": "58469215",
    "mailingAddress": "mail@bmw.com",
    "city": "Berlin",
    "stateOrProvince": "",
    "countryOrRegion": "Germany",
    "postalOrZipCode": "10115–14199",
    "firstName": "Jasn",
    "lastName": "Statham",
}
```
