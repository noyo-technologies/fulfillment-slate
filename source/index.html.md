---
title: Noyo Fulfillment API
language_tabs:
  - shell: cURL
toc_footers: []
includes: []
search: true
highlight_theme: darkula
headingLevel: 2

---

<h1 id="noyo-fulfillment-api">Noyo Fulfillment API v1.0.0</h1>

> Scroll down for code samples, example requests and responses. Select a language for code samples from the tabs above or the mobile navigation menu.

Welcome to Noyo's API! Noyo's API enables you to execute, track, and confirm the fulfillment of member transaction requests to carriers.

This guide intends to help you noyo (see what we did there?) way around carrier fulfillment.

The following transaction types are currently supported:

- New Hire Enrollment
- Termination
- Demographic Changes
- Open Enrollment (Renewal)
- Qualifying Life Event

## Authentication

Each customer account has a unique pair of API credentials. Contact Noyo support to receive an `CLIENT_ID`, `CLIENT_SECRET` pair as part the initial onboarding.

### Getting a Token
In order to make API calls to Noyo's API you just generate a temporary access token (valid for 10 minutes) by calling <https://accounts.noyoconnect.com/auth/public/token>
using your `CLIENT_ID` and `CLIENT_SECRET` as the username/password combination in a Basic Authentication header.

```bash
curl -X POST
 --header "Content-Type: application/json"
 --header "Authorization: Basic <Base64Encode(<CLIENT_ID>:<CLIENT_SECRET>)>"
 --data '{"grant_type": "client_credentials"}'
 https://accounts.noyoconnect.com/auth/public/token

# Responds with:
{
  "access_token": "<ACCESS_TOKEN>",
  "expires_in": 864000,
  "token_type": "Bearer"
}
```

### Calling the API

After generating the token using the instructions above, include the `ACCESS_TOKEN` in the Authorization header for each request: `Authorization: Bearer <ACCESS_TOKEN>`.

## Sandbox

Noyo provides a sandbox environment for the Fulfillment API to aid the development process. The API of the sandbox is identical to that of the live Fulfillment API. However,
it does not carry out your Member Requests in the carrier's system. This will allow you to develop and test your Noyo integration before going live. You can use your sandbox API
credentials to generate an access token from <https://accounts.noyoconnect.com/auth/public/token>. Just follow the instructions above!

After you have generated your access token you can access the sandbox API by hitting:

<https://fulfillment-sandbox.noyoconnect.com>

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="noyo-fulfillment-api-group-configuration">Group Configuration</h1>

## Get a list of all Groups

<a id="opIdgetGroupList"></a>

> Code samples

```shell
# You can also use wget
curl -X GET /api/v1/groups \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer {access-token}'

```

`GET /api/v1/groups`

Returns a list of all groups in the current user organization.

<h3 id="get-a-list-of-all-groups-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|page_size|query|integer|false|none|
|offset|query|integer|false|none|

> Example responses

> 200 Response

```json
[
  {
    "id": "2613a221-d3c8-4c8a-86d5-e7e885fd1da9",
    "name": "Test Company",
    "organization_id": "d61fa455-adf4-4dc4-8c57-6d779ba9475e",
    "sic_code": "7371",
    "version": "6b72d72d-cc99-4df6-8152-7183e824c80c"
  }
]
```

<h3 id="get-a-list-of-all-groups-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successful Response - Returns all matching Groups|Inline|

<h3 id="get-a-list-of-all-groups-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[[GroupResult](#schemagroupresult)]|false|none|none|
|» created|string|true|read-only|The date the record was originated|
|» id|string(uuid)|true|read-only|Unique identifier of the group in Noyo|
|» modified|string|true|read-only|The date the record was last updated|
|» name|string|true|none|Name of the group|
|» organization_id|string(uuid)|true|none|Unique identifier of the platform or broker organization in Noyo|
|» sic_code|string|false|none|SIC Code of the group|
|» version|string(uuid)|true|read-only|Version of the group record|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
Noyo
</aside>

## Get a single Group

<a id="opIdgetGroup"></a>

> Code samples

```shell
# You can also use wget
curl -X GET /api/v1/groups/{group_id} \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer {access-token}'

```

`GET /api/v1/groups/{group_id}`

Returns the latest version of a single group based on the ID provided.

<h3 id="get-a-single-group-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|group_id|path|string|true|none|

> Example responses

> 200 Response

```json
{
  "id": "2613a221-d3c8-4c8a-86d5-e7e885fd1da9",
  "name": "Test Company",
  "organization_id": "d61fa455-adf4-4dc4-8c57-6d779ba9475e",
  "sic_code": "7371",
  "version": "6b72d72d-cc99-4df6-8152-7183e824c80c"
}
```

<h3 id="get-a-single-group-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successful Response - Returns a single Group|[GroupResult](#schemagroupresult)|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
Noyo
</aside>

## Get a list of all Employees

<a id="opIdgetEmployeeList"></a>

> Code samples

```shell
# You can also use wget
curl -X GET /api/v1/groups/{group_id}/employees \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer {access-token}'

```

`GET /api/v1/groups/{group_id}/employees`

Returns a list of all employees for a given group.

<h3 id="get-a-list-of-all-employees-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|group_id|path|string|true|none|
|page_size|query|integer|false|none|
|offset|query|integer|false|none|

> Example responses

> 200 Response

```json
[
  {
    "employment": {
      "employment_dates": {
        "full_time_start": "2015-01-01",
        "hire_date": "2014-12-10"
      },
      "employment_status": "full-time",
      "hours_worked": 50,
      "occupation": "Senior Analyst",
      "salary": {
        "amount": 55000,
        "type": "salary",
        "unit": "annual"
      }
    },
    "group_id": "2613a221-d3c8-4c8a-86d5-e7e885fd1da9",
    "id": "30b74a44-d5b1-4123-a7a4-6d3aec251ba4",
    "location_id": "bd957820-4652-4ee7-8a3a-6413df21e28d",
    "person": {
      "contact": {
        "email_address": "david@testemail.com",
        "email_address_type": "home",
        "home_phone": "+12065551234",
        "preferred_method": "email",
        "work_phone": "+12065559876"
      },
      "date_of_birth": "1981-06-22",
      "first_name": "David",
      "home_address": {
        "city": "San Francisco",
        "county": "San Francisco",
        "state": "CA",
        "street_one": "1234 Home Ave",
        "zip_code": "94107"
      },
      "last_name": "Johnson",
      "marital_status": "married",
      "sex": "M",
      "ssn": "123456789"
    },
    "version": "6260146a-3486-4c3d-b2a6-fcd3f8c84043"
  }
]
```

<h3 id="get-a-list-of-all-employees-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successful Response - Returns all matching Employees|Inline|

