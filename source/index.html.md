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

## Versioning

The Marin API is versioned. This means there can be multiple versions available at any one time. Currently only one version is available:

| Version | Comments |
| ------- | -------- |
| v1.0 | Initial Version of the Marin API |

The version number is specified after the base URL:

`GET http://api.marinsoftware.com/v1.0/clients`


# Authentication 

All endpoints require an access token in order to make requests. To get an access token you will need to make a request to the auth endpoint with your username and password that you will use to access the API.

## Get Authentication Token

Authenticate the users and returns a token

### HTTP Request 
`POST /v1.0/auth` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| body | body | Post Object | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |

# Clients
## Get All Clients

Returns Clients

### HTTP Request 
`GET /v1.0/clients` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Please provide the token | Yes | string |
| id | query | Please provide the Id | No | string |
| status | query | Please provide the  status | No | string |
| currency | query | Please provide the  currency | No | string |
| legacyId | query | Please provide the  legacyId | No | string |
| name | query | Please provide the  name | No | string |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |



## Get a single client

Returns client with client ID

### HTTP Request 
`GET /v1.0/clients/{clientId}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Please provide the token | Yes | string |
| clientId | path | Client ID | Yes | string |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |

## Get all users for a client

Get Users under a client

### HTTP Request 
`GET /v1.0/clients/{clientId}/users` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Please provide the token | Yes | string |
| clientId | path | Client ID | Yes | string |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |

## Create a client

Creates Client

### HTTP Request 
`POST /v1.0/clients` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Please provide the token | Yes | string |
| body | body | Post Object | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |


## Edit a client

Update client with clientId

### HTTP Request 
`PUT /v1.0/clients/{clientId}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Please provide the token | Yes | string |
| clientId | path | Please provide the  clientId to be updated | Yes | number |
| body | body | Post Object | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |

## Delete a client

Delete Client with clientId

### HTTP Request 
`DELETE /v1.0/clients/{clientId}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Please provide the Id | Yes | string |
| clientId | path | Please provide the  clientId to be updated | Yes | number |
| body | body | Post Object | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |


# Customers
## Get all customers

Returns Customers

### HTTP Request 
`GET /v1.0/customers` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Please provide the token | Yes | string |
| id | query | Please provide the Id | No | string |
| status | query | Please provide the  status | No | string |
| legacyId | query | Please provide the  legacyId | No | string |
| name | query | Please provide the  name | No | string |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |

## Get a single customer

Returns Customer with CustomerId

### HTTP Request 
`GET /v1.0/customers/{customerId}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Please provide the token | Yes | string |
| customerId | path | Customer ID | Yes | string |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |


## Create a customer

Create Customer

### HTTP Request 
`POST /v1.0/customers` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Please provide the token | Yes | string |
| body | body | Post Object | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |


## Edit a customer

Update customer

### HTTP Request 
`PUT /v1.0/customers/{customerId}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Please provide the token | Yes | string |
| customerId | path | Please provide the  customerId to be updated | Yes | number |
| body | body | Post Object | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |

## Delete a customer

Deletes customers

### HTTP Request 
`DELETE /v1.0/customers/{customerId}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Please provide the token | Yes | string |
| customerId | path | Please provide the customer Id to be deleted | Yes | number |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |

# Publisher Accounts


## Get all publisher accounts

Returns all Publisher Accounts under a Client

### HTTP Request 
`GET /v1.0/clients/{clientId}/publisherAccounts` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Please provide the token | Yes | string |
| clientId | path | Client ID | Yes | string |
| limit | query | limit docs | Yes | number |
| offset | query | starting value | Yes | number |
| fields | query | comma seperated field names which needs to be shown | No | string |
| from | query | Date string of the format 'YYYY-MM-DD' | No | dateTime |
| to | query | Date string of the format 'YYYY-MM-DD' | No | dateTime |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |

## Get a single publisher account

Returns Publisher Account under a Client

