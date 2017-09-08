--- 

title: Marin API 

language_tabs: 
   - shell: cURL
   - http: HTTP

toc_footers: 
   - <a href='#'>Sign Up for a Developer Key</a> 
   - <a href='https://github.com/lavkumarv'>Documentation Powered by lav</a> 

includes: 
   - errors 

search: true 

--- 

# Introduction 

The Marin API is a REST API that provides users the means to programmatically retrieve and update their Marin data. The document will provide you with everything you need to get started using the API.

## Where it's Located

The Marin API's base URL is located at the following location:

`https://api.marinsoftware.com`

## Versioning

The Marin API is versioned. This means there can be multiple versions available at any one time. Currently only one version is available:

| Version | Comments |
| ------- | -------- |
| v1.0 | Initial Version of the Marin API |

The version number is specified after the base URL:

`GET https://api.marinsoftware.com/v1.0/clients`

## How it's Structured

Requests are structured using the following:

`GET {base-api-url}/v{version}/{endpoint}`

For instance, to get a list of all clients you have access to:

`GET https://api.marinsoftware.com/v1.0/clients`

Please see the API Reference section for a full list of available endpoints.


# Authentication 


All endpoints require an access token in order to make requests. To get an access token you will need to make a request to the [auth](#auth) endpoint with your username and password that you will use to access the API.

This token will need to be supplied in the header for all requests to the API.

Access token are currently valid for 48 hours. If your access token expires you will need to request a new one.


## Get access token

```shell
curl -X POST \
  https://api.marinsoftware.com/v1.0/auth \
  -H 'cache-control: no-cache' \
  -d '{
  "username": "joe.bloggs@marinsoftware.com",
  "password": "mypasword"
}'
```

```http
POST /v1.0/auth HTTP/1.1
Host: api.marinsoftware.com
Content-Type: application/json

{
  "username": "joe.bloggs@marinsoftware.com",
  "password": "mypassword"
}
```

> The above command will return an authorization token that needs to be included for all requests

```json
{
  "result": {
    "token": "st_3ce5c84d242582de600a1312a5548838f95bd1a24d25299d"
  }
}
```

Get an access token to access the API.

### HTTP Request 
`POST /v1.0/auth` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| body | body | json object of the form: `{ "username": "myUsername", "password": "mypassword"}` | Yes | string |


## Using the access token

```shell
curl -X GET \
  'https://api.marinsoftware.com/v1.0/clients?status=ACTIVE' \
  -H 'token: st_466e53e5224fb6b54efa3bda8716fe498ebb41fa65be3dbd'
```

```http
GET /clients?status=ACTIVE HTTP/1.1
Host: api.marinsoftware.com
token: st_466e53e5224fb6b54efa3bda8716fe498ebb41fa65be3dbd
```

Once you have your access token you will need to include it in the HTTP header for any requests. Please see an example on the right for including the token in your request.

# Using the Marin API

## Reading data (GET)

```shell
curl -X GET \
  'https://api.marinsoftware.com/v1.0/clients?status=ACTIVE' \
  -H 'token: st_466e53e5224fb6b54efa3bda8716fe498ebb41fa65be3dbd'
```

```http
GET /clients?status=ACTIVE HTTP/1.1
Host: api.marinsoftware.com
token: st_466e53e5224fb6b54efa3bda8716fe498ebb41fa65be3dbd

```

> The data will be sent back in JSON in the results field, along with some metadata about the request:

```json
{
  "requestId": "25832c5f-ac29-4ba5-be46-9384c100a508",
  "startedAt": "2017-07-04 15:31:11",
  "completedAt": "2017-07-04 15:31:15",
  "timeTakenMs": 4101,
  "totalResults": 12671,
  "errors": false,
  "results": [
    {
      "customerId": 1,
      "id": 1866768,
      "name": "My Client",
      "status": "ACTIVE",
      "currency": "USD"
    },
    {
      "customerId": 1,
      "id": 1866769,
      "name": "My Old Client",
      "status": "INACTIVE",
      "currency": "GBP"
    }
  ]
}
```

To retrieve data for an endpoint, simply send a GET request, using any additional parameters that are supported by the endpoint. 

## Field selection

```shell
curl -X GET \
  'https://prod-vip-marin-open-api-service-lv:3000/v1.0/clients/1808782/keywords?fields=keyword,keyword_name,type,pub_clicks,date' \
  -H 'token: st_466e53e5224fb6b54efa3bda8716fe498ebb41fa65be3dbd'
```

```http
GET /v1.0/clients/1808782/keywords?fields=keyword,keyword_name,type,pub_clicks,date HTTP/1.1
Host: api.marinsoftware.com
token: st_466e53e5224fb6b54efa3bda8716fe498ebb41fa65be3dbd

```

Select which fields you want using the `fields` parameter. If you don't select any fields you will get a small set of default fields. 

Each publisher object endpoint lists the fields available for selection. The `fields` parameter is available on all the publisher object endpoints ([publisherAccounts](#publisher-accounts), [campaigns](#campaigns), [groups](#groups), [keywords](#keywords), [creatives](#creatives)).

## Filters

```shell
curl -X GET \
  'https://api.marinsoftware.com/clients/1808782/keywords?campaign_name=Brand&status=ACTIVE' \
  -H 'token: st_466e53e5224fb6b54efa3bda8716fe498ebb41fa65be3dbd'
```

```http
GET /v1.0/clients/1808782/keywords?campaign_name=Brand&amp;status=ACTIVE HTTP/1.1
Host: api.marinsoftware.com
token: st_466e53e5224fb6b54efa3bda8716fe498ebb41fa65be3dbd
```

You can perform filtering based on equality conditions by adding a parameter of the same name to the request. The example on the right shows getting all active keywords from the “Brand” campaign.

Only equality conditions are supported currently but we will be adding support to other common operators. Multiple conditions are AND'd together.

Each publisher object endpoint lists the fields available for filtering. Filtering is available on all the publisher object endpoints ([publisherAccounts](#publisher-accounts), [campaigns](#campaigns), [groups](#groups), [keywords](#keywords), [creatives](#creatives)).


## Sorting

```shell
curl -X GET \
  'https://api.marinsoftware.com/clients/1808782/keywords?sort=-pub_clicks' \
  -H 'token: st_466e53e5224fb6b54efa3bda8716fe498ebb41fa65be3dbd'
```

```http
GET /v1.0/clients/1808782/keywords?sort=-pub_clicks HTTP/1.1
Host: api.marinsoftware.com
token: st_466e53e5224fb6b54efa3bda8716fe498ebb41fa65be3dbd
```

Sort your response using the `sort` parameter. Results can only be sorted by a single parameter. 

You can change the direction of the sort by adding a sorting modifier:

Ascending:

`sort=+pub_clicks`

Descending:

`sort=-pub_clicks`

No sorting modifier defaults to ascending. 

Each publisher object endpoint lists the fields available for sorting. The `sort` parameter is available on all the publisher object endpoints ([publisherAccounts](#publisher-accounts), [campaigns](#campaigns), [groups](#groups), [keywords](#keywords), [creatives](#creatives)).

## Paging

```shell
curl -X GET \
  'https://api.marinsoftware.com/clients/1808782/keywords?limit=25&offset=25' \
  -H 'token: st_466e53e5224fb6b54efa3bda8716fe498ebb41fa65be3dbd'
```

```http
GET /v1.0/clients/1808782/keywords?limit=25&offset=25 HTTP/1.1
Host: api.marinsoftware.com
token: st_466e53e5224fb6b54efa3bda8716fe498ebb41fa65be3dbd
```

Use the `limit` parmeters to limit results:

`limit=25`

Uesr the `limit` and `offset` parameter together to page results:

`limit=25&offset=25`

If you don't supply a limit you will get all results back. This will change in the future and we will have a maximum result size you can specify (e.g. 1000).

The `limit` and `offset` parameters are available on all the publisher object endpoints ([publisherAccounts](#publisher-accounts), [campaigns](#campaigns), [groups](#groups), [keywords](#keywords), [creatives](#creatives)).

## Dates

When requesting metrics, use the `from` and `to` parameter to specify which period you want your metrics to aggregate over.

```shell
curl -X GET \
  'https://api.marinsoftware.com/clients/1808782/keywords?
    fields=keyword,keyword_name,type,pub_clicks&
    from=2017-01-01&
    to=2017-06-26' \
  -H 'token: st_466e53e5224fb6b54efa3bda8716fe498ebb41fa65be3dbd'
```

```http
GET /v1.0/clients/1808782/keywords?fields=keyword,keyword_name,type,pub_clicks&from=2017-01-01&to=2017-06-26 HTTP/1.1
Host: api.marinsoftware.com
token: st_466e53e5224fb6b54efa3bda8716fe498ebb41fa65be3dbd
```

If you don't supply `from` and `to` parameters then all available data will be taken into account.

To view your metrics broken down by a time period (e.g. month, week, day) etc just add the appropriate date fields to your request. Metrics will automatically be aggregated by whatever date fields you choose. For instance to view the same conversion metrics above broken down by day, we'll add the `date` field to our list of fields:

```shell
curl -X GET \
  'https://api.marinsoftware.com/clients/1808782/keywords?
    fields=keyword,keyword_name,type,pub_clicks,date&
    from=2017-01-01&
    to=2017-06-26' \
  -H 'token: st_466e53e5224fb6b54efa3bda8716fe498ebb41fa65be3dbd'
```

```http
GET /v1.0/clients/1808782/keywords?fields=keyword,keyword_name,type,pub_clicks,date&from=2017-01-01&to=2017-06-26 HTTP/1.1
Host: api.marinsoftware.com
token: st_466e53e5224fb6b54efa3bda8716fe498ebb41fa65be3dbd
```

Metrics are available on all the publisher object endpoints ([publisherAccounts](#publisher-accounts), [campaigns](#campaigns), [groups](#groups), [keywords](#keywords), [creatives](#creatives)).


# Customers

A Marin customer account. Customers sit above Marin clients.

## Get all customers

```shell
curl -X GET \
  'https://api.marinsoftware.com/v1.0/customers?status=ACTIVE' \
  -H 'token: st_466e53e5224fb6b54efa3bda8716fe498ebb41fa65be3dbd'
```

```http
GET /v1.0/customers?status=ACTIVE HTTP/1.1
Host: api.marinsoftware.com
token: st_466e53e5224fb6b54efa3bda8716fe498ebb41fa65be3dbd
```

> The above command returns JSON structured like this:

```json
{
  "id": 8852,
  "name": "Pow Pow Sports",
  "country": null,
  "status": "ACTIVE",
  "apiKey": null,
  "apiOn": null,
  "apiEnabledAt": null,
  "apiDisabledAt": null,
  "parentId": null,
  "decoration": null,
  "needCustomId": null,
  "betaFeatures": null,
  "legacyId": null,
  "legacyMarinId": null,
  "createdAt": "2017-02-28 18:19:55",
  "createdBy": 1,
  "updatedAt": "2017-02-28 18:19:55",
  "updatedBy": 1,
  "passwordComplexity": "DEFAULT",
  "locale": "en_US",
  "currency": "USD",
  "trialLength": 30,
  "dataRetentionLifespan": 100,
  "autoLinkCampaigns": false,
  "disableUserInactiveWindow": 1000,
  "application": "ENTERPRISE"
}
```

Get all your customers' details.

### HTTP Request 
`GET /customers` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Authorization token| Yes | string |
| id | query | Filter on the customer's ID | No | string |
| status | query | Filter on the customer's status  | No | enum(ACTIVE, INACTIVE) |
| legacyId | query | Filter on customer's legacy (Search 1.0) ID | No | string |
| name | query | Filter on the customer's full name | No | string |

## Get a single customer

```shell
curl -X GET \
  'https://api.marinsoftware.com/v1.0/customers/8852' \
  -H 'token: st_466e53e5224fb6b54efa3bda8716fe498ebb41fa65be3dbd'
```

```http
GET /customers/8852 HTTP/1.1
Host: api.marinsoftware.com
token: st_466e53e5224fb6b54efa3bda8716fe498ebb41fa65be3dbd
```

> The above command returns JSON structured like this:

```json
{
  "id": 8852,
  "name": "Pow Pow Sports",
  "country": null,
  "status": "ACTIVE",
  "apiKey": null,
  "apiOn": null,
  "apiEnabledAt": null,
  "apiDisabledAt": null,
  "parentId": null,
  "decoration": null,
  "needCustomId": null,
  "betaFeatures": null,
  "legacyId": null,
  "legacyMarinId": null,
  "createdAt": "2017-02-28 18:19:55",
  "createdBy": 1,
  "updatedAt": "2017-02-28 18:19:55",
  "updatedBy": 1,
  "passwordComplexity": "DEFAULT",
  "locale": "en_US",
  "currency": "USD",
  "trialLength": 30,
  "dataRetentionLifespan": 100,
  "autoLinkCampaigns": false,
  "disableUserInactiveWindow": 1000,
  "application": "ENTERPRISE"
}
```

Get an individual customer's details given its ID.

### HTTP Request 
`GET /customers/{customerId}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Authorization token| Yes | string |
| customerId | path | The customer ID | Yes | string |


# Clients

Marin client accounts. 

## Get all clients

```shell
curl -X GET \
  'https://api.marinsoftware.com/v1.0/clients?status=ACTIVE' \
  -H 'token: st_466e53e5224fb6b54efa3bda8716fe498ebb41fa65be3dbd'
```

```http
GET /clients?status=ACTIVE HTTP/1.1
Host: api.marinsoftware.com
token: st_466e53e5224fb6b54efa3bda8716fe498ebb41fa65be3dbd

```

> The above command returns JSON structured like this:

```json
{
  "customerId": 8852,
  "id": 1808782,
  "name": "Pow Pow Sports - Golf",
  "status": "ACTIVE",
  "currency": "GBP",
  "logo": null,
  "reportingComponentsId": null,
  "campaignFormat": null,
  "adFormat": null,
  "contactName": null,
  "contactMail": null,
  "contactPhone": null,
  "pitchSmartAgeRangeOn": null,
  "pitchSmartAgeRange": null,
  "createdAt": "2016-08-16 09:19:43",
  "createdBy": null,
  "updatedAt": null,
  "updatedBy": null,
  "hasArchives": null,
  "autoPauseOn": null,
  "autoPauseVal": null,
  "legacyIds": [
    "57877"
  ],
  "jobTitle": null,
  "notes": null,
  "timezone": "Australia\/Sydney",
  "locale": "en_AU",
  "reportingWeek": null,
  "trackingId": "4251ll257877",
  "trackingParams": null,
  "trackingParamDelimiter": null,
  "trackingParamStopChars": null,
  "trackingParamConcat": null,
  "sitelinkParam": null,
  "sitelinkParamDelimiter": null,
  "sitelinkParamStopChar": null,
  "urlbOnSync": null,
  "phoneExtension": null,
  "dataRetentionLifespan": null,
  "customerName": "Marin Software",
  "permissions": [
    {
      "accessRoleClientId": 1808782,
      "accessRoleUserId": 4029888,
      "id": 2982232,
      "accessRoles": [
        "READ",
        "WRITE"
      ]
    },
    {
      "accessRoleClientId": 1808782,
      "accessRoleUserId": 5831303,
      "id": 6043379,
      "accessRoles": [
        "READ",
        "WRITE"
      ]
    }
  ]
}
```

Get all your Marin client accounts.

### HTTP Request 
`GET /clients` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Authorization token | Yes | string |
| id | query | Filter on the client's ID  | No | string |
| status | query | Filter on the client's status | No | enum(ACTIVE, PAUSED, DELETED) |
| currency | query | Filter on the client's currency | No | 3 digit ISO Currency Code |
| legacyId | query | Filter on the client's legacy (Search 1.0) ID | No | string |
| name | query | Filter on the client's full name | No | string |

## Get a single client

```shell
curl -X GET \
  'https://api.marinsoftware.com/v1.0/clients/1808782' \
  -H 'token: st_466e53e5224fb6b54efa3bda8716fe498ebb41fa65be3dbd'
```

```http
GET /clients/1808782 HTTP/1.1
Host: api.marinsoftware.com
token: st_466e53e5224fb6b54efa3bda8716fe498ebb41fa65be3dbd
```

> The above command returns JSON structured like this:

```json
{
  "customerId": 8852,
  "id": 1808782,
  "name": "Pow Pow Sports - Golf",
  "status": "ACTIVE",
  "currency": "GBP",
  "logo": null,
  "reportingComponentsId": null,
  "campaignFormat": null,
  "adFormat": null,
  "contactName": null,
  "contactMail": null,
  "contactPhone": null,
  "pitchSmartAgeRangeOn": null,
  "pitchSmartAgeRange": null,
  "createdAt": "2016-08-16 09:19:43",
  "createdBy": null,
  "updatedAt": null,
  "updatedBy": null,
  "hasArchives": null,
  "autoPauseOn": null,
  "autoPauseVal": null,
  "legacyIds": [
    "57877"
  ],
  "jobTitle": null,
  "notes": null,
  "timezone": "Australia\/Sydney",
  "locale": "en_AU",
  "reportingWeek": null,
  "trackingId": "4251ll257877",
  "trackingParams": null,
  "trackingParamDelimiter": null,
  "trackingParamStopChars": null,
  "trackingParamConcat": null,
  "sitelinkParam": null,
  "sitelinkParamDelimiter": null,
  "sitelinkParamStopChar": null,
  "urlbOnSync": null,
  "phoneExtension": null,
  "dataRetentionLifespan": null,
  "customerName": "Marin Software",
  "permissions": [
    {
      "accessRoleClientId": 1808782,
      "accessRoleUserId": 4029888,
      "id": 2982232,
      "accessRoles": [
        "READ",
        "WRITE"
      ]
    },
    {
      "accessRoleClientId": 1808782,
      "accessRoleUserId": 5831303,
      "id": 6043379,
      "accessRoles": [
        "READ",
        "WRITE"
      ]
    }
  ]
}
```

Get an individual Marin client account given its ID.

### HTTP Request 
`GET /clients/{clientId}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Authorization token | Yes | string |
| clientId | path | The client ID | Yes | string |

## Get all clients for a user

```shell
curl -X GET \
  https://api.marinsoftware.com/v1.0/users/37185/clients \
  -H 'token: st_3ce5c84d242582de600a1312a5548838f95bd1a24d25299d'
```

```http
GET /v1.0/users/37185/clients HTTP/1.1
Host: api.marinsoftware.com
token: st_3ce5c84d242582de600a1312a5548838f95bd1a24d25299d
```

> The above command returns JSON structured like this:

```json
{
  "customerId": 8852,
  "id": 1808782,
  "name": "Pow Pow Sports - Golf",
  "status": "ACTIVE",
  "currency": "GBP",
  "logo": null,
  "reportingComponentsId": null,
  "campaignFormat": null,
  "adFormat": null,
  "contactName": null,
  "contactMail": null,
  "contactPhone": null,
  "pitchSmartAgeRangeOn": null,
  "pitchSmartAgeRange": null,
  "createdAt": "2016-08-16 09:19:43",
  "createdBy": null,
  "updatedAt": null,
  "updatedBy": null,
  "hasArchives": null,
  "autoPauseOn": null,
  "autoPauseVal": null,
  "legacyIds": [
    "57877"
  ],
  "jobTitle": null,
  "notes": null,
  "timezone": "Australia\/Sydney",
  "locale": "en_AU",
  "reportingWeek": null,
  "trackingId": "4251ll257877",
  "trackingParams": null,
  "trackingParamDelimiter": null,
  "trackingParamStopChars": null,
  "trackingParamConcat": null,
  "sitelinkParam": null,
  "sitelinkParamDelimiter": null,
  "sitelinkParamStopChar": null,
  "urlbOnSync": null,
  "phoneExtension": null,
  "dataRetentionLifespan": null,
  "customerName": "Marin Software",
  "permissions": [
    {
      "accessRoleClientId": 1808782,
      "accessRoleUserId": 4029888,
      "id": 2982232,
      "accessRoles": [
        "READ",
        "WRITE"
      ]
    },
    {
      "accessRoleClientId": 1808782,
      "accessRoleUserId": 5831303,
      "id": 6043379,
      "accessRoles": [
        "READ",
        "WRITE"
      ]
    }
  ]
}
```

Get all clients that a particular user has access to.

### HTTP Request 
`GET /users/{userId}/clients` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Authorization token | Yes | string |
| userId | path | The user ID | Yes | number |

# Users

Marin app users.

## Get all users

```shell
curl -X GET \
  https://api.marinsoftware.com/v1.0/clients/1808782/users \
  -H 'token: st_466e53e5224fb6b54efa3bda8716fe498ebb41fa65be3dbd'
```

```http
GET /v1.0/clients/1808782/users HTTP/1.1
Host: api.marinsoftware.com
token: st_466e53e5224fb6b54efa3bda8716fe498ebb41fa65be3dbd
```

> The above command returns JSON structured like this:

```json
{
  "customerId": 8852,
  "id": 37185,
  "legacyIds": null,
  "name": "Joe Bloggs",
  "userName": "joe.bloggs@marinsoftware.com",
  "email": "joe.bloggs@marinsoftware.com",
  "jobTitle": null,
  "passwordSalt": null,
  "roles": [
    "ROLE_SUPER_ADMIN"
  ],
  "firstName": "UAT User",
  "lastName": null,
  "lastConnection": null,
  "status": "ACTIVE",
  "preferences": null,
  "image": null,
  "phoneNumber": null,
  "language": "en",
  "phoneExtension": null,
  "features": "ADMIN_FEATURES",
  "accessLevel": "FULL",
  "createdAt": "2017-02-28 23:55:27",
  "createdBy": 1,
  "updatedAt": "2017-02-28 23:55:27",
  "updatedBy": 1,
  "forcePasswordReset": null,
  "msgBstrOptObjs": null,
  "msgBstrOptHidAudCrtr": null,
  "msgBstrOptHidRlesHandl": null,
  "msgBstrOptAllPges": null,
  "customerName": "Customer Success Accounts",
  "permissions": [
    {
      "accessRoleClientId": 1808782,
      "accessRoleUserId": 37185,
      "id": 737953,
      "accessRoles": [
        "READ",
        "WRITE"
      ]
    },
    {
      "accessRoleClientId": 63240,
      "accessRoleUserId": 37185,
      "id": 2936,
      "accessRoles": [
        "READ",
        "WRITE"
      ]
    }
  ]
}
```

Gets all users under a particular client.

### HTTP Request 
`GET /clients/{clientId}/users` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Authorization token | Yes | string |
| clientId | query | The client ID | Yes | string |

## Get a single user

```shell
curl -X GET \
  https://api.marinsoftware.com/v1.0/users/37185 \
  -H 'token: st_466e53e5224fb6b54efa3bda8716fe498ebb41fa65be3dbd'
```

```http
GET /v1.0/users/37185 HTTP/1.1
Host: api.marinsoftware.com
token: st_466e53e5224fb6b54efa3bda8716fe498ebb41fa65be3dbd
```

> The above command returns JSON structured like this:

```json
{
  "customerId": 8852,
  "id": 37185,
  "legacyIds": null,
  "name": "Joe Bloggs",
  "userName": "joe.bloggs@marinsoftware.com",
  "email": "joe.bloggs@marinsoftware.com",
  "jobTitle": null,
  "passwordSalt": null,
  "roles": [
    "ROLE_SUPER_ADMIN"
  ],
  "firstName": "UAT User",
  "lastName": null,
  "lastConnection": null,
  "status": "ACTIVE",
  "preferences": null,
  "image": null,
  "phoneNumber": null,
  "language": "en",
  "phoneExtension": null,
  "features": "ADMIN_FEATURES",
  "accessLevel": "FULL",
  "createdAt": "2017-02-28 23:55:27",
  "createdBy": 1,
  "updatedAt": "2017-02-28 23:55:27",
  "updatedBy": 1,
  "forcePasswordReset": null,
  "msgBstrOptObjs": null,
  "msgBstrOptHidAudCrtr": null,
  "msgBstrOptHidRlesHandl": null,
  "msgBstrOptAllPges": null,
  "customerName": "Customer Success Accounts",
  "permissions": [
    {
      "accessRoleClientId": 1808782,
      "accessRoleUserId": 37185,
      "id": 737953,
      "accessRoles": [
        "READ",
        "WRITE"
      ]
    },
    {
      "accessRoleClientId": 63240,
      "accessRoleUserId": 37185,
      "id": 2936,
      "accessRoles": [
        "READ",
        "WRITE"
      ]
    }
  ]
}
```

Get an individual user's details given its ID.

### HTTP Request 
`GET /users/{userId}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Authorization token | Yes | string |
| userId | path | The user ID | Yes | number |


# Publisher Accounts


## Get all publisher accounts

```shell
curl -X GET \
  'https://api.marinsoftware.com/v1.0/clients/1808782/publisherAccounts?
    limit=100&
    offset=0&
    fields=pca%2Cpublisher%2Cclient%2Cpca_alias%2Cpublisher_name%2Cpub_cost%2Cpub_clicks%2Cimpressions&
    from=2017-05-01&
    to=2017-05-02&
    sort=pca_alias' \
  -H 'token: st_cfc8909710f0d020f95dcec821fad453a248ebc7a7f9438f'
```

```http
GET /v1.0/clients/1808782/publisherAccounts?limit=100&amp;offset=0&amp;fields=pca,publisher,client,pca_alias,publisher_name,pub_cost,pub_clicks,impressions&amp;from=2017-05-01&amp;to=2017-05-02&amp;sort=pca_alias HTTP/1.1
Host: api.marinsoftware.com
token: st_cfc8909710f0d020f95dcec821fad453a248ebc7a7f9438f
```

> The above command returns JSON structured like this:

```json
{
  "results": [
    {
      "pub_clicks": 475,
      "publisher_name": "MSN",
      "pca_alias": "Powpow-Bing-SEM",
      "publisher": 6,
      "client": 1808782,
      "pub_cost": 451539997,
      "pca": 1591619
    },
    {
      "pub_clicks": 0,
      "publisher_name": "Google",
      "pca_alias": "Powpow-Google-SEM",
      "publisher": 4,
      "client": 1808782,
      "pub_cost": 0,
      "pca": 1573468
    }
  ]
}
```

Get a list of publisher accounts for a client and any associated cost/conversion metrics.


### HTTP Request 
`GET /clients/{clientId}/publisherAccounts` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Authorization token | Yes | string |
| clientId | path | The Marin client ID | Yes | string |
| limit | query | Number of rows to return in a single response. See [paging](#paging) for more info. | Yes | number |
| offset | query | The starting row number for a single response. See [paging](#paging) for more info. | Yes | number |
| fields | query | A comma separated list of fields including attributes, metrics and dates to include in the response. Please see [field selection](#field-selection) for more info. | No | string |
| from | query | Any metrics requested will be retrieved from this date. See [dates](#dates) for more info. | No | date (yyyy-mm-dd) |
| to | query | Any metrics requested will be retrieved to this date. See [dates](#dates) for more info. | No | date (yyyy-mm-dd) |
| sort | query | The field you'd like to sort your response by. See [sorting](#sorting) for more info.| No | string |

In addition you can use any valid [publisher account field](#publisher-account-fields) to filter your request using equality conditions. Please see the [filters](#filters) section for more info on how to filter. 

## Get a single publisher account

```shell
curl -X GET \
  'https://api.marinsoftware.com/v1.0/clients/1808782/publisherAccounts/1591619?
    fields=pca%2Cpublisher%2Cclient%2Cpca_alias%2Cpublisher_name%2Cpub_cost%2Cpub_clicks%2Cimpressions&
    from=2017-05-01&
    to=2017-05-02' \
  -H 'token: st_cfc8909710f0d020f95dcec821fad453a248ebc7a7f9438f'
```

```http
GET /v1.0/clients/1808782/publisherAccounts/1591619?fields=pca,publisher,client,pca_alias,publisher_name,pub_cost,pub_clicks,impressions&amp;from=2017-05-01&amp;to=2017-05-02 HTTP/1.1
Host: api.marinsoftware.com
token: st_cfc8909710f0d020f95dcec821fad453a248ebc7a7f9438f
```

> The above command returns JSON structured like this:

```json
{
  "results": [
    {
      "pub_clicks": 475,
      "publisher_name": "MSN",
      "pca_alias": "Powpow-Bing-SEM",
      "publisher": 6,
      "client": 1808782,
      "pub_cost": 451539997,
      "pca": 1591619
    }
  ]
}
```

Get an individual publisher account given it's ID and any associated cost/conversion metrics

### HTTP Request 
`GET /clients/{clientId}/publisherAccounts/{pubAccountId}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Authorization token | Yes | string |
| clientId | path | The Marin client ID | Yes | string |
| pubAccountId | path | The Marin publisher account ID | Yes | string |
| limit | query | Number of rows to return in a single response. See [paging](#paging) for more info. | Yes | number |
| offset | query | The starting row number for a single response. See [paging](#paging) for more info. | Yes | number |
| fields | query | A comma separated list of fields including attributes, metrics and dates to include in the response. Please see [field selection](#field-selection) for more info. | No | string |
| from | query | Any metrics requested will be retrieved from this date. See [dates](#dates) for more info. | No | date (yyyy-mm-dd) |
| to | query | Any metrics requested will be retrieved to this date. See [dates](#dates) for more info. | No | date (yyyy-mm-dd) |
| sort | query | The field you'd like to sort your response by. See [sorting](#sorting) for more info.| No | string |

In addition you can use any valid [publisher account field](#publisher-account-fields) to filter your request using equality conditions. Please see the [filters](#filters) section for more info on how to filter. 


## Publisher account fields

Full list of attributes, metrics and date fields for publisher accounts:

| Field                            | Field Type          | Data Type      | ID Suffix Required | 
|----------------------------------|---------------------|----------------|--------------------| 
| pca                              | Hierarchy Object ID | integer        | No                 | 
| publisher                        | Hierarchy Object ID | integer        | No                 | 
| client                           | Hierarchy Object ID | integer        | No                 | 
| customer                         | Hierarchy Object ID | integer        | No                 | 
| year                             | date                | integer        | No                 | 
| month                            | date                | integer        | No                 | 
| quarter                          | date                | integer        | No                 | 
| epoch_date                       | date                | date           | No                 | 
| ssweekly_the_year                | date                | integer        | No                 | 
| msweekly_the_year                | date                | integer        | No                 | 
| tmweekly_the_year                | date                | integer        | No                 | 
| wtweekly_the_year                | date                | integer        | No                 | 
| twweekly_the_year                | date                | integer        | No                 | 
| ftweekly_the_year                | date                | integer        | No                 | 
| sfweekly_the_year                | date                | integer        | No                 | 
| ssweek_of_year                   | date                | integer        | No                 | 
| msweek_of_year                   | date                | integer        | No                 | 
| tmweek_of_year                   | date                | integer        | No                 | 
| wtweek_of_year                   | date                | integer        | No                 | 
| twweek_of_year                   | date                | integer        | No                 | 
| ftweek_of_year                   | date                | integer        | No                 | 
| sfweek_of_year                   | date                | integer        | No                 | 
| pca_tag                          | attribute           | text           | Yes                | 
| pca_alias                        | attribute           | text           | No                 | 
| pca_tracking_template            | attribute           | text           | No                 | 
| pca_operation_status             | attribute           | enum           | No                 | 
| pca_object_version               | attribute           | date           | No                 | 
| publisher_name                   | attribute           | text           | No                 | 
| client_time_zone                 | attribute           | text           | No                 | 
| client_locale                    | attribute           | text           | No                 | 
| device                           | attribute           | enum           | No                 | 
| date                             | attribute           | text           | No                 | 
| month_name                       | attribute           | enum           | No                 | 
| day_of_week                      | attribute           | enum           | No                 | 
| day_of_month                     | attribute           | integer        | No                 | 
| impressions                      | metric              | integer        | No                 | 
| pub_clicks                       | metric              | integer        | No                 | 
| pub_cost                         | metric              | micro_currency | No                 | 
| conversions                      | metric              | micro_decimal  | Yes                | 
| revenue                          | metric              | micro_currency | Yes                | 
| gross_profit                     | metric              | micro_currency | No                 | 
| agency_fee_gross_cost_percentage | metric              | percent        | No                 | 
| agency_fee_click_cost            | metric              | micro_currency | No                 | 
| potential_impressions            | metric              | integer        | No                 | 
| lost_impressions_rank            | metric              | integer        | No                 | 
| quality_score                    | metric              | integer        | No                 | 
| last_modified                    | metric              | date           | No                 | 
| total_cost                       | metric              | micro_currency | No                 | 
| avg_cpc                          | metric              | micro_currency | No                 | 
| ctr                              | metric              | percent        | No                 | 
| group_impression_share           | metric              | percent        | No                 | 
| lost_is_budget                   | metric              | percent        | No                 | 
| lost_is_rank                     | metric              | percent        | No                 | 
| profit                           | metric              | micro_currency | No                 | 
| profit_margin                    | metric              | percent        | No                 | 
| gross_profit_per_click           | metric              | micro_currency | No                 | 
| gross_profit_per_impression      | metric              | micro_currency | No                 | 
| profit_per_click                 | metric              | micro_currency | No                 | 
| gpoas                            | metric              | percent        | No                 | 
| roi                              | metric              | percent        | No                 | 
| headroom                         | metric              | percent        | No                 | 
| gross_cost                       | metric              | micro_currency | No                 | 
| ecpm                             | metric              | decimal        | No                 | 
| historical_quality_score         | metric              | decimal        | No                 | 
| average_bid                      | metric              | micro_currency | No                 | 
| average_position                 | metric              | decimal        | No                 | 


# Campaigns

## Get all campaigns

```shell
curl -X GET \
  'https://api.marinsoftware.com/v1.0/clients/1808782/campaigns?
    limit=3&amp;
    offset=0&amp;
    fields=campaign,pca,publisher,client,customer,campaign_name,campaign_status,campaign_daily_budget,campaign_start_date&amp;
    campaign_status=ACTIVE&amp;
    sort=-pub_cost' \
  -H 'token: st_cfc8909710f0d020f95dcec821fad453a248ebc7a7f9438f'
```

```http
GET /v1.0/clients/1808782/campaigns?limit=3&amp;offset=0&amp;fields=campaign,pca,publisher,client,customer,campaign_name,campaign_status,campaign_daily_budget,campaign_start_date&amp;campaign_status=ACTIVE&amp;sort=-pub_cost HTTP/1.1
Host: api.marinsoftware.com
token: st_cfc8909710f0d020f95dcec821fad453a248ebc7a7f9438f
```

> The above command returns JSON structured like this:

```json
{
  "results": [
    {
      "campaign_name": "Powpow-AU-[Planner]-{Origin}-Sydney",
      "campaign_start_date": null,
      "campaign_status": "ACTIVE",
      "campaign": 4780608,
      "publisher": 6,
      "client": 1808782,
      "campaign_daily_budget": 15000000,
      "pca": 1591619,
      "customer": 1904153
    },
    {
      "campaign_name": "Powpow-AU-[Booker]-{Brand}-Core",
      "campaign_start_date": null,
      "campaign_status": "ACTIVE",
      "campaign": 4788970,
      "publisher": 6,
      "client": 1808782,
      "campaign_daily_budget": 50000000,
      "pca": 1591619,
      "customer": 1904153
    }
  ]
}
```

Get a list of campaigns for a client and any associated cost/conversion metrics.

### HTTP Request 
`GET /clients/{clientId}/campaigns` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Authorization token | Yes | string |
| clientId | path | The Marin client ID | Yes | string |
| limit | query | Number of rows to return in a single response. See [paging](#paging) for more info. | Yes | number |
| offset | query | The starting row number for a single response. See [paging](#paging) for more info. | Yes | number |
| fields | query | A comma separated list of fields including attributes, metrics and dates to include in the response. Please see [field selection](#field-selection) for more info. | No | string |
| from | query | Any metrics requested will be retrieved from this date. See [dates](#dates) for more info. | No | date (yyyy-mm-dd) |
| to | query | Any metrics requested will be retrieved to this date. See [dates](#dates) for more info. | No | date (yyyy-mm-dd) |
| sort | query | The field you'd like to sort your response by. See [sorting](#sorting) for more info.| No | string |

In addition you can use any valid [campaign field](#campaign-fields) to filter your request using equality conditions. Please see the [filters](#filters) section for more info on how to filter. 


## Get a single campaign

```shell
curl -X GET \
  'https://api.marinsoftware.com/v1.0/clients/1808782/campaigns/4780608?
    fields=campaign,pca,publisher,client,customer,campaign_name,campaign_status,campaign_daily_budget,campaign_start_date&amp; \
  -H 'token: st_cfc8909710f0d020f95dcec821fad453a248ebc7a7f9438f'
```

```http
GET /v1.0/clients/1808782/campaigns/4780608?fields=campaign,pca,publisher,client,customer,campaign_name,campaign_status,campaign_daily_budget,campaign_start_date HTTP/1.1
Host: api.marinsoftware.com
token: st_cfc8909710f0d020f95dcec821fad453a248ebc7a7f9438f
```

> The above command returns JSON structured like this:

```json
{
  "results": [
    {
      "campaign_name": "Powpow-AU-[Planner]-{Origin}-Sydney",
      "campaign_start_date": null,
      "campaign_status": "ACTIVE",
      "campaign": 4780608,
      "publisher": 6,
      "client": 1808782,
      "campaign_daily_budget": 15000000,
      "pca": 1591619,
      "customer": 1904153
    }
  ]
}
```

Get an individual campaign given it's ID and any associated cost/conversion metrics

### HTTP Request 
`GET /clients/{clientId}/campaigns/{campaignId}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Authorization token | Yes | string |
| clientId | path | The Marin client ID | Yes | string |
| campaignId | path | The Marin campaign ID | Yes | string |
| limit | query | Number of rows to return in a single response. See [paging](#paging) for more info. | Yes | number |
| offset | query | The starting row number for a single response. See [paging](#paging) for more info. | Yes | number |
| fields | query | A comma separated list of fields including attributes, metrics and dates to include in the response. Please see [field selection](#field-selection) for more info. | No | string |
| from | query | Any metrics requested will be retrieved from this date. See [dates](#dates) for more info. | No | date (yyyy-mm-dd) |
| to | query | Any metrics requested will be retrieved to this date. See [dates](#dates) for more info. | No | date (yyyy-mm-dd) |
| sort | query | The field you'd like to sort your response by. See [sorting](#sorting) for more info.| No | string |

In addition you can use any valid [campaign field](#campaign-fields) to filter your request using equality conditions. Please see the [filters](#filters) section for more info on how to filter.  


## Campaign fields

Full list of attributes, metrics and date fields for campaigns:

| Field                                    | Field Type          | Data Type      | ID Suffix Required | 
|------------------------------------------|---------------------|----------------|--------------------| 
| campaign                                 | Hierarchy Object ID | integer        | No                 | 
| pca                                      | Hierarchy Object ID | integer        | No                 | 
| publisher                                | Hierarchy Object ID | integer        | No                 | 
| client                                   | Hierarchy Object ID | integer        | No                 | 
| customer                                 | Hierarchy Object ID | integer        | No                 | 
| year                                     | date                | integer        | No                 | 
| month                                    | date                | integer        | No                 | 
| quarter                                  | date                | integer        | No                 | 
| epoch_date                               | date                | date           | No                 | 
| ssweekly_the_year                        | date                | integer        | No                 | 
| msweekly_the_year                        | date                | integer        | No                 | 
| tmweekly_the_year                        | date                | integer        | No                 | 
| wtweekly_the_year                        | date                | integer        | No                 | 
| twweekly_the_year                        | date                | integer        | No                 | 
| ftweekly_the_year                        | date                | integer        | No                 | 
| sfweekly_the_year                        | date                | integer        | No                 | 
| ssweek_of_year                           | date                | integer        | No                 | 
| msweek_of_year                           | date                | integer        | No                 | 
| tmweek_of_year                           | date                | integer        | No                 | 
| wtweek_of_year                           | date                | integer        | No                 | 
| twweek_of_year                           | date                | integer        | No                 | 
| ftweek_of_year                           | date                | integer        | No                 | 
| sfweek_of_year                           | date                | integer        | No                 | 
| campaign_tag                             | attribute           | text           | Yes                | 
| pca_tag                                  | attribute           | text           | Yes                | 
| campaign_tag_parent                      | attribute           | integer        | Yes                | 
| campaign_folder_id                       | attribute           | integer        | No                 | 
| campaign_folder_name                     | attribute           | text           | No                 | 
| campaign_status                          | attribute           | enum           | No                 | 
| campaign_operation_status                | attribute           | enum           | No                 | 
| campaign_object_version                  | attribute           | date           | No                 | 
| campaign_creation_date                   | attribute           | date           | No                 | 
| campaign_end_date                        | attribute           | date           | No                 | 
| campaign_rec_mobile_bid_adjustment       | attribute           | percent        | No                 | 
| campaign_rec_desktop_bid_adjustment      | attribute           | percent        | No                 | 
| campaign_next_scheduled_action_date      | attribute           | date           | No                 | 
| campaign_name                            | attribute           | text           | No                 | 
| campaign_daily_budget                    | attribute           | micro_currency | No                 | 
| campaign_ext_id                          | attribute           | integer        | No                 | 
| campaign_network                         | attribute           | array          | No                 | 
| campaign_device_targets                  | attribute           | array          | No                 | 
| campaign_budget                          | attribute           | micro_currency | No                 | 
| campaign_budget_type                     | attribute           | text           | No                 | 
| campaign_budget_name                     | attribute           | text           | No                 | 
| campaign_type                            | attribute           | enum           | No                 | 
| campaign_subtype                         | attribute           | enum           | No                 | 
| campaign_start_date                      | attribute           | date           | No                 | 
| campaign_tracking_template               | attribute           | text           | No                 | 
| campaign_custom_parameters               | attribute           | json           | No                 | 
| campaign_shopping_inventory_filter       | attribute           | text           | No                 | 
| campaign_geo_targets                     | attribute           | text           | No                 | 
| campaign_language                        | attribute           | array          | No                 | 
| campaign_local_inventory_ads             | attribute           | text           | No                 | 
| campaign_mobile_bid_adjustment           | attribute           | percent        | No                 | 
| campaign_desktop_bid_adjustment          | attribute           | percent        | No                 | 
| campaign_tablet_bid_adjustment           | attribute           | percent        | No                 | 
| campaign_bidding_type                    | attribute           | text           | No                 | 
| campaign_delivery                        | attribute           | text           | No                 | 
| campaign_mobile_bid_adjustment_exclusion | attribute           | text           | No                 | 
| pca_alias                                | attribute           | text           | No                 | 
| pca_tracking_template                    | attribute           | text           | No                 | 
| pca_operation_status                     | attribute           | enum           | No                 | 
| pca_object_version                       | attribute           | date           | No                 | 
| publisher_name                           | attribute           | text           | No                 | 
| client_time_zone                         | attribute           | text           | No                 | 
| client_locale                            | attribute           | text           | No                 | 
| device                                   | attribute           | enum           | No                 | 
| date                                     | attribute           | text           | No                 | 
| month_name                               | attribute           | enum           | No                 | 
| day_of_week                              | attribute           | enum           | No                 | 
| day_of_month                             | attribute           | integer        | No                 | 
| campaign_active_groups                   | attribute           | integer        | No                 | 
| impressions                              | metric              | integer        | No                 | 
| pub_clicks                               | metric              | integer        | No                 | 
| pub_cost                                 | metric              | micro_currency | No                 | 
| conversions                              | metric              | micro_decimal  | Yes                | 
| revenue                                  | metric              | micro_currency | Yes                | 
| gross_profit                             | metric              | micro_currency | No                 | 
| agency_fee_gross_cost_percentage         | metric              | percent        | No                 | 
| agency_fee_click_cost                    | metric              | micro_currency | No                 | 
| potential_impressions                    | metric              | integer        | No                 | 
| lost_impressions_rank                    | metric              | integer        | No                 | 
| quality_score                            | metric              | integer        | No                 | 
| last_modified                            | metric              | date           | No                 | 
| total_cost                               | metric              | micro_currency | No                 | 
| avg_cpc                                  | metric              | micro_currency | No                 | 
| ctr                                      | metric              | percent        | No                 | 
| group_impression_share                   | metric              | percent        | No                 | 
| lost_is_budget                           | metric              | percent        | No                 | 
| lost_is_rank                             | metric              | percent        | No                 | 
| profit                                   | metric              | micro_currency | No                 | 
| profit_margin                            | metric              | percent        | No                 | 
| gross_profit_per_click                   | metric              | micro_currency | No                 | 
| gross_profit_per_impression              | metric              | micro_currency | No                 | 
| profit_per_click                         | metric              | micro_currency | No                 | 
| gpoas                                    | metric              | percent        | No                 | 
| roi                                      | metric              | percent        | No                 | 
| headroom                                 | metric              | percent        | No                 | 
| gross_cost                               | metric              | micro_currency | No                 | 
| ecpm                                     | metric              | decimal        | No                 | 
| historical_quality_score                 | metric              | decimal        | No                 | 
| average_bid                              | metric              | micro_currency | No                 | 
| average_position                         | metric              | decimal        | No                 | 


# Groups

## Get all groups

```shell
curl -X GET \
  'https://api.marinsoftware.com/v1.0/clients/1808782/groups?
    limit=3&amp;
    offset=0&amp;
    fields=group,campaign,pca,pca_alias,publisher_name,campaign_name,client,customer,group_name,group_status,group_max_cpc,group_max_content_cpc,pub_clicks&amp;
    sort=-pub_clicks&amp;
    from=2017-05-01&amp;
    to=2017-05-31' \
  -H 'token: st_cfc8909710f0d020f95dcec821fad453a248ebc7a7f9438f'
```

```http
GET /v1.0/clients/1808782/groups?limit=3&amp;offset=0&amp;fields=group,campaign,pca,pca_alias,publisher_name,campaign_name,client,customer,group_name,group_status,group_max_cpc,group_max_content_cpc,pub_clicks&amp;sort=-pub_clicks&amp;from=2017-05-01&amp;to=2017-05-31 HTTP/1.1
Host: api.marinsoftware.com
token: st_cfc8909710f0d020f95dcec821fad453a248ebc7a7f9438f
```

> The above command returns JSON structured like this:

```json
{
  "results": [
    {
      "group_status": "ACTIVE",
      "publisher_name": "MSN",
      "group_name": "Golf Clubs",
      "group_max_content_cpc": 0,
      "pca": 1591619,
      "campaign_name": "Powpow-Golf-Brand",
      "pub_clicks": 289,
      "pca_alias": "Powpow-Google-SEM",
      "campaign": 4788970,
      "client": 1808782,
      "group_max_cpc": 50000,
      "group": 27077094,
      "customer": 1904153
    },
    {
      "group_status": "ACTIVE",
      "publisher_name": "MSN",
      "group_name": "Golf Balls",
      "group_max_content_cpc": 0,
      "pca": 1591619,
      "campaign_name": "Powpow-Golf-Brand",
      "pub_clicks": 79,
      "pca_alias": "Powpow-Google-SEM",
      "campaign": 4782230,
      "client": 1808782,
      "group_max_cpc": 50000,
      "group": 27053945,
      "customer": 1904153
    }
  ]
}
```

Get a list of groups for a client and any associated cost/conversion metrics.

### HTTP Request 
`GET /clients/{clientId}/groups` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Authorization token | Yes | string |
| clientId | path | The Marin client ID | Yes | number |
| limit | query | Number of rows to return in a single response. See [paging](#paging) for more info. | Yes | number |
| offset | query | The starting row number for a single response. See [paging](#paging) for more info. | Yes | number |
| fields | query | A comma separated list of fields including attributes, metrics and dates to include in the response. Please see [field selection](#field-selection) for more info. | No | string |
| from | query | Any metrics requested will be retrieved from this date. See [dates](#dates) for more info. | No | date (yyyy-mm-dd) |
| to | query | Any metrics requested will be retrieved to this date. See [dates](#dates) for more info. | No | date (yyyy-mm-dd) |
| sort | query | The field you'd like to sort your response by. See [sorting](#sorting) for more info.| No | string |

In addition you can use any valid [group field](#group-fields) to filter your request using equality conditions. Please see the [filters](#filters) section for more info on how to filter. 


## Get a single group

```shell
curl -X GET \
  'https://api.marinsoftware.com/v1.0/clients/1808782/groups/27077094?
    fields=group,campaign,pca,pca_alias,publisher_name,campaign_name,client,customer,group_name,group_status,group_max_cpc,group_max_content_cpc,pub_clicks&amp;
    from=2017-05-01&amp;
    to=2017-05-31' \
  -H 'token: st_cfc8909710f0d020f95dcec821fad453a248ebc7a7f9438f'
```

```http
GET /v1.0/clients/1808782/groups/27077094?fields=group,campaign,pca,pca_alias,publisher_name,campaign_name,client,customer,group_name,group_status,group_max_cpc,group_max_content_cpc,pub_clicks&amp;from=2017-05-01&amp;to=2017-05-31 HTTP/1.1
Host: api.marinsoftware.com
token: st_cfc8909710f0d020f95dcec821fad453a248ebc7a7f9438f
```

> The above command returns JSON structured like this:

```json
{
  "results": [
    {
      "group_status": "ACTIVE",
      "publisher_name": "MSN",
      "group_name": "Golf Clubs",
      "group_max_content_cpc": 0,
      "pca": 1591619,
      "campaign_name": "Powpow-Golf-Brand",
      "pub_clicks": 289,
      "pca_alias": "Powpow-Google-SEM",
      "campaign": 4788970,
      "client": 1808782,
      "group_max_cpc": 50000,
      "group": 27077094,
      "customer": 1904153
    },
    {
      "group_status": "ACTIVE",
      "publisher_name": "MSN",
      "group_name": "Golf Balls",
      "group_max_content_cpc": 0,
      "pca": 1591619,
      "campaign_name": "Powpow-Golf-Brand",
      "pub_clicks": 79,
      "pca_alias": "Powpow-Google-SEM",
      "campaign": 4782230,
      "client": 1808782,
      "group_max_cpc": 50000,
      "group": 27053945,
      "customer": 1904153
    }
  ]
}
```

Get an individual group given it's ID and any associated cost/conversion metrics

### HTTP Request 
`GET /clients/{clientId}/groups/{groupId}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Authorization token | Yes | string |
| clientId | path | The Marin client ID | Yes | number |
| groupId | path | The Marin group ID | Yes | number |
| limit | query | Number of rows to return in a single response. See [paging](#paging) for more info. | Yes | number |
| offset | query | The starting row number for a single response. See [paging](#paging) for more info. | Yes | number |
| fields | query | A comma separated list of fields including attributes, metrics and dates to include in the response. Please see [field selection](#field-selection) for more info. | No | string |
| from | query | Any metrics requested will be retrieved from this date. See [dates](#dates) for more info. | No | date (yyyy-mm-dd) |
| to | query | Any metrics requested will be retrieved to this date. See [dates](#dates) for more info. | No | date (yyyy-mm-dd) |
| sort | query | The field you'd like to sort your response by. See [sorting](#sorting) for more info.| No | string |

In addition you can use any valid [group field](#group-fields) to filter your request using equality conditions. Please see the [filters](#filters) section for more info on how to filter. 


## Group fields

Full list of attributes, metrics and date fields for groups:

| Field                                    | Field Type          | Data Type      | ID Suffix Required | 
|------------------------------------------|---------------------|----------------|--------------------| 
| group                                    | Hierarchy Object ID | integer        | No                 | 
| campaign                                 | Hierarchy Object ID | integer        | No                 | 
| pca                                      | Hierarchy Object ID | integer        | No                 | 
| publisher                                | Hierarchy Object ID | integer        | No                 | 
| client                                   | Hierarchy Object ID | integer        | No                 | 
| customer                                 | Hierarchy Object ID | integer        | No                 | 
| year                                     | date                | integer        | No                 | 
| month                                    | date                | integer        | No                 | 
| quarter                                  | date                | integer        | No                 | 
| epoch_date                               | date                | date           | No                 | 
| ssweekly_the_year                        | date                | integer        | No                 | 
| msweekly_the_year                        | date                | integer        | No                 | 
| tmweekly_the_year                        | date                | integer        | No                 | 
| wtweekly_the_year                        | date                | integer        | No                 | 
| twweekly_the_year                        | date                | integer        | No                 | 
| ftweekly_the_year                        | date                | integer        | No                 | 
| sfweekly_the_year                        | date                | integer        | No                 | 
| ssweek_of_year                           | date                | integer        | No                 | 
| msweek_of_year                           | date                | integer        | No                 | 
| tmweek_of_year                           | date                | integer        | No                 | 
| wtweek_of_year                           | date                | integer        | No                 | 
| twweek_of_year                           | date                | integer        | No                 | 
| ftweek_of_year                           | date                | integer        | No                 | 
| sfweek_of_year                           | date                | integer        | No                 | 
| group_tag                                | attribute           | text           | Yes                | 
| campaign_tag                             | attribute           | text           | Yes                | 
| pca_tag                                  | attribute           | text           | Yes                | 
| group_tag_parent                         | attribute           | integer        | Yes                | 
| campaign_tag_parent                      | attribute           | integer        | Yes                | 
| group_status                             | attribute           | enum           | No                 | 
| group_operation_status                   | attribute           | enum           | No                 | 
| group_max_cpc                            | attribute           | micro_currency | No                 | 
| group_creation_date                      | attribute           | date           | No                 | 
| group_object_version                     | attribute           | date           | No                 | 
| group_next_scheduled_action_date         | attribute           | date           | No                 | 
| group_estimated_rpc                      | attribute           | micro_currency | No                 | 
| group_rec_mobile_bid_adjustment          | attribute           | percent        | No                 | 
| group_recommended_bid                    | attribute           | micro_currency | No                 | 
| group_rec_desktop_bid_adjustment         | attribute           | percent        | No                 | 
| group_name                               | attribute           | text           | No                 | 
| group_recommended_content_bid            | attribute           | micro_currency | No                 | 
| group_max_content_cpc                    | attribute           | micro_currency | No                 | 
| group_ext_id                             | attribute           | integer        | No                 | 
| group_tracking_template                  | attribute           | text           | No                 | 
| group_custom_parameters                  | attribute           | json           | No                 | 
| group_end_date                           | attribute           | date           | No                 | 
| group_bid_type                           | attribute           | text           | No                 | 
| group_content_bid_bing                   | attribute           | integer        | No                 | 
| group_mobile_bid_adjustment              | attribute           | percent        | No                 | 
| group_desktop_bid_adjustment             | attribute           | percent        | No                 | 
| group_tablet_bid_adjustment              | attribute           | percent        | No                 | 
| group_folder_id                          | attribute           | integer        | No                 | 
| group_folder_name                        | attribute           | text           | No                 | 
| campaign_folder_id                       | attribute           | integer        | No                 | 
| campaign_folder_name                     | attribute           | text           | No                 | 
| campaign_status                          | attribute           | enum           | No                 | 
| campaign_operation_status                | attribute           | enum           | No                 | 
| campaign_object_version                  | attribute           | date           | No                 | 
| campaign_creation_date                   | attribute           | date           | No                 | 
| campaign_end_date                        | attribute           | date           | No                 | 
| campaign_rec_mobile_bid_adjustment       | attribute           | percent        | No                 | 
| campaign_rec_desktop_bid_adjustment      | attribute           | percent        | No                 | 
| campaign_next_scheduled_action_date      | attribute           | date           | No                 | 
| campaign_name                            | attribute           | text           | No                 | 
| campaign_daily_budget                    | attribute           | micro_currency | No                 | 
| campaign_ext_id                          | attribute           | integer        | No                 | 
| campaign_network                         | attribute           | array          | No                 | 
| campaign_device_targets                  | attribute           | array          | No                 | 
| campaign_budget                          | attribute           | micro_currency | No                 | 
| campaign_budget_type                     | attribute           | text           | No                 | 
| campaign_budget_name                     | attribute           | text           | No                 | 
| campaign_type                            | attribute           | enum           | No                 | 
| campaign_subtype                         | attribute           | enum           | No                 | 
| campaign_start_date                      | attribute           | date           | No                 | 
| campaign_tracking_template               | attribute           | text           | No                 | 
| campaign_custom_parameters               | attribute           | json           | No                 | 
| campaign_shopping_inventory_filter       | attribute           | text           | No                 | 
| campaign_geo_targets                     | attribute           | text           | No                 | 
| campaign_language                        | attribute           | array          | No                 | 
| campaign_local_inventory_ads             | attribute           | text           | No                 | 
| campaign_mobile_bid_adjustment           | attribute           | percent        | No                 | 
| campaign_desktop_bid_adjustment          | attribute           | percent        | No                 | 
| campaign_tablet_bid_adjustment           | attribute           | percent        | No                 | 
| campaign_bidding_type                    | attribute           | text           | No                 | 
| campaign_delivery                        | attribute           | text           | No                 | 
| campaign_mobile_bid_adjustment_exclusion | attribute           | text           | No                 | 
| pca_alias                                | attribute           | text           | No                 | 
| pca_tracking_template                    | attribute           | text           | No                 | 
| pca_operation_status                     | attribute           | enum           | No                 | 
| pca_object_version                       | attribute           | date           | No                 | 
| publisher_name                           | attribute           | text           | No                 | 
| client_time_zone                         | attribute           | text           | No                 | 
| client_locale                            | attribute           | text           | No                 | 
| device                                   | attribute           | enum           | No                 | 
| date                                     | attribute           | text           | No                 | 
| month_name                               | attribute           | enum           | No                 | 
| day_of_week                              | attribute           | enum           | No                 | 
| day_of_month                             | attribute           | integer        | No                 | 
| campaign_active_groups                   | attribute           | integer        | No                 | 
| group_active_creatives                   | attribute           | integer        | No                 | 
| impressions                              | metric              | integer        | No                 | 
| pub_clicks                               | metric              | integer        | No                 | 
| pub_cost                                 | metric              | micro_currency | No                 | 
| conversions                              | metric              | micro_decimal  | Yes                | 
| revenue                                  | metric              | micro_currency | Yes                | 
| gross_profit                             | metric              | micro_currency | No                 | 
| agency_fee_gross_cost_percentage         | metric              | percent        | No                 | 
| agency_fee_click_cost                    | metric              | micro_currency | No                 | 
| potential_impressions                    | metric              | integer        | No                 | 
| lost_impressions_rank                    | metric              | integer        | No                 | 
| quality_score                            | metric              | integer        | No                 | 
| last_modified                            | metric              | date           | No                 | 
| total_cost                               | metric              | micro_currency | No                 | 
| avg_cpc                                  | metric              | micro_currency | No                 | 
| ctr                                      | metric              | percent        | No                 | 
| group_impression_share                   | metric              | percent        | No                 | 
| lost_is_budget                           | metric              | percent        | No                 | 
| lost_is_rank                             | metric              | percent        | No                 | 
| profit                                   | metric              | micro_currency | No                 | 
| profit_margin                            | metric              | percent        | No                 | 
| gross_profit_per_click                   | metric              | micro_currency | No                 | 
| gross_profit_per_impression              | metric              | micro_currency | No                 | 
| profit_per_click                         | metric              | micro_currency | No                 | 
| gpoas                                    | metric              | percent        | No                 | 
| roi                                      | metric              | percent        | No                 | 
| headroom                                 | metric              | percent        | No                 | 
| gross_cost                               | metric              | micro_currency | No                 | 
| ecpm                                     | metric              | decimal        | No                 | 
| historical_quality_score                 | metric              | decimal        | No                 | 
| average_bid                              | metric              | micro_currency | No                 | 
| average_position                         | metric              | decimal        | No                 | 


# Keywords

## Get all keywords

```shell
curl -X GET \
  "https://api.marinsoftware.com/v1.0/clients/1808782/keywords?
    limit=3&amp;
    offset=0&amp;
    fields=keyword,group,campaign,campaign_name,pca,pca_alias,publisher_name,client,customer,keyword_name,status,max_cpc,recommended_bid,min_cpc,type,impressions,pub_clicks,pub_cost&amp;
    sort=-impressions&amp;
    from=2017-05-01&amp;
    to=2017-05-01"
  -H 'token: st_cfc8909710f0d020f95dcec821fad453a248ebc7a7f9438f'
```

```http
GET /v1.0/clients/1808782/keywords?limit=3&amp;offset=0&amp;
fields=keyword,group,campaign,campaign_name,pca,pca_alias,publisher_name,client,customer,keyword_name,status,max_cpc,recommended_bid,min_cpc,type,impressions,pub_clicks,pub_cost&amp;sort=-impressions&amp;from=2017-05-01&amp;to=2017-05-01 HTTP/1.1
Host: api.marinsoftware.com
token: st_cfc8909710f0d020f95dcec821fad453a248ebc7a7f9438f
```

> The above command returns JSON structured like this:

```json
{
  "results": [
    {
      "publisher_name": "MSN",
      "min_cpc": 0,
      "impressions": 250,
      "type": "BROAD",
      "max_cpc": 3000000,
      "pub_cost": 23040000,
      "pca": 1591619,
      "campaign_name": "Powpow-Golf-Brand",
      "pub_clicks": 10,
      "pca_alias": "Powpow-Bing-SEM",
      "campaign": 4779468,
      "client": 1808782,
      "keyword": 10387398,
      "keyword_name": "+golf +club",
      "recommended_bid": 0,
      "group": 27052373,
      "customer": 1904153,
      "status": "ACTIVE"
    },
    {
      "publisher_name": "MSN",
      "min_cpc": 0,
      "impressions": 196,
      "type": "BROAD",
      "max_cpc": 3000000,
      "pub_cost": 39470000,
      "pca": 1591619,
      "campaign_name": "Powpow-Golf-Brand",
      "pub_clicks": 18,
      "pca_alias": "Powpow-Bing-SEM",
      "campaign": 4779468,
      "client": 1808782,
      "keyword": 10389278,
      "keyword_name": "+golf +club",
      "recommended_bid": 0,
      "group": 27064292,
      "customer": 1904153,
      "status": "ACTIVE"
    }
  ]
}
```

Get a list of keywords for a client and any associated cost/conversion metrics.

### HTTP Request 
`GET /clients/{clientId}/keywords` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Authorization token | Yes | string |
| clientId | path | The Marin client ID | Yes | number |
| limit | query | Number of rows to return in a single response. See [paging](#paging) for more info. | Yes | number |
| offset | query | The starting row number for a single response. See [paging](#paging) for more info. | Yes | number |
| fields | query | A comma separated list of fields including attributes, metrics and dates to include in the response. Please see [field selection](#field-selection) for more info. | No | string |
| from | query | Any metrics requested will be retrieved from this date. See [dates](#dates) for more info. | No | date (yyyy-mm-dd) |
| to | query | Any metrics requested will be retrieved to this date. See [dates](#dates) for more info. | No | date (yyyy-mm-dd) |
| sort | query | The field you'd like to sort your response by. See [sorting](#sorting) for more info.| No | string |

In addition you can use any valid [keyword field](#keyword-fields) to filter your request using equality conditions. Please see the [filters](#filters) section for more info on how to filter. 


## Get a single keyword

```shell
curl -X GET \
  "https://api.marinsoftware.com/v1.0/clients/1808782/keywords/10387398?
    fields=keyword,group,campaign,campaign_name,pca,pca_alias,publisher_name,client,customer,keyword_name,status,max_cpc,recommended_bid,min_cpc,type,impressions,pub_clicks,pub_cost&amp;
    from=2017-05-01&amp;
    to=2017-05-01"
  -H 'token: st_cfc8909710f0d020f95dcec821fad453a248ebc7a7f9438f'
```

```http
GET /v1.0/clients/1808782/keywords/10387398?
fields=keyword,group,campaign,campaign_name,pca,pca_alias,publisher_name,client,customer,keyword_name,status,max_cpc,recommended_bid,min_cpc,type,impressions,pub_clicks,pub_cost&amp;from=2017-05-01&amp;to=2017-05-01 HTTP/1.1
Host: api.marinsoftware.com
token: st_cfc8909710f0d020f95dcec821fad453a248ebc7a7f9438f
```

> The above command returns JSON structured like this:

```json
{
  "results": [
    {
      "publisher_name": "MSN",
      "min_cpc": 0,
      "impressions": 250,
      "type": "BROAD",
      "max_cpc": 3000000,
      "pub_cost": 23040000,
      "pca": 1591619,
      "campaign_name": "Powpow-Golf-Brand",
      "pub_clicks": 10,
      "pca_alias": "Powpow-Bing-SEM",
      "campaign": 4779468,
      "client": 1808782,
      "keyword": 10387398,
      "keyword_name": "+golf +club",
      "recommended_bid": 0,
      "group": 27052373,
      "customer": 1904153,
      "status": "ACTIVE"
    }
  ]
}
```

Get an individual keyword given it's ID and any associated cost/conversion metrics

### HTTP Request 
`GET /clients/{clientId}/keywords/{keywordId}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Authorization token | Yes | string |
| clientId | path | The Marin client ID | Yes | number |
| keywordId | path | The Marin keyword ID | Yes | number |
| limit | query | Number of rows to return in a single response. See [paging](#paging) for more info. | Yes | number |
| offset | query | The starting row number for a single response. See [paging](#paging) for more info. | Yes | number |
| fields | query | A comma separated list of fields including attributes, metrics and dates to include in the response. Please see [field selection](#field-selection) for more info. | No | string |
| from | query | Any metrics requested will be retrieved from this date. See [dates](#dates) for more info. | No | date (yyyy-mm-dd) |
| to | query | Any metrics requested will be retrieved to this date. See [dates](#dates) for more info. | No | date (yyyy-mm-dd) |
| sort | query | The field you'd like to sort your response by. See [sorting](#sorting) for more info.| No | string |

In addition you can use any valid [keyword field](#keyword-fields) to filter your request using equality conditions. Please see the [filters](#filters) section for more info on how to filter. 


## Keyword fields

Full list of attributes, metrics and date fields for keywords:

| Field                                | Field Type          | Data Type      | ID Suffix Required | 
|--------------------------------------|---------------------|----------------|--------------------| 
| keyword                              | Hierarchy Object ID | integer        | No                 | 
| group                                | Hierarchy Object ID | integer        | No                 | 
| campaign                             | Hierarchy Object ID | integer        | No                 | 
| pca                                  | Hierarchy Object ID | integer        | No                 | 
| publisher                            | Hierarchy Object ID | integer        | No                 | 
| client                               | Hierarchy Object ID | integer        | No                 | 
| customer                             | Hierarchy Object ID | integer        | No                 | 
| year                                 | date                | integer        | No                 | 
| month                                | date                | integer        | No                 | 
| quarter                              | date                | integer        | No                 | 
| epoch_date                           | date                | date           | No                 | 
| ssweekly_the_year                    | date                | integer        | No                 | 
| msweekly_the_year                    | date                | integer        | No                 | 
| tmweekly_the_year                    | date                | integer        | No                 | 
| wtweekly_the_year                    | date                | integer        | No                 | 
| twweekly_the_year                    | date                | integer        | No                 | 
| ftweekly_the_year                    | date                | integer        | No                 | 
| sfweekly_the_year                    | date                | integer        | No                 | 
| ssweek_of_year                       | date                | integer        | No                 | 
| msweek_of_year                       | date                | integer        | No                 | 
| tmweek_of_year                       | date                | integer        | No                 | 
| wtweek_of_year                       | date                | integer        | No                 | 
| twweek_of_year                       | date                | integer        | No                 | 
| ftweek_of_year                       | date                | integer        | No                 | 
| sfweek_of_year                       | date                | integer        | No                 | 
| status                               | attribute           | enum           | No                 | 
| operation_status                     | attribute           | enum           | No                 | 
| destination_url                      | attribute           | text           | No                 | 
| max_cpc                              | attribute           | micro_currency | No                 | 
| creation_date                        | attribute           | date           | No                 | 
| object_version                       | attribute           | date           | No                 | 
| recommended_bid                      | attribute           | micro_currency | No                 | 
| estimated_rpc                        | attribute           | micro_currency | No                 | 
| last_bid_date                        | attribute           | date           | No                 | 
| ext_id                               | attribute           | text           | No                 | 
| keyword_name                         | attribute           | text           | No                 | 
| min_cpc                              | attribute           | micro_currency | No                 | 
| uniq_id                              | attribute           | text           | No                 | 
| type                                 | attribute           | enum           | No                 | 
| dynamic_module_publisher_object_type | attribute           | text           | No                 | 
| dynamic_module_publisher_object_id   | attribute           | integer        | No                 | 
| beta_features                        | attribute           | text           | No                 | 
| landing_page                         | attribute           | array          | No                 | 
| mobile_landing_page                  | attribute           | array          | No                 | 
| tracking_template                    | attribute           | text           | No                 | 
| custom_parameters                    | attribute           | json           | No                 | 
| click_through_url                    | attribute           | text           | No                 | 
| rec_bid_change                       | attribute           | micro_currency | No                 | 
| rec_bid_change_percent               | attribute           | percent        | No                 | 
| min_cpc_delta                        | attribute           | micro_currency | No                 | 
| bid_override                         | attribute           | micro_currency | No                 | 
| override_until                       | attribute           | date           | No                 | 
| bid_override_target_position_range   | attribute           | integer        | No                 | 
| bid_last_calculated_date             | attribute           | date           | No                 | 
| bing_param1                          | attribute           | text           | No                 | 
| bing_param2                          | attribute           | text           | No                 | 
| bing_param3                          | attribute           | text           | No                 | 
| live_ad_param1                       | attribute           | text           | No                 | 
| live_ad_param2                       | attribute           | text           | No                 | 
| live_ads_keyword_insertion           | attribute           | text           | No                 | 
| length                               | attribute           | integer        | No                 | 
| token_count                          | attribute           | integer        | No                 | 
| keyword_tag                          | attribute           | text           | Yes                | 
| keyword_tag_parent                   | attribute           | integer        | Yes                | 
| group_name                           | attribute           | text           | No                 | 
| group_desktop_bid_adjustment         | attribute           | percent        | No                 | 
| group_mobile_bid_adjustment          | attribute           | percent        | No                 | 
| group_tablet_bid_adjustment          | attribute           | percent        | No                 | 
| keyword_folder_id                    | attribute           | integer        | No                 | 
| keyword_folder_name                  | attribute           | text           | No                 | 
| campaign_name                        | attribute           | text           | No                 | 
| campaign_desktop_bid_adjustment      | attribute           | percent        | No                 | 
| campaign_mobile_bid_adjustment       | attribute           | percent        | No                 | 
| campaign_tablet_bid_adjustment       | attribute           | percent        | No                 | 
| pca_alias                            | attribute           | text           | No                 | 
| publisher_name                       | attribute           | text           | No                 | 
| client_time_zone                     | attribute           | text           | No                 | 
| client_locale                        | attribute           | text           | No                 | 
| network                              | attribute           | enum           | No                 | 
| device                               | attribute           | enum           | No                 | 
| date                                 | attribute           | text           | No                 | 
| month_name                           | attribute           | enum           | No                 | 
| day_of_week                          | attribute           | enum           | No                 | 
| day_of_month                         | attribute           | integer        | No                 | 
| impressions                          | metric              | integer        | No                 | 
| pub_clicks                           | metric              | integer        | No                 | 
| pub_cost                             | metric              | micro_currency | No                 | 
| conversions                          | metric              | micro_decimal  | Yes                | 
| revenue                              | metric              | micro_currency | Yes                | 
| gross_profit                         | metric              | micro_currency | No                 | 
| agency_fee_gross_cost_percentage     | metric              | percent        | No                 | 
| agency_fee_click_cost                | metric              | micro_currency | No                 | 
| potential_impressions                | metric              | integer        | No                 | 
| lost_impressions_rank                | metric              | integer        | No                 | 
| quality_score                        | metric              | integer        | No                 | 
| last_modified                        | metric              | date           | No                 | 
| total_cost                           | metric              | micro_currency | No                 | 
| avg_cpc                              | metric              | micro_currency | No                 | 
| ctr                                  | metric              | percent        | No                 | 
| keyword_impression_share             | metric              | percent        | No                 | 
| lost_is_budget                       | metric              | percent        | No                 | 
| lost_is_rank                         | metric              | percent        | No                 | 
| profit                               | metric              | micro_currency | No                 | 
| profit_margin                        | metric              | percent        | No                 | 
| gross_profit_per_click               | metric              | micro_currency | No                 | 
| gross_profit_per_impression          | metric              | micro_currency | No                 | 
| profit_per_click                     | metric              | micro_currency | No                 | 
| gpoas                                | metric              | percent        | No                 | 
| roi                                  | metric              | percent        | No                 | 
| headroom                             | metric              | percent        | No                 | 
| gross_cost                           | metric              | micro_currency | No                 | 
| ecpm                                 | metric              | decimal        | No                 | 
| historical_quality_score             | metric              | decimal        | No                 | 
| average_bid                          | metric              | micro_currency | No                 | 
| average_position                     | metric              | decimal        | No                 | 
| exact_match_impression_share         | metric              | percent        | No                 | 


# Creatives

## Get all creatives

```shell
curl -X GET \
  "https://api.marinsoftware.com/v1.0/clients/1808782/creatives?
    limit=3&amp;offset=0&amp;
    fields=date,creative,group,campaign,campaign_name,pca,publisher,client,customer,ext_id,status,headline1,headline2,description1,display_url,impressions,pub_clicks,pub_cost&amp;
    sort=-pub_cost&amp;
    from=2017-04-01&amp;
    to=2017-04-30
  -H 'token: st_cfc8909710f0d020f95dcec821fad453a248ebc7a7f9438f'
```

```http
GET /v1.0/clients/1808782/creatives?limit=3&amp;offset=0&amp;
fields=date,creative,group,campaign,campaign_name,pca,publisher,client,customer,ext_id,status,headline1,headline2,description1,display_url,impressions,pub_clicks,pub_cost&amp;sort=-pub_cost&amp;from=2017-04-01&amp;to=2017-04-30 HTTP/1.1
Host: api.marinsofware.com
token: st_cfc8909710f0d020f95dcec821fad453a248ebc7a7f9438f
```

> The above command returns JSON structured like this:

```json
{
  "results": [
    {
      "date": "2017-04-17",
      "display_url": "",
      "impressions": 75,
      "creative": 14715104,
      "pub_cost": 38310000,
      "pca": 1591619,
      "description1": "Don't miss the boat. Great prices available now!",
      "campaign_name": "Powpow-Golf-Brand",
      "ext_id": "75041681461983",
      "pub_clicks": 15,
      "headline1": "Powpow Golf",
      "headline2": "Last Minute Sale",
      "campaign": 4782230,
      "publisher": 6,
      "client": 1808782,
      "group": 27053945,
      "customer": 1904153,
      "status": "ACTIVE"
    },
    {
      "date": "2017-04-29",
      "display_url": "",
      "impressions": 161,
      "creative": 14650343,
      "pub_cost": 36700000,
      "pca": 1591619,
      "description1": "description 1",
      "campaign_name": "Powpow-Golf-Brand",
      "ext_id": "74560645136570",
      "pub_clicks": 15,
      "headline1": "Powpow Golf,
      "headline2": "Get your Golf Clubs Today",
      "campaign": 4779468,
      "publisher": 6,
      "client": 1808782,
      "group": 27052373,
      "customer": 1904153,
      "status": "ACTIVE"
    }
  ]
}
```

Get a list of creatives for a client and any associated cost/conversion metrics.

### HTTP Request 
`GET /clients/{clientId}/creatives` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Authorization token | Yes | string |
| clientId | path | The Marin client ID | Yes | number |
| limit | query | Number of rows to return in a single response. See [paging](#paging) for more info. | Yes | number |
| offset | query | The starting row number for a single response. See [paging](#paging) for more info. | Yes | number |
| fields | query | A comma separated list of fields including attributes, metrics and dates to include in the response. Please see [field selection](#field-selection) for more info. | No | string |
| from | query | Any metrics requested will be retrieved from this date. See [dates](#dates) for more info. | No | date (yyyy-mm-dd) |
| to | query | Any metrics requested will be retrieved to this date. See [dates](#dates) for more info. | No | date (yyyy-mm-dd) |
| sort | query | The field you'd like to sort your response by. See [sorting](#sorting) for more info.| No | string |

In addition you can use any valid [creative field](#creative-fields) to filter your request using equality conditions. Please see the [filters](#filters) section for more info on how to filter. 


## Get a single creative

```shell
curl -X GET \
  "https://api.marinsoftware.com/v1.0/clients/1808782/creatives/14715104?
    fields=date,creative,group,campaign,campaign_name,pca,publisher,client,customer,ext_id,status,headline1,headline2,description1,display_url,impressions,pub_clicks,pub_cost&amp;
    from=2017-04-01&amp;
    to=2017-04-30
  -H 'token: st_cfc8909710f0d020f95dcec821fad453a248ebc7a7f9438f'
```

```http
GET /v1.0/clients/1808782/creatives/14715104?
fields=date,creative,group,campaign,campaign_name,pca,publisher,client,customer,ext_id,status,headline1,headline2,description1,display_url,impressions,pub_clicks,pub_cost&amp;from=2017-04-01&amp;to=2017-04-30 HTTP/1.1
Host: api.marinsofware.com
token: st_cfc8909710f0d020f95dcec821fad453a248ebc7a7f9438f
```

> The above command returns JSON structured like this:

```json
{
  "results": [
    {
      "date": "2017-04-17",
      "display_url": "",
      "impressions": 75,
      "creative": 14715104,
      "pub_cost": 38310000,
      "pca": 1591619,
      "description1": "Don't miss the boat. Great prices available now!",
      "campaign_name": "Powpow-Golf-Brand",
      "ext_id": "75041681461983",
      "pub_clicks": 15,
      "headline1": "Powpow Golf",
      "headline2": "Last Minute Sale",
      "campaign": 4782230,
      "publisher": 6,
      "client": 1808782,
      "group": 27053945,
      "customer": 1904153,
      "status": "ACTIVE"
    }
  ]
}
```

Get an individual creative given it's ID and any associated cost/conversion metrics

### HTTP Request 
`GET /clients/{clientId}/creatives/{creativeId}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Authorization token | Yes | string |
| clientId | path | The Marin client ID | Yes | number |
| creativeId | path | The Marin creative ID | Yes | number |
| limit | query | Number of rows to return in a single response. See [paging](#paging) for more info. | Yes | number |
| offset | query | The starting row number for a single response. See [paging](#paging) for more info. | Yes | number |
| fields | query | A comma separated list of fields including attributes, metrics and dates to include in the response. Please see [field selection](#field-selection) for more info. | No | string |
| from | query | Any metrics requested will be retrieved from this date. See [dates](#dates) for more info. | No | date (yyyy-mm-dd) |
| to | query | Any metrics requested will be retrieved to this date. See [dates](#dates) for more info. | No | date (yyyy-mm-dd) |
| sort | query | The field you'd like to sort your response by. See [sorting](#sorting) for more info.| No | string |

In addition you can use any valid [creative field](#creative-fields) to filter your request using equality conditions. Please see the [filters](#filters) section for more info on how to filter. 


## Creative fields

Full list of attributes, metrics and date fields for keywords:

| Field                                    | Field Type          | Data Type      | ID Suffix Required | 
|------------------------------------------|---------------------|----------------|--------------------| 
| creative                                 | Hierarchy Object ID | integer        | No                 | 
| group                                    | Hierarchy Object ID | integer        | No                 | 
| campaign                                 | Hierarchy Object ID | integer        | No                 | 
| pca                                      | Hierarchy Object ID | integer        | No                 | 
| publisher                                | Hierarchy Object ID | integer        | No                 | 
| client                                   | Hierarchy Object ID | integer        | No                 | 
| customer                                 | Hierarchy Object ID | integer        | No                 | 
| year                                     | date                | integer        | No                 | 
| month                                    | date                | integer        | No                 | 
| quarter                                  | date                | integer        | No                 | 
| epoch_date                               | date                | date           | No                 | 
| ssweekly_the_year                        | date                | integer        | No                 | 
| msweekly_the_year                        | date                | integer        | No                 | 
| tmweekly_the_year                        | date                | integer        | No                 | 
| wtweekly_the_year                        | date                | integer        | No                 | 
| twweekly_the_year                        | date                | integer        | No                 | 
| ftweekly_the_year                        | date                | integer        | No                 | 
| sfweekly_the_year                        | date                | integer        | No                 | 
| ssweek_of_year                           | date                | integer        | No                 | 
| msweek_of_year                           | date                | integer        | No                 | 
| tmweek_of_year                           | date                | integer        | No                 | 
| wtweek_of_year                           | date                | integer        | No                 | 
| twweek_of_year                           | date                | integer        | No                 | 
| ftweek_of_year                           | date                | integer        | No                 | 
| sfweek_of_year                           | date                | integer        | No                 | 
| object_version                           | attribute           | date           | No                 | 
| ext_id                                   | attribute           | text           | No                 | 
| uniq_id                                  | attribute           | text           | No                 | 
| status                                   | attribute           | enum           | No                 | 
| operation_status                         | attribute           | enum           | No                 | 
| destination_url                          | attribute           | text           | No                 | 
| creation_date                            | attribute           | date           | No                 | 
| headline1                                | attribute           | text           | No                 | 
| headline2                                | attribute           | text           | No                 | 
| creative_type                            | attribute           | enum           | No                 | 
| description1                             | attribute           | text           | No                 | 
| description2                             | attribute           | text           | No                 | 
| display_url                              | attribute           | text           | No                 | 
| display_url_nav1                         | attribute           | text           | No                 | 
| display_url_nav2                         | attribute           | text           | No                 | 
| judgment                                 | attribute           | text           | No                 | 
| next_scheduled_action_date               | attribute           | date           | No                 | 
| mobile_preferred                         | attribute           | boolean        | No                 | 
| creative_tag                             | attribute           | text           | Yes                | 
| group_tag                                | attribute           | text           | Yes                | 
| campaign_tag                             | attribute           | text           | Yes                | 
| pca_tag                                  | attribute           | text           | Yes                | 
| creative_tag_parent                      | attribute           | integer        | Yes                | 
| group_tag_parent                         | attribute           | integer        | Yes                | 
| campaign_tag_parent                      | attribute           | integer        | Yes                | 
| group_status                             | attribute           | enum           | No                 | 
| group_operation_status                   | attribute           | enum           | No                 | 
| group_max_cpc                            | attribute           | micro_currency | No                 | 
| group_creation_date                      | attribute           | date           | No                 | 
| group_object_version                     | attribute           | date           | No                 | 
| group_next_scheduled_action_date         | attribute           | date           | No                 | 
| group_estimated_rpc                      | attribute           | micro_currency | No                 | 
| group_rec_mobile_bid_adjustment          | attribute           | percent        | No                 | 
| group_recommended_bid                    | attribute           | micro_currency | No                 | 
| group_rec_desktop_bid_adjustment         | attribute           | percent        | No                 | 
| group_name                               | attribute           | text           | No                 | 
| group_recommended_content_bid            | attribute           | micro_currency | No                 | 
| group_max_content_cpc                    | attribute           | micro_currency | No                 | 
| group_ext_id                             | attribute           | integer        | No                 | 
| group_tracking_template                  | attribute           | text           | No                 | 
| group_custom_parameters                  | attribute           | json           | No                 | 
| group_end_date                           | attribute           | date           | No                 | 
| group_bid_type                           | attribute           | text           | No                 | 
| group_content_bid_bing                   | attribute           | integer        | No                 | 
| group_mobile_bid_adjustment              | attribute           | percent        | No                 | 
| group_desktop_bid_adjustment             | attribute           | percent        | No                 | 
| group_tablet_bid_adjustment              | attribute           | percent        | No                 | 
| creative_folder_id                       | attribute           | integer        | No                 | 
| creative_folder_name                     | attribute           | text           | No                 | 
| group_folder_id                          | attribute           | integer        | No                 | 
| group_folder_name                        | attribute           | text           | No                 | 
| campaign_folder_id                       | attribute           | integer        | No                 | 
| campaign_folder_name                     | attribute           | text           | No                 | 
| campaign_status                          | attribute           | enum           | No                 | 
| campaign_operation_status                | attribute           | enum           | No                 | 
| campaign_object_version                  | attribute           | date           | No                 | 
| campaign_creation_date                   | attribute           | date           | No                 | 
| campaign_end_date                        | attribute           | date           | No                 | 
| campaign_rec_mobile_bid_adjustment       | attribute           | percent        | No                 | 
| campaign_rec_desktop_bid_adjustment      | attribute           | percent        | No                 | 
| campaign_next_scheduled_action_date      | attribute           | date           | No                 | 
| campaign_name                            | attribute           | text           | No                 | 
| campaign_daily_budget                    | attribute           | micro_currency | No                 | 
| campaign_ext_id                          | attribute           | integer        | No                 | 
| campaign_network                         | attribute           | array          | No                 | 
| campaign_device_targets                  | attribute           | array          | No                 | 
| campaign_budget                          | attribute           | micro_currency | No                 | 
| campaign_budget_type                     | attribute           | text           | No                 | 
| campaign_budget_name                     | attribute           | text           | No                 | 
| campaign_type                            | attribute           | enum           | No                 | 
| campaign_subtype                         | attribute           | enum           | No                 | 
| campaign_start_date                      | attribute           | date           | No                 | 
| campaign_tracking_template               | attribute           | text           | No                 | 
| campaign_custom_parameters               | attribute           | json           | No                 | 
| campaign_shopping_inventory_filter       | attribute           | text           | No                 | 
| campaign_geo_targets                     | attribute           | text           | No                 | 
| campaign_language                        | attribute           | array          | No                 | 
| campaign_local_inventory_ads             | attribute           | text           | No                 | 
| campaign_mobile_bid_adjustment           | attribute           | percent        | No                 | 
| campaign_desktop_bid_adjustment          | attribute           | percent        | No                 | 
| campaign_tablet_bid_adjustment           | attribute           | percent        | No                 | 
| campaign_bidding_type                    | attribute           | text           | No                 | 
| campaign_delivery                        | attribute           | text           | No                 | 
| campaign_mobile_bid_adjustment_exclusion | attribute           | text           | No                 | 
| pca_alias                                | attribute           | text           | No                 | 
| pca_tracking_template                    | attribute           | text           | No                 | 
| pca_operation_status                     | attribute           | enum           | No                 | 
| pca_object_version                       | attribute           | date           | No                 | 
| publisher_name                           | attribute           | text           | No                 | 
| client_time_zone                         | attribute           | text           | No                 | 
| client_locale                            | attribute           | text           | No                 | 
| dynamic_module_publisher_object_type     | attribute           | text           | No                 | 
| dynamic_module_publisher_object_id       | attribute           | integer        | No                 | 
| beta_features                            | attribute           | text           | No                 | 
| ad_name                                  | attribute           | text           | No                 | 
| landing_page                             | attribute           | array          | No                 | 
| mobile_landing_page                      | attribute           | array          | No                 | 
| tracking_template                        | attribute           | text           | No                 | 
| custom_parameters                        | attribute           | json           | No                 | 
| network                                  | attribute           | enum           | No                 | 
| device                                   | attribute           | enum           | No                 | 
| date                                     | attribute           | text           | No                 | 
| month_name                               | attribute           | enum           | No                 | 
| day_of_week                              | attribute           | enum           | No                 | 
| day_of_month                             | attribute           | integer        | No                 | 
| campaign_active_groups                   | attribute           | integer        | No                 | 
| group_active_creatives                   | attribute           | integer        | No                 | 
| impressions                              | metric              | integer        | No                 | 
| pub_clicks                               | metric              | integer        | No                 | 
| pub_cost                                 | metric              | micro_currency | No                 | 
| conversions                              | metric              | micro_decimal  | Yes                | 
| revenue                                  | metric              | micro_currency | Yes                | 
| gross_profit                             | metric              | micro_currency | No                 | 
| agency_fee_gross_cost_percentage         | metric              | percent        | No                 | 
| agency_fee_click_cost                    | metric              | micro_currency | No                 | 
| potential_impressions                    | metric              | integer        | No                 | 
| lost_impressions_rank                    | metric              | integer        | No                 | 
| quality_score                            | metric              | integer        | No                 | 
| last_modified                            | metric              | date           | No                 | 
| total_cost                               | metric              | micro_currency | No                 | 
| avg_cpc                                  | metric              | micro_currency | No                 | 
| ctr                                      | metric              | percent        | No                 | 
| group_impression_share                   | metric              | percent        | No                 | 
| lost_is_budget                           | metric              | percent        | No                 | 
| lost_is_rank                             | metric              | percent        | No                 | 
| profit                                   | metric              | micro_currency | No                 | 
| profit_margin                            | metric              | percent        | No                 | 
| gross_profit_per_click                   | metric              | micro_currency | No                 | 
| gross_profit_per_impression              | metric              | micro_currency | No                 | 
| profit_per_click                         | metric              | micro_currency | No                 | 
| gpoas                                    | metric              | percent        | No                 | 
| roi                                      | metric              | percent        | No                 | 
| headroom                                 | metric              | percent        | No                 | 
| gross_cost                               | metric              | micro_currency | No                 | 
| ecpm                                     | metric              | decimal        | No                 | 
| historical_quality_score                 | metric              | decimal        | No                 | 
| average_bid                              | metric              | micro_currency | No                 | 
| average_position                         | metric              | decimal        | No                 | 


<!-- Converted with the swagger-to-slate https://github.com/lavkumarv/swagger-to-slate -->