<h3 id="get-a-list-of-all-employees-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[[EmployeeResult](#schemaemployeeresult)]|false|none|none|
|» created|string|true|read-only|The date the record was originated|
|» employment|object|true|none|Employment information for the employee|
|»» employment_dates|object|true|none|Employee employment dates|
|»»» full_time_start|string|false|none|ISO-8601 date string for start date as a full time employee with the group (if applicable)|
|»»» hire_date|string(date)|true|none|ISO-8601 date string for the hire date of the employee|
|»»» rehire|string|false|none|ISO-8601 date string for the rehire date of the employee (if applicable)|
|»»» retirement|string|false|none|ISO-8601 date string for the retirement date of the employee (if appliable)|
|»»» terminated|string|false|none|ISO-8601 date string for the termination date of the employee (if applicable)|
|»» employment_status|string|true|none|Employee employment status|
|»» hours_worked|integer(int32)|true|none|Number of hours worked per week by the employee|
|»» occupation|string|false|none|Employee occupation or job title|
|»» salary|object|false|none|Employee salary information|
|»»» amount|number|true|none|Amount of salary earned by the employee in US dollars|
|»»» type|string|true|none|Type of salary earned by the employee|
|»»» unit|string|true|none|Unit of salary earned by the employee|
|»» group_id|string(uuid)|true|read-only|Unique identifier of the group in Noyo|
|»» id|string(uuid)|true|read-only|Unique identifier of the employee in Noyo|
|»» location_id|string(uuid)|true|none|Unique identifier of the employee group location in Noyo|
|»» modified|string|true|read-only|The date the record was last updated|
|»» person|[Person](#schemaperson)|true|none|none|
|»»» contact|object|true|none|Contact information for the person|
|»»»» email_address|string(email)|true|none|Email address of the person|
|»»»» email_address_type|string|false|none|Type of email address|
|»»»» home_phone|string|true|none|Home phone number in E.164 format|
|»»»» preferred_language|string|false|none|Preferred written or spoken language of the person|
|»»»» preferred_method|string|false|none|Preferred method of contact for the person|
|»»»» speaks_english|boolean|false|none|True if the person can speak or communicate in English|
|»»»» work_phone|string|false|none|Work phone number in E.164 format|
|»»» date_of_birth|string(date)|true|none|ISO-8601 date string for the date of birth of the person|
|»»» details|object|false|none|Additional personal details of the person|
|»»»» american_indian|object|false|none|American Indian status details (if applicable)|
|»»»»» state|string|false|none|Primary state of the federally-recognized American Indian or Alaska Native tribe, if applicable|
|»»»»» tribe|string|false|none|Name of the federally-recognized American Indian or Alaska Native tribe, if applicable|
|»»»» disability|object|false|none|Disability details (if applicable)|
|»»»»» communication|boolean|false|none|True if the disability impacts the ability to communicate or read|
|»»»»» reason|string|false|none|Description of the disability|
|»»»» is_military|boolean|false|none|True if the person is in the military|
|»»»» is_student|boolean|false|none|True if the person is a student|
|»»»» tobacco|object|false|none|Tobacco usage details (if applicable)|
|»»»»» duration|string|false|none|Duration of tobacco use|
|»»»»» frequency|string|false|none|Frequency of tobacco use|
|»»»»» types|[string]|false|none|Type of tobacco use|
|»»»» first_name|string|true|none|First name of the person|
|»»»» home_address|[Address](#schemaaddress)|true|none|none|
|»»»»» city|string|true|none|City of the address|
|»»»»» county|string|false|none|County of the address|
|»»»»» state|string|true|none|State postal code of the address|
|»»»»» street_one|string|true|none|Line one of the address|
|»»»»» street_two|string|false|none|Line two of the address|
|»»»»» zip_code|string|true|none|Zip code of the address|
|»»»» last_name|string|true|none|Last name of the person|
|»»»» maiden_name|string|false|none|Maiden name of the person (if applicable)|
|»»»» mailing_address|[Address](#schemaaddress)|false|none|none|
|»»»» marital_status|string|false|none|Marital status of the person|
|»»»» middle_name|string|false|none|Middle name of the person|
|»»»» sex|string|true|none|Sex of the person|
|»»»» ssn|string|false|none|Social Security Number of the person|
|»»»» suffix|string|false|none|Suffix of the person|
|»»» version|string(uuid)|true|read-only|Version of the employee record|

#### Enumerated Values

|Property|Value|
|---|---|
|employment_status|full-time|
|employment_status|part-time|
|employment_status|contract|
|employment_status|disabled|
|employment_status|terminated|
|employment_status|retired|
|type|hourly|
|type|salary|
|unit|annual|
|unit|month|
|unit|week|
|unit|hour|
|email_address_type|home|
|email_address_type|work|
|preferred_method|mail|
|preferred_method|email|
|preferred_method|home-phone|
|preferred_method|work-phone|
|preferred_method|other|
|state|AL|
|state|AK|
|state|AZ|
|state|AR|
|state|CA|
|state|CO|
|state|CT|
|state|DC|
|state|DE|
|state|FL|
|state|GA|
|state|HI|
|state|ID|
|state|IL|
|state|IN|
|state|IA|
|state|KS|
|state|KY|
|state|LA|
|state|ME|
|state|MD|
|state|MA|
|state|MI|
|state|MN|
|state|MS|
|state|MO|
|state|MT|
|state|NE|
|state|NV|
|state|NH|
|state|NJ|
|state|NM|
|state|NY|
|state|NC|
|state|ND|
|state|OH|
|state|OK|
|state|OR|
|state|PA|
|state|RI|
|state|SC|
|state|SD|
|state|TN|
|state|TX|
|state|UT|
|state|VT|
|state|VA|
|state|WA|
|state|WV|
|state|WI|
|state|WY|
|state|AL|
|state|AK|
|state|AZ|
|state|AR|
|state|CA|
|state|CO|
|state|CT|
|state|DC|
|state|DE|
|state|FL|
|state|GA|
|state|HI|
|state|ID|
|state|IL|
|state|IN|
|state|IA|
|state|KS|
|state|KY|
|state|LA|
|state|ME|
|state|MD|
|state|MA|
|state|MI|
|state|MN|
|state|MS|
|state|MO|
|state|MT|
|state|NE|
|state|NV|
|state|NH|
|state|NJ|
|state|NM|
|state|NY|
|state|NC|
|state|ND|
|state|OH|
|state|OK|
|state|OR|
|state|PA|
|state|RI|
|state|SC|
|state|SD|
|state|TN|
|state|TX|
|state|UT|
|state|VT|
|state|VA|
|state|WA|
|state|WV|
|state|WI|
|state|WY|
|marital_status|single|
|marital_status|married|
|marital_status|domestic-partner|
|marital_status|divorced|
|marital_status|widowed|
|marital_status|legally-separated|
|sex|F|
|sex|M|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
Noyo
</aside>

## Get a single Employee

<a id="opIdgetEmployee"></a>

> Code samples

```shell
# You can also use wget
curl -X GET /api/v1/employees/{employee_id} \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer {access-token}'

```

`GET /api/v1/employees/{employee_id}`

Returns the latest version of a single employee based on the ID provided.

<h3 id="get-a-single-employee-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|employee_id|path|string|true|none|

> Example responses

> 200 Response

```json
{
  "employment": {
    "employment_dates": {
      "full_time_start": "2015-01-01",
      "hire_date": "2014-12-10"
    },
    "employment_status": "full-time",
    "hours_worked": 50,
    "occupation": "Senior Analyst",
    "salary": {
      "amount": 55000,
      "type": "salary",
      "unit": "annual"
    }
  },
  "group_id": "2613a221-d3c8-4c8a-86d5-e7e885fd1da9",
  "id": "30b74a44-d5b1-4123-a7a4-6d3aec251ba4",
  "location_id": "bd957820-4652-4ee7-8a3a-6413df21e28d",
  "person": {
    "contact": {
      "email_address": "david@testemail.com",
      "email_address_type": "home",
      "home_phone": "+12065551234",
      "preferred_method": "email",
      "work_phone": "+12065559876"
    },
    "date_of_birth": "1981-06-22",
    "first_name": "David",
    "home_address": {
      "city": "San Francisco",
      "county": "San Francisco",
      "state": "CA",
      "street_one": "1234 Home Ave",
      "zip_code": "94107"
    },
    "last_name": "Johnson",
    "marital_status": "married",
    "sex": "M",
    "ssn": "123456789"
  },
  "version": "6260146a-3486-4c3d-b2a6-fcd3f8c84043"
}
```

<h3 id="get-a-single-employee-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successful Response - Returns a single Employee|[EmployeeResult](#schemaemployeeresult)|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
Noyo
</aside>

## Get a list of all Dependents

<a id="opIdgetDependentList"></a>

> Code samples

```shell
# You can also use wget
curl -X GET /api/v1/employees/{employee_id}/dependents \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer {access-token}'

```

`GET /api/v1/employees/{employee_id}/dependents`

Returns a list of all dependents for a given employee.

<h3 id="get-a-list-of-all-dependents-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|employee_id|path|string|true|none|
|page_size|query|integer|false|none|
|offset|query|integer|false|none|

> Example responses

> 200 Response

```json
[
  {
    "employee_id": "30b74a44-d5b1-4123-a7a4-6d3aec251ba4",
    "id": "fd62665c-0846-4e9d-bd29-80779b5f685c",
    "person": {
      "contact": {
        "email_address": "diane@testemail.com",
        "email_address_type": "home",
        "home_phone": "+12065551234"
      },
      "date_of_birth": "1982-01-02",
      "first_name": "Diane",
      "home_address": {
        "city": "San Francisco",
        "county": "San Francisco",
        "state": "CA",
        "street_one": "1234 Home Ave",
        "zip_code": "94107"
      },
      "last_name": "Johnson",
      "marital_status": "married",
      "sex": "F",
      "ssn": "987654321"
    },
    "relationship": "child",
    "version": "669fbe16-9960-4069-91aa-832ada3f4e42"
  }
]
```

<h3 id="get-a-list-of-all-dependents-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successful Response - Returns all matching Dependents|Inline|

<h3 id="get-a-list-of-all-dependents-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[[DependentResult](#schemadependentresult)]|false|none|none|
|» created|string|true|read-only|The date the record was originated|
|» employee_id|string(uuid)|true|read-only|Unique identifier of the employee in Noyo|
|» id|string(uuid)|true|read-only|Unique identifier of the dependent in Noyo|
|» modified|string|true|read-only|The date the record was last updated|
|» person|[Person](#schemaperson)|true|none|none|
|»» contact|object|true|none|Contact information for the person|
|»»» email_address|string(email)|true|none|Email address of the person|
|»»» email_address_type|string|false|none|Type of email address|
|»»» home_phone|string|true|none|Home phone number in E.164 format|
|»»» preferred_language|string|false|none|Preferred written or spoken language of the person|
|»»» preferred_method|string|false|none|Preferred method of contact for the person|
|»»» speaks_english|boolean|false|none|True if the person can speak or communicate in English|
|»»» work_phone|string|false|none|Work phone number in E.164 format|
|»» date_of_birth|string(date)|true|none|ISO-8601 date string for the date of birth of the person|
|»» details|object|false|none|Additional personal details of the person|
|»»» american_indian|object|false|none|American Indian status details (if applicable)|
|»»»» state|string|false|none|Primary state of the federally-recognized American Indian or Alaska Native tribe, if applicable|
|»»»» tribe|string|false|none|Name of the federally-recognized American Indian or Alaska Native tribe, if applicable|
|»»» disability|object|false|none|Disability details (if applicable)|
|»»»» communication|boolean|false|none|True if the disability impacts the ability to communicate or read|
|»»»» reason|string|false|none|Description of the disability|
|»»» is_military|boolean|false|none|True if the person is in the military|
|»»» is_student|boolean|false|none|True if the person is a student|
|»»» tobacco|object|false|none|Tobacco usage details (if applicable)|
|»»»» duration|string|false|none|Duration of tobacco use|
|»»»» frequency|string|false|none|Frequency of tobacco use|
|»»»» types|[string]|false|none|Type of tobacco use|
|»»» first_name|string|true|none|First name of the person|
|»»» home_address|[Address](#schemaaddress)|true|none|none|
|»»»» city|string|true|none|City of the address|
|»»»» county|string|false|none|County of the address|
|»»»» state|string|true|none|State postal code of the address|
|»»»» street_one|string|true|none|Line one of the address|
|»»»» street_two|string|false|none|Line two of the address|
|»»»» zip_code|string|true|none|Zip code of the address|
|»»» last_name|string|true|none|Last name of the person|
|»»» maiden_name|string|false|none|Maiden name of the person (if applicable)|
|»»» mailing_address|[Address](#schemaaddress)|false|none|none|
|»»» marital_status|string|false|none|Marital status of the person|
|»»» middle_name|string|false|none|Middle name of the person|
|»»» sex|string|true|none|Sex of the person|
|»»» ssn|string|false|none|Social Security Number of the person|
|»»» suffix|string|false|none|Suffix of the person|
|»» relationship|string|true|none|Relationship of the dependent to the employee|
|»» version|string(uuid)|true|read-only|Version of the dependent record|

#### Enumerated Values

|Property|Value|
|---|---|
|email_address_type|home|
|email_address_type|work|
|preferred_method|mail|
|preferred_method|email|
|preferred_method|home-phone|
|preferred_method|work-phone|
|preferred_method|other|
|state|AL|
|state|AK|
|state|AZ|
|state|AR|
|state|CA|
|state|CO|
|state|CT|
|state|DC|
|state|DE|
|state|FL|
|state|GA|
|state|HI|
|state|ID|
|state|IL|
|state|IN|
|state|IA|
|state|KS|
|state|KY|
|state|LA|
|state|ME|
|state|MD|
|state|MA|
|state|MI|
|state|MN|
|state|MS|
|state|MO|
|state|MT|
|state|NE|
|state|NV|
|state|NH|
|state|NJ|
|state|NM|
|state|NY|
|state|NC|
|state|ND|
|state|OH|
|state|OK|
|state|OR|
|state|PA|
|state|RI|
|state|SC|
|state|SD|
|state|TN|
|state|TX|
|state|UT|
|state|VT|
|state|VA|
|state|WA|
|state|WV|
|state|WI|
|state|WY|
|state|AL|
|state|AK|
|state|AZ|
|state|AR|
|state|CA|
|state|CO|
|state|CT|
|state|DC|
|state|DE|
|state|FL|
|state|GA|
|state|HI|
|state|ID|
|state|IL|
|state|IN|
|state|IA|
|state|KS|
|state|KY|
|state|LA|
|state|ME|
|state|MD|
|state|MA|
|state|MI|
|state|MN|
|state|MS|
|state|MO|
|state|MT|
|state|NE|
|state|NV|
|state|NH|
|state|NJ|
|state|NM|
|state|NY|
|state|NC|
|state|ND|
|state|OH|
|state|OK|
|state|OR|
|state|PA|
|state|RI|
|state|SC|
|state|SD|
|state|TN|
|state|TX|
|state|UT|
|state|VT|
|state|VA|
|state|WA|
|state|WV|
|state|WI|
|state|WY|
|marital_status|single|
|marital_status|married|
|marital_status|domestic-partner|
|marital_status|divorced|
|marital_status|widowed|
|marital_status|legally-separated|
|sex|F|
|sex|M|
|relationship|spouse|
|relationship|child|
|relationship|domestic-partner|
|relationship|civil-union|
|relationship|step-child|
|relationship|foster-child|
|relationship|adopted-child|
|relationship|legal-guardianship|
|relationship|ex-spouse|
|relationship|other|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
Noyo
</aside>

## Create a new Dependent

<a id="opIdcreateDependent"></a>

> Code samples

```shell
# You can also use wget
curl -X POST /api/v1/employees/{employee_id}/dependents \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer {access-token}'

```

`POST /api/v1/employees/{employee_id}/dependents`

Create a new dependent for an employee.

> Body parameter

```json
{
  "person": {
    "contact": {
      "email_address": "catherine.briggs@example.com",
      "email_address_type": "home",
      "home_phone": "+15555551212",
      "preferred_language": "Engish",
      "preferred_method": "email",
      "speaks_english": true,
      "work_phone": "+15555551212"
    },
    "date_of_birth": "1977-06-03",
    "details": {
      "american_indian": {
        "state": "AL",
        "tribe": "Ojibwe"
      },
      "disability": {
        "communication": true,
        "reason": "string"
      },
      "is_military": true,
      "is_student": true,
      "tobacco": {
        "duration": "5 years",
        "frequency": "daily",
        "types": "cigarettes"
      }
    },
    "first_name": "Catherine",
    "home_address": {
      "city": "Bend",
      "county": "Deschutes",
      "state": "OR",
      "street_one": "339 Hickory Street",
      "street_two": "Apartment 5",
      "zip_code": "97701"
    },
    "last_name": "Briggs",
    "maiden_name": "Adams",
    "mailing_address": {
      "city": "Bend",
      "county": "Deschutes",
      "state": "OR",
      "street_one": "339 Hickory Street",
      "street_two": "Apartment 5",
      "zip_code": "97701"
    },
    "marital_status": "married",
    "middle_name": "S",
    "sex": "F",
    "ssn": "123456789",
    "suffix": "string"
  },
  "relationship": "spouse"
}
```

<h3 id="create-a-new-dependent-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|employee_id|path|string|true|none|
|body|body|[DependentCreateRequest](#schemadependentcreaterequest)|true|none|

> Example responses

> 201 Response

```json
{
  "employee_id": "30b74a44-d5b1-4123-a7a4-6d3aec251ba4",
  "id": "fd62665c-0846-4e9d-bd29-80779b5f685c",
  "person": {
    "contact": {
      "email_address": "diane@testemail.com",
      "email_address_type": "home",
      "home_phone": "+12065551234"
    },
    "date_of_birth": "1982-01-02",
    "first_name": "Diane",
    "home_address": {
      "city": "San Francisco",
      "county": "San Francisco",
      "state": "CA",
      "street_one": "1234 Home Ave",
      "zip_code": "94107"
    },
    "last_name": "Johnson",
    "marital_status": "married",
    "sex": "F",
    "ssn": "987654321"
  },
  "relationship": "child",
  "version": "669fbe16-9960-4069-91aa-832ada3f4e42"
}
```

<h3 id="create-a-new-dependent-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Successful Response - Returns the new Dependent|[DependentResult](#schemadependentresult)|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
Noyo
</aside>

## Get a single Dependent

<a id="opIdgetDependent"></a>

> Code samples

```shell
# You can also use wget
curl -X GET /api/v1/employees/{employee_id}/dependents/{dependent_id} \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer {access-token}'

```

`GET /api/v1/employees/{employee_id}/dependents/{dependent_id}`

Returns the latest version of a single dependent based on the ID provided.

<h3 id="get-a-single-dependent-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|employee_id|path|string|true|none|
|dependent_id|path|string|true|none|

> Example responses

> 200 Response

```json
{
  "employee_id": "30b74a44-d5b1-4123-a7a4-6d3aec251ba4",
  "id": "fd62665c-0846-4e9d-bd29-80779b5f685c",
  "person": {
    "contact": {
      "email_address": "diane@testemail.com",
      "email_address_type": "home",
      "home_phone": "+12065551234"
    },
    "date_of_birth": "1982-01-02",
    "first_name": "Diane",
    "home_address": {
      "city": "San Francisco",
      "county": "San Francisco",
      "state": "CA",
      "street_one": "1234 Home Ave",
      "zip_code": "94107"
    },
    "last_name": "Johnson",
    "marital_status": "married",
    "sex": "F",
    "ssn": "987654321"
  },
  "relationship": "child",
  "version": "669fbe16-9960-4069-91aa-832ada3f4e42"
}
```

<h3 id="get-a-single-dependent-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successful Response - Returns a single Dependent|[DependentResult](#schemadependentresult)|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
Noyo
</aside>

## Get a list of all Group Contacts

<a id="opIdgetContactList"></a>

> Code samples

```shell
# You can also use wget
curl -X GET /api/v1/groups/{group_id}/contacts \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer {access-token}'

```

`GET /api/v1/groups/{group_id}/contacts`

Returns a list of all group contacts for a given group.

<h3 id="get-a-list-of-all-group-contacts-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|group_id|path|string|true|none|
|page_size|query|integer|false|none|
|offset|query|integer|false|none|

> Example responses

> 200 Response

```json
[
  {
    "email": "davidsmith@testcorp.com",
    "first_name": "David",
    "group_id": "2613a221-d3c8-4c8a-86d5-e7e885fd1da9",
    "id": "c2f5489d-36d1-4fb0-bf9f-105ad5dd0b3e",
    "last_name": "Smith",
    "phone_number": "+14255551234",
    "primary_contact": true,
    "title": "Chief People Officer",
    "types": [
      "company",
      "billing"
    ],
    "version": "6ba43c6e-69f2-435f-a126-8595a798eb52"
  },
  {
    "email": "carolinejohnson@testcorp.com",
    "first_name": "Caroline",
    "group_id": "2613a221-d3c8-4c8a-86d5-e7e885fd1da9",
    "id": "7e3c1f14-e7e7-42b8-81d8-3fcd7bd7d418",
    "last_name": "Johnson",
    "phone_number": "+14255551234",
    "primary_contact": false,
    "title": "VP of Human Resources",
    "types": [
      "executive"
    ],
    "version": "88bfbe28-7f8b-4497-a50b-932ee3387b68"
  }
]
```

<h3 id="get-a-list-of-all-group-contacts-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successful Response - Returns all matching Group Contacts|Inline|

<h3 id="get-a-list-of-all-group-contacts-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[[ContactResult](#schemacontactresult)]|false|none|none|
|» created|string|true|read-only|The date the record was originated|
|» email|string|false|read-only|Work email address of the group contact|
|» first_name|string|false|read-only|First name of the group contact|
|» group_id|string(uuid)|true|read-only|Unique identifier of the group in Noyo|
|» id|string(uuid)|true|read-only|Unique identifier of the group contact in Noyo|
|» last_name|string|false|read-only|Last name of the group contact|
|» modified|string|true|read-only|The date the record was last updated|
|» phone_number|string|false|read-only|Work phone number of the group contact in E.164 format|
|» primary_contact|boolean|false|read-only|True if contact is the primary contact for the group|
|» title|string|false|read-only|Job title of the group contact|
|» types|[string]|true|none|Group contact types for the group contact|
|» version|string(uuid)|true|read-only|Version of the group contact record|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
Noyo
</aside>

## Get a single Group Contact

<a id="opIdgetContact"></a>

> Code samples

```shell
# You can also use wget
curl -X GET /api/v1/groups/{group_id}/contacts/{contact_id} \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer {access-token}'

```

`GET /api/v1/groups/{group_id}/contacts/{contact_id}`

Returns the latest version of a single group contact based on the ID provided.

<h3 id="get-a-single-group-contact-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|group_id|path|string|true|none|
|contact_id|path|string|true|none|

> Example responses

> 200 Response

```json
{
  "email": "davidsmith@testcorp.com",
  "first_name": "David",
  "group_id": "2613a221-d3c8-4c8a-86d5-e7e885fd1da9",
  "id": "c2f5489d-36d1-4fb0-bf9f-105ad5dd0b3e",
  "last_name": "Smith",
  "phone_number": "+14255551234",
  "primary_contact": true,
  "title": "Chief People Officer",
  "types": [
    "company",
    "billing",
    "executive"
  ],
  "version": "ef3fef8d-4889-40f4-a0f8-377b9dd23d98"
}
```

<h3 id="get-a-single-group-contact-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successful Response - Returns a single Group Contact|[ContactResult](#schemacontactresult)|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
Noyo
</aside>

## Get a list of all Group Location

<a id="opIdgetLocationList"></a>

> Code samples

```shell
# You can also use wget
curl -X GET /api/v1/groups/{group_id}/locations \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer {access-token}'

```

`GET /api/v1/groups/{group_id}/locations`

Returns a list of all group locations for a given group.

<h3 id="get-a-list-of-all-group-location-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|group_id|path|string|true|none|
|page_size|query|integer|false|none|
|offset|query|integer|false|none|

> Example responses

> 200 Response

```json
[
  {
    "address": {
      "city": "San Francisco",
      "county": "San Francisco",
      "state": "CA",
      "street_one": "1234 Test Ln",
      "zip_code": "94107"
    },
    "group_id": "2613a221-d3c8-4c8a-86d5-e7e885fd1da9",
    "id": "bd957820-4652-4ee7-8a3a-6413df21e28d",
    "version": "f4142cba-f3eb-4917-b3bd-123d683f18d7"
  }
]
```

<h3 id="get-a-list-of-all-group-location-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successful Response - Returns all matching Group Locations|Inline|

<h3 id="get-a-list-of-all-group-location-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[[LocationResult](#schemalocationresult)]|false|none|none|
|» address|[Address](#schemaaddress)|true|none|none|
|»» city|string|true|none|City of the address|
|»» county|string|false|none|County of the address|
|»» state|string|true|none|State postal code of the address|
|»» street_one|string|true|none|Line one of the address|
|»» street_two|string|false|none|Line two of the address|
|»» zip_code|string|true|none|Zip code of the address|
|» created|string|true|read-only|The date the record was originated|
|» display_name|string|true|none|Display name for the group location|
|» group_id|string(uuid)|true|read-only|Unique identifier of the group in Noyo|
|» id|string(uuid)|true|read-only|Unique identifer of the group location in Noyo|
|» modified|string|true|read-only|The date the record was last updated|
|» version|string(uuid)|true|read-only|Version of the group location record|

#### Enumerated Values

|Property|Value|
|---|---|
|state|AL|
|state|AK|
|state|AZ|
|state|AR|
|state|CA|
|state|CO|
|state|CT|
|state|DC|
|state|DE|
|state|FL|
|state|GA|
|state|HI|
|state|ID|
|state|IL|
|state|IN|
|state|IA|
|state|KS|
|state|KY|
|state|LA|
|state|ME|
|state|MD|
|state|MA|
|state|MI|
|state|MN|
|state|MS|
|state|MO|
|state|MT|
|state|NE|
|state|NV|
|state|NH|
|state|NJ|
|state|NM|
|state|NY|
|state|NC|
|state|ND|
|state|OH|
|state|OK|
|state|OR|
|state|PA|
|state|RI|
|state|SC|
|state|SD|
|state|TN|
|state|TX|
|state|UT|
|state|VT|
|state|VA|
|state|WA|
|state|WV|
|state|WI|
|state|WY|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
Noyo
</aside>

## Get a single Group Location

<a id="opIdgetLocation"></a>

> Code samples

```shell
# You can also use wget
curl -X GET /api/v1/groups/{group_id}/locations/{location_id} \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer {access-token}'

```

`GET /api/v1/groups/{group_id}/locations/{location_id}`

Returns the latest version of a single group location based on the ID provided.

<h3 id="get-a-single-group-location-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|group_id|path|string|true|none|
|location_id|path|string|true|none|

> Example responses

> 200 Response

```json
{
  "address": {
    "city": "San Francisco",
    "county": "San Francisco",
    "state": "CA",
    "street_one": "1234 Test Ln",
    "zip_code": "94107"
  },
  "group_id": "2613a221-d3c8-4c8a-86d5-e7e885fd1da9",
  "id": "bd957820-4652-4ee7-8a3a-6413df21e28d",
  "version": "f4142cba-f3eb-4917-b3bd-123d683f18d7"
}
```

<h3 id="get-a-single-group-location-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successful Response - Returns a single Group Location|[LocationResult](#schemalocationresult)|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
Noyo
</aside>

## Create a new Employee

<a id="opIdcreateEmployee"></a>

> Code samples

```shell
# You can also use wget
curl -X POST /api/v1/employees \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer {access-token}'

```

`POST /api/v1/employees`

Create a new employee for a group.

> Body parameter

```json
{
  "employment": {
    "employment_dates": {
      "full_time_start": "2017-01-17",
      "hire_date": "2017-01-17",
      "rehire": "2018-02-01",
      "retirement": "2019-01-01",
      "terminated": "2018-01-01"
    },
    "employment_status": "full-time",
    "hours_worked": 40,
    "occupation": "Engineer",
    "salary": {
      "amount": "55000",
      "type": "salary",
      "unit": "annual"
    }
  },
  "group_id": "string",
  "location_id": "string",
  "person": {
    "contact": {
      "email_address": "catherine.briggs@example.com",
      "email_address_type": "home",
      "home_phone": "+15555551212",
      "preferred_language": "Engish",
      "preferred_method": "email",
      "speaks_english": true,
      "work_phone": "+15555551212"
    },
    "date_of_birth": "1977-06-03",
    "details": {
      "american_indian": {
        "state": "AL",
        "tribe": "Ojibwe"
      },
      "disability": {
        "communication": true,
        "reason": "string"
      },
      "is_military": true,
      "is_student": true,
      "tobacco": {
        "duration": "5 years",
        "frequency": "daily",
        "types": "cigarettes"
      }
    },
    "first_name": "Catherine",
    "home_address": {
      "city": "Bend",
      "county": "Deschutes",
      "state": "OR",
      "street_one": "339 Hickory Street",
      "street_two": "Apartment 5",
      "zip_code": "97701"
    },
    "last_name": "Briggs",
    "maiden_name": "Adams",
    "mailing_address": {
      "city": "Bend",
      "county": "Deschutes",
      "state": "OR",
      "street_one": "339 Hickory Street",
      "street_two": "Apartment 5",
      "zip_code": "97701"
    },
    "marital_status": "married",
    "middle_name": "S",
    "sex": "F",
    "ssn": "123456789",
    "suffix": "string"
  }
}
```

<h3 id="create-a-new-employee-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[EmployeeCreateRequest](#schemaemployeecreaterequest)|true|none|

> Example responses

> 201 Response

```json
{
  "employment": {
    "employment_dates": {
      "full_time_start": "2015-01-01",
      "hire_date": "2014-12-10"
    },
    "employment_status": "full-time",
    "hours_worked": 50,
    "occupation": "Senior Analyst",
    "salary": {
      "amount": 55000,
      "type": "salary",
      "unit": "annual"
    }
  },
  "group_id": "2613a221-d3c8-4c8a-86d5-e7e885fd1da9",
  "id": "30b74a44-d5b1-4123-a7a4-6d3aec251ba4",
  "location_id": "bd957820-4652-4ee7-8a3a-6413df21e28d",
  "person": {
    "contact": {
      "email_address": "david@testemail.com",
      "email_address_type": "home",
      "home_phone": "+12065551234",
      "preferred_method": "email",
      "work_phone": "+12065559876"
    },
    "date_of_birth": "1981-06-22",
    "first_name": "David",
    "home_address": {
      "city": "San Francisco",
      "county": "San Francisco",
      "state": "CA",
      "street_one": "1234 Home Ave",
      "zip_code": "94107"
    },
    "last_name": "Johnson",
    "marital_status": "married",
    "sex": "M",
    "ssn": "123456789"
  },
  "version": "6260146a-3486-4c3d-b2a6-fcd3f8c84043"
}
```

<h3 id="create-a-new-employee-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Successful Response - Returns the new Employee|[EmployeeResult](#schemaemployeeresult)|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
Noyo
</aside>

<h1 id="noyo-fulfillment-api-group-plans-and-enrollment">Group Plans & Enrollment</h1>

## Get a list of all Group Enrollments

<a id="opIdgetGroupEnrollmentList"></a>

> Code samples

```shell
# You can also use wget
curl -X GET /api/v1/groups/{group_id}/group_enrollments \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer {access-token}'

```

`GET /api/v1/groups/{group_id}/group_enrollments`

Returns a list of all group enrollments for a given group.

<h3 id="get-a-list-of-all-group-enrollments-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|group_id|path|string|true|none|
|effective_date|query|string|false|none|
|page_size|query|integer|false|none|
|offset|query|integer|false|none|

> Example responses

> 200 Response

```json
[
  {
    "carrier_id": "8fac0992-933a-4743-8dab-0d606ef2569e",
    "group_id": "2613a221-d3c8-4c8a-86d5-e7e885fd1da9",
    "id": "7499ea07-76c9-40f2-992b-952259e98384",
    "line_of_coverage": "medical",
    "open_enrollment_end_date": "2019-02-01",
    "open_enrollment_start_date": "2019-01-01",
    "status": "active",
    "version": "c2292b67-0382-45e9-a378-b52c4129f672"
  },
  {
    "carrier_id": "8fac0992-933a-4743-8dab-0d606ef2569e",
    "group_id": "2613a221-d3c8-4c8a-86d5-e7e885fd1da9",
    "id": "25cdcb18-7d01-4599-9f65-d38b36d7c370",
    "line_of_coverage": "dental",
    "open_enrollment_end_date": "2019-02-01",
    "open_enrollment_start_date": "2019-01-01",
    "status": "active",
    "version": "55c4bcbb-98ab-448d-a5b4-d7d835327f72"
  }
]
```

<h3 id="get-a-list-of-all-group-enrollments-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successful Response - Returns all matching Group Enrollments|Inline|

<h3 id="get-a-list-of-all-group-enrollments-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[[GroupEnrollmentResult](#schemagroupenrollmentresult)]|false|none|none|
|» carrier_id|string(uuid)|true|read-only|Unique identifier of the carrier in Noyo|
|» created|string|true|read-only|The date the record was originated|
|» group_id|string(uuid)|true|read-only|Unique identifier of the group in Noyo|
|» id|string(uuid)|true|read-only|Unique identifier of the group enrollment in Noyo|
|» line_of_coverage|string|true|read-only|Line of coverage for the group enrollment|
|» modified|string|true|read-only|The date the record was last updated|
|» open_enrollment_end_date|string(date)|false|read-only|ISO-8601 date string for the open enrollment end date of the group enrollment|
|» open_enrollment_start_date|string(date)|false|read-only|ISO-8601 date string for the open enrollment start date of the group enrollment|
|» status|string|true|read-only|Status of the group enrollment|
|» version|string(uuid)|true|read-only|Version of the group enrollment record|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
Noyo
</aside>

<h1 id="noyo-fulfillment-api-individual-enrollments">Individual Enrollments</h1>

## Get a list of all Individual Enrollments

<a id="opIdgetIndividualEnrollmentList"></a>

> Code samples

```shell
# You can also use wget
curl -X GET /api/v1/group_enrollments/{group_enrollment_id}/individual_enrollments \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer {access-token}'

```

`GET /api/v1/group_enrollments/{group_enrollment_id}/individual_enrollments`

Returns a list of all individual enrollments for a given group enrollment.

<h3 id="get-a-list-of-all-individual-enrollments-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|group_enrollment_id|path|string|true|none|
|effective_date|query|string|false|none|
|page_size|query|integer|false|none|
|offset|query|integer|false|none|

> Example responses

> 200 Response

```json
[
  {
    "enroll_data": {
      "premium": {
        "amount": 50,
        "unit": "Month"
      }
    },
    "group_enrollment_id": "7499ea07-76c9-40f2-992b-952259e98384",
    "id": "6670f290-4236-4690-bece-a110f9ad924b",
    "individual_id": "30b74a44-d5b1-4123-a7a4-6d3aec251ba4",
    "individual_type": "employee",
    "line_of_coverage": "medical",
    "plan_id": "b988cd26-121a-4dd6-b0be-09399f6ecc0a",
    "status": "active",
    "version": "27bbf929-78ca-4339-8bb3-39b75f92c4a4"
  }
]
```

<h3 id="get-a-list-of-all-individual-enrollments-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successful Response - Returns all matching Individual Enrollments|Inline|

<h3 id="get-a-list-of-all-individual-enrollments-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[[IndividualEnrollmentResult](#schemaindividualenrollmentresult)]|false|none|none|
|» created|string|true|read-only|The date the record was originated|
|» enroll_data|string|true|read-only|Line of coverage specific enroll data about premiums and volumes|
|» group_enrollment_id|string(uuid)|true|read-only|Unique identifier of the group enrollment in Noyo|
|» id|string(uuid)|true|read-only|Unique identifier of the individual enrollment in Noyo|
|» individual_id|string(uuid)|true|read-only|Unique identifier of the employee or dependent in Noyo|
|» individual_type|string|true|read-only|Type of member for the individual enrollment|
|» line_of_coverage|string|true|read-only|Line of coverage for the individual enrollment|
|» modified|string|true|read-only|The date the record was last updated|
|» plan_id|string(uuid)|true|read-only|Unique identifier of the group plan in Noyo|
|» status|string|true|read-only|Status of the individual enrollment|
|» version|string(uuid)|true|read-only|Version of the individual enrollment record|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
Noyo
</aside>

<h1 id="noyo-fulfillment-api-member-requests">Member Requests</h1>

## Get a list of all Member Requests

<a id="opIdgetMemberRequestListByOrg"></a>

> Code samples

```shell
# You can also use wget
curl -X GET /api/v1/member_requests \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer {access-token}'

```

`GET /api/v1/member_requests`

Returns a list of all member requests for a given organization. Each member request may have one or more associated member transactions.

<h3 id="get-a-list-of-all-member-requests-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|page_size|query|integer|false|none|
|offset|query|integer|false|none|

> Example responses

> 200 Response

```json
[
  {
    "body": {
      "coverages": [
        {
          "carrier_config": {
            "bill_group": "1000001",
            "member_group": "99999"
          },
          "carrier_id": "d0003042-eaae-4491-b219-4825456a3d16",
          "lines_of_coverage": {
            "dental": {
              "waiving_members": [
                {
                  "id": "f471a562-fa8f-41c9-93fd-12b372e16c72",
                  "member_type": "employee",
                  "reason": "other-spouse-group"
                }
              ]
            }
          }
        }
      ]
    },
    "employee_id": "30b74a44-d5b1-4123-a7a4-6d3aec251ba4",
    "id": "4f57e463-f4d5-4255-83d4-806b0cabaac5",
    "request_type": "new_hire",
    "status": "processing",
    "transactions": [
      "a6e30204-87b2-4802-95a4-a156bd0f7435"
    ]
  },
  {
    "body": {
      "employee_id": "6dc22bed-27fc-458c-b732-bdaf5b5a1031",
      "last_work_date": "2018-01-15",
      "reason": "voluntary"
    },
    "employee_id": "30b74a44-d5b1-4123-a7a4-6d3aec251ba4",
    "id": "dd9a1813-34f7-4c7e-86bc-f041f2cbd9a1",
    "request_type": "termination",
    "status": "processing",
    "transactions": [
      "f4ecdaa5-e019-4a24-98c7-2caee9a58ccd"
    ]
  }
]
```

<h3 id="get-a-list-of-all-member-requests-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successful Response - Returns all matching Member Requests|Inline|

<h3 id="get-a-list-of-all-member-requests-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[[MemberRequestResult](#schemamemberrequestresult)]|false|none|none|
|» body|string|true|none|Carrier-specific data required to execute a member request|
|» created|string|true|read-only|The date the record was originated|
|» employee_id|string(uuid)|true|read-only|Unique identifier of the employee in Noyo|
|» id|string(uuid)|true|read-only|Unique identifer of the group location in Noyo|
|» modified|string|true|read-only|The date the record was last updated|
|» request_type|string|true|none|Transaction type for the member request|
|» result|string|true|none|Result from executing the member request|
|» status|string|true|none|Status of the member request|
|» transactions|[string]|false|none|List of unique identifiers of all associated member transactions in Noyo|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
Noyo
</aside>

## Get a single Member Request

<a id="opIdgetMemberRequest"></a>

> Code samples

```shell
# You can also use wget
curl -X GET /api/v1/member_requests/{request_id} \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer {access-token}'

```

`GET /api/v1/member_requests/{request_id}`

Returns the latest version of a single member request based on the ID provided.

<h3 id="get-a-single-member-request-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|request_id|path|string|true|none|

> Example responses

> 200 Response

```json
{
  "body": {
    "employee_id": "6dc22bed-27fc-458c-b732-bdaf5b5a1031",
    "last_work_date": "2018-01-15",
    "reason": "voluntary"
  },
  "employee_id": "30b74a44-d5b1-4123-a7a4-6d3aec251ba4",
  "id": "dd9a1813-34f7-4c7e-86bc-f041f2cbd9a1",
  "request_type": "termination",
  "status": "processing",
  "transactions": [
    "f4ecdaa5-e019-4a24-98c7-2caee9a58ccd"
  ]
}
```

<h3 id="get-a-single-member-request-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successful Response - Returns a single Member Request|[MemberRequestResult](#schemamemberrequestresult)|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
Noyo
</aside>

## Create a New Hire Enrollment Member Request

<a id="opIdnewHireEnrollmentMemberRequest"></a>

> Code samples

```shell
# You can also use wget
curl -X POST /api/v1/employees/{employee_id}/member_requests/new_hire \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer {access-token}'

```

`POST /api/v1/employees/{employee_id}/member_requests/new_hire`

Create a member request for a New Hire Enrollment.

> Body parameter

```json
{
  "coverages": [
    {
      "carrier_config": {},
      "carrier_id": "9a0a7437-4097-4251-9e71-85384c0eb1c9",
      "lines_of_coverage": {
        "dental": {
          "enrolling_members": [
            {
              "id": "72af10df-a8b3-46f1-a114-ac36d4b8a6ea",
              "member_type": "employee",
              "plan_id": "ff033377-59e2-40df-8206-8cec3a725411"
            },
            {
              "id": "165d4537-be5f-4744-8a97-a5b10fcb75b1",
              "member_type": "dependent",
              "plan_id": "ff033377-59e2-40df-8206-8cec3a725411"
            }
          ]
        },
        "vision": {
          "enrolling_members": [
            {
              "id": "72af10df-a8b3-46f1-a114-ac36d4b8a6ea",
              "member_type": "employee",
              "plan_id": "08f9e430-9686-4b07-9e2c-6b26b8133dc3"
            },
            {
              "id": "165d4537-be5f-4744-8a97-a5b10fcb75b1",
              "member_type": "dependent",
              "plan_id": "08f9e430-9686-4b07-9e2c-6b26b8133dc3"
            }
          ]
        }
      }
    }
  ]
}
```

<h3 id="create-a-new-hire-enrollment-member-request-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|employee_id|path|string|true|none|
|body|body|[MemberRequestNewHireEnrollmentRequest](#schemamemberrequestnewhireenrollmentrequest)|true|none|

> Example responses

> 201 Response

```json
{
  "body": {
    "coverages": [
      {
        "carrier_config": {},
        "carrier_id": "9a0a7437-4097-4251-9e71-85384c0eb1c9",
        "lines_of_coverage": {
          "dental": {
            "enrolling_members": [
              {
                "id": "72af10df-a8b3-46f1-a114-ac36d4b8a6ea",
                "member_type": "employee",
                "plan_id": "ff033377-59e2-40df-8206-8cec3a725411"
              },
              {
                "id": "165d4537-be5f-4744-8a97-a5b10fcb75b1",
                "member_type": "dependent",
                "plan_id": "ff033377-59e2-40df-8206-8cec3a725411"
              }
            ]
          },
          "vision": {
            "enrolling_members": [
              {
                "id": "72af10df-a8b3-46f1-a114-ac36d4b8a6ea",
                "member_type": "employee",
                "plan_id": "08f9e430-9686-4b07-9e2c-6b26b8133dc3"
              },
              {
                "id": "165d4537-be5f-4744-8a97-a5b10fcb75b1",
                "member_type": "dependent",
                "plan_id": "08f9e430-9686-4b07-9e2c-6b26b8133dc3"
              }
            ]
          }
        }
      }
    ]
  },
  "employee_id": "72af10df-a8b3-46f1-a114-ac36d4b8a6ea",
  "id": "4f57e463-f4d5-4255-83d4-806b0cabaac5",
  "request_type": "new_hire",
  "status": "processing",
  "transactions": [
    "a6e30204-87b2-4802-95a4-a156bd0f7435"
  ]
}
```

<h3 id="create-a-new-hire-enrollment-member-request-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Successful Response - Returns the new Member Request|[MemberRequestResult](#schemamemberrequestresult)|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
Noyo
</aside>

## Create a Termination Member Request

<a id="opIdterminationMemberRequest"></a>

> Code samples

```shell
# You can also use wget
curl -X POST /api/v1/employees/{employee_id}/member_requests/termination \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer {access-token}'

```

`POST /api/v1/employees/{employee_id}/member_requests/termination`

Create a member request to termination Member.

> Body parameter

```json
{
  "employee_id": "6dc22bed-27fc-458c-b732-bdaf5b5a1031",
  "last_work_date": "2018-01-15",
  "reason": "voluntary"
}
```

<h3 id="create-a-termination-member-request-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|employee_id|path|string|true|none|
|body|body|[MemberRequestTerminationRequest](#schemamemberrequestterminationrequest)|true|none|

> Example responses

> 201 Response

```json
{
  "body": {
    "employee_id": "6dc22bed-27fc-458c-b732-bdaf5b5a1031",
    "last_work_date": "2018-01-15",
    "reason": "voluntary"
  },
  "employee_id": "6dc22bed-27fc-458c-b732-bdaf5b5a1031",
  "id": "dd9a1813-34f7-4c7e-86bc-f041f2cbd9a1",
  "request_type": "termination",
  "status": "processing",
  "transactions": [
    "f4ecdaa5-e019-4a24-98c7-2caee9a58ccd"
  ]
}
```

<h3 id="create-a-termination-member-request-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Successful Response - Returns the new Member Request|[MemberRequestResult](#schemamemberrequestresult)|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
Noyo
</aside>

## Create a Demographic Change Member Request

<a id="opIddemographicChangeMemberRequest"></a>

> Code samples

```shell
# You can also use wget
curl -X POST /api/v1/employees/{employee_id}/member_requests/demographic \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer {access-token}'

```

`POST /api/v1/employees/{employee_id}/member_requests/demographic`

Create a member request for a Demographic Change.

> Body parameter

```json
{
  "change_date": "2019-03-15",
  "member_changes": [
    {
      "address_change": {
        "home_address": {
          "city": "Toledo",
          "county": "string",
          "state": "OH",
          "street_one": "7303 Laurel Street",
          "street_two": "Apartment 2",
          "zip_code": "43460"
        },
        "mailing_address": {
          "city": "Toledo",
          "county": "string",
          "state": "OH",
          "street_one": "7303 Laurel Street",
          "street_two": "Apartment 2",
          "zip_code": "43460"
        }
      },
      "contact_change": {
        "email_address": "jane.doe@example.com",
        "email_address_type": "home",
        "home_phone": "+15555551212",
        "work_phone": "+15555551212"
      },
      "dob_change": {
        "date_of_birth": "1982-05-13"
      },
      "employment_change": {
        "employment_status": "full-time",
        "hire_date": "2017-05-23",
        "hours_worked": 40,
        "occupation": "Engineer"
      },
      "marital_status_change": {
        "marital_status": "single"
      },
      "member": {
        "id": "string",
        "type": "dependent"
      },
      "name_change": {
        "first_name": "Jane",
        "last_name": "Doe",
        "middle_name": "Ellen"
      },
      "salary_change": {
        "amount": 55000,
        "type": "salary",
        "unit": "annual"
      },
      "sex_change": {
        "sex": "F"
      },
      "ssn_change": {
        "ssn": "string"
      }
    }
  ]
}
```

<h3 id="create-a-demographic-change-member-request-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|employee_id|path|string|true|none|
|body|body|[MemberRequestDemographicChangeRequest](#schemamemberrequestdemographicchangerequest)|true|none|

> Example responses

> 201 Response

```json
{
  "body": {
    "member_changes": [
      {
        "member": {
          "id": "f471a562-fa8f-41c9-93fd-12b372e16c72",
          "member_type": "employee"
        },
        "name_change": {
          "first_name": "David",
          "last_name": "Williamson"
        }
      }
    ]
  },
  "employee_id": "30b74a44-d5b1-4123-a7a4-6d3aec251ba4",
  "id": "db6f362a-9f90-49b2-a73d-830cd2726537",
  "request_type": "demographic",
  "status": "processing",
  "transactions": [
    "b56bfe70-2632-49e9-b269-1eaa36ed7fd6"
  ]
}
```

<h3 id="create-a-demographic-change-member-request-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Successful Response - Returns the new Member Request|[MemberRequestResult](#schemamemberrequestresult)|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
Noyo
</aside>

## Create a Open Enrollment Member Request

<a id="opIdopenEnrollmentMemberRequest"></a>

> Code samples

```shell
# You can also use wget
curl -X POST /api/v1/employees/{employee_id}/member_requests/open_enrollment \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer {access-token}'

```

`POST /api/v1/employees/{employee_id}/member_requests/open_enrollment`

Create a member request for an Open Enrollment.

> Body parameter

```json
{
  "add": {
    "adding_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "prior_coverage": {
          "carrier_name": "string",
          "last_coverage_date": "string"
        },
        "type": "dependent"
      }
    ],
    "modifying_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "type": "dependent"
      }
    ],
    "removing_coverage": [
      {
        "id": "string",
        "reason": "other-employee-group",
        "type": "dependent"
      }
    ]
  },
  "dental": {
    "adding_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "prior_coverage": {
          "carrier_name": "string",
          "last_coverage_date": "string"
        },
        "type": "dependent"
      }
    ],
    "modifying_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "type": "dependent"
      }
    ],
    "removing_coverage": [
      {
        "id": "string",
        "reason": "other-employee-group",
        "type": "dependent"
      }
    ]
  },
  "life": {
    "adding_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "prior_coverage": {
          "carrier_name": "string",
          "last_coverage_date": "string"
        },
        "type": "dependent"
      }
    ],
    "modifying_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "type": "dependent"
      }
    ],
    "removing_coverage": [
      {
        "id": "string",
        "reason": "other-employee-group",
        "type": "dependent"
      }
    ]
  },
  "ltd": {
    "adding_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "prior_coverage": {
          "carrier_name": "string",
          "last_coverage_date": "string"
        },
        "type": "dependent"
      }
    ],
    "modifying_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "type": "dependent"
      }
    ],
    "removing_coverage": [
      {
        "id": "string",
        "reason": "other-employee-group",
        "type": "dependent"
      }
    ]
  },
  "medical": {
    "adding_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "prior_coverage": {
          "carrier_name": "string",
          "last_coverage_date": "string"
        },
        "type": "dependent"
      }
    ],
    "modifying_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "type": "dependent"
      }
    ],
    "removing_coverage": [
      {
        "id": "string",
        "reason": "other-employee-group",
        "type": "dependent"
      }
    ]
  },
  "std": {
    "adding_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "prior_coverage": {
          "carrier_name": "string",
          "last_coverage_date": "string"
        },
        "type": "dependent"
      }
    ],
    "modifying_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "type": "dependent"
      }
    ],
    "removing_coverage": [
      {
        "id": "string",
        "reason": "other-employee-group",
        "type": "dependent"
      }
    ]
  },
  "vision": {
    "adding_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "prior_coverage": {
          "carrier_name": "string",
          "last_coverage_date": "string"
        },
        "type": "dependent"
      }
    ],
    "modifying_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "type": "dependent"
      }
    ],
    "removing_coverage": [
      {
        "id": "string",
        "reason": "other-employee-group",
        "type": "dependent"
      }
    ]
  }
}
```

<h3 id="create-a-open-enrollment-member-request-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|employee_id|path|string|true|none|
|body|body|[MemberRequestOpenEnrollmentRequest](#schemamemberrequestopenenrollmentrequest)|true|none|

> Example responses

> 201 Response

```json
{
  "body": {
    "vision": {
      "removing_coverage": [
        {
          "id": "fd62665c-0846-4e9d-bd29-80779b5f685c",
          "reason": "other-parent-group",
          "type": "dependent"
        }
      ]
    }
  },
  "employee_id": "30b74a44-d5b1-4123-a7a4-6d3aec251ba4",
  "id": "1c0b921c-c104-48bd-a60d-900b556b5839",
  "request_type": "open_enrollment",
  "status": "processing",
  "transactions": [
    "25e169ec-564f-45a3-92f2-a947eeff7faa"
  ]
}
```

<h3 id="create-a-open-enrollment-member-request-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Successful Response - Returns the new Member Request|[MemberRequestResult](#schemamemberrequestresult)|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
Noyo
</aside>

## Create a Qualifying Life Event Member Request

<a id="opIdqualifyingLifeEventMemberRequest"></a>

> Code samples

```shell
# You can also use wget
curl -X POST /api/v1/employees/{employee_id}/member_requests/qualifying_life_event \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer {access-token}'

```

`POST /api/v1/employees/{employee_id}/member_requests/qualifying_life_event`

Create a member request for a Qualifying Life Event.

> Body parameter

```json
{
  "add": {
    "adding_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "prior_coverage": {
          "carrier_name": "string",
          "last_coverage_date": "string"
        },
        "type": "dependent"
      }
    ],
    "modifying_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "type": "dependent"
      }
    ],
    "removing_coverage": [
      {
        "id": "string",
        "reason": "other-employee-group",
        "type": "dependent"
      }
    ]
  },
  "dental": {
    "adding_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "prior_coverage": {
          "carrier_name": "string",
          "last_coverage_date": "string"
        },
        "type": "dependent"
      }
    ],
    "modifying_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "type": "dependent"
      }
    ],
    "removing_coverage": [
      {
        "id": "string",
        "reason": "other-employee-group",
        "type": "dependent"
      }
    ]
  },
  "event": {
    "event_date": "string",
    "event_type": "adoption"
  },
  "life": {
    "adding_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "prior_coverage": {
          "carrier_name": "string",
          "last_coverage_date": "string"
        },
        "type": "dependent"
      }
    ],
    "modifying_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "type": "dependent"
      }
    ],
    "removing_coverage": [
      {
        "id": "string",
        "reason": "other-employee-group",
        "type": "dependent"
      }
    ]
  },
  "ltd": {
    "adding_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "prior_coverage": {
          "carrier_name": "string",
          "last_coverage_date": "string"
        },
        "type": "dependent"
      }
    ],
    "modifying_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "type": "dependent"
      }
    ],
    "removing_coverage": [
      {
        "id": "string",
        "reason": "other-employee-group",
        "type": "dependent"
      }
    ]
  },
  "medical": {
    "adding_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "prior_coverage": {
          "carrier_name": "string",
          "last_coverage_date": "string"
        },
        "type": "dependent"
      }
    ],
    "modifying_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "type": "dependent"
      }
    ],
    "removing_coverage": [
      {
        "id": "string",
        "reason": "other-employee-group",
        "type": "dependent"
      }
    ]
  },
  "std": {
    "adding_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "prior_coverage": {
          "carrier_name": "string",
          "last_coverage_date": "string"
        },
        "type": "dependent"
      }
    ],
    "modifying_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "type": "dependent"
      }
    ],
    "removing_coverage": [
      {
        "id": "string",
        "reason": "other-employee-group",
        "type": "dependent"
      }
    ]
  },
  "vision": {
    "adding_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "prior_coverage": {
          "carrier_name": "string",
          "last_coverage_date": "string"
        },
        "type": "dependent"
      }
    ],
    "modifying_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "type": "dependent"
      }
    ],
    "removing_coverage": [
      {
        "id": "string",
        "reason": "other-employee-group",
        "type": "dependent"
      }
    ]
  }
}
```

<h3 id="create-a-qualifying-life-event-member-request-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|employee_id|path|string|true|none|
|body|body|[MemberRequestQualifyingLifeEventRequest](#schemamemberrequestqualifyinglifeeventrequest)|true|none|

> Example responses

> 201 Response

```json
{
  "body": {
    "event": {
      "event_date": "2019-02-15",
      "event_type": "newborn"
    },
    "medical": {
      "adding_coverage": [
        {
          "id": "fd62665c-0846-4e9d-bd29-80779b5f685c",
          "plan_id": "b988cd26-121a-4dd6-b0be-09399f6ecc0a",
          "type": "dependent"
        }
      ]
    }
  },
  "employee_id": "30b74a44-d5b1-4123-a7a4-6d3aec251ba4",
  "id": "f4517b87-275a-42e1-85e5-47ea6ab5312b",
  "request_type": "qualifying_life_event",
  "status": "processing",
  "transactions": [
    "579952dd-9a66-4587-a4f7-72a63bf9ec86"
  ]
}
```

<h3 id="create-a-qualifying-life-event-member-request-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Successful Response - Returns the new Member Request|[MemberRequestResult](#schemamemberrequestresult)|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
Noyo
</aside>

<h1 id="noyo-fulfillment-api-member-transactions">Member Transactions</h1>

## Get a single Member Transaction

<a id="opIdgetMemberTransaction"></a>

> Code samples

```shell
# You can also use wget
curl -X GET /api/v1/member_transactions/{transaction_id} \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer {access-token}'

```

`GET /api/v1/member_transactions/{transaction_id}`

Returns the latest version of a single member transaction based on the ID provided.

<h3 id="get-a-single-member-transaction-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|transaction_id|path|string|true|none|

> Example responses

> 200 Response

```json
{
  "body": {
    "coverages": [
      {
        "carrier_config": {
          "bill_group": "1000001",
          "member_group": "99999"
        },
        "carrier_id": "d0003042-eaae-4491-b219-4825456a3d16",
        "lines_of_coverage": {
          "dental": {
            "enrolling_members": [
              {
                "id": "25434576-e85d-481b-a825-962fd575046f",
                "member_type": "employee",
                "plan_id": "2e6a04b6-21f3-40d7-a9e2-735196cc451b"
              }
            ],
            "waiving_members": []
          },
          "medical": {
            "enrolling_members": [
              {
                "id": "25434576-e85d-481b-a825-962fd575046f",
                "member_type": "employee",
                "plan_id": "1873df52-c9e9-4e3a-995c-f47f32d0bd62"
              }
            ],
            "waiving_members": []
          },
          "vision": {
            "enrolling_members": [
              {
                "id": "25434576-e85d-481b-a825-962fd575046f",
                "member_type": "employee",
                "plan_id": "9cb64800-022e-40af-ab80-8d945e430592"
              }
            ],
            "waiving_members": []
          }
        }
      }
    ]
  },
  "carrier_id": "478e066e-9696-490e-b60f-47f40c96dc7c",
  "employee_id": "25434576-e85d-481b-a825-962fd575046f",
  "id": "a6e30204-87b2-4802-95a4-a156bd0f7435",
  "lines_of_coverage": [
    "medical",
    "dental",
    "vision"
  ],
  "member_request_id": "bba6ea27-4004-4a52-a3d9-f986ace0d3da",
  "status": "processing"
}
```

<h3 id="get-a-single-member-transaction-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successful Response - Returns a single Member Transaction|[MemberTransactionResult](#schemamembertransactionresult)|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
Noyo
</aside>

## Get a list of all Member Transactions

<a id="opIdgetMemberTransactionsList"></a>

> Code samples

```shell
# You can also use wget
curl -X GET /api/v1/member_transactions \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer {access-token}'

```

`GET /api/v1/member_transactions`

Returns a list of all member transactions

<h3 id="get-a-list-of-all-member-transactions-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|page_size|query|integer|false|none|
|offset|query|integer|false|none|

> Example responses

> 200 Response

```json
[
  {
    "body": {
      "coverages": [
        {
          "carrier_config": {
            "bill_group": "1000001",
            "member_group": "99999"
          },
          "carrier_id": "d0003042-eaae-4491-b219-4825456a3d16",
          "lines_of_coverage": {
            "dental": {
              "enrolling_members": [
                {
                  "id": "25434576-e85d-481b-a825-962fd575046f",
                  "member_type": "employee",
                  "plan_id": "2e6a04b6-21f3-40d7-a9e2-735196cc451b"
                }
              ],
              "waiving_members": []
            },
            "medical": {
              "enrolling_members": [
                {
                  "id": "25434576-e85d-481b-a825-962fd575046f",
                  "member_type": "employee",
                  "plan_id": "1873df52-c9e9-4e3a-995c-f47f32d0bd62"
                }
              ],
              "waiving_members": []
            },
            "vision": {
              "enrolling_members": [
                {
                  "id": "25434576-e85d-481b-a825-962fd575046f",
                  "member_type": "employee",
                  "plan_id": "9cb64800-022e-40af-ab80-8d945e430592"
                }
              ],
              "waiving_members": []
            }
          }
        }
      ]
    },
    "carrier_id": "478e066e-9696-490e-b60f-47f40c96dc7c",
    "employee_id": "25434576-e85d-481b-a825-962fd575046f",
    "id": "a6e30204-87b2-4802-95a4-a156bd0f7435",
    "lines_of_coverage": [
      "medical",
      "dental",
      "vision"
    ],
    "member_request_id": "bba6ea27-4004-4a52-a3d9-f986ace0d3da",
    "status": "processing"
  }
]
```

<h3 id="get-a-list-of-all-member-transactions-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successful Response - Returns all matching Member Transactions|Inline|

<h3 id="get-a-list-of-all-member-transactions-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[[MemberTransactionResult](#schemamembertransactionresult)]|false|none|none|
|» body|string|true|none|Carrier-specific data required to execute a member transaction|
|» carrier_id|string(uuid)|false|read-only|Unique identifer of the carrier in Noyo|
|» created|string|true|read-only|The date the record was originated|
|» employee_id|string(uuid)|true|read-only|Unique identifer of the employee in Noyo|
|» id|string(uuid)|true|read-only|Unique identifer of the member transaction in Noyo|
|» lines_of_coverage|[string]|false|none|List of lines of coverage in the member transaction|
|» member_request_id|string(uuid)|true|read-only|Unique identifer of the member request in Noyo|
|» modified|string|true|read-only|The date the record was last updated|
|» result|string|true|none|Carrier-specific data describing the result of a completed member transaction|
|» status|string|true|none|Status of the member transaction|
|» transaction_type|string|true|none|Type of the member transaction|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
Noyo
</aside>

# Schemas

<h2 id="tocSaddress">Address</h2>

<a id="schemaaddress"></a>

```json
{
  "city": "Bend",
  "county": "Deschutes",
  "state": "OR",
  "street_one": "339 Hickory Street",
  "street_two": "Apartment 5",
  "zip_code": "97701"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|city|string|true|none|City of the address|
|county|string|false|none|County of the address|
|state|string|true|none|State postal code of the address|
|street_one|string|true|none|Line one of the address|
|street_two|string|false|none|Line two of the address|
|zip_code|string|true|none|Zip code of the address|

#### Enumerated Values

|Property|Value|
|---|---|
|state|AL|
|state|AK|
|state|AZ|
|state|AR|
|state|CA|
|state|CO|
|state|CT|
|state|DC|
|state|DE|
|state|FL|
|state|GA|
|state|HI|
|state|ID|
|state|IL|
|state|IN|
|state|IA|
|state|KS|
|state|KY|
|state|LA|
|state|ME|
|state|MD|
|state|MA|
|state|MI|
|state|MN|
|state|MS|
|state|MO|
|state|MT|
|state|NE|
|state|NV|
|state|NH|
|state|NJ|
|state|NM|
|state|NY|
|state|NC|
|state|ND|
|state|OH|
|state|OK|
|state|OR|
|state|PA|
|state|RI|
|state|SC|
|state|SD|
|state|TN|
|state|TX|
|state|UT|
|state|VT|
|state|VA|
|state|WA|
|state|WV|
|state|WI|
|state|WY|

<h2 id="tocScontactresult">ContactResult</h2>

<a id="schemacontactresult"></a>

```json
{
  "created": "string",
  "email": "catherine.briggs@example.com",
  "first_name": "Catherine",
  "group_id": "string",
  "id": "string",
  "last_name": "Briggs",
  "modified": "string",
  "phone_number": "string",
  "primary_contact": true,
  "title": "Engineer",
  "types": [
    "company"
  ],
  "version": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|created|string|true|read-only|The date the record was originated|
|email|string|false|read-only|Work email address of the group contact|
|first_name|string|false|read-only|First name of the group contact|
|group_id|string(uuid)|true|read-only|Unique identifier of the group in Noyo|
|id|string(uuid)|true|read-only|Unique identifier of the group contact in Noyo|
|last_name|string|false|read-only|Last name of the group contact|
|modified|string|true|read-only|The date the record was last updated|
|phone_number|string|false|read-only|Work phone number of the group contact in E.164 format|
|primary_contact|boolean|false|read-only|True if contact is the primary contact for the group|
|title|string|false|read-only|Job title of the group contact|
|types|[string]|true|none|Group contact types for the group contact|
|version|string(uuid)|true|read-only|Version of the group contact record|

<h2 id="tocSdependentcreaterequest">DependentCreateRequest</h2>

<a id="schemadependentcreaterequest"></a>

```json
{
  "person": {
    "contact": {
      "email_address": "catherine.briggs@example.com",
      "email_address_type": "home",
      "home_phone": "+15555551212",
      "preferred_language": "Engish",
      "preferred_method": "email",
      "speaks_english": true,
      "work_phone": "+15555551212"
    },
    "date_of_birth": "1977-06-03",
    "details": {
      "american_indian": {
        "state": "AL",
        "tribe": "Ojibwe"
      },
      "disability": {
        "communication": true,
        "reason": "string"
      },
      "is_military": true,
      "is_student": true,
      "tobacco": {
        "duration": "5 years",
        "frequency": "daily",
        "types": "cigarettes"
      }
    },
    "first_name": "Catherine",
    "home_address": {
      "city": "Bend",
      "county": "Deschutes",
      "state": "OR",
      "street_one": "339 Hickory Street",
      "street_two": "Apartment 5",
      "zip_code": "97701"
    },
    "last_name": "Briggs",
    "maiden_name": "Adams",
    "mailing_address": {
      "city": "Bend",
      "county": "Deschutes",
      "state": "OR",
      "street_one": "339 Hickory Street",
      "street_two": "Apartment 5",
      "zip_code": "97701"
    },
    "marital_status": "married",
    "middle_name": "S",
    "sex": "F",
    "ssn": "123456789",
    "suffix": "string"
  },
  "relationship": "spouse"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|person|[Person](#schemaperson)|true|none|Personal information for the dependent|
|relationship|string|true|none|Relationship of the dependent to the employee|

#### Enumerated Values

|Property|Value|
|---|---|
|relationship|spouse|
|relationship|child|
|relationship|domestic-partner|
|relationship|civil-union|
|relationship|step-child|
|relationship|foster-child|
|relationship|adopted-child|
|relationship|legal-guardianship|
|relationship|ex-spouse|
|relationship|other|

<h2 id="tocSdependentresult">DependentResult</h2>

<a id="schemadependentresult"></a>

```json
{
  "created": "string",
  "employee_id": "string",
  "id": "string",
  "modified": "string",
  "person": {
    "contact": {
      "email_address": "catherine.briggs@example.com",
      "email_address_type": "home",
      "home_phone": "+15555551212",
      "preferred_language": "Engish",
      "preferred_method": "email",
      "speaks_english": true,
      "work_phone": "+15555551212"
    },
    "date_of_birth": "1977-06-03",
    "details": {
      "american_indian": {
        "state": "AL",
        "tribe": "Ojibwe"
      },
      "disability": {
        "communication": true,
        "reason": "string"
      },
      "is_military": true,
      "is_student": true,
      "tobacco": {
        "duration": "5 years",
        "frequency": "daily",
        "types": "cigarettes"
      }
    },
    "first_name": "Catherine",
    "home_address": {
      "city": "Bend",
      "county": "Deschutes",
      "state": "OR",
      "street_one": "339 Hickory Street",
      "street_two": "Apartment 5",
      "zip_code": "97701"
    },
    "last_name": "Briggs",
    "maiden_name": "Adams",
    "mailing_address": {
      "city": "Bend",
      "county": "Deschutes",
      "state": "OR",
      "street_one": "339 Hickory Street",
      "street_two": "Apartment 5",
      "zip_code": "97701"
    },
    "marital_status": "married",
    "middle_name": "S",
    "sex": "F",
    "ssn": "123456789",
    "suffix": "string"
  },
  "relationship": "spouse",
  "version": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|created|string|true|read-only|The date the record was originated|
|employee_id|string(uuid)|true|read-only|Unique identifier of the employee in Noyo|
|id|string(uuid)|true|read-only|Unique identifier of the dependent in Noyo|
|modified|string|true|read-only|The date the record was last updated|
|person|[Person](#schemaperson)|true|none|Personal information for the dependent|
|relationship|string|true|none|Relationship of the dependent to the employee|
|version|string(uuid)|true|read-only|Version of the dependent record|

#### Enumerated Values

|Property|Value|
|---|---|
|relationship|spouse|
|relationship|child|
|relationship|domestic-partner|
|relationship|civil-union|
|relationship|step-child|
|relationship|foster-child|
|relationship|adopted-child|
|relationship|legal-guardianship|
|relationship|ex-spouse|
|relationship|other|

<h2 id="tocSemployeecreaterequest">EmployeeCreateRequest</h2>

<a id="schemaemployeecreaterequest"></a>

```json
{
  "employment": {
    "employment_dates": {
      "full_time_start": "2017-01-17",
      "hire_date": "2017-01-17",
      "rehire": "2018-02-01",
      "retirement": "2019-01-01",
      "terminated": "2018-01-01"
    },
    "employment_status": "full-time",
    "hours_worked": 40,
    "occupation": "Engineer",
    "salary": {
      "amount": "55000",
      "type": "salary",
      "unit": "annual"
    }
  },
  "group_id": "string",
  "location_id": "string",
  "person": {
    "contact": {
      "email_address": "catherine.briggs@example.com",
      "email_address_type": "home",
      "home_phone": "+15555551212",
      "preferred_language": "Engish",
      "preferred_method": "email",
      "speaks_english": true,
      "work_phone": "+15555551212"
    },
    "date_of_birth": "1977-06-03",
    "details": {
      "american_indian": {
        "state": "AL",
        "tribe": "Ojibwe"
      },
      "disability": {
        "communication": true,
        "reason": "string"
      },
      "is_military": true,
      "is_student": true,
      "tobacco": {
        "duration": "5 years",
        "frequency": "daily",
        "types": "cigarettes"
      }
    },
    "first_name": "Catherine",
    "home_address": {
      "city": "Bend",
      "county": "Deschutes",
      "state": "OR",
      "street_one": "339 Hickory Street",
      "street_two": "Apartment 5",
      "zip_code": "97701"
    },
    "last_name": "Briggs",
    "maiden_name": "Adams",
    "mailing_address": {
      "city": "Bend",
      "county": "Deschutes",
      "state": "OR",
      "street_one": "339 Hickory Street",
      "street_two": "Apartment 5",
      "zip_code": "97701"
    },
    "marital_status": "married",
    "middle_name": "S",
    "sex": "F",
    "ssn": "123456789",
    "suffix": "string"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|employment|object|true|none|Employment information for the employee|
|» employment_dates|object|true|none|Employee employment dates|
|»» full_time_start|string|false|none|ISO-8601 date string for start date as a full time employee with the group (if applicable)|
|»» hire_date|string(date)|true|none|ISO-8601 date string for the hire date of the employee|
|»» rehire|string|false|none|ISO-8601 date string for the rehire date of the employee (if applicable)|
|»» retirement|string|false|none|ISO-8601 date string for the retirement date of the employee (if appliable)|
|»» terminated|string|false|none|ISO-8601 date string for the termination date of the employee (if applicable)|
|» employment_status|string|true|none|Employee employment status|
|» hours_worked|integer(int32)|true|none|Number of hours worked per week by the employee|
|» occupation|string|false|none|Employee occupation or job title|
|» salary|object|false|none|Employee salary information|
|»» amount|number|true|none|Amount of salary earned by the employee in US dollars|
|»» type|string|true|none|Type of salary earned by the employee|
|»» unit|string|true|none|Unit of salary earned by the employee|
|» group_id|string(uuid)|true|none|Unique identifier of the group in Noyo|
|» location_id|string(uuid)|true|none|Unique identifier of the employee group location in Noyo|
|» person|[Person](#schemaperson)|true|none|Personal information for the employee|

#### Enumerated Values

|Property|Value|
|---|---|
|employment_status|full-time|
|employment_status|part-time|
|employment_status|contract|
|employment_status|disabled|
|employment_status|terminated|
|employment_status|retired|
|type|hourly|
|type|salary|
|unit|annual|
|unit|month|
|unit|week|
|unit|hour|

<h2 id="tocSemployeeresult">EmployeeResult</h2>

<a id="schemaemployeeresult"></a>

```json
{
  "created": "string",
  "employment": {
    "employment_dates": {
      "full_time_start": "2017-01-17",
      "hire_date": "2017-01-17",
      "rehire": "2018-02-01",
      "retirement": "2019-01-01",
      "terminated": "2018-01-01"
    },
    "employment_status": "full-time",
    "hours_worked": 40,
    "occupation": "Engineer",
    "salary": {
      "amount": "55000",
      "type": "salary",
      "unit": "annual"
    }
  },
  "group_id": "string",
  "id": "string",
  "location_id": "string",
  "modified": "string",
  "person": {
    "contact": {
      "email_address": "catherine.briggs@example.com",
      "email_address_type": "home",
      "home_phone": "+15555551212",
      "preferred_language": "Engish",
      "preferred_method": "email",
      "speaks_english": true,
      "work_phone": "+15555551212"
    },
    "date_of_birth": "1977-06-03",
    "details": {
      "american_indian": {
        "state": "AL",
        "tribe": "Ojibwe"
      },
      "disability": {
        "communication": true,
        "reason": "string"
      },
      "is_military": true,
      "is_student": true,
      "tobacco": {
        "duration": "5 years",
        "frequency": "daily",
        "types": "cigarettes"
      }
    },
    "first_name": "Catherine",
    "home_address": {
      "city": "Bend",
      "county": "Deschutes",
      "state": "OR",
      "street_one": "339 Hickory Street",
      "street_two": "Apartment 5",
      "zip_code": "97701"
    },
    "last_name": "Briggs",
    "maiden_name": "Adams",
    "mailing_address": {
      "city": "Bend",
      "county": "Deschutes",
      "state": "OR",
      "street_one": "339 Hickory Street",
      "street_two": "Apartment 5",
      "zip_code": "97701"
    },
    "marital_status": "married",
    "middle_name": "S",
    "sex": "F",
    "ssn": "123456789",
    "suffix": "string"
  },
  "version": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|created|string|true|read-only|The date the record was originated|
|employment|object|true|none|Employment information for the employee|
|» employment_dates|object|true|none|Employee employment dates|
|»» full_time_start|string|false|none|ISO-8601 date string for start date as a full time employee with the group (if applicable)|
|»» hire_date|string(date)|true|none|ISO-8601 date string for the hire date of the employee|
|»» rehire|string|false|none|ISO-8601 date string for the rehire date of the employee (if applicable)|
|»» retirement|string|false|none|ISO-8601 date string for the retirement date of the employee (if appliable)|
|»» terminated|string|false|none|ISO-8601 date string for the termination date of the employee (if applicable)|
|» employment_status|string|true|none|Employee employment status|
|» hours_worked|integer(int32)|true|none|Number of hours worked per week by the employee|
|» occupation|string|false|none|Employee occupation or job title|
|» salary|object|false|none|Employee salary information|
|»» amount|number|true|none|Amount of salary earned by the employee in US dollars|
|»» type|string|true|none|Type of salary earned by the employee|
|»» unit|string|true|none|Unit of salary earned by the employee|
|» group_id|string(uuid)|true|read-only|Unique identifier of the group in Noyo|
|» id|string(uuid)|true|read-only|Unique identifier of the employee in Noyo|
|» location_id|string(uuid)|true|none|Unique identifier of the employee group location in Noyo|
|» modified|string|true|read-only|The date the record was last updated|
|» person|[Person](#schemaperson)|true|none|Personal information for the employee|
|» version|string(uuid)|true|read-only|Version of the employee record|

#### Enumerated Values

|Property|Value|
|---|---|
|employment_status|full-time|
|employment_status|part-time|
|employment_status|contract|
|employment_status|disabled|
|employment_status|terminated|
|employment_status|retired|
|type|hourly|
|type|salary|
|unit|annual|
|unit|month|
|unit|week|
|unit|hour|

<h2 id="tocSgroupaddplanresult">GroupADDPlanResult</h2>

<a id="schemagroupaddplanresult"></a>

```json
{
  "code": "ADD-P1K",
  "created": "string",
  "eligible_member_types": [
    "employee"
  ],
  "group_enrollment_id": "string",
  "id": "string",
  "modified": "string",
  "name": "AD&D Plan 10000/50000",
  "plan_type": "basic",
  "version": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|code|string|true|read-only|Plan code or product code assigned by the carrier|
|created|string|true|read-only|The date the record was originated|
|eligible_member_types|[string]|true|read-only|List of member types eligible for the group AD&D plan|
|group_enrollment_id|string(uuid)|true|read-only|Unique identifier of the group enrollment in Noyo|
|id|string(uuid)|true|read-only|Unique identifier of the group AD&D plan in Noyo|
|modified|string|true|read-only|The date the record was last updated|
|name|string|true|read-only|Name of the group AD&D plan|
|plan_type|string|true|read-only|Type of group AD&D plan|
|version|string(uuid)|true|read-only|Version of the group AD&D plan record|

<h2 id="tocSgroupdentalplanresult">GroupDentalPlanResult</h2>

<a id="schemagroupdentalplanresult"></a>

```json
{
  "code": "PDS-11",
  "created": "string",
  "eligible_member_types": [
    "all"
  ],
  "group_enrollment_id": "string",
  "id": "string",
  "modified": "string",
  "name": "Premium Dental Select 1100",
  "network": "Dental Select",
  "plan_type": "dhmo",
  "version": "string",
  "voluntary": true
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|code|string|true|read-only|Plan code or product code assigned by the carrier|
|created|string|true|read-only|The date the record was originated|
|eligible_member_types|[string]|true|read-only|List of member types eligible for the group dental plan|
|group_enrollment_id|string(uuid)|true|read-only|Unique identifier of the group enrollment in Noyo|
|id|string(uuid)|true|read-only|Unique identifier of the group dental plan in Noyo|
|modified|string|true|read-only|The date the record was last updated|
|name|string|true|read-only|Name of the group dental plan|
|network|string|true|read-only|Network name of the plan designated by the carrier|
|plan_type|string|true|read-only|Type of group dental plan|
|version|string(uuid)|true|read-only|Version of the group dental plan record|
|voluntary|boolean|true|read-only|True if the group dental plan is a voluntary plan|

<h2 id="tocSgroupenrollmentresult">GroupEnrollmentResult</h2>

<a id="schemagroupenrollmentresult"></a>

```json
{
  "carrier_id": "string",
  "created": "string",
  "group_id": "string",
  "id": "string",
  "line_of_coverage": "medical",
  "modified": "string",
  "open_enrollment_end_date": "2019-01-01",
  "open_enrollment_start_date": "2018-11-01",
  "status": "active",
  "version": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|carrier_id|string(uuid)|true|read-only|Unique identifier of the carrier in Noyo|
|created|string|true|read-only|The date the record was originated|
|group_id|string(uuid)|true|read-only|Unique identifier of the group in Noyo|
|id|string(uuid)|true|read-only|Unique identifier of the group enrollment in Noyo|
|line_of_coverage|string|true|read-only|Line of coverage for the group enrollment|
|modified|string|true|read-only|The date the record was last updated|
|open_enrollment_end_date|string(date)|false|read-only|ISO-8601 date string for the open enrollment end date of the group enrollment|
|open_enrollment_start_date|string(date)|false|read-only|ISO-8601 date string for the open enrollment start date of the group enrollment|
|status|string|true|read-only|Status of the group enrollment|
|version|string(uuid)|true|read-only|Version of the group enrollment record|

<h2 id="tocSgroupltdplanresult">GroupLTDPlanResult</h2>

<a id="schemagroupltdplanresult"></a>

```json
{
  "code": "LTD-P1M",
  "created": "string",
  "eligible_member_types": [
    "employee"
  ],
  "group_enrollment_id": "string",
  "id": "string",
  "modified": "string",
  "name": "LTD Plan 100000/500000",
  "plan_type": "basic",
  "version": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|code|string|true|read-only|Plan code or product code assigned by the carrier|
|created|string|true|read-only|The date the record was originated|
|eligible_member_types|[string]|true|read-only|List of member types eligible for the group LTD plan|
|group_enrollment_id|string(uuid)|true|read-only|Unique identifier of the group enrollment in Noyo|
|id|string(uuid)|true|read-only|Unique identifier of the group LTD plan in Noyo|
|modified|string|true|read-only|The date the record was last updated|
|name|string|true|read-only|Name of the group LTD plan|
|plan_type|string|true|read-only|Type of group LTD plan|
|version|string(uuid)|true|read-only|Version of the group LTD plan record|

<h2 id="tocSgrouplifeplanresult">GroupLifePlanResult</h2>

<a id="schemagrouplifeplanresult"></a>

```json
{
  "code": "LB-P2K",
  "created": "string",
  "eligible_member_types": [
    "employee"
  ],
  "group_enrollment_id": "string",
  "id": "string",
  "modified": "string",
  "name": "Life Plan Voluntary 200k",
  "plan_type": "voluntary",
  "version": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|code|string|true|read-only|Plan code or product code assigned by the carrier|
|created|string|true|read-only|The date the record was originated|
|eligible_member_types|[string]|true|read-only|List of member types eligible for the group life plan|
|group_enrollment_id|string(uuid)|true|read-only|Unique identifier of the group enrollment in Noyo|
|id|string(uuid)|true|read-only|Unique identifier of the group life plan in Noyo|
|modified|string|true|read-only|The date the record was last updated|
|name|string|true|read-only|Name of the group life plan|
|plan_type|string|true|read-only|Type of group life plan|
|version|string(uuid)|true|read-only|Version of the group life plan record|

<h2 id="tocSgroupmedicalplanresult">GroupMedicalPlanResult</h2>

<a id="schemagroupmedicalplanresult"></a>

```json
{
  "code": "GHMO-25",
  "created": "string",
  "group_enrollment_id": "string",
  "id": "string",
  "metal_tier": "gold",
  "modified": "string",
  "name": "Gold HMO 2500",
  "network": "Premium Select",
  "plan_type": "hmo",
  "version": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|code|string|true|read-only|Plan code or product code assigned by the carrier|
|created|string|true|read-only|The date the record was originated|
|group_enrollment_id|string(uuid)|true|read-only|Unique identifier of the group enrollment in Noyo|
|id|string(uuid)|true|read-only|Unique identifier of the group medical plan in Noyo|
|metal_tier|string|true|read-only|Metal tier of plan of the group medical plan|
|modified|string|true|read-only|The date the record was last updated|
|name|string|true|read-only|Name of the group medical plan|
|network|string|true|read-only|Network name of the plan designated by the carrier|
|plan_type|string|true|read-only|Type of group medical plan|
|version|string(uuid)|true|read-only|Version of the group medical plan record|

<h2 id="tocSgroupresult">GroupResult</h2>

<a id="schemagroupresult"></a>

```json
{
  "created": "string",
  "id": "string",
  "modified": "string",
  "name": "ACME Flooring",
  "organization_id": "string",
  "sic_code": "5713",
  "version": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|created|string|true|read-only|The date the record was originated|
|id|string(uuid)|true|read-only|Unique identifier of the group in Noyo|
|modified|string|true|read-only|The date the record was last updated|
|name|string|true|none|Name of the group|
|organization_id|string(uuid)|true|none|Unique identifier of the platform or broker organization in Noyo|
|sic_code|string|false|none|SIC Code of the group|
|version|string(uuid)|true|read-only|Version of the group record|

<h2 id="tocSgroupstdplanresult">GroupSTDPlanResult</h2>

<a id="schemagroupstdplanresult"></a>

```json
{
  "code": "STD-P1M",
  "created": "string",
  "eligible_member_types": [
    "employee"
  ],
  "group_enrollment_id": "string",
  "id": "string",
  "modified": "string",
  "name": "STD Plan 100000/500000",
  "plan_type": "voluntary",
  "version": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|code|string|true|read-only|Plan code or product code assigned by the carrier|
|created|string|true|read-only|The date the record was originated|
|eligible_member_types|[string]|true|read-only|List of member types eligible for the group STD plan|
|group_enrollment_id|string(uuid)|true|read-only|Unique identifier of the group enrollment in Noyo|
|id|string(uuid)|true|read-only|Unique identifier of the group STD plan in Noyo|
|modified|string|true|read-only|The date the record was last updated|
|name|string|true|read-only|Name of the group STD plan|
|plan_type|string|true|read-only|Type of group STD plan|
|version|string(uuid)|true|read-only|Version of the group STD plan record|

<h2 id="tocSgroupvisionplanresult">GroupVisionPlanResult</h2>

<a id="schemagroupvisionplanresult"></a>

```json
{
  "code": "VE-21",
  "created": "string",
  "eligible_member_types": [
    "all"
  ],
  "group_enrollment_id": "string",
  "id": "string",
  "modified": "string",
  "name": "Vision EyeCare 2100",
  "version": "string",
  "voluntary": true
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|code|string|true|read-only|Plan code or product code assigned by the carrier|
|created|string|true|read-only|The date the record was originated|
|eligible_member_types|[string]|true|read-only|List of member types eligible for the group vision plan|
|group_enrollment_id|string(uuid)|true|read-only|Unique identifier of the group enrollment in Noyo|
|id|string(uuid)|true|read-only|Unique identifier of the group vision plan in Noyo|
|modified|string|true|read-only|The date the record was last updated|
|name|string|true|read-only|Name of the group vision plan|
|version|string(uuid)|true|read-only|Version of the group vision plan record|
|voluntary|boolean|true|read-only|True if the group vision plan is a voluntary plan|

<h2 id="tocSindividualenrollmentresult">IndividualEnrollmentResult</h2>

<a id="schemaindividualenrollmentresult"></a>

```json
{
  "created": "string",
  "enroll_data": "string",
  "group_enrollment_id": "string",
  "id": "string",
  "individual_id": "string",
  "individual_type": "employee",
  "line_of_coverage": "dental",
  "modified": "string",
  "plan_id": "string",
  "status": "active",
  "version": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|created|string|true|read-only|The date the record was originated|
|enroll_data|string|true|read-only|Line of coverage specific enroll data about premiums and volumes|
|group_enrollment_id|string(uuid)|true|read-only|Unique identifier of the group enrollment in Noyo|
|id|string(uuid)|true|read-only|Unique identifier of the individual enrollment in Noyo|
|individual_id|string(uuid)|true|read-only|Unique identifier of the employee or dependent in Noyo|
|individual_type|string|true|read-only|Type of member for the individual enrollment|
|line_of_coverage|string|true|read-only|Line of coverage for the individual enrollment|
|modified|string|true|read-only|The date the record was last updated|
|plan_id|string(uuid)|true|read-only|Unique identifier of the group plan in Noyo|
|status|string|true|read-only|Status of the individual enrollment|
|version|string(uuid)|true|read-only|Version of the individual enrollment record|

<h2 id="tocSlocationresult">LocationResult</h2>

<a id="schemalocationresult"></a>

```json
{
  "address": {
    "city": "Bend",
    "county": "Deschutes",
    "state": "OR",
    "street_one": "339 Hickory Street",
    "street_two": "Apartment 5",
    "zip_code": "97701"
  },
  "created": "string",
  "display_name": "Primary Company Location",
  "group_id": "string",
  "id": "string",
  "modified": "string",
  "version": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|address|[Address](#schemaaddress)|true|none|Address of the group location|
|created|string|true|read-only|The date the record was originated|
|display_name|string|true|none|Display name for the group location|
|group_id|string(uuid)|true|read-only|Unique identifier of the group in Noyo|
|id|string(uuid)|true|read-only|Unique identifer of the group location in Noyo|
|modified|string|true|read-only|The date the record was last updated|
|version|string(uuid)|true|read-only|Version of the group location record|

<h2 id="tocSmemberrequestdemographicchangerequest">MemberRequestDemographicChangeRequest</h2>

<a id="schemamemberrequestdemographicchangerequest"></a>

```json
{
  "change_date": "2019-03-15",
  "member_changes": [
    {
      "address_change": {
        "home_address": {
          "city": "Toledo",
          "county": "string",
          "state": "OH",
          "street_one": "7303 Laurel Street",
          "street_two": "Apartment 2",
          "zip_code": "43460"
        },
        "mailing_address": {
          "city": "Toledo",
          "county": "string",
          "state": "OH",
          "street_one": "7303 Laurel Street",
          "street_two": "Apartment 2",
          "zip_code": "43460"
        }
      },
      "contact_change": {
        "email_address": "jane.doe@example.com",
        "email_address_type": "home",
        "home_phone": "+15555551212",
        "work_phone": "+15555551212"
      },
      "dob_change": {
        "date_of_birth": "1982-05-13"
      },
      "employment_change": {
        "employment_status": "full-time",
        "hire_date": "2017-05-23",
        "hours_worked": 40,
        "occupation": "Engineer"
      },
      "marital_status_change": {
        "marital_status": "single"
      },
      "member": {
        "id": "string",
        "type": "dependent"
      },
      "name_change": {
        "first_name": "Jane",
        "last_name": "Doe",
        "middle_name": "Ellen"
      },
      "salary_change": {
        "amount": 55000,
        "type": "salary",
        "unit": "annual"
      },
      "sex_change": {
        "sex": "F"
      },
      "ssn_change": {
        "ssn": "string"
      }
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|change_date|string|false|none|ISO-8601 date string for effective date of demographic change|
|member_changes|[object]|true|none|List of employee or dependent demographic change requests|
|» address_change|object|false|none|Details about a demographic address change request|
|»» home_address|object|false|none|New home address of the person|
|»»» city|string|true|none|City of the new address|
|»»» county|string|false|none|County of the new address|
|»»» state|string|true|none|State postal code of the new address|
|»»» street_one|string|true|none|Line one of the new address|
|»»» street_two|string|false|none|Line two of the new address|
|»»» zip_code|string|true|none|Zip code of the new address|
|»» mailing_address|object|false|none|New mailing address of the person|
|»»» city|string|true|none|City of the new address|
|»»» county|string|false|none|County of the new address|
|»»» state|string|true|none|State postal code of the new address|
|»»» street_one|string|true|none|Line one of the new address|
|»»» street_two|string|false|none|Line two of the new address|
|»»» zip_code|string|true|none|Zip code of the new address|
|»» contact_change|object|false|none|Details about a demographic contact change request|
|»»» email_address|string(email)|true|none|New email address of the person|
|»»» email_address_type|string|false|none|Type of new email address|
|»»» home_phone|string|true|none|New home phone number in E.164 format|
|»»» work_phone|string|false|none|New work phone number in E.164 format|
|»» dob_change|object|false|none|Details about a demographic date of birth change request|
|»»» date_of_birth|string|true|none|ISO-8601 date string for the new or corrected date of birth of the person|
|»» employment_change|object|false|none|Details about a demographic employment change request|
|»»» employment_status|string|true|none|New or updated employee employment status|
|»»» hire_date|string|true|none|ISO-8601 date string for the new or corrected hire date of the employee|
|»»» hours_worked|integer(int32)|true|none|New number of hours worked per week by the employee|
|»»» occupation|string|false|none|New or updated employee occupation or job title|
|»» marital_status_change|object|false|none|Details about a demographic marital status change request|
|»»» marital_status|string|true|none|New marital status of the person|
|»» member|object|true|none|Details about the member making a demographic change request|
|»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»» type|string|true|none|Type of member making a demographic request|
|»» name_change|object|false|none|Details about a demographic name change request|
|»»» first_name|string|true|none|New first name of the person|
|»»» last_name|string|true|none|New last name of the person|
|»»» middle_name|string|false|none|New middle name of the person|
|»» salary_change|object|false|none|Details about a demographic salary change request|
|»»» amount|number|true|none|New or updated amount of salary earned by the employee in US dollars|
|»»» type|string|true|none|New or updated type of salary earned by the employee|
|»»» unit|string|true|none|New or updated unit of salary earned by the employee|
|»» sex_change|object|false|none|Details about a demographic sex change request|
|»»» sex|string|true|none|New sex of the person|
|»» ssn_change|object|false|none|Details about a demographic Social Security Number change request|
|»»» ssn|string|true|none|New or corrected Social Security Number of the person|

#### Enumerated Values

|Property|Value|
|---|---|
|state|AL|
|state|AK|
|state|AZ|
|state|AR|
|state|CA|
|state|CO|
|state|CT|
|state|DC|
|state|DE|
|state|FL|
|state|GA|
|state|HI|
|state|ID|
|state|IL|
|state|IN|
|state|IA|
|state|KS|
|state|KY|
|state|LA|
|state|ME|
|state|MD|
|state|MA|
|state|MI|
|state|MN|
|state|MS|
|state|MO|
|state|MT|
|state|NE|
|state|NV|
|state|NH|
|state|NJ|
|state|NM|
|state|NY|
|state|NC|
|state|ND|
|state|OH|
|state|OK|
|state|OR|
|state|PA|
|state|RI|
|state|SC|
|state|SD|
|state|TN|
|state|TX|
|state|UT|
|state|VT|
|state|VA|
|state|WA|
|state|WV|
|state|WI|
|state|WY|
|state|AL|
|state|AK|
|state|AZ|
|state|AR|
|state|CA|
|state|CO|
|state|CT|
|state|DC|
|state|DE|
|state|FL|
|state|GA|
|state|HI|
|state|ID|
|state|IL|
|state|IN|
|state|IA|
|state|KS|
|state|KY|
|state|LA|
|state|ME|
|state|MD|
|state|MA|
|state|MI|
|state|MN|
|state|MS|
|state|MO|
|state|MT|
|state|NE|
|state|NV|
|state|NH|
|state|NJ|
|state|NM|
|state|NY|
|state|NC|
|state|ND|
|state|OH|
|state|OK|
|state|OR|
|state|PA|
|state|RI|
|state|SC|
|state|SD|
|state|TN|
|state|TX|
|state|UT|
|state|VT|
|state|VA|
|state|WA|
|state|WV|
|state|WI|
|state|WY|
|email_address_type|home|
|email_address_type|work|
|employment_status|full-time|
|employment_status|part-time|
|employment_status|contract|
|employment_status|disabled|
|employment_status|terminated|
|employment_status|retired|
|marital_status|single|
|marital_status|married|
|marital_status|domestic-partner|
|marital_status|divorced|
|marital_status|widowed|
|marital_status|legally-separated|
|type|dependent|
|type|employee|
|type|hourly|
|type|salary|
|unit|annual|
|unit|month|
|unit|week|
|unit|hour|
|sex|F|
|sex|M|

<h2 id="tocSmemberrequestnewhireenrollmentrequest">MemberRequestNewHireEnrollmentRequest</h2>

<a id="schemamemberrequestnewhireenrollmentrequest"></a>

```json
{
  "coverages": [
    {
      "carrier_config": "string",
      "carrier_id": "string",
      "lines_of_coverage": {
        "add": {
          "enrolling_members": [
            {
              "id": "string",
              "member_type": "dependent",
              "plan_id": "string",
              "volume": 0
            }
          ],
          "waiving_members": [
            {
              "id": "string",
              "member_type": "dependent",
              "reason": "other-employee-group"
            }
          ]
        },
        "dental": {
          "enrolling_members": [
            {
              "id": "string",
              "member_type": "dependent",
              "plan_id": "string"
            }
          ],
          "waiving_members": [
            {
              "id": "string",
              "member_type": "dependent",
              "reason": "other-employee-group"
            }
          ]
        },
        "life": {
          "enrolling_members": [
            {
              "id": "string",
              "member_type": "dependent",
              "plan_id": "string",
              "volume": 0
            }
          ],
          "waiving_members": [
            {
              "id": "string",
              "member_type": "dependent",
              "reason": "other-employee-group"
            }
          ]
        },
        "ltd": {
          "enrolling_members": [
            {
              "id": "string",
              "member_type": "dependent",
              "plan_id": "string",
              "volume": 0
            }
          ],
          "waiving_members": [
            {
              "id": "string",
              "member_type": "dependent",
              "reason": "other-employee-group"
            }
          ]
        },
        "medical": {
          "enrolling_members": [
            {
              "id": "string",
              "member_type": "dependent",
              "plan_id": "string"
            }
          ],
          "waiving_members": [
            {
              "id": "string",
              "member_type": "dependent",
              "reason": "other-employee-group"
            }
          ]
        },
        "std": {
          "enrolling_members": [
            {
              "id": "string",
              "member_type": "dependent",
              "plan_id": "string",
              "volume": 0
            }
          ],
          "waiving_members": [
            {
              "id": "string",
              "member_type": "dependent",
              "reason": "other-employee-group"
            }
          ]
        },
        "vision": {
          "enrolling_members": [
            {
              "id": "string",
              "member_type": "dependent",
              "plan_id": "string"
            }
          ],
          "waiving_members": [
            {
              "id": "string",
              "member_type": "dependent",
              "reason": "other-employee-group"
            }
          ]
        }
      }
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|coverages|[object]|true|none|none|
|» carrier_config|string|true|none|Carrier-specific data required for new hire enrollment|
|» carrier_id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|» lines_of_coverage|object|true|none|object containing enrollment information for each line of coverage|
|»» add|object|false|none|Member transaction requests relating to AD&D coverage|
|»»» enrolling_members|[object]|false|none|none|
|»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»» member_type|string|true|none|Type of member making a member transaction request|
|»»»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»»»» volume|integer(int32)|true|none|Elected volume for the type of coverage being selected|
|»»» waiving_members|[object]|false|none|none|
|»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»» member_type|string|true|none|Type of member making a member transaction request|
|»»»» reason|string|true|none|Reason the member is choosing to waive coverage|
|»»» dental|object|false|none|Member transaction requests relating to dental coverage|
|»»»» enrolling_members|[object]|false|none|none|
|»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»» member_type|string|true|none|Type of member making a member transaction request|
|»»»»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»»»» waiving_members|[object]|false|none|none|
|»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»» member_type|string|true|none|Type of member making a member transaction request|
|»»»»» reason|string|true|none|Reason the member is choosing to waive coverage|
|»»»» life|object|false|none|Member transaction requests relating to life coverage|
|»»»»» enrolling_members|[object]|false|none|none|
|»»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»»» member_type|string|true|none|Type of member making a member transaction request|
|»»»»»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»»»»»» volume|integer(int32)|true|none|Elected volume for the type of coverage being selected|
|»»»»» waiving_members|[object]|false|none|none|
|»»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»»» member_type|string|true|none|Type of member making a member transaction request|
|»»»»»» reason|string|true|none|Reason the member is choosing to waive coverage|
|»»»»» ltd|object|false|none|Member transaction requests relating to LTD coverage|
|»»»»»» enrolling_members|[object]|false|none|none|
|»»»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»»»» member_type|string|true|none|Type of member making a member transaction request|
|»»»»»»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»»»»»»» volume|integer(int32)|false|none|Elected volume for the type of coverage being selected|
|»»»»»» waiving_members|[object]|false|none|none|
|»»»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»»»» member_type|string|true|none|Type of member making a member transaction request|
|»»»»»»» reason|string|true|none|Reason the member is choosing to waive coverage|
|»»»»»» medical|object|false|none|Member transaction requests relating to medical coverage|
|»»»»»»» enrolling_members|[object]|false|none|none|
|»»»»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»»»»» member_type|string|true|none|Type of member making a member transaction request|
|»»»»»»»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»»»»»»» waiving_members|[object]|false|none|none|
|»»»»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»»»»» member_type|string|true|none|Type of member making a member transaction request|
|»»»»»»»» reason|string|true|none|Reason the member is choosing to waive coverage|
|»»»»»»» std|object|false|none|Member transaction requests relating to STD coverage|
|»»»»»»»» enrolling_members|[object]|false|none|none|
|»»»»»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»»»»»» member_type|string|true|none|Type of member making a member transaction request|
|»»»»»»»»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»»»»»»»»» volume|integer(int32)|false|none|Elected volume for the type of coverage being selected|
|»»»»»»»» waiving_members|[object]|false|none|none|
|»»»»»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»»»»»» member_type|string|true|none|Type of member making a member transaction request|
|»»»»»»»»» reason|string|true|none|Reason the member is choosing to waive coverage|
|»»»»»»»» vision|object|false|none|Member transaction requests relating to vision coverage|
|»»»»»»»»» enrolling_members|[object]|false|none|none|
|»»»»»»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»»»»»»» member_type|string|true|none|Type of member making a member transaction request|
|»»»»»»»»»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»»»»»»»»» waiving_members|[object]|false|none|none|
|»»»»»»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»»»»»»» member_type|string|true|none|Type of member making a member transaction request|
|»»»»»»»»»» reason|string|true|none|Reason the member is choosing to waive coverage|

#### Enumerated Values

|Property|Value|
|---|---|
|member_type|dependent|
|member_type|employee|
|member_type|dependent|
|member_type|employee|
|reason|other-employee-group|
|reason|other-spouse-group|
|reason|other-parent-group|
|reason|other-ind-on-exchange|
|reason|other-ind-off-exchange|
|reason|other-cobra|
|reason|other-medicare|
|reason|medicaid|
|reason|medi-cal|
|reason|va-coverage|
|reason|tricare-coverage|
|reason|retiree-coverage|
|reason|no-coverage|
|member_type|dependent|
|member_type|employee|
|member_type|dependent|
|member_type|employee|
|reason|other-employee-group|
|reason|other-spouse-group|
|reason|other-parent-group|
|reason|other-ind-on-exchange|
|reason|other-ind-off-exchange|
|reason|other-cobra|
|reason|other-medicare|
|reason|medicaid|
|reason|medi-cal|
|reason|va-coverage|
|reason|tricare-coverage|
|reason|retiree-coverage|
|reason|no-coverage|
|member_type|dependent|
|member_type|employee|
|member_type|dependent|
|member_type|employee|
|reason|other-employee-group|
|reason|other-spouse-group|
|reason|other-parent-group|
|reason|other-ind-on-exchange|
|reason|other-ind-off-exchange|
|reason|other-cobra|
|reason|other-medicare|
|reason|medicaid|
|reason|medi-cal|
|reason|va-coverage|
|reason|tricare-coverage|
|reason|retiree-coverage|
|reason|no-coverage|
|member_type|dependent|
|member_type|employee|
|member_type|dependent|
|member_type|employee|
|reason|other-employee-group|
|reason|other-spouse-group|
|reason|other-parent-group|
|reason|other-ind-on-exchange|
|reason|other-ind-off-exchange|
|reason|other-cobra|
|reason|other-medicare|
|reason|medicaid|
|reason|medi-cal|
|reason|va-coverage|
|reason|tricare-coverage|
|reason|retiree-coverage|
|reason|no-coverage|
|member_type|dependent|
|member_type|employee|
|member_type|dependent|
|member_type|employee|
|reason|other-employee-group|
|reason|other-spouse-group|
|reason|other-parent-group|
|reason|other-ind-on-exchange|
|reason|other-ind-off-exchange|
|reason|other-cobra|
|reason|other-medicare|
|reason|medicaid|
|reason|medi-cal|
|reason|va-coverage|
|reason|tricare-coverage|
|reason|retiree-coverage|
|reason|no-coverage|
|member_type|dependent|
|member_type|employee|
|member_type|dependent|
|member_type|employee|
|reason|other-employee-group|
|reason|other-spouse-group|
|reason|other-parent-group|
|reason|other-ind-on-exchange|
|reason|other-ind-off-exchange|
|reason|other-cobra|
|reason|other-medicare|
|reason|medicaid|
|reason|medi-cal|
|reason|va-coverage|
|reason|tricare-coverage|
|reason|retiree-coverage|
|reason|no-coverage|
|member_type|dependent|
|member_type|employee|
|member_type|dependent|
|member_type|employee|
|reason|other-employee-group|
|reason|other-spouse-group|
|reason|other-parent-group|
|reason|other-ind-on-exchange|
|reason|other-ind-off-exchange|
|reason|other-cobra|
|reason|other-medicare|
|reason|medicaid|
|reason|medi-cal|
|reason|va-coverage|
|reason|tricare-coverage|
|reason|retiree-coverage|
|reason|no-coverage|

<h2 id="tocSmemberrequestopenenrollmentrequest">MemberRequestOpenEnrollmentRequest</h2>

<a id="schemamemberrequestopenenrollmentrequest"></a>

```json
{
  "add": {
    "adding_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "prior_coverage": {
          "carrier_name": "string",
          "last_coverage_date": "string"
        },
        "type": "dependent"
      }
    ],
    "modifying_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "type": "dependent"
      }
    ],
    "removing_coverage": [
      {
        "id": "string",
        "reason": "other-employee-group",
        "type": "dependent"
      }
    ]
  },
  "dental": {
    "adding_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "prior_coverage": {
          "carrier_name": "string",
          "last_coverage_date": "string"
        },
        "type": "dependent"
      }
    ],
    "modifying_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "type": "dependent"
      }
    ],
    "removing_coverage": [
      {
        "id": "string",
        "reason": "other-employee-group",
        "type": "dependent"
      }
    ]
  },
  "life": {
    "adding_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "prior_coverage": {
          "carrier_name": "string",
          "last_coverage_date": "string"
        },
        "type": "dependent"
      }
    ],
    "modifying_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "type": "dependent"
      }
    ],
    "removing_coverage": [
      {
        "id": "string",
        "reason": "other-employee-group",
        "type": "dependent"
      }
    ]
  },
  "ltd": {
    "adding_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "prior_coverage": {
          "carrier_name": "string",
          "last_coverage_date": "string"
        },
        "type": "dependent"
      }
    ],
    "modifying_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "type": "dependent"
      }
    ],
    "removing_coverage": [
      {
        "id": "string",
        "reason": "other-employee-group",
        "type": "dependent"
      }
    ]
  },
  "medical": {
    "adding_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "prior_coverage": {
          "carrier_name": "string",
          "last_coverage_date": "string"
        },
        "type": "dependent"
      }
    ],
    "modifying_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "type": "dependent"
      }
    ],
    "removing_coverage": [
      {
        "id": "string",
        "reason": "other-employee-group",
        "type": "dependent"
      }
    ]
  },
  "std": {
    "adding_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "prior_coverage": {
          "carrier_name": "string",
          "last_coverage_date": "string"
        },
        "type": "dependent"
      }
    ],
    "modifying_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "type": "dependent"
      }
    ],
    "removing_coverage": [
      {
        "id": "string",
        "reason": "other-employee-group",
        "type": "dependent"
      }
    ]
  },
  "vision": {
    "adding_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "prior_coverage": {
          "carrier_name": "string",
          "last_coverage_date": "string"
        },
        "type": "dependent"
      }
    ],
    "modifying_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "type": "dependent"
      }
    ],
    "removing_coverage": [
      {
        "id": "string",
        "reason": "other-employee-group",
        "type": "dependent"
      }
    ]
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|add|object|false|none|Member transaction requests relating to AD&D coverage|
|» adding_coverage|[object]|false|none|Details for members adding coverage|
|»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»» prior_coverage|object|false|none|Details about prior insurance coverage, if applicable|
|»»» carrier_name|string|true|none|Name of insurance carrier providing prior coverage|
|»»» last_coverage_date|string|true|none|ISO-8601 date string of the last day of coverage with the prior insurance carrier|
|»» type|string|true|none|Type of member adding coverage|
|» modifying_coverage|[object]|false|none|Details for members modifying coverage|
|»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»» type|string|true|none|Type of member modifying coverage|
|» removing_coverage|[object]|false|none|Details for members removing coverage|
|»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»» reason|string|true|none|Reason the member is removing or canceling coverage|
|»» type|string|true|none|Type of member removing coverage|
|» dental|object|false|none|Member transaction requests relating to dental coverage|
|»» adding_coverage|[object]|false|none|Details for members adding coverage|
|»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»»» prior_coverage|object|false|none|Details about prior insurance coverage, if applicable|
|»»»» carrier_name|string|true|none|Name of insurance carrier providing prior coverage|
|»»»» last_coverage_date|string|true|none|ISO-8601 date string of the last day of coverage with the prior insurance carrier|
|»»» type|string|true|none|Type of member adding coverage|
|»» modifying_coverage|[object]|false|none|Details for members modifying coverage|
|»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»»» type|string|true|none|Type of member modifying coverage|
|»» removing_coverage|[object]|false|none|Details for members removing coverage|
|»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»» reason|string|true|none|Reason the member is removing or canceling coverage|
|»»» type|string|true|none|Type of member removing coverage|
|»» life|object|false|none|Member transaction requests relating to life coverage|
|»»» adding_coverage|[object]|false|none|Details for members adding coverage|
|»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»»»» prior_coverage|object|false|none|Details about prior insurance coverage, if applicable|
|»»»»» carrier_name|string|true|none|Name of insurance carrier providing prior coverage|
|»»»»» last_coverage_date|string|true|none|ISO-8601 date string of the last day of coverage with the prior insurance carrier|
|»»»» type|string|true|none|Type of member adding coverage|
|»»» modifying_coverage|[object]|false|none|Details for members modifying coverage|
|»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»»»» type|string|true|none|Type of member modifying coverage|
|»»» removing_coverage|[object]|false|none|Details for members removing coverage|
|»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»» reason|string|true|none|Reason the member is removing or canceling coverage|
|»»»» type|string|true|none|Type of member removing coverage|
|»»» ltd|object|false|none|Member transaction requests relating to LTD coverage|
|»»»» adding_coverage|[object]|false|none|Details for members adding coverage|
|»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»»»»» prior_coverage|object|false|none|Details about prior insurance coverage, if applicable|
|»»»»»» carrier_name|string|true|none|Name of insurance carrier providing prior coverage|
|»»»»»» last_coverage_date|string|true|none|ISO-8601 date string of the last day of coverage with the prior insurance carrier|
|»»»»» type|string|true|none|Type of member adding coverage|
|»»»» modifying_coverage|[object]|false|none|Details for members modifying coverage|
|»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»»»»» type|string|true|none|Type of member modifying coverage|
|»»»» removing_coverage|[object]|false|none|Details for members removing coverage|
|»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»» reason|string|true|none|Reason the member is removing or canceling coverage|
|»»»»» type|string|true|none|Type of member removing coverage|
|»»»» medical|object|false|none|Member transaction requests relating to medical coverage|
|»»»»» adding_coverage|[object]|false|none|Details for members adding coverage|
|»»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»»»»»» prior_coverage|object|false|none|Details about prior insurance coverage, if applicable|
|»»»»»»» carrier_name|string|true|none|Name of insurance carrier providing prior coverage|
|»»»»»»» last_coverage_date|string|true|none|ISO-8601 date string of the last day of coverage with the prior insurance carrier|
|»»»»»» type|string|true|none|Type of member adding coverage|
|»»»»» modifying_coverage|[object]|false|none|Details for members modifying coverage|
|»»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»»»»»» type|string|true|none|Type of member modifying coverage|
|»»»»» removing_coverage|[object]|false|none|Details for members removing coverage|
|»»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»»» reason|string|true|none|Reason the member is removing or canceling coverage|
|»»»»»» type|string|true|none|Type of member removing coverage|
|»»»»» std|object|false|none|Member transaction requests relating to STD coverage|
|»»»»»» adding_coverage|[object]|false|none|Details for members adding coverage|
|»»»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»»»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»»»»»»» prior_coverage|object|false|none|Details about prior insurance coverage, if applicable|
|»»»»»»»» carrier_name|string|true|none|Name of insurance carrier providing prior coverage|
|»»»»»»»» last_coverage_date|string|true|none|ISO-8601 date string of the last day of coverage with the prior insurance carrier|
|»»»»»»» type|string|true|none|Type of member adding coverage|
|»»»»»» modifying_coverage|[object]|false|none|Details for members modifying coverage|
|»»»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»»»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»»»»»»» type|string|true|none|Type of member modifying coverage|
|»»»»»» removing_coverage|[object]|false|none|Details for members removing coverage|
|»»»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»»»» reason|string|true|none|Reason the member is removing or canceling coverage|
|»»»»»»» type|string|true|none|Type of member removing coverage|
|»»»»»» vision|object|false|none|Member transaction requests relating to vision coverage|
|»»»»»»» adding_coverage|[object]|false|none|Details for members adding coverage|
|»»»»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»»»»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»»»»»»»» prior_coverage|object|false|none|Details about prior insurance coverage, if applicable|
|»»»»»»»»» carrier_name|string|true|none|Name of insurance carrier providing prior coverage|
|»»»»»»»»» last_coverage_date|string|true|none|ISO-8601 date string of the last day of coverage with the prior insurance carrier|
|»»»»»»»» type|string|true|none|Type of member adding coverage|
|»»»»»»» modifying_coverage|[object]|false|none|Details for members modifying coverage|
|»»»»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»»»»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»»»»»»»» type|string|true|none|Type of member modifying coverage|
|»»»»»»» removing_coverage|[object]|false|none|Details for members removing coverage|
|»»»»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»»»»» reason|string|true|none|Reason the member is removing or canceling coverage|
|»»»»»»»» type|string|true|none|Type of member removing coverage|