### HTTP Request 
`GET /v1.0/clients/{clientId}/publisherAccounts/{pubAccountId}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Please provide the token | Yes | string |
| clientId | path | Client ID | Yes | string |
| pubAccountId | path | Publisher Accounts ID | Yes | string |
| limit | query | limit docs | Yes | number |
| offset | query | starting value | Yes | number |
| fields | query | comma seperated field names which needs to be shown | No | string |
| from | query | Date string of the format 'YYYY-MM-DD' | No | dateTime |
| to | query | Date string of the format 'YYYY-MM-DD' | No | dateTime |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |

# Campaigns

## Get all campaigns

Returns all Campaigns under a Client

### HTTP Request 
`GET /v1.0/clients/{clientId}/campaigns` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Please provide the token | Yes | string |
| clientId | path | Client ID | Yes | string |
| limit | query | limit docs | Yes | number |
| offset | query | starting value | Yes | number |
| fields | query | comma seperated field names which needs to be shown | No | string |
| from | query | Date string of the format 'YYYY-MM-DD' | No | dateTime |
| to | query | Date string of the format 'YYYY-MM-DD' | No | dateTime |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |

## Get a single campaign

Returns a Campaign under a Client

### HTTP Request 
`GET /v1.0/clients/{clientId}/campaigns/{campaignId}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Please provide the token | Yes | string |
| clientId | path | Client ID | Yes | string |
| campaignId | path | Campaign ID | Yes | string |
| limit | query | limit docs | Yes | number |
| offset | query | starting value | Yes | number |
| fields | query | comma seperated field names which needs to be shown | No | string |
| from | query | Date string of the format 'YYYY-MM-DD' | No | dateTime |
| to | query | Date string of the format 'YYYY-MM-DD' | No | dateTime |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |

# Groups

## Get all groups

Returns all Groups under a Client

### HTTP Request 
`GET /v1.0/clients/{clientId}/groups` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Please provide the token | Yes | string |
| clientId | path | Client ID | Yes | number |
| limit | query | limit docs | Yes | number |
| offset | query | starting value | Yes | number |
| fields | query | comma seperated field names which needs to be shown | No | string |
| from | query | Date string of the format 'YYYY-MM-DD' | No | dateTime |
| to | query | Date string of the format 'YYYY-MM-DD' | No | dateTime |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |

## Get a single group

Returns a Group under a Client

### HTTP Request 
`GET /v1.0/clients/{clientId}/groups/{groupId}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Please provide the token | Yes | string |
| clientId | path | Client ID | Yes | number |
| groupId | path | Group ID | Yes | number |
| limit | query | limit docs | Yes | number |
| offset | query | starting value | Yes | number |
| fields | query | comma seperated field names which needs to be shown | No | string |
| from | query | Date string of the format 'YYYY-MM-DD' | No | dateTime |
| to | query | Date string of the format 'YYYY-MM-DD' | No | dateTime |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |

# Keywords

## Get all keywords

```shell
curl -X GET \
  "http://api.marinsoftware.com/v1.0/clients/1808782/keywords"
  -H 'token: st_cfc8909710f0d020f95dcec821fad453a248ebc7a7f9438f'
```

```http
GET /v1.0/clients/1808782/keywords HTTP/1.1
Host: api.marinsoftware.com
token: st_cfc8909710f0d020f95dcec821fad453a248ebc7a7f9438f
```

> The above command returns JSON structured like this:

```json
{
  "publisher_name": "MSN",
  "min_cpc": 0,
  "impressions": 250,
  "type": "BROAD",
  "max_cpc": 3000000,
  "pub_cost": 23040000,
  "pca": 1591619,
  "campaign_name": "Powpow-AU-[Looker]-{Dates}-Year",
  "pub_clicks": 10,
  "pca_alias": "Powpow-Bing-SEM",
  "campaign": 4779468,
  "client": 1808782,
  "keyword": 10387398,
  "keyword_name": "+cruises +2017",
  "recommended_bid": 0,
  "group": 27052373,
  "customer": 1904153,
  "status": "ACTIVE"
}
```

Returns all Keywords under Client

### HTTP Request 
`GET /v1.0/clients/{clientId}/keywords` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Please provide the token | Yes | string |
| clientId | path | Client ID | Yes | number |
| limit | query | limit docs | Yes | number |
| offset | query | starting value | Yes | number |
| fields | query | comma seperated field names which needs to be shown | No | string |
| from | query | Date string of the format 'YYYY-MM-DD' | No | dateTime |
| to | query | Date string of the format 'YYYY-MM-DD' | No | dateTime |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |

## Get a single keyword

Returns a Keyword under a Client

### HTTP Request 
`GET /v1.0/clients/{clientId}/keywords/{keywordId}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Please provide the token | Yes | string |
| clientId | path | Client ID | Yes | number |
| keywordId | path | Keyword ID | Yes | number |
| limit | query | limit docs | Yes | number |
| offset | query | starting value | Yes | number |
| fields | query | comma seperated field names which needs to be shown | No | string |
| from | query | Date string of the format 'YYYY-MM-DD' | No | dateTime |
| to | query | Date string of the format 'YYYY-MM-DD' | No | dateTime |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |


# Creatives

## Get all creatives

Returns all Creatives under a Client