#### Enumerated Values

|Property|Value|
|---|---|
|type|dependent|
|type|employee|
|type|dependent|
|type|employee|
|reason|other-employee-group|
|reason|other-spouse-group|
|reason|other-parent-group|
|reason|other-ind-on-exchange|
|reason|other-ind-off-exchange|
|reason|other-cobra|
|reason|other-medicare|
|reason|medicaid|
|reason|medi-cal|
|reason|va-coverage|
|reason|tricare-coverage|
|reason|retiree-coverage|
|reason|no-coverage|
|type|dependent|
|type|employee|
|type|dependent|
|type|employee|
|type|dependent|
|type|employee|
|reason|other-employee-group|
|reason|other-spouse-group|
|reason|other-parent-group|
|reason|other-ind-on-exchange|
|reason|other-ind-off-exchange|
|reason|other-cobra|
|reason|other-medicare|
|reason|medicaid|
|reason|medi-cal|
|reason|va-coverage|
|reason|tricare-coverage|
|reason|retiree-coverage|
|reason|no-coverage|
|type|dependent|
|type|employee|
|type|dependent|
|type|employee|
|type|dependent|
|type|employee|
|reason|other-employee-group|
|reason|other-spouse-group|
|reason|other-parent-group|
|reason|other-ind-on-exchange|
|reason|other-ind-off-exchange|
|reason|other-cobra|
|reason|other-medicare|
|reason|medicaid|
|reason|medi-cal|
|reason|va-coverage|
|reason|tricare-coverage|
|reason|retiree-coverage|
|reason|no-coverage|
|type|dependent|
|type|employee|
|type|dependent|
|type|employee|
|type|dependent|
|type|employee|
|reason|other-employee-group|
|reason|other-spouse-group|
|reason|other-parent-group|
|reason|other-ind-on-exchange|
|reason|other-ind-off-exchange|
|reason|other-cobra|
|reason|other-medicare|
|reason|medicaid|
|reason|medi-cal|
|reason|va-coverage|
|reason|tricare-coverage|
|reason|retiree-coverage|
|reason|no-coverage|
|type|dependent|
|type|employee|
|type|dependent|
|type|employee|
|type|dependent|
|type|employee|
|reason|other-employee-group|
|reason|other-spouse-group|
|reason|other-parent-group|
|reason|other-ind-on-exchange|
|reason|other-ind-off-exchange|
|reason|other-cobra|
|reason|other-medicare|
|reason|medicaid|
|reason|medi-cal|
|reason|va-coverage|
|reason|tricare-coverage|
|reason|retiree-coverage|
|reason|no-coverage|
|type|dependent|
|type|employee|
|type|dependent|
|type|employee|
|type|dependent|
|type|employee|
|reason|other-employee-group|
|reason|other-spouse-group|
|reason|other-parent-group|
|reason|other-ind-on-exchange|
|reason|other-ind-off-exchange|
|reason|other-cobra|
|reason|other-medicare|
|reason|medicaid|
|reason|medi-cal|
|reason|va-coverage|
|reason|tricare-coverage|
|reason|retiree-coverage|
|reason|no-coverage|
|type|dependent|
|type|employee|
|type|dependent|
|type|employee|
|type|dependent|
|type|employee|
|reason|other-employee-group|
|reason|other-spouse-group|
|reason|other-parent-group|
|reason|other-ind-on-exchange|
|reason|other-ind-off-exchange|
|reason|other-cobra|
|reason|other-medicare|
|reason|medicaid|
|reason|medi-cal|
|reason|va-coverage|
|reason|tricare-coverage|
|reason|retiree-coverage|
|reason|no-coverage|
|type|dependent|
|type|employee|

<h2 id="tocSmemberrequestqualifyinglifeeventrequest">MemberRequestQualifyingLifeEventRequest</h2>

<a id="schemamemberrequestqualifyinglifeeventrequest"></a>

```json
{
  "add": {
    "adding_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "prior_coverage": {
          "carrier_name": "string",
          "last_coverage_date": "string"
        },
        "type": "dependent"
      }
    ],
    "modifying_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "type": "dependent"
      }
    ],
    "removing_coverage": [
      {
        "id": "string",
        "reason": "other-employee-group",
        "type": "dependent"
      }
    ]
  },
  "dental": {
    "adding_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "prior_coverage": {
          "carrier_name": "string",
          "last_coverage_date": "string"
        },
        "type": "dependent"
      }
    ],
    "modifying_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "type": "dependent"
      }
    ],
    "removing_coverage": [
      {
        "id": "string",
        "reason": "other-employee-group",
        "type": "dependent"
      }
    ]
  },
  "event": {
    "event_date": "string",
    "event_type": "adoption"
  },
  "life": {
    "adding_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "prior_coverage": {
          "carrier_name": "string",
          "last_coverage_date": "string"
        },
        "type": "dependent"
      }
    ],
    "modifying_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "type": "dependent"
      }
    ],
    "removing_coverage": [
      {
        "id": "string",
        "reason": "other-employee-group",
        "type": "dependent"
      }
    ]
  },
  "ltd": {
    "adding_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "prior_coverage": {
          "carrier_name": "string",
          "last_coverage_date": "string"
        },
        "type": "dependent"
      }
    ],
    "modifying_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "type": "dependent"
      }
    ],
    "removing_coverage": [
      {
        "id": "string",
        "reason": "other-employee-group",
        "type": "dependent"
      }
    ]
  },
  "medical": {
    "adding_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "prior_coverage": {
          "carrier_name": "string",
          "last_coverage_date": "string"
        },
        "type": "dependent"
      }
    ],
    "modifying_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "type": "dependent"
      }
    ],
    "removing_coverage": [
      {
        "id": "string",
        "reason": "other-employee-group",
        "type": "dependent"
      }
    ]
  },
  "std": {
    "adding_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "prior_coverage": {
          "carrier_name": "string",
          "last_coverage_date": "string"
        },
        "type": "dependent"
      }
    ],
    "modifying_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "type": "dependent"
      }
    ],
    "removing_coverage": [
      {
        "id": "string",
        "reason": "other-employee-group",
        "type": "dependent"
      }
    ]
  },
  "vision": {
    "adding_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "prior_coverage": {
          "carrier_name": "string",
          "last_coverage_date": "string"
        },
        "type": "dependent"
      }
    ],
    "modifying_coverage": [
      {
        "id": "string",
        "plan_id": "string",
        "type": "dependent"
      }
    ],
    "removing_coverage": [
      {
        "id": "string",
        "reason": "other-employee-group",
        "type": "dependent"
      }
    ]
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|add|object|false|none|Member transaction requests relating to AD&D coverage|
|» adding_coverage|[object]|false|none|Details for members adding coverage|
|»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»» prior_coverage|object|false|none|Details about prior insurance coverage, if applicable|
|»»» carrier_name|string|true|none|Name of insurance carrier providing prior coverage|
|»»» last_coverage_date|string|true|none|ISO-8601 date string of the last day of coverage with the prior insurance carrier|
|»» type|string|true|none|Type of member adding coverage|
|» modifying_coverage|[object]|false|none|Details for members modifying coverage|
|»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»» type|string|true|none|Type of member modifying coverage|
|» removing_coverage|[object]|false|none|Details for members removing coverage|
|»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»» reason|string|true|none|Reason the member is removing or canceling coverage|
|»» type|string|true|none|Type of member removing coverage|
|» dental|object|false|none|Member transaction requests relating to dental coverage|
|»» adding_coverage|[object]|false|none|Details for members adding coverage|
|»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»»» prior_coverage|object|false|none|Details about prior insurance coverage, if applicable|
|»»»» carrier_name|string|true|none|Name of insurance carrier providing prior coverage|
|»»»» last_coverage_date|string|true|none|ISO-8601 date string of the last day of coverage with the prior insurance carrier|
|»»» type|string|true|none|Type of member adding coverage|
|»» modifying_coverage|[object]|false|none|Details for members modifying coverage|
|»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»»» type|string|true|none|Type of member modifying coverage|
|»» removing_coverage|[object]|false|none|Details for members removing coverage|
|»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»» reason|string|true|none|Reason the member is removing or canceling coverage|
|»»» type|string|true|none|Type of member removing coverage|
|»» event|object|true|none|Details about the eligible qualifying life event|
|»»» event_date|string|true|none|ISO-8601 date string for qualifying life event date|
|»»» event_type|string(uuid)|true|none|Qualifying life event type|
|»» life|object|false|none|Member transaction requests relating to life coverage|
|»»» adding_coverage|[object]|false|none|Details for members adding coverage|
|»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»»»» prior_coverage|object|false|none|Details about prior insurance coverage, if applicable|
|»»»»» carrier_name|string|true|none|Name of insurance carrier providing prior coverage|
|»»»»» last_coverage_date|string|true|none|ISO-8601 date string of the last day of coverage with the prior insurance carrier|
|»»»» type|string|true|none|Type of member adding coverage|
|»»» modifying_coverage|[object]|false|none|Details for members modifying coverage|
|»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»»»» type|string|true|none|Type of member modifying coverage|
|»»» removing_coverage|[object]|false|none|Details for members removing coverage|
|»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»» reason|string|true|none|Reason the member is removing or canceling coverage|
|»»»» type|string|true|none|Type of member removing coverage|
|»»» ltd|object|false|none|Member transaction requests relating to LTD coverage|
|»»»» adding_coverage|[object]|false|none|Details for members adding coverage|
|»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»»»»» prior_coverage|object|false|none|Details about prior insurance coverage, if applicable|
|»»»»»» carrier_name|string|true|none|Name of insurance carrier providing prior coverage|
|»»»»»» last_coverage_date|string|true|none|ISO-8601 date string of the last day of coverage with the prior insurance carrier|
|»»»»» type|string|true|none|Type of member adding coverage|
|»»»» modifying_coverage|[object]|false|none|Details for members modifying coverage|
|»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»»»»» type|string|true|none|Type of member modifying coverage|
|»»»» removing_coverage|[object]|false|none|Details for members removing coverage|
|»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»» reason|string|true|none|Reason the member is removing or canceling coverage|
|»»»»» type|string|true|none|Type of member removing coverage|
|»»»» medical|object|false|none|Member transaction requests relating to medical coverage|
|»»»»» adding_coverage|[object]|false|none|Details for members adding coverage|
|»»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»»»»»» prior_coverage|object|false|none|Details about prior insurance coverage, if applicable|
|»»»»»»» carrier_name|string|true|none|Name of insurance carrier providing prior coverage|
|»»»»»»» last_coverage_date|string|true|none|ISO-8601 date string of the last day of coverage with the prior insurance carrier|
|»»»»»» type|string|true|none|Type of member adding coverage|
|»»»»» modifying_coverage|[object]|false|none|Details for members modifying coverage|
|»»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»»»»»» type|string|true|none|Type of member modifying coverage|
|»»»»» removing_coverage|[object]|false|none|Details for members removing coverage|
|»»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»»» reason|string|true|none|Reason the member is removing or canceling coverage|
|»»»»»» type|string|true|none|Type of member removing coverage|
|»»»»» std|object|false|none|Member transaction requests relating to STD coverage|
|»»»»»» adding_coverage|[object]|false|none|Details for members adding coverage|
|»»»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»»»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»»»»»»» prior_coverage|object|false|none|Details about prior insurance coverage, if applicable|
|»»»»»»»» carrier_name|string|true|none|Name of insurance carrier providing prior coverage|
|»»»»»»»» last_coverage_date|string|true|none|ISO-8601 date string of the last day of coverage with the prior insurance carrier|
|»»»»»»» type|string|true|none|Type of member adding coverage|
|»»»»»» modifying_coverage|[object]|false|none|Details for members modifying coverage|
|»»»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»»»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»»»»»»» type|string|true|none|Type of member modifying coverage|
|»»»»»» removing_coverage|[object]|false|none|Details for members removing coverage|
|»»»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»»»» reason|string|true|none|Reason the member is removing or canceling coverage|
|»»»»»»» type|string|true|none|Type of member removing coverage|
|»»»»»» vision|object|false|none|Member transaction requests relating to vision coverage|
|»»»»»»» adding_coverage|[object]|false|none|Details for members adding coverage|
|»»»»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»»»»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»»»»»»»» prior_coverage|object|false|none|Details about prior insurance coverage, if applicable|
|»»»»»»»»» carrier_name|string|true|none|Name of insurance carrier providing prior coverage|
|»»»»»»»»» last_coverage_date|string|true|none|ISO-8601 date string of the last day of coverage with the prior insurance carrier|
|»»»»»»»» type|string|true|none|Type of member adding coverage|
|»»»»»»» modifying_coverage|[object]|false|none|Details for members modifying coverage|
|»»»»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»»»»» plan_id|string(uuid)|true|none|Unique identifier of the group plan in Noyo|
|»»»»»»»» type|string|true|none|Type of member modifying coverage|
|»»»»»»» removing_coverage|[object]|false|none|Details for members removing coverage|
|»»»»»»»» id|string(uuid)|true|none|Unique identifier of the employee or dependent in Noyo|
|»»»»»»»» reason|string|true|none|Reason the member is removing or canceling coverage|
|»»»»»»»» type|string|true|none|Type of member removing coverage|

#### Enumerated Values

|Property|Value|
|---|---|
|type|dependent|
|type|employee|
|type|dependent|
|type|employee|
|reason|other-employee-group|
|reason|other-spouse-group|
|reason|other-parent-group|
|reason|other-ind-on-exchange|
|reason|other-ind-off-exchange|
|reason|other-cobra|
|reason|other-medicare|
|reason|medicaid|
|reason|medi-cal|
|reason|va-coverage|
|reason|tricare-coverage|
|reason|retiree-coverage|
|reason|no-coverage|
|type|dependent|
|type|employee|
|type|dependent|
|type|employee|
|type|dependent|
|type|employee|
|reason|other-employee-group|
|reason|other-spouse-group|
|reason|other-parent-group|
|reason|other-ind-on-exchange|
|reason|other-ind-off-exchange|
|reason|other-cobra|
|reason|other-medicare|
|reason|medicaid|
|reason|medi-cal|
|reason|va-coverage|
|reason|tricare-coverage|
|reason|retiree-coverage|
|reason|no-coverage|
|type|dependent|
|type|employee|
|event_type|adoption|
|event_type|court_order|
|event_type|death|
|event_type|lost_coverage|
|event_type|divorce|
|event_type|marriage|
|event_type|newborn|
|type|dependent|
|type|employee|
|type|dependent|
|type|employee|
|reason|other-employee-group|
|reason|other-spouse-group|
|reason|other-parent-group|
|reason|other-ind-on-exchange|
|reason|other-ind-off-exchange|
|reason|other-cobra|
|reason|other-medicare|
|reason|medicaid|
|reason|medi-cal|
|reason|va-coverage|
|reason|tricare-coverage|
|reason|retiree-coverage|
|reason|no-coverage|
|type|dependent|
|type|employee|
|type|dependent|
|type|employee|
|type|dependent|
|type|employee|
|reason|other-employee-group|
|reason|other-spouse-group|
|reason|other-parent-group|
|reason|other-ind-on-exchange|
|reason|other-ind-off-exchange|
|reason|other-cobra|
|reason|other-medicare|
|reason|medicaid|
|reason|medi-cal|
|reason|va-coverage|
|reason|tricare-coverage|
|reason|retiree-coverage|
|reason|no-coverage|
|type|dependent|
|type|employee|
|type|dependent|
|type|employee|
|type|dependent|
|type|employee|
|reason|other-employee-group|
|reason|other-spouse-group|
|reason|other-parent-group|
|reason|other-ind-on-exchange|
|reason|other-ind-off-exchange|
|reason|other-cobra|
|reason|other-medicare|
|reason|medicaid|
|reason|medi-cal|
|reason|va-coverage|
|reason|tricare-coverage|
|reason|retiree-coverage|
|reason|no-coverage|
|type|dependent|
|type|employee|
|type|dependent|
|type|employee|
|type|dependent|
|type|employee|
|reason|other-employee-group|
|reason|other-spouse-group|
|reason|other-parent-group|
|reason|other-ind-on-exchange|
|reason|other-ind-off-exchange|
|reason|other-cobra|
|reason|other-medicare|
|reason|medicaid|
|reason|medi-cal|
|reason|va-coverage|
|reason|tricare-coverage|
|reason|retiree-coverage|
|reason|no-coverage|
|type|dependent|
|type|employee|
|type|dependent|
|type|employee|
|type|dependent|
|type|employee|
|reason|other-employee-group|
|reason|other-spouse-group|
|reason|other-parent-group|
|reason|other-ind-on-exchange|
|reason|other-ind-off-exchange|
|reason|other-cobra|
|reason|other-medicare|
|reason|medicaid|
|reason|medi-cal|
|reason|va-coverage|
|reason|tricare-coverage|
|reason|retiree-coverage|
|reason|no-coverage|
|type|dependent|
|type|employee|

<h2 id="tocSmemberrequestresult">MemberRequestResult</h2>

<a id="schemamemberrequestresult"></a>

```json
{
  "body": "string",
  "created": "string",
  "employee_id": "string",
  "id": "string",
  "modified": "string",
  "request_type": "string",
  "result": "string",
  "status": "string",
  "transactions": [
    "string"
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|body|string|true|none|Carrier-specific data required to execute a member request|
|created|string|true|read-only|The date the record was originated|
|employee_id|string(uuid)|true|read-only|Unique identifier of the employee in Noyo|
|id|string(uuid)|true|read-only|Unique identifer of the group location in Noyo|
|modified|string|true|read-only|The date the record was last updated|
|request_type|string|true|none|Transaction type for the member request|
|result|string|true|none|Result from executing the member request|
|status|string|true|none|Status of the member request|
|transactions|[string]|false|none|List of unique identifiers of all associated member transactions in Noyo|

<h2 id="tocSmemberrequestresultempty">MemberRequestResultEmpty</h2>

<a id="schemamemberrequestresultempty"></a>

```json
{}

```

### Properties

*None*

<h2 id="tocSmemberrequestresultfailed">MemberRequestResultFailed</h2>

<a id="schemamemberrequestresultfailed"></a>

```json
{
  "error": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|error|string|true|none|Message describing the reason a member request failed|

<h2 id="tocSmemberrequestterminationrequest">MemberRequestTerminationRequest</h2>

<a id="schemamemberrequestterminationrequest"></a>

```json
{
  "last_work_date": "2019-03-04",
  "reason": "voluntary"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|last_work_date|string|true|none|ISO-8601 date string for last day of work of the terminated employee|
|reason|string|true|none|Reason for employee termination of coverage|

#### Enumerated Values

|Property|Value|
|---|---|
|reason|voluntary|
|reason|involuntary|
|reason|job-eliminated|
|reason|loss-of-eligibility|
|reason|member-deceased|
|reason|military-leave|
|reason|leave-of-absence|

<h2 id="tocSmembertransactionresult">MemberTransactionResult</h2>

<a id="schemamembertransactionresult"></a>

```json
{
  "body": "string",
  "carrier_id": "string",
  "created": "string",
  "employee_id": "string",
  "id": "string",
  "lines_of_coverage": [
    "string"
  ],
  "member_request_id": "string",
  "modified": "string",
  "result": "string",
  "status": "string",
  "transaction_type": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|body|string|true|none|Carrier-specific data required to execute a member transaction|
|carrier_id|string(uuid)|false|read-only|Unique identifer of the carrier in Noyo|
|created|string|true|read-only|The date the record was originated|
|employee_id|string(uuid)|true|read-only|Unique identifer of the employee in Noyo|
|id|string(uuid)|true|read-only|Unique identifer of the member transaction in Noyo|
|lines_of_coverage|[string]|false|none|List of lines of coverage in the member transaction|
|member_request_id|string(uuid)|true|read-only|Unique identifer of the member request in Noyo|
|modified|string|true|read-only|The date the record was last updated|
|result|string|true|none|Carrier-specific data describing the result of a completed member transaction|
|status|string|true|none|Status of the member transaction|
|transaction_type|string|true|none|Type of the member transaction|

<h2 id="tocSmembertransactionresultempty">MemberTransactionResultEmpty</h2>

<a id="schemamembertransactionresultempty"></a>

```json
{}

```

### Properties

*None*

<h2 id="tocSmembertransactionresultfailed">MemberTransactionResultFailed</h2>

<a id="schemamembertransactionresultfailed"></a>

```json
{
  "error": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|error|string|true|none|Message describing the reason a member transaction failed|

<h2 id="tocSperson">Person</h2>

<a id="schemaperson"></a>

```json
{
  "contact": {
    "email_address": "catherine.briggs@example.com",
    "email_address_type": "home",
    "home_phone": "+15555551212",
    "preferred_language": "Engish",
    "preferred_method": "email",
    "speaks_english": true,
    "work_phone": "+15555551212"
  },
  "date_of_birth": "1977-06-03",
  "details": {
    "american_indian": {
      "state": "AL",
      "tribe": "Ojibwe"
    },
    "disability": {
      "communication": true,
      "reason": "string"
    },
    "is_military": true,
    "is_student": true,
    "tobacco": {
      "duration": "5 years",
      "frequency": "daily",
      "types": "cigarettes"
    }
  },
  "first_name": "Catherine",
  "home_address": {
    "city": "Bend",
    "county": "Deschutes",
    "state": "OR",
    "street_one": "339 Hickory Street",
    "street_two": "Apartment 5",
    "zip_code": "97701"
  },
  "last_name": "Briggs",
  "maiden_name": "Adams",
  "mailing_address": {
    "city": "Bend",
    "county": "Deschutes",
    "state": "OR",
    "street_one": "339 Hickory Street",
    "street_two": "Apartment 5",
    "zip_code": "97701"
  },
  "marital_status": "married",
  "middle_name": "S",
  "sex": "F",
  "ssn": "123456789",
  "suffix": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|contact|object|true|none|Contact information for the person|
|» email_address|string(email)|true|none|Email address of the person|
|» email_address_type|string|false|none|Type of email address|
|» home_phone|string|true|none|Home phone number in E.164 format|
|» preferred_language|string|false|none|Preferred written or spoken language of the person|
|» preferred_method|string|false|none|Preferred method of contact for the person|
|» speaks_english|boolean|false|none|True if the person can speak or communicate in English|
|» work_phone|string|false|none|Work phone number in E.164 format|
|date_of_birth|string(date)|true|none|ISO-8601 date string for the date of birth of the person|
|details|object|false|none|Additional personal details of the person|
|» american_indian|object|false|none|American Indian status details (if applicable)|
|»» state|string|false|none|Primary state of the federally-recognized American Indian or Alaska Native tribe, if applicable|
|»» tribe|string|false|none|Name of the federally-recognized American Indian or Alaska Native tribe, if applicable|
|» disability|object|false|none|Disability details (if applicable)|
|»» communication|boolean|false|none|True if the disability impacts the ability to communicate or read|
|»» reason|string|false|none|Description of the disability|
|» is_military|boolean|false|none|True if the person is in the military|
|» is_student|boolean|false|none|True if the person is a student|
|» tobacco|object|false|none|Tobacco usage details (if applicable)|
|»» duration|string|false|none|Duration of tobacco use|
|»» frequency|string|false|none|Frequency of tobacco use|
|»» types|[string]|false|none|Type of tobacco use|
|» first_name|string|true|none|First name of the person|
|» home_address|[Address](#schemaaddress)|true|none|Home address of the person|
|» last_name|string|true|none|Last name of the person|
|» maiden_name|string|false|none|Maiden name of the person (if applicable)|
|» mailing_address|[Address](#schemaaddress)|false|none|Mailing address of the person, if different from home address|
|» marital_status|string|false|none|Marital status of the person|
|» middle_name|string|false|none|Middle name of the person|
|» sex|string|true|none|Sex of the person|
|» ssn|string|false|none|Social Security Number of the person|
|» suffix|string|false|none|Suffix of the person|

#### Enumerated Values

|Property|Value|
|---|---|
|email_address_type|home|
|email_address_type|work|
|preferred_method|mail|
|preferred_method|email|
|preferred_method|home-phone|
|preferred_method|work-phone|
|preferred_method|other|
|state|AL|
|state|AK|
|state|AZ|
|state|AR|
|state|CA|
|state|CO|
|state|CT|
|state|DC|
|state|DE|
|state|FL|
|state|GA|
|state|HI|
|state|ID|
|state|IL|
|state|IN|
|state|IA|
|state|KS|
|state|KY|
|state|LA|
|state|ME|
|state|MD|
|state|MA|
|state|MI|
|state|MN|
|state|MS|
|state|MO|
|state|MT|
|state|NE|
|state|NV|
|state|NH|
|state|NJ|
|state|NM|
|state|NY|
|state|NC|
|state|ND|
|state|OH|
|state|OK|
|state|OR|
|state|PA|
|state|RI|
|state|SC|
|state|SD|
|state|TN|
|state|TX|
|state|UT|
|state|VT|
|state|VA|
|state|WA|
|state|WV|
|state|WI|
|state|WY|
|marital_status|single|
|marital_status|married|
|marital_status|domestic-partner|
|marital_status|divorced|
|marital_status|widowed|
|marital_status|legally-separated|
|sex|F|
|sex|M|