### HTTP Request 
`GET /v1.0/clients/{clientId}/creatives` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Please provide the token | Yes | string |
| clientId | path | Client ID | Yes | number |
| limit | query | limit docs | Yes | number |
| offset | query | starting value | Yes | number |
| fields | query | comma seperated field names which needs to be shown | No | string |
| from | query | Date string of the format 'YYYY-MM-DD' | No | dateTime |
| to | query | Date string of the format 'YYYY-MM-DD' | No | dateTime |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |

## Get a single creative

Returns a Creative under a Client

### HTTP Request 
`GET /v1.0/clients/{clientId}/creatives/{creativeId}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Please provide the token | Yes | string |
| clientId | path | Client ID | Yes | number |
| creativeId | path | Creative ID | Yes | number |
| limit | query | limit docs | Yes | number |
| offset | query | starting value | Yes | number |
| fields | query | comma seperated field names which needs to be shown | No | string |
| from | query | Date string of the format 'YYYY-MM-DD' | No | dateTime |
| to | query | Date string of the format 'YYYY-MM-DD' | No | dateTime |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |

# Users
## Get all users

Returns Users under a Client

### HTTP Request 
`GET /v1.0/users` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Please provide the token | Yes | string |
| clientId | query | Please provide the respective client Id | Yes | string |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |

## Create a user

Create Users

### HTTP Request 
`POST /v1.0/users` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Please provide the token | Yes | string |
| customerId | query | Please provide the customer Id for authorization | Yes | string |
| body | body | Post Object | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |

## Delete a user

Delete Users

### HTTP Request 
`DELETE /v1.0/users` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Please provide the token | Yes | string |
| body | body | Post Object | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |

# Jobs
## Get all jobs 

Returns jobs

### HTTP Request 
`GET /v1.0/jobs` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Please provide the token | Yes | string |
| clientId | query | Please provide the respective client Id for the respective job | Yes | string |
| customerId | query | Please provide the customer Id for the job | Yes | string |
| type | query | Please provide the customer Id for the job | No | string |
| status | query | Please provide the customer Id for the job | No | string |
| parentJobId | query | Please provide the customer Id for the job | No | number |
| createdBy | query | Please provide the customer Id for the job | No | number |
| limit | query | Please provide the customer Id for the job | No | number |
| phase | query | Please provide the customer Id for the job | No | string |
| startDate | query | Date string of the format 'YYYY-MM-DD' | No | dateTime |
| endDate | query | Date string of the format 'YYYY-MM-DD' | No | dateTime |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |

## Get a single job

Returns Job with JobId

### HTTP Request 
`GET /v1.0/jobs/{jobId}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Please provide the token | Yes | string |
| jobId | path | Please provide the  job Id | Yes | number |
| clientId | query | Please provide the respective client Id for the respective job | Yes | string |
| customerId | query | Please provide the customer Id | Yes | string |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |


## Create a job

Create Jobs

### HTTP Request 
`POST /v1.0/jobs` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Please provide the token | Yes | string |
| body | body | Post Object | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |


## Edit a job

Update Job with JobId

### HTTP Request 
`PUT /v1.0/jobs/{jobId}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Please provide the token | Yes | string |
| jobId | path | Please provide the job Id | Yes | number |
| body | body | Post Object | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |

## Update a job's phase

update job phase

### HTTP Request 
`PUT /v1.0/jobs/{jobId}/phase` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Please provide the token | Yes | string |
| jobId | path | Please provide the job Id | Yes | number |
| body | body | Post Object | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |


## Update a job's status

Update job status

### HTTP Request 
`PUT /v1.0/jobs/{jobId}/status` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Please provide the  token | Yes | string |
| jobId | path | Please provide the job Id | Yes | number |
| body | body | Post Object | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |

# Users

## Get a single user

Get User with UserId

### HTTP Request 
`GET /v1.0/users/{userId}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Please provide the token | Yes | string |
| userId | path | User ID | Yes | number |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |

## Get all clients a user has access to

Get Clients with UserId

### HTTP Request 
`GET /v1.0/users/{userId}/clients` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Please provide the token | Yes | string |
| userId | path | User ID | Yes | number |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |

## Update a user

Update User with UserId

### HTTP Request 
`PUT /v1.0/users/{userId}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | header | Please provide the token | Yes | string |
| userId | path | User ID | Yes | number |
| body | body | Post Object | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |

# Health Check
## Get L1 health status

healthcheckUrls

### HTTP Request 
`GET /admin/status/marin-open-api-service/L1` 

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |


## Get L2 health status

healthcheckUrls

### HTTP Request 
`GET /admin/status/marin-open-api-service/L2` 

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Success |
| default | Error |

<!-- Converted with the swagger-to-slate https://github.com/lavkumarv/swagger-to-slate -->
