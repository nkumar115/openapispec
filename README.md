---
title: Account and Transaction API Specification v3.1.7
language_tabs:
  - python: Python
  - ruby: Ruby
toc_footers: []
includes: []
search: true
highlight_theme: darkula
headingLevel: 2

---

<!-- Generator: Widdershins v4.0.1 -->

<h1 id="account-and-transaction-api-specification">Account and Transaction API Specification v3.1.7</h1>

> Scroll down for code samples, example requests and responses. Select a language for code samples from the tabs above or the mobile navigation menu.

Swagger for Account and Transaction API Specification

Base URLs:

* <a href="https://sandbox-mtls.ob.hsbc.co.uk/open-banking/v3.1/aisp">https://sandbox-mtls.ob.hsbc.co.uk/open-banking/v3.1/aisp</a>

Email: <a href="mailto:xyz@hsbc.com">hsbc</a> Web: <a href="https://www.hsbc.co.in/">hsbc</a> 
License: <a href="https://www.hsbc.co.in/">For internal use only</a>

# Authentication

- oAuth2 authentication. TPP client credential authorisation flow with the ASPSP

    - Flow: clientCredentials

    - Token URL = [https://authserver.example/token](https://authserver.example/token)

|Scope|Scope Description|
|---|---|
|accounts|Ability to read Accounts information|

- oAuth2 authentication. OAuth flow, it is required when the PSU needs to perform SCA with the ASPSP when a TPP wants to access an ASPSP resource owned by the PSU

    - Flow: authorizationCode
    - Authorization URL = [https://authserver.example/authorization](https://authserver.example/authorization)
    - Token URL = [https://authserver.example/token](https://authserver.example/token)

|Scope|Scope Description|
|---|---|
|accounts|Ability to read Accounts information|

<h1 id="account-and-transaction-api-specification-account-access">Account Access</h1>

## CreateAccountAccessConsents

<a id="opIdCreateAccountAccessConsents"></a>

`POST /account-access-consents`

Create Account Access Consents

> Body parameter

```json
{
  "type": "object",
  "required": [
    "Data",
    "Risk"
  ],
  "properties": {
    "Data": {
      "type": "object",
      "required": [
        "Permissions"
      ],
      "properties": {
        "Permissions": {
          "type": "array",
          "items": {
            "description": "Specifies the Open Banking account access data types. This is a list of the data clusters being consented by the PSU, and requested for authorisation with the ASPSP.",
            "type": "string",
            "enum": [
              "ReadAccountsBasic",
              "ReadAccountsDetail",
              "ReadBalances",
              "ReadBeneficiariesBasic",
              "ReadBeneficiariesDetail",
              "ReadDirectDebits",
              "ReadPAN",
              "ReadParty",
              "ReadPartyPSU",
              "ReadProducts",
              "ReadScheduledPaymentsBasic",
              "ReadScheduledPaymentsDetail",
              "ReadStandingOrdersBasic",
              "ReadStandingOrdersDetail",
              "ReadTransactionsBasic",
              "ReadTransactionsCredits",
              "ReadTransactionsDebits",
              "ReadTransactionsDetail"
            ]
          },
          "minItems": 1
        },
        "ExpirationDateTime": {
          "description": "Specified date and time the permissions will expire.\nIf this is not populated, the permissions will be open ended.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "TransactionFromDateTime": {
          "description": "Specified start date and time for the transaction query period.\nIf this is not populated, the start date will be open ended, and data will be returned from the earliest available transaction.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "TransactionToDateTime": {
          "description": "Specified end date and time for the transaction query period.\nIf this is not populated, the end date will be open ended, and data will be returned to the latest available transaction.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        }
      }
    },
    "Risk": {
      "description": "The Risk section is sent by the initiating party to the ASPSP. It is used to specify additional details for risk scoring for Account Info.",
      "type": "object",
      "properties": {},
      "additionalProperties": false
    }
  },
  "additionalProperties": false
}
```

<h3 id="createaccountaccessconsents-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|x-fapi-auth-date|header|string|false|The time when the PSU last logged in with the TPP. |
|x-fapi-customer-ip-address|header|string|false|The PSU's IP address if the PSU is currently logged in with the TPP.|
|x-fapi-interaction-id|header|string|false|An RFC4122 UID used as a correlation id.|
|Authorization|header|string|true|An Authorisation Token as per https://tools.ietf.org/html/rfc6750|
|x-customer-user-agent|header|string|false|Indicates the user-agent that the PSU is using.|
|body|body|[OBReadConsent1](#schemaobreadconsent1)|true|Default|

#### Detailed descriptions

**x-fapi-auth-date**: The time when the PSU last logged in with the TPP. 
All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: 
Sun, 10 Sep 2017 19:43:31 UTC

> Example responses

> 201 Response

```json
{
  "type": "object",
  "required": [
    "Data",
    "Risk"
  ],
  "properties": {
    "Data": {
      "type": "object",
      "required": [
        "ConsentId",
        "CreationDateTime",
        "Status",
        "StatusUpdateDateTime",
        "Permissions"
      ],
      "properties": {
        "ConsentId": {
          "description": "Unique identification as assigned to identify the account access consent resource.",
          "type": "string",
          "minLength": 1,
          "maxLength": 128
        },
        "CreationDateTime": {
          "description": "Date and time at which the resource was created.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "Status": {
          "description": "Specifies the status of consent resource in code form.",
          "type": "string",
          "enum": [
            "Authorised",
            "AwaitingAuthorisation",
            "Rejected",
            "Revoked"
          ]
        },
        "StatusUpdateDateTime": {
          "description": "Date and time at which the resource status was updated.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "Permissions": {
          "type": "array",
          "items": {
            "description": "Specifies the Open Banking account access data types. This is a list of the data clusters being consented by the PSU, and requested for authorisation with the ASPSP.",
            "type": "string",
            "enum": [
              "ReadAccountsBasic",
              "ReadAccountsDetail",
              "ReadBalances",
              "ReadBeneficiariesBasic",
              "ReadBeneficiariesDetail",
              "ReadDirectDebits",
              "ReadPAN",
              "ReadParty",
              "ReadPartyPSU",
              "ReadProducts",
              "ReadScheduledPaymentsBasic",
              "ReadScheduledPaymentsDetail",
              "ReadStandingOrdersBasic",
              "ReadStandingOrdersDetail",
              "ReadTransactionsBasic",
              "ReadTransactionsCredits",
              "ReadTransactionsDebits",
              "ReadTransactionsDetail"
            ]
          },
          "minItems": 1
        },
        "ExpirationDateTime": {
          "description": "Specified date and time the permissions will expire.\nIf this is not populated, the permissions will be open ended.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "TransactionFromDateTime": {
          "description": "Specified start date and time for the transaction query period.\nIf this is not populated, the start date will be open ended, and data will be returned from the earliest available transaction.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "TransactionToDateTime": {
          "description": "Specified end date and time for the transaction query period.\nIf this is not populated, the end date will be open ended, and data will be returned to the latest available transaction.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        }
      }
    },
    "Risk": {
      "description": "The Risk section is sent by the initiating party to the ASPSP. It is used to specify additional details for risk scoring for Account Info.",
      "type": "object",
      "properties": {},
      "additionalProperties": false
    },
    "Links": {
      "type": "object",
      "description": "Links relevant to the payload",
      "properties": {
        "Self": {
          "type": "string",
          "format": "uri"
        },
        "First": {
          "type": "string",
          "format": "uri"
        },
        "Prev": {
          "type": "string",
          "format": "uri"
        },
        "Next": {
          "type": "string",
          "format": "uri"
        },
        "Last": {
          "type": "string",
          "format": "uri"
        }
      },
      "additionalProperties": false,
      "required": [
        "Self"
      ]
    },
    "Meta": {
      "title": "MetaData",
      "type": "object",
      "description": "Meta Data relevant to the payload",
      "properties": {
        "TotalPages": {
          "type": "integer",
          "format": "int32"
        },
        "FirstAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "LastAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        }
      },
      "additionalProperties": false
    }
  },
  "additionalProperties": false
}
```

<h3 id="createaccountaccessconsents-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Account Access Consents Created|[OBReadConsentResponse1](#schemaobreadconsentresponse1)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[OBErrorResponse1](#schemaoberrorresponse1)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[OBErrorResponse1](#schemaoberrorresponse1)|
|405|[Method Not Allowed](https://tools.ietf.org/html/rfc7231#section-6.5.5)|Method Not Allowed|None|
|406|[Not Acceptable](https://tools.ietf.org/html/rfc7231#section-6.5.6)|Not Acceptable|None|
|415|[Unsupported Media Type](https://tools.ietf.org/html/rfc7231#section-6.5.13)|Unsupported Media Type|None|
|429|[Too Many Requests](https://tools.ietf.org/html/rfc6585#section-4)|Too Many Requests|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Internal Server Error|[OBErrorResponse1](#schemaoberrorresponse1)|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|201|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|400|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|401|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|403|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|405|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|406|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|415|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|429|Retry-After|integer||Number in seconds to wait|
|429|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|500|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
TPPOAuth2Security ( Scopes: accounts )
</aside>

## GetAccountAccessConsentsConsentId

<a id="opIdGetAccountAccessConsentsConsentId"></a>

`GET /account-access-consents/{ConsentId}`

Get Account Access Consents

<h3 id="getaccountaccessconsentsconsentid-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|ConsentId|path|string|true|ConsentId|
|x-fapi-auth-date|header|string|false|The time when the PSU last logged in with the TPP. |
|x-fapi-customer-ip-address|header|string|false|The PSU's IP address if the PSU is currently logged in with the TPP.|
|x-fapi-interaction-id|header|string|false|An RFC4122 UID used as a correlation id.|
|Authorization|header|string|true|An Authorisation Token as per https://tools.ietf.org/html/rfc6750|
|x-customer-user-agent|header|string|false|Indicates the user-agent that the PSU is using.|

#### Detailed descriptions

**x-fapi-auth-date**: The time when the PSU last logged in with the TPP. 
All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: 
Sun, 10 Sep 2017 19:43:31 UTC

> Example responses

> 200 Response

```json
{
  "type": "object",
  "required": [
    "Data",
    "Risk"
  ],
  "properties": {
    "Data": {
      "type": "object",
      "required": [
        "ConsentId",
        "CreationDateTime",
        "Status",
        "StatusUpdateDateTime",
        "Permissions"
      ],
      "properties": {
        "ConsentId": {
          "description": "Unique identification as assigned to identify the account access consent resource.",
          "type": "string",
          "minLength": 1,
          "maxLength": 128
        },
        "CreationDateTime": {
          "description": "Date and time at which the resource was created.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "Status": {
          "description": "Specifies the status of consent resource in code form.",
          "type": "string",
          "enum": [
            "Authorised",
            "AwaitingAuthorisation",
            "Rejected",
            "Revoked"
          ]
        },
        "StatusUpdateDateTime": {
          "description": "Date and time at which the resource status was updated.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "Permissions": {
          "type": "array",
          "items": {
            "description": "Specifies the Open Banking account access data types. This is a list of the data clusters being consented by the PSU, and requested for authorisation with the ASPSP.",
            "type": "string",
            "enum": [
              "ReadAccountsBasic",
              "ReadAccountsDetail",
              "ReadBalances",
              "ReadBeneficiariesBasic",
              "ReadBeneficiariesDetail",
              "ReadDirectDebits",
              "ReadPAN",
              "ReadParty",
              "ReadPartyPSU",
              "ReadProducts",
              "ReadScheduledPaymentsBasic",
              "ReadScheduledPaymentsDetail",
              "ReadStandingOrdersBasic",
              "ReadStandingOrdersDetail",
              "ReadTransactionsBasic",
              "ReadTransactionsCredits",
              "ReadTransactionsDebits",
              "ReadTransactionsDetail"
            ]
          },
          "minItems": 1
        },
        "ExpirationDateTime": {
          "description": "Specified date and time the permissions will expire.\nIf this is not populated, the permissions will be open ended.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "TransactionFromDateTime": {
          "description": "Specified start date and time for the transaction query period.\nIf this is not populated, the start date will be open ended, and data will be returned from the earliest available transaction.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "TransactionToDateTime": {
          "description": "Specified end date and time for the transaction query period.\nIf this is not populated, the end date will be open ended, and data will be returned to the latest available transaction.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        }
      }
    },
    "Risk": {
      "description": "The Risk section is sent by the initiating party to the ASPSP. It is used to specify additional details for risk scoring for Account Info.",
      "type": "object",
      "properties": {},
      "additionalProperties": false
    },
    "Links": {
      "type": "object",
      "description": "Links relevant to the payload",
      "properties": {
        "Self": {
          "type": "string",
          "format": "uri"
        },
        "First": {
          "type": "string",
          "format": "uri"
        },
        "Prev": {
          "type": "string",
          "format": "uri"
        },
        "Next": {
          "type": "string",
          "format": "uri"
        },
        "Last": {
          "type": "string",
          "format": "uri"
        }
      },
      "additionalProperties": false,
      "required": [
        "Self"
      ]
    },
    "Meta": {
      "title": "MetaData",
      "type": "object",
      "description": "Meta Data relevant to the payload",
      "properties": {
        "TotalPages": {
          "type": "integer",
          "format": "int32"
        },
        "FirstAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "LastAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        }
      },
      "additionalProperties": false
    }
  },
  "additionalProperties": false
}
```

<h3 id="getaccountaccessconsentsconsentid-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Account Access Consents Read|[OBReadConsentResponse1](#schemaobreadconsentresponse1)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[OBErrorResponse1](#schemaoberrorresponse1)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[OBErrorResponse1](#schemaoberrorresponse1)|
|405|[Method Not Allowed](https://tools.ietf.org/html/rfc7231#section-6.5.5)|Method Not Allowed|None|
|406|[Not Acceptable](https://tools.ietf.org/html/rfc7231#section-6.5.6)|Not Acceptable|None|
|429|[Too Many Requests](https://tools.ietf.org/html/rfc6585#section-4)|Too Many Requests|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Internal Server Error|[OBErrorResponse1](#schemaoberrorresponse1)|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|400|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|401|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|403|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|405|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|406|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|429|Retry-After|integer||Number in seconds to wait|
|429|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|500|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
TPPOAuth2Security ( Scopes: accounts )
</aside>

## DeleteAccountAccessConsentsConsentId

<a id="opIdDeleteAccountAccessConsentsConsentId"></a>

`DELETE /account-access-consents/{ConsentId}`

Delete Account Access Consents

<h3 id="deleteaccountaccessconsentsconsentid-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|ConsentId|path|string|true|ConsentId|
|x-fapi-auth-date|header|string|false|The time when the PSU last logged in with the TPP. |
|x-fapi-customer-ip-address|header|string|false|The PSU's IP address if the PSU is currently logged in with the TPP.|
|x-fapi-interaction-id|header|string|false|An RFC4122 UID used as a correlation id.|
|Authorization|header|string|true|An Authorisation Token as per https://tools.ietf.org/html/rfc6750|
|x-customer-user-agent|header|string|false|Indicates the user-agent that the PSU is using.|

#### Detailed descriptions

**x-fapi-auth-date**: The time when the PSU last logged in with the TPP. 
All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: 
Sun, 10 Sep 2017 19:43:31 UTC

> Example responses

> 400 Response

```json
{
  "description": "An array of detail error codes, and messages, and URLs to documentation to help remediation.",
  "type": "object",
  "properties": {
    "Code": {
      "description": "High level textual error code, to help categorize the errors.",
      "type": "string",
      "minLength": 1,
      "maxLength": 40
    },
    "Id": {
      "description": "A unique reference for the error instance, for audit purposes, in case of unknown/unclassified errors.",
      "type": "string",
      "minLength": 1,
      "maxLength": 40
    },
    "Message": {
      "description": "Brief Error message, e.g., 'There is something wrong with the request parameters provided'",
      "type": "string",
      "minLength": 1,
      "maxLength": 500
    },
    "Errors": {
      "items": {
        "type": "object",
        "properties": {
          "ErrorCode": {
            "description": "Low level textual error code, e.g., UK.OBIE.Field.Missing",
            "type": "string",
            "x-namespaced-enum": [
              "UK.OBIE.Field.Expected",
              "UK.OBIE.Field.Invalid",
              "UK.OBIE.Field.InvalidDate",
              "UK.OBIE.Field.Missing",
              "UK.OBIE.Field.Unexpected",
              "UK.OBIE.Header.Invalid",
              "UK.OBIE.Header.Missing",
              "UK.OBIE.Reauthenticate",
              "UK.OBIE.Resource.ConsentMismatch",
              "UK.OBIE.Resource.InvalidConsentStatus",
              "UK.OBIE.Resource.InvalidFormat",
              "UK.OBIE.Resource.NotFound",
              "UK.OBIE.Rules.AfterCutOffDateTime",
              "UK.OBIE.Rules.DuplicateReference",
              "UK.OBIE.Signature.Invalid",
              "UK.OBIE.Signature.InvalidClaim",
              "UK.OBIE.Signature.Malformed",
              "UK.OBIE.Signature.Missing",
              "UK.OBIE.Signature.MissingClaim",
              "UK.OBIE.Signature.Unexpected",
              "UK.OBIE.UnexpectedError",
              "UK.OBIE.Unsupported.AccountIdentifier",
              "UK.OBIE.Unsupported.AccountSecondaryIdentifier",
              "UK.OBIE.Unsupported.Currency",
              "UK.OBIE.Unsupported.Frequency",
              "UK.OBIE.Unsupported.LocalInstrument",
              "UK.OBIE.Unsupported.Scheme"
            ]
          },
          "Message": {
            "description": "A description of the error that occurred. e.g., 'A mandatory field isn't supplied' or 'RequestedExecutionDateTime must be in future'\nOBIE doesn't standardise this field",
            "type": "string",
            "minLength": 1,
            "maxLength": 500
          },
          "Path": {
            "description": "Recommended but optional reference to the JSON Path of the field with error, e.g., Data.Initiation.InstructedAmount.Currency",
            "type": "string",
            "minLength": 1,
            "maxLength": 500
          },
          "Url": {
            "description": "URL to help remediate the problem, or provide more information, or to API Reference, or help etc",
            "type": "string"
          }
        },
        "required": [
          "ErrorCode",
          "Message"
        ],
        "additionalProperties": false,
        "minProperties": 1
      },
      "type": "array",
      "minItems": 1
    }
  },
  "required": [
    "Code",
    "Message",
    "Errors"
  ],
  "additionalProperties": false
}
```

<h3 id="deleteaccountaccessconsentsconsentid-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|204|[No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)|Account Access Consents Deleted|None|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[OBErrorResponse1](#schemaoberrorresponse1)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[OBErrorResponse1](#schemaoberrorresponse1)|
|405|[Method Not Allowed](https://tools.ietf.org/html/rfc7231#section-6.5.5)|Method Not Allowed|None|
|406|[Not Acceptable](https://tools.ietf.org/html/rfc7231#section-6.5.6)|Not Acceptable|None|
|429|[Too Many Requests](https://tools.ietf.org/html/rfc6585#section-4)|Too Many Requests|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Internal Server Error|[OBErrorResponse1](#schemaoberrorresponse1)|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|204|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|400|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|401|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|403|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|405|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|406|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|429|Retry-After|integer||Number in seconds to wait|
|429|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|500|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
TPPOAuth2Security ( Scopes: accounts )
</aside>

<h1 id="account-and-transaction-api-specification-accounts">Accounts</h1>

## GetAccounts

<a id="opIdGetAccounts"></a>

`GET /accounts`

Get Accounts

<h3 id="getaccounts-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|x-fapi-auth-date|header|string|false|The time when the PSU last logged in with the TPP. |
|x-fapi-customer-ip-address|header|string|false|The PSU's IP address if the PSU is currently logged in with the TPP.|
|x-fapi-interaction-id|header|string|false|An RFC4122 UID used as a correlation id.|
|Authorization|header|string|true|An Authorisation Token as per https://tools.ietf.org/html/rfc6750|
|x-customer-user-agent|header|string|false|Indicates the user-agent that the PSU is using.|

#### Detailed descriptions

**x-fapi-auth-date**: The time when the PSU last logged in with the TPP. 
All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: 
Sun, 10 Sep 2017 19:43:31 UTC

> Example responses

> 200 Response

```json
{
  "type": "object",
  "required": [
    "Data"
  ],
  "properties": {
    "Data": {
      "type": "object",
      "properties": {
        "Account": {
          "type": "array",
          "items": {
            "type": "object",
            "description": "Unambiguous identification of the account to which credit and debit entries are made. The following fields are optional only for accounts that are switched:\n\n  * Data.Currency  \n  * Data.AccountType  \n  * Data.AccountSubType\n\nFor all other accounts, the fields must be populated by the ASPSP.",
            "required": [
              "AccountId"
            ],
            "properties": {
              "AccountId": {
                "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
                "type": "string",
                "minLength": 1,
                "maxLength": 40
              },
              "Status": {
                "description": "Specifies the status of account resource in code form.",
                "type": "string",
                "enum": [
                  "Disabled",
                  "Enabled"
                ]
              },
              "StatusUpdateDateTime": {
                "description": "Date and time at which the resource status was updated.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
                "type": "string",
                "format": "date-time"
              },
              "Currency": {
                "description": "Identification of the currency in which the account is held. \nUsage: Currency should only be used in case one and the same account number covers several currencies\nand the initiating party needs to identify which currency needs to be used for settlement on the account.",
                "type": "string",
                "pattern": "^[A-Z]{3,3}$"
              },
              "AccountType": {
                "description": "Specifies the type of account (personal or business).",
                "type": "string",
                "enum": [
                  "Business",
                  "Personal"
                ]
              },
              "AccountSubType": {
                "description": "Specifies the sub type of account (product family group).",
                "type": "string",
                "enum": [
                  "CreditCard",
                  "CurrentAccount",
                  "Savings"
                ]
              },
              "Description": {
                "description": "Specifies the description of the account type.",
                "type": "string",
                "minLength": 1,
                "maxLength": 35
              },
              "Nickname": {
                "description": "The nickname of the account, assigned by the account owner in order to provide an additional means of identification of the account.",
                "type": "string",
                "minLength": 1,
                "maxLength": 70
              },
              "SwitchStatus": {
                "description": "Specifies the switch status for the account, in a coded form.",
                "type": "string",
                "x-namespaced-enum": [
                  "UK.CASS.NotSwitced",
                  "UK.CASS.SwitchCompleted"
                ]
              },
              "Account": {
                "type": "array",
                "items": {
                  "type": "object",
                  "description": "Provides the details to identify an account.",
                  "required": [
                    "SchemeName",
                    "Identification"
                  ],
                  "properties": {
                    "SchemeName": {
                      "description": "Name of the identification scheme, in a coded form as published in an external list.",
                      "type": "string",
                      "x-namespaced-enum": [
                        "UK.OBIE.SortCodeAccountNumber"
                      ]
                    },
                    "Identification": {
                      "description": "Identification assigned by an institution to identify an account. This identification is known by the account owner.",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 256
                    },
                    "Name": {
                      "description": "The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.\nNote, the account name is not the product name or the nickname of the account.",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 350
                    },
                    "SecondaryIdentification": {
                      "description": "This is secondary identification of the account, as assigned by the account servicing institution. \nThis can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 34
                    }
                  }
                }
              },
              "Servicer": {
                "type": "object",
                "required": [
                  "SchemeName",
                  "Identification"
                ],
                "description": "Party that manages the account on behalf of the account owner, that is manages the registration and booking of entries on the account, calculates balances on the account and provides information about the account.",
                "properties": {
                  "SchemeName": {
                    "description": "Name of the identification scheme, in a coded form as published in an external list.",
                    "type": "string",
                    "x-namespaced-enum": [
                      "UK.OBIE.BICFI"
                    ]
                  },
                  "Identification": {
                    "description": "Unique and unambiguous identification of the servicing institution.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 35
                  }
                }
              }
            },
            "additionalProperties": false
          }
        }
      }
    },
    "Links": {
      "type": "object",
      "description": "Links relevant to the payload",
      "properties": {
        "Self": {
          "type": "string",
          "format": "uri"
        },
        "First": {
          "type": "string",
          "format": "uri"
        },
        "Prev": {
          "type": "string",
          "format": "uri"
        },
        "Next": {
          "type": "string",
          "format": "uri"
        },
        "Last": {
          "type": "string",
          "format": "uri"
        }
      },
      "additionalProperties": false,
      "required": [
        "Self"
      ]
    },
    "Meta": {
      "title": "MetaData",
      "type": "object",
      "description": "Meta Data relevant to the payload",
      "properties": {
        "TotalPages": {
          "type": "integer",
          "format": "int32"
        },
        "FirstAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "LastAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        }
      },
      "additionalProperties": false
    }
  },
  "additionalProperties": false
}
```

<h3 id="getaccounts-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Accounts Read|[OBReadAccount6](#schemaobreadaccount6)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[OBErrorResponse1](#schemaoberrorresponse1)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[OBErrorResponse1](#schemaoberrorresponse1)|
|405|[Method Not Allowed](https://tools.ietf.org/html/rfc7231#section-6.5.5)|Method Not Allowed|None|
|406|[Not Acceptable](https://tools.ietf.org/html/rfc7231#section-6.5.6)|Not Acceptable|None|
|429|[Too Many Requests](https://tools.ietf.org/html/rfc6585#section-4)|Too Many Requests|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Internal Server Error|[OBErrorResponse1](#schemaoberrorresponse1)|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|400|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|401|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|403|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|405|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|406|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|429|Retry-After|integer||Number in seconds to wait|
|429|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|500|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
PSUOAuth2Security ( Scopes: accounts )
</aside>

## GetAccountsAccountId

<a id="opIdGetAccountsAccountId"></a>

`GET /accounts/{AccountId}`

Get Accounts

<h3 id="getaccountsaccountid-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|AccountId|path|string|true|AccountId|
|x-fapi-auth-date|header|string|false|The time when the PSU last logged in with the TPP. |
|x-fapi-customer-ip-address|header|string|false|The PSU's IP address if the PSU is currently logged in with the TPP.|
|x-fapi-interaction-id|header|string|false|An RFC4122 UID used as a correlation id.|
|Authorization|header|string|true|An Authorisation Token as per https://tools.ietf.org/html/rfc6750|
|x-customer-user-agent|header|string|false|Indicates the user-agent that the PSU is using.|

#### Detailed descriptions

**x-fapi-auth-date**: The time when the PSU last logged in with the TPP. 
All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: 
Sun, 10 Sep 2017 19:43:31 UTC

> Example responses

> 200 Response

```json
{
  "type": "object",
  "required": [
    "Data"
  ],
  "properties": {
    "Data": {
      "type": "object",
      "properties": {
        "Account": {
          "type": "array",
          "items": {
            "type": "object",
            "description": "Unambiguous identification of the account to which credit and debit entries are made. The following fields are optional only for accounts that are switched:\n\n  * Data.Currency  \n  * Data.AccountType  \n  * Data.AccountSubType\n\nFor all other accounts, the fields must be populated by the ASPSP.",
            "required": [
              "AccountId"
            ],
            "properties": {
              "AccountId": {
                "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
                "type": "string",
                "minLength": 1,
                "maxLength": 40
              },
              "Status": {
                "description": "Specifies the status of account resource in code form.",
                "type": "string",
                "enum": [
                  "Disabled",
                  "Enabled"
                ]
              },
              "StatusUpdateDateTime": {
                "description": "Date and time at which the resource status was updated.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
                "type": "string",
                "format": "date-time"
              },
              "Currency": {
                "description": "Identification of the currency in which the account is held. \nUsage: Currency should only be used in case one and the same account number covers several currencies\nand the initiating party needs to identify which currency needs to be used for settlement on the account.",
                "type": "string",
                "pattern": "^[A-Z]{3,3}$"
              },
              "AccountType": {
                "description": "Specifies the type of account (personal or business).",
                "type": "string",
                "enum": [
                  "Business",
                  "Personal"
                ]
              },
              "AccountSubType": {
                "description": "Specifies the sub type of account (product family group).",
                "type": "string",
                "enum": [
                  "CreditCard",
                  "CurrentAccount",
                  "Savings"
                ]
              },
              "Description": {
                "description": "Specifies the description of the account type.",
                "type": "string",
                "minLength": 1,
                "maxLength": 35
              },
              "Nickname": {
                "description": "The nickname of the account, assigned by the account owner in order to provide an additional means of identification of the account.",
                "type": "string",
                "minLength": 1,
                "maxLength": 70
              },
              "SwitchStatus": {
                "description": "Specifies the switch status for the account, in a coded form.",
                "type": "string",
                "x-namespaced-enum": [
                  "UK.CASS.NotSwitced",
                  "UK.CASS.SwitchCompleted"
                ]
              },
              "Account": {
                "type": "array",
                "items": {
                  "type": "object",
                  "description": "Provides the details to identify an account.",
                  "required": [
                    "SchemeName",
                    "Identification"
                  ],
                  "properties": {
                    "SchemeName": {
                      "description": "Name of the identification scheme, in a coded form as published in an external list.",
                      "type": "string",
                      "x-namespaced-enum": [
                        "UK.OBIE.SortCodeAccountNumber"
                      ]
                    },
                    "Identification": {
                      "description": "Identification assigned by an institution to identify an account. This identification is known by the account owner.",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 256
                    },
                    "Name": {
                      "description": "The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.\nNote, the account name is not the product name or the nickname of the account.",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 350
                    },
                    "SecondaryIdentification": {
                      "description": "This is secondary identification of the account, as assigned by the account servicing institution. \nThis can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 34
                    }
                  }
                }
              },
              "Servicer": {
                "type": "object",
                "required": [
                  "SchemeName",
                  "Identification"
                ],
                "description": "Party that manages the account on behalf of the account owner, that is manages the registration and booking of entries on the account, calculates balances on the account and provides information about the account.",
                "properties": {
                  "SchemeName": {
                    "description": "Name of the identification scheme, in a coded form as published in an external list.",
                    "type": "string",
                    "x-namespaced-enum": [
                      "UK.OBIE.BICFI"
                    ]
                  },
                  "Identification": {
                    "description": "Unique and unambiguous identification of the servicing institution.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 35
                  }
                }
              }
            },
            "additionalProperties": false
          }
        }
      }
    },
    "Links": {
      "type": "object",
      "description": "Links relevant to the payload",
      "properties": {
        "Self": {
          "type": "string",
          "format": "uri"
        },
        "First": {
          "type": "string",
          "format": "uri"
        },
        "Prev": {
          "type": "string",
          "format": "uri"
        },
        "Next": {
          "type": "string",
          "format": "uri"
        },
        "Last": {
          "type": "string",
          "format": "uri"
        }
      },
      "additionalProperties": false,
      "required": [
        "Self"
      ]
    },
    "Meta": {
      "title": "MetaData",
      "type": "object",
      "description": "Meta Data relevant to the payload",
      "properties": {
        "TotalPages": {
          "type": "integer",
          "format": "int32"
        },
        "FirstAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "LastAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        }
      },
      "additionalProperties": false
    }
  },
  "additionalProperties": false
}
```

<h3 id="getaccountsaccountid-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Accounts Read|[OBReadAccount6](#schemaobreadaccount6)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[OBErrorResponse1](#schemaoberrorresponse1)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[OBErrorResponse1](#schemaoberrorresponse1)|
|405|[Method Not Allowed](https://tools.ietf.org/html/rfc7231#section-6.5.5)|Method Not Allowed|None|
|406|[Not Acceptable](https://tools.ietf.org/html/rfc7231#section-6.5.6)|Not Acceptable|None|
|429|[Too Many Requests](https://tools.ietf.org/html/rfc6585#section-4)|Too Many Requests|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Internal Server Error|[OBErrorResponse1](#schemaoberrorresponse1)|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|400|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|401|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|403|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|405|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|406|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|429|Retry-After|integer||Number in seconds to wait|
|429|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|500|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
PSUOAuth2Security ( Scopes: accounts )
</aside>

<h1 id="account-and-transaction-api-specification-balances">Balances</h1>

## GetAccountsAccountIdBalances

<a id="opIdGetAccountsAccountIdBalances"></a>

`GET /accounts/{AccountId}/balances`

Get Balances

<h3 id="getaccountsaccountidbalances-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|AccountId|path|string|true|AccountId|
|x-fapi-auth-date|header|string|false|The time when the PSU last logged in with the TPP. |
|x-fapi-customer-ip-address|header|string|false|The PSU's IP address if the PSU is currently logged in with the TPP.|
|x-fapi-interaction-id|header|string|false|An RFC4122 UID used as a correlation id.|
|Authorization|header|string|true|An Authorisation Token as per https://tools.ietf.org/html/rfc6750|
|x-customer-user-agent|header|string|false|Indicates the user-agent that the PSU is using.|

#### Detailed descriptions

**x-fapi-auth-date**: The time when the PSU last logged in with the TPP. 
All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: 
Sun, 10 Sep 2017 19:43:31 UTC

> Example responses

> 200 Response

```json
{
  "type": "object",
  "required": [
    "Data"
  ],
  "properties": {
    "Data": {
      "type": "object",
      "required": [
        "Balance"
      ],
      "properties": {
        "Balance": {
          "type": "array",
          "items": {
            "type": "object",
            "description": "Set of elements used to define the balance details.",
            "required": [
              "AccountId",
              "CreditDebitIndicator",
              "Type",
              "DateTime",
              "Amount"
            ],
            "properties": {
              "AccountId": {
                "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
                "type": "string",
                "minLength": 1,
                "maxLength": 40
              },
              "CreditDebitIndicator": {
                "description": "Indicates whether the balance is a credit or a debit balance. \nUsage: A zero balance is considered to be a credit balance.",
                "type": "string",
                "enum": [
                  "Credit",
                  "Debit"
                ]
              },
              "Type": {
                "description": "Balance type, in a coded form.",
                "type": "string",
                "enum": [
                  "Expected",
                  "InterimBooked"
                ]
              },
              "DateTime": {
                "description": "Indicates the date (and time) of the balance.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
                "type": "string",
                "format": "date-time"
              },
              "Amount": {
                "type": "object",
                "required": [
                  "Amount",
                  "Currency"
                ],
                "description": "Amount of money of the cash balance.",
                "properties": {
                  "Amount": {
                    "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
                    "type": "string",
                    "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
                  },
                  "Currency": {
                    "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
                    "type": "string",
                    "pattern": "^[A-Z]{3,3}$"
                  }
                }
              },
              "CreditLine": {
                "type": "array",
                "items": {
                  "type": "object",
                  "description": "Set of elements used to provide details on the credit line.",
                  "required": [
                    "Included"
                  ],
                  "properties": {
                    "Included": {
                      "description": "Indicates whether or not the credit line is included in the balance of the account.\nUsage: If not present, credit line is not included in the balance amount of the account.",
                      "type": "boolean"
                    },
                    "Type": {
                      "description": "Limit type, in a coded form.",
                      "type": "string",
                      "enum": [
                        "Pre-Agreed",
                        "Temporary"
                      ]
                    },
                    "Amount": {
                      "type": "object",
                      "required": [
                        "Amount",
                        "Currency"
                      ],
                      "description": "Amount of money of the credit line.",
                      "properties": {
                        "Amount": {
                          "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
                          "type": "string",
                          "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
                        },
                        "Currency": {
                          "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
                          "type": "string",
                          "pattern": "^[A-Z]{3,3}$"
                        }
                      }
                    }
                  }
                }
              }
            }
          },
          "minItems": 1
        }
      }
    },
    "Links": {
      "type": "object",
      "description": "Links relevant to the payload",
      "properties": {
        "Self": {
          "type": "string",
          "format": "uri"
        },
        "First": {
          "type": "string",
          "format": "uri"
        },
        "Prev": {
          "type": "string",
          "format": "uri"
        },
        "Next": {
          "type": "string",
          "format": "uri"
        },
        "Last": {
          "type": "string",
          "format": "uri"
        }
      },
      "additionalProperties": false,
      "required": [
        "Self"
      ]
    },
    "Meta": {
      "title": "MetaData",
      "type": "object",
      "description": "Meta Data relevant to the payload",
      "properties": {
        "TotalPages": {
          "type": "integer",
          "format": "int32"
        },
        "FirstAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "LastAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        }
      },
      "additionalProperties": false
    }
  },
  "additionalProperties": false
}
```

<h3 id="getaccountsaccountidbalances-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Balances Read|[OBReadBalance1](#schemaobreadbalance1)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[OBErrorResponse1](#schemaoberrorresponse1)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[OBErrorResponse1](#schemaoberrorresponse1)|
|405|[Method Not Allowed](https://tools.ietf.org/html/rfc7231#section-6.5.5)|Method Not Allowed|None|
|406|[Not Acceptable](https://tools.ietf.org/html/rfc7231#section-6.5.6)|Not Acceptable|None|
|429|[Too Many Requests](https://tools.ietf.org/html/rfc6585#section-4)|Too Many Requests|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Internal Server Error|[OBErrorResponse1](#schemaoberrorresponse1)|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|400|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|401|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|403|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|405|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|406|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|429|Retry-After|integer||Number in seconds to wait|
|429|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|500|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
PSUOAuth2Security ( Scopes: accounts )
</aside>

<h1 id="account-and-transaction-api-specification-beneficiaries">Beneficiaries</h1>

## GetAccountsAccountIdBeneficiaries

<a id="opIdGetAccountsAccountIdBeneficiaries"></a>

`GET /accounts/{AccountId}/beneficiaries`

Get Beneficiaries

<h3 id="getaccountsaccountidbeneficiaries-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|AccountId|path|string|true|AccountId|
|x-fapi-auth-date|header|string|false|The time when the PSU last logged in with the TPP. |
|x-fapi-customer-ip-address|header|string|false|The PSU's IP address if the PSU is currently logged in with the TPP.|
|x-fapi-interaction-id|header|string|false|An RFC4122 UID used as a correlation id.|
|Authorization|header|string|true|An Authorisation Token as per https://tools.ietf.org/html/rfc6750|
|x-customer-user-agent|header|string|false|Indicates the user-agent that the PSU is using.|

#### Detailed descriptions

**x-fapi-auth-date**: The time when the PSU last logged in with the TPP. 
All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: 
Sun, 10 Sep 2017 19:43:31 UTC

> Example responses

> 200 Response

```json
{
  "type": "object",
  "required": [
    "Data"
  ],
  "properties": {
    "Data": {
      "type": "object",
      "properties": {
        "Beneficiary": {
          "type": "array",
          "items": {
            "type": "object",
            "properties": {
              "AccountId": {
                "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
                "type": "string",
                "minLength": 1,
                "maxLength": 40
              },
              "BeneficiaryId": {
                "description": "A unique and immutable identifier used to identify the beneficiary resource. This identifier has no meaning to the account owner.",
                "type": "string",
                "minLength": 1,
                "maxLength": 40
              },
              "Reference": {
                "description": "Unique reference, as assigned by the creditor, to unambiguously refer to the payment transaction.\nUsage: If available, the initiating party should provide this reference in the structured remittance information, to enable reconciliation by the creditor upon receipt of the amount of money.\nIf the business context requires the use of a creditor reference or a payment remit identification, and only one identifier can be passed through the end-to-end chain, the creditor's reference or payment remittance identification should be quoted in the end-to-end transaction identification.",
                "type": "string",
                "minLength": 1,
                "maxLength": 35
              },
              "CreditorAccount": {
                "type": "object",
                "required": [
                  "SchemeName",
                  "Identification"
                ],
                "description": "Provides the details to identify the beneficiary account.",
                "properties": {
                  "SchemeName": {
                    "description": "Name of the identification scheme, in a coded form as published in an external list.",
                    "type": "string",
                    "x-namespaced-enum": [
                      "UK.OBIE.SortCodeAccountNumber"
                    ]
                  },
                  "Identification": {
                    "description": "Identification assigned by an institution to identify an account. This identification is known by the account owner.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 256
                  },
                  "Name": {
                    "description": "The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.\nNote, the account name is not the product name or the nickname of the account.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 350
                  },
                  "SecondaryIdentification": {
                    "description": "This is secondary identification of the account, as assigned by the account servicing institution. \nThis can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 34
                  }
                }
              }
            },
            "additionalProperties": false
          }
        }
      }
    },
    "Links": {
      "type": "object",
      "description": "Links relevant to the payload",
      "properties": {
        "Self": {
          "type": "string",
          "format": "uri"
        },
        "First": {
          "type": "string",
          "format": "uri"
        },
        "Prev": {
          "type": "string",
          "format": "uri"
        },
        "Next": {
          "type": "string",
          "format": "uri"
        },
        "Last": {
          "type": "string",
          "format": "uri"
        }
      },
      "additionalProperties": false,
      "required": [
        "Self"
      ]
    },
    "Meta": {
      "title": "MetaData",
      "type": "object",
      "description": "Meta Data relevant to the payload",
      "properties": {
        "TotalPages": {
          "type": "integer",
          "format": "int32"
        },
        "FirstAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "LastAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        }
      },
      "additionalProperties": false
    }
  },
  "additionalProperties": false
}
```

<h3 id="getaccountsaccountidbeneficiaries-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Beneficiaries Read|[OBReadBeneficiary5](#schemaobreadbeneficiary5)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[OBErrorResponse1](#schemaoberrorresponse1)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[OBErrorResponse1](#schemaoberrorresponse1)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Not found|None|
|405|[Method Not Allowed](https://tools.ietf.org/html/rfc7231#section-6.5.5)|Method Not Allowed|None|
|406|[Not Acceptable](https://tools.ietf.org/html/rfc7231#section-6.5.6)|Not Acceptable|None|
|429|[Too Many Requests](https://tools.ietf.org/html/rfc6585#section-4)|Too Many Requests|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Internal Server Error|[OBErrorResponse1](#schemaoberrorresponse1)|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|400|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|401|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|403|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|404|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|405|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|406|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|429|Retry-After|integer||Number in seconds to wait|
|429|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|500|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
PSUOAuth2Security ( Scopes: accounts )
</aside>

<h1 id="account-and-transaction-api-specification-direct-debits">Direct Debits</h1>

## GetAccountsAccountIdDirectDebits

<a id="opIdGetAccountsAccountIdDirectDebits"></a>

`GET /accounts/{AccountId}/direct-debits`

Get Direct Debits

<h3 id="getaccountsaccountiddirectdebits-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|AccountId|path|string|true|AccountId|
|x-fapi-auth-date|header|string|false|The time when the PSU last logged in with the TPP. |
|x-fapi-customer-ip-address|header|string|false|The PSU's IP address if the PSU is currently logged in with the TPP.|
|x-fapi-interaction-id|header|string|false|An RFC4122 UID used as a correlation id.|
|Authorization|header|string|true|An Authorisation Token as per https://tools.ietf.org/html/rfc6750|
|x-customer-user-agent|header|string|false|Indicates the user-agent that the PSU is using.|

#### Detailed descriptions

**x-fapi-auth-date**: The time when the PSU last logged in with the TPP. 
All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: 
Sun, 10 Sep 2017 19:43:31 UTC

> Example responses

> 200 Response

```json
{
  "type": "object",
  "required": [
    "Data"
  ],
  "properties": {
    "Data": {
      "type": "object",
      "properties": {
        "DirectDebit": {
          "type": "array",
          "items": {
            "type": "object",
            "description": "Account to or from which a cash entry is made.",
            "required": [
              "AccountId",
              "MandateIdentification",
              "Name"
            ],
            "properties": {
              "AccountId": {
                "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
                "type": "string",
                "minLength": 1,
                "maxLength": 40
              },
              "DirectDebitId": {
                "description": "A unique and immutable identifier used to identify the direct debit resource. This identifier has no meaning to the account owner.",
                "type": "string",
                "minLength": 1,
                "maxLength": 40
              },
              "MandateIdentification": {
                "description": "Direct Debit reference. For AUDDIS service users provide Core Reference. For non AUDDIS service users provide Core reference if possible or last used reference.",
                "type": "string",
                "minLength": 1,
                "maxLength": 35
              },
              "DirectDebitStatusCode": {
                "description": "Specifies the status of the direct debit in code form.",
                "type": "string",
                "enum": [
                  "Active",
                  "Inactive"
                ]
              },
              "Name": {
                "description": "Name of Service User.",
                "type": "string",
                "minLength": 1,
                "maxLength": 70
              },
              "PreviousPaymentDateTime": {
                "description": "Date of most recent direct debit collection.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
                "type": "string",
                "format": "date-time"
              },
              "PreviousPaymentAmount": {
                "type": "object",
                "required": [
                  "Amount",
                  "Currency"
                ],
                "description": "The amount of the most recent direct debit collection.",
                "properties": {
                  "Amount": {
                    "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
                    "type": "string",
                    "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
                  },
                  "Currency": {
                    "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
                    "type": "string",
                    "pattern": "^[A-Z]{3,3}$"
                  }
                }
              }
            }
          }
        }
      }
    },
    "Links": {
      "type": "object",
      "description": "Links relevant to the payload",
      "properties": {
        "Self": {
          "type": "string",
          "format": "uri"
        },
        "First": {
          "type": "string",
          "format": "uri"
        },
        "Prev": {
          "type": "string",
          "format": "uri"
        },
        "Next": {
          "type": "string",
          "format": "uri"
        },
        "Last": {
          "type": "string",
          "format": "uri"
        }
      },
      "additionalProperties": false,
      "required": [
        "Self"
      ]
    },
    "Meta": {
      "title": "MetaData",
      "type": "object",
      "description": "Meta Data relevant to the payload",
      "properties": {
        "TotalPages": {
          "type": "integer",
          "format": "int32"
        },
        "FirstAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "LastAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        }
      },
      "additionalProperties": false
    }
  },
  "additionalProperties": false
}
```

<h3 id="getaccountsaccountiddirectdebits-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Direct Debits Read|[OBReadDirectDebit2](#schemaobreaddirectdebit2)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[OBErrorResponse1](#schemaoberrorresponse1)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[OBErrorResponse1](#schemaoberrorresponse1)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Not found|None|
|405|[Method Not Allowed](https://tools.ietf.org/html/rfc7231#section-6.5.5)|Method Not Allowed|None|
|406|[Not Acceptable](https://tools.ietf.org/html/rfc7231#section-6.5.6)|Not Acceptable|None|
|429|[Too Many Requests](https://tools.ietf.org/html/rfc6585#section-4)|Too Many Requests|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Internal Server Error|[OBErrorResponse1](#schemaoberrorresponse1)|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|400|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|401|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|403|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|404|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|405|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|406|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|429|Retry-After|integer||Number in seconds to wait|
|429|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|500|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
PSUOAuth2Security ( Scopes: accounts )
</aside>

<h1 id="account-and-transaction-api-specification-parties">Parties</h1>

## GetAccountsAccountIdParties

<a id="opIdGetAccountsAccountIdParties"></a>

`GET /accounts/{AccountId}/parties`

Get Parties

<h3 id="getaccountsaccountidparties-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|AccountId|path|string|true|AccountId|
|x-fapi-auth-date|header|string|false|The time when the PSU last logged in with the TPP. |
|x-fapi-customer-ip-address|header|string|false|The PSU's IP address if the PSU is currently logged in with the TPP.|
|x-fapi-interaction-id|header|string|false|An RFC4122 UID used as a correlation id.|
|Authorization|header|string|true|An Authorisation Token as per https://tools.ietf.org/html/rfc6750|
|x-customer-user-agent|header|string|false|Indicates the user-agent that the PSU is using.|

#### Detailed descriptions

**x-fapi-auth-date**: The time when the PSU last logged in with the TPP. 
All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: 
Sun, 10 Sep 2017 19:43:31 UTC

> Example responses

> 200 Response

```json
{
  "type": "object",
  "required": [
    "Data"
  ],
  "properties": {
    "Data": {
      "type": "object",
      "properties": {
        "Party": {
          "type": "array",
          "items": {
            "type": "object",
            "required": [
              "PartyId"
            ],
            "properties": {
              "PartyId": {
                "description": "A unique and immutable identifier used to identify the customer resource. This identifier has no meaning to the account owner.",
                "type": "string",
                "minLength": 1,
                "maxLength": 40
              },
              "PartyType": {
                "description": "Party type, in a coded form.",
                "type": "string",
                "enum": [
                  "Delegate",
                  "Joint",
                  "Sole"
                ]
              },
              "Name": {
                "description": "Name by which a party is known and which is usually used to identify that party.",
                "type": "string",
                "minLength": 1,
                "maxLength": 350
              },
              "FullLegalName": {
                "description": "Specifies a character string with a maximum length of 350 characters.",
                "type": "string",
                "minLength": 1,
                "maxLength": 350
              },
              "EmailAddress": {
                "description": "Address for electronic mail (e-mail).",
                "type": "string",
                "minLength": 1,
                "maxLength": 256
              },
              "Relationships": {
                "type": "object",
                "description": "The Party's relationships with other resources.",
                "properties": {
                  "Account": {
                    "type": "object",
                    "required": [
                      "Related",
                      "Id"
                    ],
                    "description": "Relationship to the Account resource.",
                    "properties": {
                      "Related": {
                        "description": "Absolute URI to the related resource.",
                        "type": "string",
                        "format": "uri"
                      },
                      "Id": {
                        "description": "Unique identification as assigned by the ASPSP to uniquely identify the related resource.",
                        "type": "string",
                        "minLength": 1,
                        "maxLength": 40
                      }
                    }
                  }
                }
              },
              "Address": {
                "type": "array",
                "items": {
                  "type": "object",
                  "description": "Postal address of a party.",
                  "required": [
                    "Country"
                  ],
                  "properties": {
                    "AddressType": {
                      "description": "Identifies the nature of the postal address.",
                      "type": "string",
                      "enum": [
                        "Business",
                        "Correspondence",
                        "DeliveryTo",
                        "MailTo",
                        "POBox",
                        "Postal",
                        "Residential",
                        "Statement"
                      ]
                    },
                    "AddressLine": {
                      "type": "array",
                      "items": {
                        "description": "Information that locates and identifies a specific address, as defined by postal services, that is presented in free format text.",
                        "type": "string",
                        "minLength": 1,
                        "maxLength": 70
                      },
                      "minItems": 0,
                      "maxItems": 5
                    },
                    "StreetName": {
                      "description": "Name of a street or thoroughfare.",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 70
                    },
                    "BuildingNumber": {
                      "description": "Number that identifies the position of a building on a street.",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 16
                    },
                    "PostCode": {
                      "description": "Identifier consisting of a group of letters and/or numbers that is added to a postal address to assist the sorting of mail.",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 16
                    },
                    "TownName": {
                      "description": "Name of a built-up area, with defined boundaries, and a local government.",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 35
                    },
                    "CountrySubDivision": {
                      "description": "Identifies a subdivision of a country eg, state, region, county.",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 35
                    },
                    "Country": {
                      "description": "Nation with its own government, occupying a particular territory.",
                      "type": "string",
                      "pattern": "^[A-Z]{2,2}$"
                    }
                  }
                }
              }
            },
            "additionalProperties": false
          }
        }
      }
    },
    "Links": {
      "type": "object",
      "description": "Links relevant to the payload",
      "properties": {
        "Self": {
          "type": "string",
          "format": "uri"
        },
        "First": {
          "type": "string",
          "format": "uri"
        },
        "Prev": {
          "type": "string",
          "format": "uri"
        },
        "Next": {
          "type": "string",
          "format": "uri"
        },
        "Last": {
          "type": "string",
          "format": "uri"
        }
      },
      "additionalProperties": false,
      "required": [
        "Self"
      ]
    },
    "Meta": {
      "title": "MetaData",
      "type": "object",
      "description": "Meta Data relevant to the payload",
      "properties": {
        "TotalPages": {
          "type": "integer",
          "format": "int32"
        },
        "FirstAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "LastAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        }
      },
      "additionalProperties": false
    }
  },
  "additionalProperties": false
}
```

<h3 id="getaccountsaccountidparties-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Parties Read|[OBReadParty3](#schemaobreadparty3)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[OBErrorResponse1](#schemaoberrorresponse1)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[OBErrorResponse1](#schemaoberrorresponse1)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Not found|None|
|405|[Method Not Allowed](https://tools.ietf.org/html/rfc7231#section-6.5.5)|Method Not Allowed|None|
|406|[Not Acceptable](https://tools.ietf.org/html/rfc7231#section-6.5.6)|Not Acceptable|None|
|429|[Too Many Requests](https://tools.ietf.org/html/rfc6585#section-4)|Too Many Requests|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Internal Server Error|[OBErrorResponse1](#schemaoberrorresponse1)|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|400|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|401|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|403|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|404|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|405|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|406|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|429|Retry-After|integer||Number in seconds to wait|
|429|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|500|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
PSUOAuth2Security ( Scopes: accounts )
</aside>

## GetAccountsAccountIdParty

<a id="opIdGetAccountsAccountIdParty"></a>

`GET /accounts/{AccountId}/party`

Get Parties

<h3 id="getaccountsaccountidparty-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|AccountId|path|string|true|AccountId|
|x-fapi-auth-date|header|string|false|The time when the PSU last logged in with the TPP. |
|x-fapi-customer-ip-address|header|string|false|The PSU's IP address if the PSU is currently logged in with the TPP.|
|x-fapi-interaction-id|header|string|false|An RFC4122 UID used as a correlation id.|
|Authorization|header|string|true|An Authorisation Token as per https://tools.ietf.org/html/rfc6750|
|x-customer-user-agent|header|string|false|Indicates the user-agent that the PSU is using.|

#### Detailed descriptions

**x-fapi-auth-date**: The time when the PSU last logged in with the TPP. 
All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: 
Sun, 10 Sep 2017 19:43:31 UTC

> Example responses

> 200 Response

```json
{
  "type": "object",
  "required": [
    "Data"
  ],
  "properties": {
    "Data": {
      "type": "object",
      "properties": {
        "Party": {
          "type": "object",
          "required": [
            "PartyId"
          ],
          "properties": {
            "PartyId": {
              "description": "A unique and immutable identifier used to identify the customer resource. This identifier has no meaning to the account owner.",
              "type": "string",
              "minLength": 1,
              "maxLength": 40
            },
            "PartyType": {
              "description": "Party type, in a coded form.",
              "type": "string",
              "enum": [
                "Delegate",
                "Joint",
                "Sole"
              ]
            },
            "Name": {
              "description": "Name by which a party is known and which is usually used to identify that party.",
              "type": "string",
              "minLength": 1,
              "maxLength": 350
            },
            "FullLegalName": {
              "description": "Specifies a character string with a maximum length of 350 characters.",
              "type": "string",
              "minLength": 1,
              "maxLength": 350
            },
            "EmailAddress": {
              "description": "Address for electronic mail (e-mail).",
              "type": "string",
              "minLength": 1,
              "maxLength": 256
            },
            "Relationships": {
              "type": "object",
              "description": "The Party's relationships with other resources.",
              "properties": {
                "Account": {
                  "type": "object",
                  "required": [
                    "Related",
                    "Id"
                  ],
                  "description": "Relationship to the Account resource.",
                  "properties": {
                    "Related": {
                      "description": "Absolute URI to the related resource.",
                      "type": "string",
                      "format": "uri"
                    },
                    "Id": {
                      "description": "Unique identification as assigned by the ASPSP to uniquely identify the related resource.",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 40
                    }
                  }
                }
              }
            },
            "Address": {
              "type": "array",
              "items": {
                "type": "object",
                "description": "Postal address of a party.",
                "required": [
                  "Country"
                ],
                "properties": {
                  "AddressType": {
                    "description": "Identifies the nature of the postal address.",
                    "type": "string",
                    "enum": [
                      "Business",
                      "Correspondence",
                      "DeliveryTo",
                      "MailTo",
                      "POBox",
                      "Postal",
                      "Residential",
                      "Statement"
                    ]
                  },
                  "AddressLine": {
                    "type": "array",
                    "items": {
                      "description": "Information that locates and identifies a specific address, as defined by postal services, that is presented in free format text.",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 70
                    },
                    "minItems": 0,
                    "maxItems": 5
                  },
                  "StreetName": {
                    "description": "Name of a street or thoroughfare.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 70
                  },
                  "BuildingNumber": {
                    "description": "Number that identifies the position of a building on a street.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 16
                  },
                  "PostCode": {
                    "description": "Identifier consisting of a group of letters and/or numbers that is added to a postal address to assist the sorting of mail.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 16
                  },
                  "TownName": {
                    "description": "Name of a built-up area, with defined boundaries, and a local government.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 35
                  },
                  "CountrySubDivision": {
                    "description": "Identifies a subdivision of a country eg, state, region, county.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 35
                  },
                  "Country": {
                    "description": "Nation with its own government, occupying a particular territory.",
                    "type": "string",
                    "pattern": "^[A-Z]{2,2}$"
                  }
                }
              }
            }
          },
          "additionalProperties": false
        }
      }
    },
    "Links": {
      "type": "object",
      "description": "Links relevant to the payload",
      "properties": {
        "Self": {
          "type": "string",
          "format": "uri"
        },
        "First": {
          "type": "string",
          "format": "uri"
        },
        "Prev": {
          "type": "string",
          "format": "uri"
        },
        "Next": {
          "type": "string",
          "format": "uri"
        },
        "Last": {
          "type": "string",
          "format": "uri"
        }
      },
      "additionalProperties": false,
      "required": [
        "Self"
      ]
    },
    "Meta": {
      "title": "MetaData",
      "type": "object",
      "description": "Meta Data relevant to the payload",
      "properties": {
        "TotalPages": {
          "type": "integer",
          "format": "int32"
        },
        "FirstAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "LastAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        }
      },
      "additionalProperties": false
    }
  },
  "additionalProperties": false
}
```

<h3 id="getaccountsaccountidparty-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Parties Read|[OBReadParty2](#schemaobreadparty2)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[OBErrorResponse1](#schemaoberrorresponse1)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[OBErrorResponse1](#schemaoberrorresponse1)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Not found|None|
|405|[Method Not Allowed](https://tools.ietf.org/html/rfc7231#section-6.5.5)|Method Not Allowed|None|
|406|[Not Acceptable](https://tools.ietf.org/html/rfc7231#section-6.5.6)|Not Acceptable|None|
|429|[Too Many Requests](https://tools.ietf.org/html/rfc6585#section-4)|Too Many Requests|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Internal Server Error|[OBErrorResponse1](#schemaoberrorresponse1)|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|400|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|401|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|403|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|404|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|405|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|406|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|429|Retry-After|integer||Number in seconds to wait|
|429|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|500|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
PSUOAuth2Security ( Scopes: accounts )
</aside>

<h1 id="account-and-transaction-api-specification-products">Products</h1>

## GetAccountsAccountIdProduct

<a id="opIdGetAccountsAccountIdProduct"></a>

`GET /accounts/{AccountId}/product`

Get Products

<h3 id="getaccountsaccountidproduct-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|AccountId|path|string|true|AccountId|
|x-fapi-auth-date|header|string|false|The time when the PSU last logged in with the TPP. |
|x-fapi-customer-ip-address|header|string|false|The PSU's IP address if the PSU is currently logged in with the TPP.|
|x-fapi-interaction-id|header|string|false|An RFC4122 UID used as a correlation id.|
|Authorization|header|string|true|An Authorisation Token as per https://tools.ietf.org/html/rfc6750|
|x-customer-user-agent|header|string|false|Indicates the user-agent that the PSU is using.|

#### Detailed descriptions

**x-fapi-auth-date**: The time when the PSU last logged in with the TPP. 
All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: 
Sun, 10 Sep 2017 19:43:31 UTC

> Example responses

> 200 Response

```json
{
  "type": "object",
  "required": [
    "Data"
  ],
  "description": "Product details of Other Product which is not avaiable in the standard list",
  "properties": {
    "Data": {
      "type": "object",
      "description": "Aligning with the read write specs structure.",
      "properties": {
        "Product": {
          "type": "array",
          "items": {
            "type": "object",
            "description": "Product details associated with the Account",
            "required": [
              "AccountId",
              "ProductType"
            ],
            "properties": {
              "ProductName": {
                "description": "The name of the Product used for marketing purposes from a customer perspective. I.e. what the customer would recognise.",
                "type": "string",
                "minLength": 1,
                "maxLength": 350
              },
              "ProductId": {
                "description": "The unique ID that has been internally assigned by the financial institution to each of the current account banking products they market to their retail and/or small to medium enterprise (SME) customers.",
                "type": "string",
                "minLength": 1,
                "maxLength": 40
              },
              "AccountId": {
                "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
                "type": "string",
                "minLength": 1,
                "maxLength": 40
              },
              "ProductType": {
                "description": "Product type : Personal Current Account, Business Current Account",
                "type": "string",
                "enum": [
                  "CommercialCreditCard",
                  "PersonalCurrentAccount"
                ]
              },
              "MarketingStateId": {
                "description": "Unique and unambiguous identification of a  Product Marketing State.",
                "type": "string",
                "minLength": 1,
                "maxLength": 35
              },
              "OtherProductType": {
                "type": "object",
                "required": [
                  "Name",
                  "Description"
                ],
                "description": "Other product type details associated with the account.",
                "properties": {
                  "Name": {
                    "description": "Long name associated with the product",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 350
                  },
                  "Description": {
                    "description": "Description of the Product associated with the account",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 350
                  },
                  "CreditInterest": {
                    "type": "object",
                    "required": [
                      "TierBandSet"
                    ],
                    "description": "Details about the interest that may be payable to the Account holders",
                    "properties": {
                      "TierBandSet": {
                        "type": "array",
                        "items": {
                          "type": "object",
                          "description": "The group of tiers or bands for which credit interest can be applied.",
                          "required": [
                            "TierBandMethod",
                            "Destination",
                            "TierBand"
                          ],
                          "properties": {
                            "TierBandMethod": {
                              "description": "The methodology of how credit interest is paid/applied. It can be:-\n1. Banded\nInterest rates are banded. i.e. Increasing rate on whole balance as balance increases.\n2. Tiered\nInterest rates are tiered. i.e. increasing rate for each tier as balance increases, but interest paid on tier fixed for that tier and not on whole balance.\n3. Whole\nThe same interest rate is applied irrespective of the product holder's account balance",
                              "type": "string",
                              "enum": [
                                "INWH"
                              ]
                            },
                            "Destination": {
                              "description": "Describes whether accrued interest is payable only to the BCA or to another bank account",
                              "type": "string",
                              "enum": [
                                "INSC"
                              ]
                            },
                            "TierBand": {
                              "type": "array",
                              "items": {
                                "type": "object",
                                "description": "Tier Band Details",
                                "required": [
                                  "TierValueMinimum",
                                  "ApplicationFrequency",
                                  "FixedVariableInterestRateType",
                                  "AER"
                                ],
                                "properties": {
                                  "TierValueMinimum": {
                                    "description": "Minimum deposit value for which the credit interest tier applies.",
                                    "type": "string",
                                    "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                                  },
                                  "ApplicationFrequency": {
                                    "description": "How often is interest applied to the Product for this tier/band i.e. how often the financial institution pays accumulated interest to the customer's account.",
                                    "type": "string",
                                    "enum": [
                                      "FQDY",
                                      "FQWY",
                                      "FQMY",
                                      "FQQY",
                                      "FQHY",
                                      "FQYY"
                                    ]
                                  },
                                  "FixedVariableInterestRateType": {
                                    "description": "Type of interest rate, Fixed or Variable",
                                    "type": "string",
                                    "enum": [
                                      "INVA"
                                    ]
                                  },
                                  "AER": {
                                    "description": "The annual equivalent rate (AER) is interest that is calculated under the assumption that any interest paid is combined with the original balance and the next interest payment will be based on the slightly higher account balance. Overall, this means that interest can be compounded several times in a year depending on the number of times that interest payments are made. \nRead more: Annual Equivalent Rate (AER) http://www.investopedia.com/terms/a/aer.asp#ixzz4gfR7IO1A",
                                    "type": "string",
                                    "pattern": "^(-?\\d{1,3}){1}(\\.\\d{1,4}){0,1}$"
                                  }
                                }
                              }
                            }
                          }
                        }
                      }
                    }
                  }
                }
              },
              "PCA": {
                "type": "object",
                "title": "PCA",
                "properties": {
                  "ProductDetails": {
                    "type": "object",
                    "title": "ProductDetails",
                    "properties": {
                      "Segment": {
                        "description": "Market segmentation is a marketing term referring to the aggregating of prospective buyers into groups, or segments, that have common needs and respond similarly to a marketing action. Market segmentation enables companies to target different categories of consumers who perceive the full value of certain products and services differently from one another.\n\nRead more: Market Segmentation http://www.investopedia.com/terms/m/marketsegmentation.asp#ixzz4gfEEalTd \nWith respect to PCA products, they are segmented in relation to different markets that they wish to focus on. ",
                        "title": "Segment",
                        "type": "array",
                        "items": {
                          "description": "Market segmentation is a marketing term referring to the aggregating of prospective buyers into groups, or segments, that have common needs and respond similarly to a marketing action. Market segmentation enables companies to target different categories of consumers who perceive the full value of certain products and services differently from one another.\n\nRead more: Market Segmentation http://www.investopedia.com/terms/m/marketsegmentation.asp#ixzz4gfEEalTd \nWith respect to PCA products, they are segmented in relation to different markets that they wish to focus on. ",
                          "type": "string",
                          "enum": [
                            "Basic",
                            "General",
                            "Graduate",
                            "Other",
                            "Premium",
                            "Student",
                            "Youth"
                          ]
                        }
                      },
                      "MonthlyMaximumCharge": {
                        "description": "The maximum relevant charges that could accrue as defined fully in Part 7 of the CMA order",
                        "title": "MonthlyMaximumCharge",
                        "type": "string",
                        "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                      }
                    },
                    "additionalProperties": false
                  },
                  "CreditInterest": {
                    "description": "Details about the interest that may be payable to the PCA account holders",
                    "type": "object",
                    "title": "CreditInterest",
                    "properties": {
                      "TierBandSet": {
                        "description": "The group of tiers or bands for which credit interest can be applied.",
                        "type": "array",
                        "title": "TierBandSet",
                        "items": {
                          "description": "The group of tiers or bands for which credit interest can be applied.",
                          "type": "object",
                          "properties": {
                            "TierBandMethod": {
                              "description": "The methodology of how credit interest is charged. It can be:-\n\n1. Banded\nInterest rates are banded. i.e. Increasing rate on whole balance as balance increases.\n\n2. Tiered\nInterest rates are tiered. i.e. increasing rate for each tier as balance increases, but interest paid on tier fixed for that tier and not on whole balance.\n\n3. Whole\nThe same interest rate is applied irrespective of the PCA balance",
                              "title": "TierBandMethod",
                              "type": "string",
                              "enum": [
                                "Tiered",
                                "Whole"
                              ]
                            },
                            "CalculationMethod": {
                              "description": "Methods of calculating interest",
                              "title": "CalculationMethod",
                              "type": "string",
                              "enum": [
                                "SimpleInterest"
                              ]
                            },
                            "Destination": {
                              "description": "Describes whether accrued interest is payable only to the PCA or to another bank account",
                              "title": "Destination",
                              "type": "string",
                              "enum": [
                                "PayAway",
                                "SelfCredit"
                              ]
                            },
                            "TierBand": {
                              "description": "Tier Band Details",
                              "type": "array",
                              "title": "TierBand",
                              "items": {
                                "description": "Tier Band Details",
                                "type": "object",
                                "properties": {
                                  "Identification": {
                                    "description": "Unique and unambiguous identification of a  Tier Band for a PCA.",
                                    "title": "Identification",
                                    "type": "string",
                                    "minLength": 1,
                                    "maxLength": 35
                                  },
                                  "TierValueMinimum": {
                                    "description": "Minimum deposit value for which the credit interest tier applies.",
                                    "title": "TierValueMinimum",
                                    "type": "string",
                                    "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                                  },
                                  "TierValueMaximum": {
                                    "description": "Maximum deposit value for which the credit interest tier applies.",
                                    "title": "TierValueMaximum",
                                    "type": "string",
                                    "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                                  },
                                  "CalculationFrequency": {
                                    "description": "How often is credit interest calculated for the account.",
                                    "title": "CalculationFrequency",
                                    "type": "string",
                                    "enum": [
                                      "Daily",
                                      "HalfYearly",
                                      "Monthly",
                                      "Quarterly",
                                      "Weekly",
                                      "Yearly"
                                    ]
                                  },
                                  "ApplicationFrequency": {
                                    "description": "How often is interest applied to the PCA for this tier/band i.e. how often the financial institution pays accumulated interest to the customer's PCA.",
                                    "title": "ApplicationFrequency",
                                    "type": "string",
                                    "enum": [
                                      "Daily",
                                      "HalfYearly",
                                      "Monthly",
                                      "Quarterly",
                                      "Weekly",
                                      "Yearly"
                                    ]
                                  },
                                  "FixedVariableInterestRateType": {
                                    "description": "Type of interest rate, Fixed or Variable",
                                    "title": "FixedVariableInterestRateType",
                                    "type": "string",
                                    "enum": [
                                      "Variable"
                                    ]
                                  },
                                  "AER": {
                                    "description": "The annual equivalent rate (AER) is interest that is calculated under the assumption that any interest paid is combined with the original balance and the next interest payment will be based on the slightly higher account balance. Overall, this means that interest can be compounded several times in a year depending on the number of times that interest payments are made. \n\nRead more: Annual Equivalent Rate (AER) http://www.investopedia.com/terms/a/aer.asp#ixzz4gfR7IO1A",
                                    "title": "AER",
                                    "type": "string",
                                    "pattern": "^(-?\\d{1,3}){1}(\\.\\d{1,4}){0,1}$"
                                  }
                                },
                                "required": [
                                  "TierValueMinimum",
                                  "ApplicationFrequency",
                                  "FixedVariableInterestRateType",
                                  "AER"
                                ]
                              },
                              "minItems": 1
                            }
                          },
                          "required": [
                            "TierBandMethod",
                            "TierBand"
                          ]
                        },
                        "minItems": 1
                      }
                    },
                    "additionalProperties": false,
                    "required": [
                      "TierBandSet"
                    ]
                  },
                  "Overdraft": {
                    "description": "Details about Overdraft rates, fees & charges",
                    "type": "object",
                    "title": "Overdraft",
                    "properties": {
                      "OverdraftTierBandSet": {
                        "description": "Tier band set details",
                        "type": "array",
                        "title": "OverdraftTierBandSet",
                        "items": {
                          "description": "Tier band set details",
                          "type": "object",
                          "properties": {
                            "TierBandMethod": {
                              "description": "The methodology of how overdraft is charged. It can be:\n'Whole'  Where the same charge/rate is applied to the entirety of the overdraft balance (where charges are applicable). \n'Tiered' Where different charges/rates are applied dependent on overdraft maximum and minimum balance amount tiers defined by the lending financial organisation\n'Banded' Where different charges/rates are applied dependent on overdraft maximum and minimum balance amount bands defined by a government organisation.",
                              "title": "TierBandMethod",
                              "type": "string",
                              "enum": [
                                "Tiered",
                                "Whole"
                              ]
                            },
                            "Identification": {
                              "description": "Unique and unambiguous identification of a  Tier Band for a overdraft product.",
                              "title": "Identification",
                              "type": "string",
                              "minLength": 1,
                              "maxLength": 35
                            },
                            "OverdraftTierBand": {
                              "description": "Provides overdraft details for a specific tier or band",
                              "type": "array",
                              "title": "OverdraftTierBand",
                              "items": {
                                "description": "Provides overdraft details for a specific tier or band",
                                "type": "object",
                                "properties": {
                                  "Identification": {
                                    "description": "Unique and unambiguous identification of a  Tier Band for a overdraft.",
                                    "title": "Identification",
                                    "type": "string",
                                    "minLength": 1,
                                    "maxLength": 35
                                  },
                                  "TierValueMin": {
                                    "description": "Minimum value of Overdraft Tier/Band",
                                    "title": "TierValueMin",
                                    "type": "string",
                                    "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                                  },
                                  "TierValueMax": {
                                    "description": "Maximum value of Overdraft Tier/Band",
                                    "title": "TierValueMax",
                                    "type": "string",
                                    "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                                  },
                                  "EAR": {
                                    "description": "EAR means Effective Annual Rate and/or Equivalent Annual Rate (frequently\nused interchangeably), being the actual annual interest rate of an Overdraft.",
                                    "title": "EAR",
                                    "type": "string",
                                    "pattern": "^(-?\\d{1,3}){1}(\\.\\d{1,4}){0,1}$"
                                  },
                                  "OverdraftFeesCharges": {
                                    "description": "Overdraft fees and charges",
                                    "type": "array",
                                    "title": "OverdraftFeesCharges",
                                    "items": {
                                      "description": "Overdraft fees and charges",
                                      "type": "object",
                                      "properties": {
                                        "OverdraftFeeChargeCap": {
                                          "description": "Details about any caps (maximum charges) that apply to a particular fee/charge",
                                          "type": "array",
                                          "title": "OverdraftFeeChargeCap",
                                          "items": {
                                            "description": "Details about any caps (maximum charges) that apply to a particular fee/charge",
                                            "type": "object",
                                            "properties": {
                                              "FeeType": {
                                                "description": "Fee/charge type which is being capped",
                                                "title": "FeeType",
                                                "type": "array",
                                                "items": {
                                                  "description": "Overdraft fee type",
                                                  "type": "string",
                                                  "enum": [
                                                    "ArrangedOverdraft"
                                                  ]
                                                },
                                                "minItems": 1
                                              },
                                              "MinMaxType": {
                                                "description": "Indicates that this is the minimum/ maximum fee/charge that can be applied by the financial institution",
                                                "title": "MinMaxType",
                                                "type": "string",
                                                "enum": [
                                                  "Minimum",
                                                  "Maximum"
                                                ]
                                              },
                                              "FeeCapAmount": {
                                                "description": "Cap amount charged for a fee/charge",
                                                "title": "FeeCapAmount",
                                                "type": "string",
                                                "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                                              },
                                              "CappingPeriod": {
                                                "description": "Period e.g. day, week, month etc. for which the fee/charge is capped",
                                                "title": "CappingPeriod",
                                                "type": "string",
                                                "enum": [
                                                  "Day",
                                                  "Half Year",
                                                  "Month",
                                                  "Quarter",
                                                  "Week",
                                                  "Year"
                                                ]
                                              }
                                            },
                                            "required": [
                                              "FeeType",
                                              "MinMaxType"
                                            ]
                                          }
                                        },
                                        "OverdraftFeeChargeDetail": {
                                          "description": "Details about the fees/charges",
                                          "type": "array",
                                          "title": "OverdraftFeeChargeDetail",
                                          "items": {
                                            "description": "Details about the fees/charges",
                                            "type": "object",
                                            "properties": {
                                              "FeeType": {
                                                "description": "Overdraft fee type",
                                                "title": "FeeType",
                                                "type": "string",
                                                "enum": [
                                                  "ArrangedOverdraft"
                                                ]
                                              },
                                              "FeeRate": {
                                                "description": "Rate charged for overdraft fee/charge (where it is charged in terms of a rate rather than an amount)",
                                                "title": "FeeRate",
                                                "type": "string",
                                                "pattern": "^(-?\\d{1,3}){1}(\\.\\d{1,4}){0,1}$"
                                              },
                                              "ApplicationFrequency": {
                                                "description": "Frequency at which the overdraft charge is applied to the account",
                                                "title": "ApplicationFrequency",
                                                "type": "string",
                                                "enum": [
                                                  "Daily",
                                                  "Monthly",
                                                  "Quarterly",
                                                  "SixMonthly",
                                                  "Weekly",
                                                  "Yearly"
                                                ]
                                              },
                                              "CalculationFrequency": {
                                                "description": "How often is the overdraft fee/charge calculated for the account.",
                                                "title": "CalculationFrequency",
                                                "type": "string",
                                                "enum": [
                                                  "Daily",
                                                  "Monthly",
                                                  "Quarterly",
                                                  "SixMonthly",
                                                  "Weekly",
                                                  "Yearly"
                                                ]
                                              }
                                            },
                                            "required": [
                                              "FeeType",
                                              "ApplicationFrequency"
                                            ]
                                          },
                                          "minItems": 1
                                        }
                                      },
                                      "required": [
                                        "OverdraftFeeChargeDetail"
                                      ]
                                    }
                                  }
                                },
                                "required": [
                                  "TierValueMin"
                                ]
                              },
                              "minItems": 1
                            }
                          },
                          "required": [
                            "TierBandMethod",
                            "OverdraftTierBand"
                          ]
                        },
                        "minItems": 1
                      }
                    },
                    "additionalProperties": false,
                    "required": [
                      "OverdraftTierBandSet"
                    ]
                  }
                },
                "additionalProperties": false
              }
            }
          }
        }
      }
    },
    "Links": {
      "type": "object",
      "description": "Links relevant to the payload",
      "properties": {
        "Self": {
          "type": "string",
          "format": "uri"
        },
        "First": {
          "type": "string",
          "format": "uri"
        },
        "Prev": {
          "type": "string",
          "format": "uri"
        },
        "Next": {
          "type": "string",
          "format": "uri"
        },
        "Last": {
          "type": "string",
          "format": "uri"
        }
      },
      "additionalProperties": false,
      "required": [
        "Self"
      ]
    },
    "Meta": {
      "title": "MetaData",
      "type": "object",
      "description": "Meta Data relevant to the payload",
      "properties": {
        "TotalPages": {
          "type": "integer",
          "format": "int32"
        },
        "FirstAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "LastAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        }
      },
      "additionalProperties": false
    }
  },
  "additionalProperties": false
}
```

<h3 id="getaccountsaccountidproduct-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Products Read|[OBReadProduct2](#schemaobreadproduct2)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[OBErrorResponse1](#schemaoberrorresponse1)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[OBErrorResponse1](#schemaoberrorresponse1)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Not found|None|
|405|[Method Not Allowed](https://tools.ietf.org/html/rfc7231#section-6.5.5)|Method Not Allowed|None|
|406|[Not Acceptable](https://tools.ietf.org/html/rfc7231#section-6.5.6)|Not Acceptable|None|
|429|[Too Many Requests](https://tools.ietf.org/html/rfc6585#section-4)|Too Many Requests|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Internal Server Error|[OBErrorResponse1](#schemaoberrorresponse1)|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|400|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|401|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|403|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|404|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|405|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|406|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|429|Retry-After|integer||Number in seconds to wait|
|429|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|500|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
PSUOAuth2Security ( Scopes: accounts )
</aside>

<h1 id="account-and-transaction-api-specification-scheduled-payments">Scheduled Payments</h1>

## GetAccountsAccountIdScheduledPayments

<a id="opIdGetAccountsAccountIdScheduledPayments"></a>

`GET /accounts/{AccountId}/scheduled-payments`

Get Scheduled Payments

<h3 id="getaccountsaccountidscheduledpayments-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|AccountId|path|string|true|AccountId|
|x-fapi-auth-date|header|string|false|The time when the PSU last logged in with the TPP. |
|x-fapi-customer-ip-address|header|string|false|The PSU's IP address if the PSU is currently logged in with the TPP.|
|x-fapi-interaction-id|header|string|false|An RFC4122 UID used as a correlation id.|
|Authorization|header|string|true|An Authorisation Token as per https://tools.ietf.org/html/rfc6750|
|x-customer-user-agent|header|string|false|Indicates the user-agent that the PSU is using.|

#### Detailed descriptions

**x-fapi-auth-date**: The time when the PSU last logged in with the TPP. 
All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: 
Sun, 10 Sep 2017 19:43:31 UTC

> Example responses

> 200 Response

```json
{
  "type": "object",
  "required": [
    "Data"
  ],
  "properties": {
    "Data": {
      "type": "object",
      "properties": {
        "ScheduledPayment": {
          "type": "array",
          "items": {
            "type": "object",
            "required": [
              "AccountId",
              "ScheduledPaymentDateTime",
              "ScheduledType",
              "InstructedAmount"
            ],
            "properties": {
              "AccountId": {
                "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
                "type": "string",
                "minLength": 1,
                "maxLength": 40
              },
              "ScheduledPaymentId": {
                "description": "A unique and immutable identifier used to identify the scheduled payment resource. This identifier has no meaning to the account owner.",
                "type": "string",
                "minLength": 1,
                "maxLength": 40
              },
              "ScheduledPaymentDateTime": {
                "description": "The date on which the scheduled payment will be made.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
                "type": "string",
                "format": "date-time"
              },
              "ScheduledType": {
                "description": "Specifies the scheduled payment date type requested",
                "type": "string",
                "enum": [
                  "Arrival",
                  "Execution"
                ]
              },
              "Reference": {
                "description": "Unique reference, as assigned by the creditor, to unambiguously refer to the payment transaction.\nUsage: If available, the initiating party should provide this reference in the structured remittance information, to enable reconciliation by the creditor upon receipt of the amount of money.\nIf the business context requires the use of a creditor reference or a payment remit identification, and only one identifier can be passed through the end-to-end chain, the creditor's reference or payment remittance identification should be quoted in the end-to-end transaction identification.",
                "type": "string",
                "minLength": 1,
                "maxLength": 35
              },
              "InstructedAmount": {
                "type": "object",
                "required": [
                  "Amount",
                  "Currency"
                ],
                "description": "Amount of money to be moved between the debtor and creditor, before deduction of charges, expressed in the currency as ordered by the initiating party.\nUsage: This amount has to be transported unchanged through the transaction chain.",
                "properties": {
                  "Amount": {
                    "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
                    "type": "string",
                    "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
                  },
                  "Currency": {
                    "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
                    "type": "string",
                    "pattern": "^[A-Z]{3,3}$"
                  }
                }
              },
              "CreditorAccount": {
                "type": "object",
                "required": [
                  "SchemeName",
                  "Identification"
                ],
                "description": "Provides the details to identify the beneficiary account.",
                "properties": {
                  "SchemeName": {
                    "description": "Name of the identification scheme, in a coded form as published in an external list.",
                    "type": "string",
                    "x-namespaced-enum": [
                      "UK.OBIE.SortCodeAccountNumber"
                    ]
                  },
                  "Identification": {
                    "description": "Beneficiary account identification.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 256
                  },
                  "Name": {
                    "description": "The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.\nNote, the account name is not the product name or the nickname of the account.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 350
                  },
                  "SecondaryIdentification": {
                    "description": "This is secondary identification of the account, as assigned by the account servicing institution. \nThis can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 34
                  }
                }
              }
            },
            "additionalProperties": false
          }
        }
      }
    },
    "Links": {
      "type": "object",
      "description": "Links relevant to the payload",
      "properties": {
        "Self": {
          "type": "string",
          "format": "uri"
        },
        "First": {
          "type": "string",
          "format": "uri"
        },
        "Prev": {
          "type": "string",
          "format": "uri"
        },
        "Next": {
          "type": "string",
          "format": "uri"
        },
        "Last": {
          "type": "string",
          "format": "uri"
        }
      },
      "additionalProperties": false,
      "required": [
        "Self"
      ]
    },
    "Meta": {
      "title": "MetaData",
      "type": "object",
      "description": "Meta Data relevant to the payload",
      "properties": {
        "TotalPages": {
          "type": "integer",
          "format": "int32"
        },
        "FirstAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "LastAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        }
      },
      "additionalProperties": false
    }
  },
  "additionalProperties": false
}
```

<h3 id="getaccountsaccountidscheduledpayments-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Scheduled Payments Read|[OBReadScheduledPayment3](#schemaobreadscheduledpayment3)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[OBErrorResponse1](#schemaoberrorresponse1)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[OBErrorResponse1](#schemaoberrorresponse1)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Not found|None|
|405|[Method Not Allowed](https://tools.ietf.org/html/rfc7231#section-6.5.5)|Method Not Allowed|None|
|406|[Not Acceptable](https://tools.ietf.org/html/rfc7231#section-6.5.6)|Not Acceptable|None|
|429|[Too Many Requests](https://tools.ietf.org/html/rfc6585#section-4)|Too Many Requests|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Internal Server Error|[OBErrorResponse1](#schemaoberrorresponse1)|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|400|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|401|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|403|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|404|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|405|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|406|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|429|Retry-After|integer||Number in seconds to wait|
|429|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|500|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
PSUOAuth2Security ( Scopes: accounts )
</aside>

<h1 id="account-and-transaction-api-specification-standing-orders">Standing Orders</h1>

## GetAccountsAccountIdStandingOrders

<a id="opIdGetAccountsAccountIdStandingOrders"></a>

`GET /accounts/{AccountId}/standing-orders`

Get Standing Orders

<h3 id="getaccountsaccountidstandingorders-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|AccountId|path|string|true|AccountId|
|x-fapi-auth-date|header|string|false|The time when the PSU last logged in with the TPP. |
|x-fapi-customer-ip-address|header|string|false|The PSU's IP address if the PSU is currently logged in with the TPP.|
|x-fapi-interaction-id|header|string|false|An RFC4122 UID used as a correlation id.|
|Authorization|header|string|true|An Authorisation Token as per https://tools.ietf.org/html/rfc6750|
|x-customer-user-agent|header|string|false|Indicates the user-agent that the PSU is using.|

#### Detailed descriptions

**x-fapi-auth-date**: The time when the PSU last logged in with the TPP. 
All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: 
Sun, 10 Sep 2017 19:43:31 UTC

> Example responses

> 200 Response

```json
{
  "type": "object",
  "required": [
    "Data"
  ],
  "properties": {
    "Data": {
      "type": "object",
      "properties": {
        "StandingOrder": {
          "type": "array",
          "items": {
            "type": "object",
            "required": [
              "AccountId",
              "Frequency"
            ],
            "properties": {
              "AccountId": {
                "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
                "type": "string",
                "minLength": 1,
                "maxLength": 40
              },
              "StandingOrderId": {
                "description": "A unique and immutable identifier used to identify the standing order resource. This identifier has no meaning to the account owner.",
                "type": "string",
                "minLength": 1,
                "maxLength": 40
              },
              "Frequency": {
                "description": "Individual Definitions:\nNotKnown - Not Known\nEvryDay - Every day\nEvryWorkgDay - Every working day\nIntrvlDay - An interval specified in number of calendar days (02 to 31)\nIntrvlWkDay - An interval specified in weeks (01 to 09), and the day within the week (01 to 07)\nWkInMnthDay - A monthly interval, specifying the week of the month (01 to 05) and day within the week (01 to 07)\nIntrvlMnthDay - An interval specified in months (between 01 to 06, 12, 24), specifying the day within the month (-05 to -01, 01 to 31)\nQtrDay - Quarterly (either ENGLISH, SCOTTISH, or RECEIVED)\nENGLISH = Paid on the 25th March, 24th June, 29th September and 25th December.\nSCOTTISH = Paid on the 2nd February, 15th May, 1st August and 11th November.\nRECEIVED = Paid on the 20th March, 19th June, 24th September and 20th December.\nIndividual Patterns:\nNotKnown (ScheduleCode)\nEvryDay (ScheduleCode)\nEvryWorkgDay (ScheduleCode)\nIntrvlDay:NoOfDay (ScheduleCode + NoOfDay)\nIntrvlWkDay:IntervalInWeeks:DayInWeek (ScheduleCode + IntervalInWeeks + DayInWeek)\nWkInMnthDay:WeekInMonth:DayInWeek (ScheduleCode + WeekInMonth + DayInWeek)\nIntrvlMnthDay:IntervalInMonths:DayInMonth (ScheduleCode + IntervalInMonths + DayInMonth)\nQtrDay: + either (ENGLISH, SCOTTISH or RECEIVED) ScheduleCode + QuarterDay\nThe regular expression for this element combines five smaller versions for each permitted pattern. To aid legibility - the components are presented individually here:\nNotKnown\nEvryDay\nEvryWorkgDay\nIntrvlDay:((0[2-9])|([1-2][0-9])|3[0-1])\nIntrvlWkDay:0[1-9]:0[1-7]\nWkInMnthDay:0[1-5]:0[1-7]\nIntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01])\nQtrDay:(ENGLISH|SCOTTISH|RECEIVED)\nFull Regular Expression:\n^(NotKnown)$|^(EvryDay)$|^(EvryWorkgDay)$|^(IntrvlDay:((0[2-9])|([1-2][0-9])|3[0-1]))$|^(IntrvlWkDay:0[1-9]:0[1-7])$|^(WkInMnthDay:0[1-5]:0[1-7])$|^(IntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01]))$|^(QtrDay:(ENGLISH|SCOTTISH|RECEIVED))$",
                "type": "string",
                "pattern": "^(NotKnown)$|^(EvryDay)$|^(EvryWorkgDay)$|^(IntrvlDay:((0[2-9])|([1-2][0-9])|3[0-1]))$|^(IntrvlWkDay:0[1-9]:0[1-7])$|^(WkInMnthDay:0[1-5]:0[1-7])$|^(IntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01]))$|^(QtrDay:(ENGLISH|SCOTTISH|RECEIVED))$"
              },
              "Reference": {
                "description": "Unique reference, as assigned by the creditor, to unambiguously refer to the payment transaction.\nUsage: If available, the initiating party should provide this reference in the structured remittance information, to enable reconciliation by the creditor upon receipt of the amount of money.\nIf the business context requires the use of a creditor reference or a payment remit identification, and only one identifier can be passed through the end-to-end chain, the creditor's reference or payment remittance identification should be quoted in the end-to-end transaction identification.",
                "type": "string",
                "minLength": 1,
                "maxLength": 35
              },
              "NextPaymentDateTime": {
                "description": "The date on which the next payment for a Standing Order schedule will be made.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
                "type": "string",
                "format": "date-time"
              },
              "StandingOrderStatusCode": {
                "description": "Specifies the status of the standing order in code form.",
                "type": "string",
                "enum": [
                  "Active",
                  "Inactive"
                ]
              },
              "NextPaymentAmount": {
                "type": "object",
                "required": [
                  "Amount",
                  "Currency"
                ],
                "description": "The amount of the next Standing Order.",
                "properties": {
                  "Amount": {
                    "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
                    "type": "string",
                    "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
                  },
                  "Currency": {
                    "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
                    "type": "string",
                    "pattern": "^[A-Z]{3,3}$"
                  }
                }
              },
              "CreditorAccount": {
                "type": "object",
                "required": [
                  "SchemeName",
                  "Identification"
                ],
                "description": "Provides the details to identify the beneficiary account.",
                "properties": {
                  "SchemeName": {
                    "description": "Name of the identification scheme, in a coded form as published in an external list.",
                    "type": "string",
                    "x-namespaced-enum": [
                      "UK.OBIE.SortCodeAccountNumber"
                    ]
                  },
                  "Identification": {
                    "description": "Beneficiary account identification.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 256
                  },
                  "Name": {
                    "description": "The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.\nNote, the account name is not the product name or the nickname of the account.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 350
                  },
                  "SecondaryIdentification": {
                    "description": "This is secondary identification of the account, as assigned by the account servicing institution. \nThis can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 34
                  }
                }
              }
            },
            "additionalProperties": false
          }
        }
      }
    },
    "Links": {
      "type": "object",
      "description": "Links relevant to the payload",
      "properties": {
        "Self": {
          "type": "string",
          "format": "uri"
        },
        "First": {
          "type": "string",
          "format": "uri"
        },
        "Prev": {
          "type": "string",
          "format": "uri"
        },
        "Next": {
          "type": "string",
          "format": "uri"
        },
        "Last": {
          "type": "string",
          "format": "uri"
        }
      },
      "additionalProperties": false,
      "required": [
        "Self"
      ]
    },
    "Meta": {
      "title": "MetaData",
      "type": "object",
      "description": "Meta Data relevant to the payload",
      "properties": {
        "TotalPages": {
          "type": "integer",
          "format": "int32"
        },
        "FirstAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "LastAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        }
      },
      "additionalProperties": false
    }
  },
  "additionalProperties": false
}
```

<h3 id="getaccountsaccountidstandingorders-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Standing Orders Read|[OBReadStandingOrder6](#schemaobreadstandingorder6)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[OBErrorResponse1](#schemaoberrorresponse1)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[OBErrorResponse1](#schemaoberrorresponse1)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Not found|None|
|405|[Method Not Allowed](https://tools.ietf.org/html/rfc7231#section-6.5.5)|Method Not Allowed|None|
|406|[Not Acceptable](https://tools.ietf.org/html/rfc7231#section-6.5.6)|Not Acceptable|None|
|429|[Too Many Requests](https://tools.ietf.org/html/rfc6585#section-4)|Too Many Requests|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Internal Server Error|[OBErrorResponse1](#schemaoberrorresponse1)|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|400|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|401|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|403|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|404|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|405|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|406|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|429|Retry-After|integer||Number in seconds to wait|
|429|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|500|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
PSUOAuth2Security ( Scopes: accounts )
</aside>

<h1 id="account-and-transaction-api-specification-transactions">Transactions</h1>

## GetAccountsAccountIdTransactions

<a id="opIdGetAccountsAccountIdTransactions"></a>

`GET /accounts/{AccountId}/transactions`

Get Transactions

<h3 id="getaccountsaccountidtransactions-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|AccountId|path|string|true|AccountId|
|x-fapi-auth-date|header|string|false|The time when the PSU last logged in with the TPP. |
|x-fapi-customer-ip-address|header|string|false|The PSU's IP address if the PSU is currently logged in with the TPP.|
|x-fapi-interaction-id|header|string|false|An RFC4122 UID used as a correlation id.|
|Authorization|header|string|true|An Authorisation Token as per https://tools.ietf.org/html/rfc6750|
|x-customer-user-agent|header|string|false|Indicates the user-agent that the PSU is using.|
|fromBookingDateTime|query|string(date-time)|false|The UTC ISO 8601 Date Time to filter transactions FROM|
|toBookingDateTime|query|string(date-time)|false|The UTC ISO 8601 Date Time to filter transactions TO|

#### Detailed descriptions

**x-fapi-auth-date**: The time when the PSU last logged in with the TPP. 
All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: 
Sun, 10 Sep 2017 19:43:31 UTC

**fromBookingDateTime**: The UTC ISO 8601 Date Time to filter transactions FROM
NB Time component is optional - set to 00:00:00 for just Date.
If the Date Time contains a timezone, the ASPSP must ignore the timezone component.

**toBookingDateTime**: The UTC ISO 8601 Date Time to filter transactions TO
NB Time component is optional - set to 00:00:00 for just Date.
If the Date Time contains a timezone, the ASPSP must ignore the timezone component.

> Example responses

> 200 Response

```json
{
  "type": "object",
  "required": [
    "Data"
  ],
  "properties": {
    "Data": {
      "type": "object",
      "properties": {
        "Transaction": {
          "type": "array",
          "items": {
            "type": "object",
            "description": "Provides further details on an entry in the report.",
            "required": [
              "AccountId",
              "CreditDebitIndicator",
              "Status",
              "BookingDateTime",
              "Amount"
            ],
            "properties": {
              "AccountId": {
                "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
                "type": "string",
                "minLength": 1,
                "maxLength": 40
              },
              "TransactionId": {
                "description": "Unique identifier for the transaction within an servicing institution. This identifier is both unique and immutable.",
                "type": "string",
                "minLength": 1,
                "maxLength": 210
              },
              "TransactionReference": {
                "description": "Unique reference for the transaction. This reference is optionally populated, and may as an example be the FPID in the Faster Payments context.",
                "type": "string",
                "minLength": 1,
                "maxLength": 210
              },
              "CreditDebitIndicator": {
                "description": "Indicates whether the transaction is a credit or a debit entry.",
                "type": "string",
                "enum": [
                  "Credit",
                  "Debit"
                ]
              },
              "Status": {
                "description": "Status of a transaction entry on the books of the account servicer.",
                "type": "string",
                "enum": [
                  "Booked",
                  "Pending"
                ]
              },
              "BookingDateTime": {
                "description": "Date and time when a transaction entry is posted to an account on the account servicer's books.\nUsage: Booking date is the expected booking date, unless the status is booked, in which case it is the actual booking date.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
                "type": "string",
                "format": "date-time"
              },
              "ValueDateTime": {
                "description": "Date and time at which assets become available to the account owner in case of a credit entry, or cease to be available to the account owner in case of a debit transaction entry.\nUsage: If transaction entry status is pending and value date is present, then the value date refers to an expected/requested value date.\nFor transaction entries subject to availability/float and for which availability information is provided, the value date must not be used. In this case the availability component identifies the number of availability days.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
                "type": "string",
                "format": "date-time"
              },
              "TransactionInformation": {
                "description": "Further details of the transaction. \nThis is the transaction narrative, which is unstructured text.",
                "type": "string",
                "minLength": 1,
                "maxLength": 500
              },
              "AddressLine": {
                "description": "Information that locates and identifies a specific address for a transaction entry, that is presented in free format text.",
                "type": "string",
                "minLength": 1,
                "maxLength": 70
              },
              "Amount": {
                "type": "object",
                "required": [
                  "Amount",
                  "Currency"
                ],
                "description": "Amount of money in the cash transaction entry.",
                "properties": {
                  "Amount": {
                    "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
                    "type": "string",
                    "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
                  },
                  "Currency": {
                    "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
                    "type": "string",
                    "pattern": "^[A-Z]{3,3}$"
                  }
                }
              },
              "CurrencyExchange": {
                "type": "object",
                "required": [
                  "SourceCurrency",
                  "ExchangeRate"
                ],
                "description": "Set of elements used to provide details on the currency exchange.",
                "properties": {
                  "SourceCurrency": {
                    "description": "Currency from which an amount is to be converted in a currency conversion.",
                    "type": "string",
                    "pattern": "^[A-Z]{3,3}$"
                  },
                  "TargetCurrency": {
                    "description": "Currency into which an amount is to be converted in a currency conversion.",
                    "type": "string",
                    "pattern": "^[A-Z]{3,3}$"
                  },
                  "UnitCurrency": {
                    "description": "Currency in which the rate of exchange is expressed in a currency exchange. In the example 1GBP = xxxCUR, the unit currency is GBP.",
                    "type": "string",
                    "pattern": "^[A-Z]{3,3}$"
                  },
                  "ExchangeRate": {
                    "description": "Factor used to convert an amount from one currency into another. This reflects the price at which one currency was bought with another currency.\nUsage: ExchangeRate expresses the ratio between UnitCurrency and QuotedCurrency (ExchangeRate = UnitCurrency/QuotedCurrency).",
                    "type": "number"
                  },
                  "InstructedAmount": {
                    "type": "object",
                    "required": [
                      "Amount",
                      "Currency"
                    ],
                    "description": "Amount of money to be moved between the debtor and creditor, before deduction of charges, expressed in the currency as ordered by the initiating party.",
                    "properties": {
                      "Amount": {
                        "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
                        "type": "string",
                        "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
                      },
                      "Currency": {
                        "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
                        "type": "string",
                        "pattern": "^[A-Z]{3,3}$"
                      }
                    }
                  }
                }
              },
              "ProprietaryBankTransactionCode": {
                "type": "object",
                "required": [
                  "Code"
                ],
                "description": "Set of elements to fully identify a proprietary bank transaction code.",
                "properties": {
                  "Code": {
                    "description": "Proprietary bank transaction code to identify the underlying transaction.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 35
                  }
                },
                "additionalProperties": false
              },
              "Balance": {
                "type": "object",
                "required": [
                  "CreditDebitIndicator",
                  "Type",
                  "Amount"
                ],
                "description": "Set of elements used to define the balance as a numerical representation of the net increases and decreases in an account after a transaction entry is applied to the account.",
                "properties": {
                  "CreditDebitIndicator": {
                    "description": "Indicates whether the balance is a credit or a debit balance. \nUsage: A zero balance is considered to be a credit balance.",
                    "type": "string",
                    "enum": [
                      "Credit",
                      "Debit"
                    ]
                  },
                  "Type": {
                    "description": "Balance type, in a coded form.",
                    "type": "string",
                    "enum": [
                      "Expected",
                      "InterimBooked"
                    ]
                  },
                  "Amount": {
                    "type": "object",
                    "required": [
                      "Amount",
                      "Currency"
                    ],
                    "description": "Amount of money of the cash balance after a transaction entry is applied to the account..",
                    "properties": {
                      "Amount": {
                        "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
                        "type": "string",
                        "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
                      },
                      "Currency": {
                        "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
                        "type": "string",
                        "pattern": "^[A-Z]{3,3}$"
                      }
                    }
                  }
                },
                "additionalProperties": false
              },
              "MerchantDetails": {
                "type": "object",
                "description": "Details of the merchant involved in the transaction.",
                "properties": {
                  "MerchantCategoryCode": {
                    "description": "Category code conform to ISO 18245, related to the type of services or goods the merchant provides for the transaction.",
                    "type": "string",
                    "minLength": 3,
                    "maxLength": 4
                  }
                }
              },
              "CreditorAccount": {
                "type": "object",
                "description": "Unambiguous identification of the account of the creditor, in the case of a debit transaction.",
                "properties": {
                  "SchemeName": {
                    "description": "Name of the identification scheme, in a coded form as published in an external list.",
                    "type": "string",
                    "x-namespaced-enum": [
                      "UK.OBIE.SortCodeAccountNumber"
                    ]
                  },
                  "Identification": {
                    "description": "Identification assigned by an institution to identify an account. This identification is known by the account owner.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 256
                  },
                  "Name": {
                    "description": "The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.\nNote, the account name is not the product name or the nickname of the account.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 350
                  },
                  "SecondaryIdentification": {
                    "description": "This is secondary identification of the account, as assigned by the account servicing institution. \nThis can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 34
                  }
                }
              },
              "DebtorAccount": {
                "type": "object",
                "description": "Unambiguous identification of the account of the debtor, in the case of a crebit transaction.",
                "properties": {
                  "SchemeName": {
                    "description": "Name of the identification scheme, in a coded form as published in an external list.",
                    "type": "string",
                    "x-namespaced-enum": [
                      "UK.OBIE.SortCodeAccountNumber"
                    ]
                  },
                  "Identification": {
                    "description": "Identification assigned by an institution to identify an account. This identification is known by the account owner.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 256
                  },
                  "Name": {
                    "description": "The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.\nNote, the account name is not the product name or the nickname of the account.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 350
                  },
                  "SecondaryIdentification": {
                    "description": "This is secondary identification of the account, as assigned by the account servicing institution. \nThis can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 34
                  }
                }
              },
              "CardInstrument": {
                "type": "object",
                "required": [
                  "CardSchemeName"
                ],
                "description": "Set of elements to describe the card instrument used in the transaction.",
                "properties": {
                  "CardSchemeName": {
                    "description": "Name of the card scheme.",
                    "type": "string",
                    "enum": [
                      "AmericanExpress",
                      "Diners",
                      "Discover",
                      "MasterCard",
                      "VISA"
                    ]
                  },
                  "AuthorisationType": {
                    "description": "The card authorisation type.",
                    "type": "string",
                    "enum": [
                      "ConsumerDevice",
                      "Contactless",
                      "None",
                      "PIN"
                    ]
                  },
                  "Name": {
                    "description": "Name of the cardholder using the card instrument.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 70
                  },
                  "Identification": {
                    "description": "Identification assigned by an institution to identify the card instrument used in the transaction. This identification is known by the account owner, and may be masked.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 34
                  }
                },
                "additionalProperties": false
              }
            },
            "additionalProperties": false
          }
        }
      },
      "additionalProperties": false
    },
    "Links": {
      "type": "object",
      "description": "Links relevant to the payload",
      "properties": {
        "Self": {
          "type": "string",
          "format": "uri"
        },
        "First": {
          "type": "string",
          "format": "uri"
        },
        "Prev": {
          "type": "string",
          "format": "uri"
        },
        "Next": {
          "type": "string",
          "format": "uri"
        },
        "Last": {
          "type": "string",
          "format": "uri"
        }
      },
      "additionalProperties": false,
      "required": [
        "Self"
      ]
    },
    "Meta": {
      "title": "MetaData",
      "type": "object",
      "description": "Meta Data relevant to the payload",
      "properties": {
        "TotalPages": {
          "type": "integer",
          "format": "int32"
        },
        "FirstAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "LastAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        }
      },
      "additionalProperties": false
    }
  },
  "additionalProperties": false
}
```

<h3 id="getaccountsaccountidtransactions-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Transactions Read|[OBReadTransaction6](#schemaobreadtransaction6)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[OBErrorResponse1](#schemaoberrorresponse1)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[OBErrorResponse1](#schemaoberrorresponse1)|
|405|[Method Not Allowed](https://tools.ietf.org/html/rfc7231#section-6.5.5)|Method Not Allowed|None|
|406|[Not Acceptable](https://tools.ietf.org/html/rfc7231#section-6.5.6)|Not Acceptable|None|
|429|[Too Many Requests](https://tools.ietf.org/html/rfc6585#section-4)|Too Many Requests|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Internal Server Error|[OBErrorResponse1](#schemaoberrorresponse1)|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|400|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|401|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|403|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|405|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|406|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|429|Retry-After|integer||Number in seconds to wait|
|429|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|
|500|x-fapi-interaction-id|string||An RFC4122 UID used as a correlation id.|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
PSUOAuth2Security ( Scopes: accounts )
</aside>

# Schemas

<h2 id="tocS_AccountId">AccountId</h2>
<!-- backwards compatibility -->
<a id="schemaaccountid"></a>
<a id="schema_AccountId"></a>
<a id="tocSaccountid"></a>
<a id="tocsaccountid"></a>

```json
{
  "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
  "type": "string",
  "minLength": 1,
  "maxLength": 40
}

```

A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.|

<h2 id="tocS_ActiveOrHistoricCurrencyCode_0">ActiveOrHistoricCurrencyCode_0</h2>
<!-- backwards compatibility -->
<a id="schemaactiveorhistoriccurrencycode_0"></a>
<a id="schema_ActiveOrHistoricCurrencyCode_0"></a>
<a id="tocSactiveorhistoriccurrencycode_0"></a>
<a id="tocsactiveorhistoriccurrencycode_0"></a>

```json
{
  "description": "Identification of the currency in which the account is held. \nUsage: Currency should only be used in case one and the same account number covers several currencies\nand the initiating party needs to identify which currency needs to be used for settlement on the account.",
  "type": "string",
  "pattern": "^[A-Z]{3,3}$"
}

```

Identification of the currency in which the account is held. 
Usage: Currency should only be used in case one and the same account number covers several currencies
and the initiating party needs to identify which currency needs to be used for settlement on the account.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Identification of the currency in which the account is held. <br>Usage: Currency should only be used in case one and the same account number covers several currencies<br>and the initiating party needs to identify which currency needs to be used for settlement on the account.|

<h2 id="tocS_ActiveOrHistoricCurrencyCode_1">ActiveOrHistoricCurrencyCode_1</h2>
<!-- backwards compatibility -->
<a id="schemaactiveorhistoriccurrencycode_1"></a>
<a id="schema_ActiveOrHistoricCurrencyCode_1"></a>
<a id="tocSactiveorhistoriccurrencycode_1"></a>
<a id="tocsactiveorhistoriccurrencycode_1"></a>

```json
{
  "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
  "type": "string",
  "pattern": "^[A-Z]{3,3}$"
}

```

A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 "Codes for the representation of currencies and funds".

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 "Codes for the representation of currencies and funds".|

<h2 id="tocS_AddressLine">AddressLine</h2>
<!-- backwards compatibility -->
<a id="schemaaddressline"></a>
<a id="schema_AddressLine"></a>
<a id="tocSaddressline"></a>
<a id="tocsaddressline"></a>

```json
{
  "description": "Information that locates and identifies a specific address for a transaction entry, that is presented in free format text.",
  "type": "string",
  "minLength": 1,
  "maxLength": 70
}

```

Information that locates and identifies a specific address for a transaction entry, that is presented in free format text.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Information that locates and identifies a specific address for a transaction entry, that is presented in free format text.|

<h2 id="tocS_BeneficiaryId">BeneficiaryId</h2>
<!-- backwards compatibility -->
<a id="schemabeneficiaryid"></a>
<a id="schema_BeneficiaryId"></a>
<a id="tocSbeneficiaryid"></a>
<a id="tocsbeneficiaryid"></a>

```json
{
  "description": "A unique and immutable identifier used to identify the beneficiary resource. This identifier has no meaning to the account owner.",
  "type": "string",
  "minLength": 1,
  "maxLength": 40
}

```

A unique and immutable identifier used to identify the beneficiary resource. This identifier has no meaning to the account owner.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|A unique and immutable identifier used to identify the beneficiary resource. This identifier has no meaning to the account owner.|

<h2 id="tocS_BookingDateTime">BookingDateTime</h2>
<!-- backwards compatibility -->
<a id="schemabookingdatetime"></a>
<a id="schema_BookingDateTime"></a>
<a id="tocSbookingdatetime"></a>
<a id="tocsbookingdatetime"></a>

```json
{
  "description": "Date and time when a transaction entry is posted to an account on the account servicer's books.\nUsage: Booking date is the expected booking date, unless the status is booked, in which case it is the actual booking date.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
  "type": "string",
  "format": "date-time"
}

```

Date and time when a transaction entry is posted to an account on the account servicer's books.
Usage: Booking date is the expected booking date, unless the status is booked, in which case it is the actual booking date.All dates in the JSON payloads are represented in ISO 8601 date-time format. 
All date-time fields in responses must include the timezone. An example is below:
2017-04-05T10:43:07+00:00

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string(date-time)|false|none|Date and time when a transaction entry is posted to an account on the account servicer's books.<br>Usage: Booking date is the expected booking date, unless the status is booked, in which case it is the actual booking date.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|

<h2 id="tocS_BuildingNumber">BuildingNumber</h2>
<!-- backwards compatibility -->
<a id="schemabuildingnumber"></a>
<a id="schema_BuildingNumber"></a>
<a id="tocSbuildingnumber"></a>
<a id="tocsbuildingnumber"></a>

```json
{
  "description": "Number that identifies the position of a building on a street.",
  "type": "string",
  "minLength": 1,
  "maxLength": 16
}

```

Number that identifies the position of a building on a street.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Number that identifies the position of a building on a street.|

<h2 id="tocS_CountryCode">CountryCode</h2>
<!-- backwards compatibility -->
<a id="schemacountrycode"></a>
<a id="schema_CountryCode"></a>
<a id="tocScountrycode"></a>
<a id="tocscountrycode"></a>

```json
{
  "description": "Nation with its own government, occupying a particular territory.",
  "type": "string",
  "pattern": "^[A-Z]{2,2}$"
}

```

Nation with its own government, occupying a particular territory.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Nation with its own government, occupying a particular territory.|

<h2 id="tocS_CountrySubDivision">CountrySubDivision</h2>
<!-- backwards compatibility -->
<a id="schemacountrysubdivision"></a>
<a id="schema_CountrySubDivision"></a>
<a id="tocScountrysubdivision"></a>
<a id="tocscountrysubdivision"></a>

```json
{
  "description": "Identifies a subdivision of a country eg, state, region, county.",
  "type": "string",
  "minLength": 1,
  "maxLength": 35
}

```

Identifies a subdivision of a country eg, state, region, county.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Identifies a subdivision of a country eg, state, region, county.|

<h2 id="tocS_CreationDateTime">CreationDateTime</h2>
<!-- backwards compatibility -->
<a id="schemacreationdatetime"></a>
<a id="schema_CreationDateTime"></a>
<a id="tocScreationdatetime"></a>
<a id="tocscreationdatetime"></a>

```json
{
  "description": "Date and time at which the resource was created.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
  "type": "string",
  "format": "date-time"
}

```

Date and time at which the resource was created.All dates in the JSON payloads are represented in ISO 8601 date-time format. 
All date-time fields in responses must include the timezone. An example is below:
2017-04-05T10:43:07+00:00

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string(date-time)|false|none|Date and time at which the resource was created.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|

<h2 id="tocS_DateTime">DateTime</h2>
<!-- backwards compatibility -->
<a id="schemadatetime"></a>
<a id="schema_DateTime"></a>
<a id="tocSdatetime"></a>
<a id="tocsdatetime"></a>

```json
{
  "description": "Date and time associated with the date time type.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
  "type": "string",
  "format": "date-time"
}

```

Date and time associated with the date time type.All dates in the JSON payloads are represented in ISO 8601 date-time format. 
All date-time fields in responses must include the timezone. An example is below:
2017-04-05T10:43:07+00:00

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string(date-time)|false|none|Date and time associated with the date time type.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|

<h2 id="tocS_DebtorReference">DebtorReference</h2>
<!-- backwards compatibility -->
<a id="schemadebtorreference"></a>
<a id="schema_DebtorReference"></a>
<a id="tocSdebtorreference"></a>
<a id="tocsdebtorreference"></a>

```json
{
  "description": "A reference value provided by the PSU to the PISP while setting up the scheduled payment.",
  "type": "string",
  "minLength": 1,
  "maxLength": 35
}

```

A reference value provided by the PSU to the PISP while setting up the scheduled payment.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|A reference value provided by the PSU to the PISP while setting up the scheduled payment.|

<h2 id="tocS_Description_0">Description_0</h2>
<!-- backwards compatibility -->
<a id="schemadescription_0"></a>
<a id="schema_Description_0"></a>
<a id="tocSdescription_0"></a>
<a id="tocsdescription_0"></a>

```json
{
  "description": "Specifies the description of the account type.",
  "type": "string",
  "minLength": 1,
  "maxLength": 35
}

```

Specifies the description of the account type.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Specifies the description of the account type.|

<h2 id="tocS_Description_1">Description_1</h2>
<!-- backwards compatibility -->
<a id="schemadescription_1"></a>
<a id="schema_Description_1"></a>
<a id="tocSdescription_1"></a>
<a id="tocsdescription_1"></a>

```json
{
  "description": "Description that may be available for the statement fee.",
  "type": "string",
  "minLength": 1,
  "maxLength": 128
}

```

Description that may be available for the statement fee.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Description that may be available for the statement fee.|

<h2 id="tocS_Description_2">Description_2</h2>
<!-- backwards compatibility -->
<a id="schemadescription_2"></a>
<a id="schema_Description_2"></a>
<a id="tocSdescription_2"></a>
<a id="tocsdescription_2"></a>

```json
{
  "description": "Description that may be available for the statement interest.",
  "type": "string",
  "minLength": 1,
  "maxLength": 128
}

```

Description that may be available for the statement interest.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Description that may be available for the statement interest.|

<h2 id="tocS_Description_3">Description_3</h2>
<!-- backwards compatibility -->
<a id="schemadescription_3"></a>
<a id="schema_Description_3"></a>
<a id="tocSdescription_3"></a>
<a id="tocsdescription_3"></a>

```json
{
  "description": "Description to describe the purpose of the code",
  "type": "string",
  "minLength": 1,
  "maxLength": 350
}

```

Description to describe the purpose of the code

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Description to describe the purpose of the code|

<h2 id="tocS_DirectDebitId">DirectDebitId</h2>
<!-- backwards compatibility -->
<a id="schemadirectdebitid"></a>
<a id="schema_DirectDebitId"></a>
<a id="tocSdirectdebitid"></a>
<a id="tocsdirectdebitid"></a>

```json
{
  "description": "A unique and immutable identifier used to identify the direct debit resource. This identifier has no meaning to the account owner.",
  "type": "string",
  "minLength": 1,
  "maxLength": 40
}

```

A unique and immutable identifier used to identify the direct debit resource. This identifier has no meaning to the account owner.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|A unique and immutable identifier used to identify the direct debit resource. This identifier has no meaning to the account owner.|

<h2 id="tocS_EmailAddress">EmailAddress</h2>
<!-- backwards compatibility -->
<a id="schemaemailaddress"></a>
<a id="schema_EmailAddress"></a>
<a id="tocSemailaddress"></a>
<a id="tocsemailaddress"></a>

```json
{
  "description": "Address for electronic mail (e-mail).",
  "type": "string",
  "minLength": 1,
  "maxLength": 256
}

```

Address for electronic mail (e-mail).

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Address for electronic mail (e-mail).|

<h2 id="tocS_EndDateTime">EndDateTime</h2>
<!-- backwards compatibility -->
<a id="schemaenddatetime"></a>
<a id="schema_EndDateTime"></a>
<a id="tocSenddatetime"></a>
<a id="tocsenddatetime"></a>

```json
{
  "description": "Date and time at which the statement period ends.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
  "type": "string",
  "format": "date-time"
}

```

Date and time at which the statement period ends.All dates in the JSON payloads are represented in ISO 8601 date-time format. 
All date-time fields in responses must include the timezone. An example is below:
2017-04-05T10:43:07+00:00

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string(date-time)|false|none|Date and time at which the statement period ends.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|

<h2 id="tocS_FinalPaymentDateTime">FinalPaymentDateTime</h2>
<!-- backwards compatibility -->
<a id="schemafinalpaymentdatetime"></a>
<a id="schema_FinalPaymentDateTime"></a>
<a id="tocSfinalpaymentdatetime"></a>
<a id="tocsfinalpaymentdatetime"></a>

```json
{
  "description": "The date on which the final payment for a Standing Order schedule will be made.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
  "type": "string",
  "format": "date-time"
}

```

The date on which the final payment for a Standing Order schedule will be made.All dates in the JSON payloads are represented in ISO 8601 date-time format. 
All date-time fields in responses must include the timezone. An example is below:
2017-04-05T10:43:07+00:00

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string(date-time)|false|none|The date on which the final payment for a Standing Order schedule will be made.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|

<h2 id="tocS_FirstPaymentDateTime">FirstPaymentDateTime</h2>
<!-- backwards compatibility -->
<a id="schemafirstpaymentdatetime"></a>
<a id="schema_FirstPaymentDateTime"></a>
<a id="tocSfirstpaymentdatetime"></a>
<a id="tocsfirstpaymentdatetime"></a>

```json
{
  "description": "The date on which the first payment for a Standing Order schedule will be made.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
  "type": "string",
  "format": "date-time"
}

```

The date on which the first payment for a Standing Order schedule will be made.All dates in the JSON payloads are represented in ISO 8601 date-time format. 
All date-time fields in responses must include the timezone. An example is below:
2017-04-05T10:43:07+00:00

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string(date-time)|false|none|The date on which the first payment for a Standing Order schedule will be made.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|

<h2 id="tocS_Frequency_0">Frequency_0</h2>
<!-- backwards compatibility -->
<a id="schemafrequency_0"></a>
<a id="schema_Frequency_0"></a>
<a id="tocSfrequency_0"></a>
<a id="tocsfrequency_0"></a>

```json
{
  "description": "Individual Definitions:\nEvryDay - Every day\nEvryWorkgDay - Every working day\nIntrvlWkDay - An interval specified in weeks (01 to 09), and the day within the week (01 to 07)\nWkInMnthDay - A monthly interval, specifying the week of the month (01 to 05) and day within the week (01 to 07)\nIntrvlMnthDay - An interval specified in months (between 01 to 06, 12, 24), specifying the day within the month (-5 to -1, 1 to 31)\nQtrDay - Quarterly (either ENGLISH, SCOTTISH, or RECEIVED)\nENGLISH = Paid on the 25th March, 24th June, 29th September and 25th December. \nSCOTTISH = Paid on the 2nd February, 15th May, 1st August and 11th November.\nRECEIVED = Paid on the 20th March, 19th June, 24th September and 20th December. \nIndividual Patterns:\nEvryDay (ScheduleCode)\nEvryWorkgDay (ScheduleCode)\nIntrvlWkDay:IntervalInWeeks:DayInWeek (ScheduleCode + IntervalInWeeks + DayInWeek)\nWkInMnthDay:WeekInMonth:DayInWeek (ScheduleCode + WeekInMonth + DayInWeek)\nIntrvlMnthDay:IntervalInMonths:DayInMonth (ScheduleCode + IntervalInMonths + DayInMonth)\nQtrDay: + either (ENGLISH, SCOTTISH or RECEIVED) ScheduleCode + QuarterDay\nThe regular expression for this element combines five smaller versions for each permitted pattern. To aid legibility - the components are presented individually here:\nEvryDay\nEvryWorkgDay\nIntrvlWkDay:0[1-9]:0[1-7]\nWkInMnthDay:0[1-5]:0[1-7]\nIntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01])\nQtrDay:(ENGLISH|SCOTTISH|RECEIVED)\nFull Regular Expression:\n^(EvryDay)$|^(EvryWorkgDay)$|^(IntrvlWkDay:0[1-9]:0[1-7])$|^(WkInMnthDay:0[1-5]:0[1-7])$|^(IntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01]))$|^(QtrDay:(ENGLISH|SCOTTISH|RECEIVED))$",
  "type": "string",
  "pattern": "^(EvryDay)$|^(EvryWorkgDay)$|^(IntrvlWkDay:0[1-9]:0[1-7])$|^(WkInMnthDay:0[1-5]:0[1-7])$|^(IntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01]))$|^(QtrDay:(ENGLISH|SCOTTISH|RECEIVED))$"
}

```

Individual Definitions:
EvryDay - Every day
EvryWorkgDay - Every working day
IntrvlWkDay - An interval specified in weeks (01 to 09), and the day within the week (01 to 07)
WkInMnthDay - A monthly interval, specifying the week of the month (01 to 05) and day within the week (01 to 07)
IntrvlMnthDay - An interval specified in months (between 01 to 06, 12, 24), specifying the day within the month (-5 to -1, 1 to 31)
QtrDay - Quarterly (either ENGLISH, SCOTTISH, or RECEIVED)
ENGLISH = Paid on the 25th March, 24th June, 29th September and 25th December. 
SCOTTISH = Paid on the 2nd February, 15th May, 1st August and 11th November.
RECEIVED = Paid on the 20th March, 19th June, 24th September and 20th December. 
Individual Patterns:
EvryDay (ScheduleCode)
EvryWorkgDay (ScheduleCode)
IntrvlWkDay:IntervalInWeeks:DayInWeek (ScheduleCode + IntervalInWeeks + DayInWeek)
WkInMnthDay:WeekInMonth:DayInWeek (ScheduleCode + WeekInMonth + DayInWeek)
IntrvlMnthDay:IntervalInMonths:DayInMonth (ScheduleCode + IntervalInMonths + DayInMonth)
QtrDay: + either (ENGLISH, SCOTTISH or RECEIVED) ScheduleCode + QuarterDay
The regular expression for this element combines five smaller versions for each permitted pattern. To aid legibility - the components are presented individually here:
EvryDay
EvryWorkgDay
IntrvlWkDay:0[1-9]:0[1-7]
WkInMnthDay:0[1-5]:0[1-7]
IntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01])
QtrDay:(ENGLISH|SCOTTISH|RECEIVED)
Full Regular Expression:
^(EvryDay)$|^(EvryWorkgDay)$|^(IntrvlWkDay:0[1-9]:0[1-7])$|^(WkInMnthDay:0[1-5]:0[1-7])$|^(IntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01]))$|^(QtrDay:(ENGLISH|SCOTTISH|RECEIVED))$

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Individual Definitions:<br>EvryDay - Every day<br>EvryWorkgDay - Every working day<br>IntrvlWkDay - An interval specified in weeks (01 to 09), and the day within the week (01 to 07)<br>WkInMnthDay - A monthly interval, specifying the week of the month (01 to 05) and day within the week (01 to 07)<br>IntrvlMnthDay - An interval specified in months (between 01 to 06, 12, 24), specifying the day within the month (-5 to -1, 1 to 31)<br>QtrDay - Quarterly (either ENGLISH, SCOTTISH, or RECEIVED)<br>ENGLISH = Paid on the 25th March, 24th June, 29th September and 25th December. <br>SCOTTISH = Paid on the 2nd February, 15th May, 1st August and 11th November.<br>RECEIVED = Paid on the 20th March, 19th June, 24th September and 20th December. <br>Individual Patterns:<br>EvryDay (ScheduleCode)<br>EvryWorkgDay (ScheduleCode)<br>IntrvlWkDay:IntervalInWeeks:DayInWeek (ScheduleCode + IntervalInWeeks + DayInWeek)<br>WkInMnthDay:WeekInMonth:DayInWeek (ScheduleCode + WeekInMonth + DayInWeek)<br>IntrvlMnthDay:IntervalInMonths:DayInMonth (ScheduleCode + IntervalInMonths + DayInMonth)<br>QtrDay: + either (ENGLISH, SCOTTISH or RECEIVED) ScheduleCode + QuarterDay<br>The regular expression for this element combines five smaller versions for each permitted pattern. To aid legibility - the components are presented individually here:<br>EvryDay<br>EvryWorkgDay<br>IntrvlWkDay:0[1-9]:0[1-7]<br>WkInMnthDay:0[1-5]:0[1-7]<br>IntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01])<br>QtrDay:(ENGLISH|SCOTTISH|RECEIVED)<br>Full Regular Expression:<br>^(EvryDay)$|^(EvryWorkgDay)$|^(IntrvlWkDay:0[1-9]:0[1-7])$|^(WkInMnthDay:0[1-5]:0[1-7])$|^(IntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01]))$|^(QtrDay:(ENGLISH|SCOTTISH|RECEIVED))$|

<h2 id="tocS_Frequency_1">Frequency_1</h2>
<!-- backwards compatibility -->
<a id="schemafrequency_1"></a>
<a id="schema_Frequency_1"></a>
<a id="tocSfrequency_1"></a>
<a id="tocsfrequency_1"></a>

```json
{
  "description": "Individual Definitions:\nNotKnown - Not Known\nEvryDay - Every day\nEvryWorkgDay - Every working day\nIntrvlDay - An interval specified in number of calendar days (02 to 31)\nIntrvlWkDay - An interval specified in weeks (01 to 09), and the day within the week (01 to 07)\nWkInMnthDay - A monthly interval, specifying the week of the month (01 to 05) and day within the week (01 to 07)\nIntrvlMnthDay - An interval specified in months (between 01 to 06, 12, 24), specifying the day within the month (-05 to -01, 01 to 31)\nQtrDay - Quarterly (either ENGLISH, SCOTTISH, or RECEIVED)\nENGLISH = Paid on the 25th March, 24th June, 29th September and 25th December.\nSCOTTISH = Paid on the 2nd February, 15th May, 1st August and 11th November.\nRECEIVED = Paid on the 20th March, 19th June, 24th September and 20th December.\nIndividual Patterns:\nNotKnown (ScheduleCode)\nEvryDay (ScheduleCode)\nEvryWorkgDay (ScheduleCode)\nIntrvlDay:NoOfDay (ScheduleCode + NoOfDay)\nIntrvlWkDay:IntervalInWeeks:DayInWeek (ScheduleCode + IntervalInWeeks + DayInWeek)\nWkInMnthDay:WeekInMonth:DayInWeek (ScheduleCode + WeekInMonth + DayInWeek)\nIntrvlMnthDay:IntervalInMonths:DayInMonth (ScheduleCode + IntervalInMonths + DayInMonth)\nQtrDay: + either (ENGLISH, SCOTTISH or RECEIVED) ScheduleCode + QuarterDay\nThe regular expression for this element combines five smaller versions for each permitted pattern. To aid legibility - the components are presented individually here:\nNotKnown\nEvryDay\nEvryWorkgDay\nIntrvlDay:((0[2-9])|([1-2][0-9])|3[0-1])\nIntrvlWkDay:0[1-9]:0[1-7]\nWkInMnthDay:0[1-5]:0[1-7]\nIntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01])\nQtrDay:(ENGLISH|SCOTTISH|RECEIVED)\nFull Regular Expression:\n^(NotKnown)$|^(EvryDay)$|^(EvryWorkgDay)$|^(IntrvlDay:((0[2-9])|([1-2][0-9])|3[0-1]))$|^(IntrvlWkDay:0[1-9]:0[1-7])$|^(WkInMnthDay:0[1-5]:0[1-7])$|^(IntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01]))$|^(QtrDay:(ENGLISH|SCOTTISH|RECEIVED))$",
  "type": "string",
  "pattern": "^(NotKnown)$|^(EvryDay)$|^(EvryWorkgDay)$|^(IntrvlDay:((0[2-9])|([1-2][0-9])|3[0-1]))$|^(IntrvlWkDay:0[1-9]:0[1-7])$|^(WkInMnthDay:0[1-5]:0[1-7])$|^(IntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01]))$|^(QtrDay:(ENGLISH|SCOTTISH|RECEIVED))$"
}

```

Individual Definitions:
NotKnown - Not Known
EvryDay - Every day
EvryWorkgDay - Every working day
IntrvlDay - An interval specified in number of calendar days (02 to 31)
IntrvlWkDay - An interval specified in weeks (01 to 09), and the day within the week (01 to 07)
WkInMnthDay - A monthly interval, specifying the week of the month (01 to 05) and day within the week (01 to 07)
IntrvlMnthDay - An interval specified in months (between 01 to 06, 12, 24), specifying the day within the month (-05 to -01, 01 to 31)
QtrDay - Quarterly (either ENGLISH, SCOTTISH, or RECEIVED)
ENGLISH = Paid on the 25th March, 24th June, 29th September and 25th December.
SCOTTISH = Paid on the 2nd February, 15th May, 1st August and 11th November.
RECEIVED = Paid on the 20th March, 19th June, 24th September and 20th December.
Individual Patterns:
NotKnown (ScheduleCode)
EvryDay (ScheduleCode)
EvryWorkgDay (ScheduleCode)
IntrvlDay:NoOfDay (ScheduleCode + NoOfDay)
IntrvlWkDay:IntervalInWeeks:DayInWeek (ScheduleCode + IntervalInWeeks + DayInWeek)
WkInMnthDay:WeekInMonth:DayInWeek (ScheduleCode + WeekInMonth + DayInWeek)
IntrvlMnthDay:IntervalInMonths:DayInMonth (ScheduleCode + IntervalInMonths + DayInMonth)
QtrDay: + either (ENGLISH, SCOTTISH or RECEIVED) ScheduleCode + QuarterDay
The regular expression for this element combines five smaller versions for each permitted pattern. To aid legibility - the components are presented individually here:
NotKnown
EvryDay
EvryWorkgDay
IntrvlDay:((0[2-9])|([1-2][0-9])|3[0-1])
IntrvlWkDay:0[1-9]:0[1-7]
WkInMnthDay:0[1-5]:0[1-7]
IntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01])
QtrDay:(ENGLISH|SCOTTISH|RECEIVED)
Full Regular Expression:
^(NotKnown)$|^(EvryDay)$|^(EvryWorkgDay)$|^(IntrvlDay:((0[2-9])|([1-2][0-9])|3[0-1]))$|^(IntrvlWkDay:0[1-9]:0[1-7])$|^(WkInMnthDay:0[1-5]:0[1-7])$|^(IntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01]))$|^(QtrDay:(ENGLISH|SCOTTISH|RECEIVED))$

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Individual Definitions:<br>NotKnown - Not Known<br>EvryDay - Every day<br>EvryWorkgDay - Every working day<br>IntrvlDay - An interval specified in number of calendar days (02 to 31)<br>IntrvlWkDay - An interval specified in weeks (01 to 09), and the day within the week (01 to 07)<br>WkInMnthDay - A monthly interval, specifying the week of the month (01 to 05) and day within the week (01 to 07)<br>IntrvlMnthDay - An interval specified in months (between 01 to 06, 12, 24), specifying the day within the month (-05 to -01, 01 to 31)<br>QtrDay - Quarterly (either ENGLISH, SCOTTISH, or RECEIVED)<br>ENGLISH = Paid on the 25th March, 24th June, 29th September and 25th December.<br>SCOTTISH = Paid on the 2nd February, 15th May, 1st August and 11th November.<br>RECEIVED = Paid on the 20th March, 19th June, 24th September and 20th December.<br>Individual Patterns:<br>NotKnown (ScheduleCode)<br>EvryDay (ScheduleCode)<br>EvryWorkgDay (ScheduleCode)<br>IntrvlDay:NoOfDay (ScheduleCode + NoOfDay)<br>IntrvlWkDay:IntervalInWeeks:DayInWeek (ScheduleCode + IntervalInWeeks + DayInWeek)<br>WkInMnthDay:WeekInMonth:DayInWeek (ScheduleCode + WeekInMonth + DayInWeek)<br>IntrvlMnthDay:IntervalInMonths:DayInMonth (ScheduleCode + IntervalInMonths + DayInMonth)<br>QtrDay: + either (ENGLISH, SCOTTISH or RECEIVED) ScheduleCode + QuarterDay<br>The regular expression for this element combines five smaller versions for each permitted pattern. To aid legibility - the components are presented individually here:<br>NotKnown<br>EvryDay<br>EvryWorkgDay<br>IntrvlDay:((0[2-9])|([1-2][0-9])|3[0-1])<br>IntrvlWkDay:0[1-9]:0[1-7]<br>WkInMnthDay:0[1-5]:0[1-7]<br>IntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01])<br>QtrDay:(ENGLISH|SCOTTISH|RECEIVED)<br>Full Regular Expression:<br>^(NotKnown)$|^(EvryDay)$|^(EvryWorkgDay)$|^(IntrvlDay:((0[2-9])|([1-2][0-9])|3[0-1]))$|^(IntrvlWkDay:0[1-9]:0[1-7])$|^(WkInMnthDay:0[1-5]:0[1-7])$|^(IntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01]))$|^(QtrDay:(ENGLISH|SCOTTISH|RECEIVED))$|

<h2 id="tocS_FullLegalName">FullLegalName</h2>
<!-- backwards compatibility -->
<a id="schemafulllegalname"></a>
<a id="schema_FullLegalName"></a>
<a id="tocSfulllegalname"></a>
<a id="tocsfulllegalname"></a>

```json
{
  "description": "Specifies a character string with a maximum length of 350 characters.",
  "type": "string",
  "minLength": 1,
  "maxLength": 350
}

```

Specifies a character string with a maximum length of 350 characters.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Specifies a character string with a maximum length of 350 characters.|

<h2 id="tocS_ISODateTime">ISODateTime</h2>
<!-- backwards compatibility -->
<a id="schemaisodatetime"></a>
<a id="schema_ISODateTime"></a>
<a id="tocSisodatetime"></a>
<a id="tocsisodatetime"></a>

```json
{
  "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
  "type": "string",
  "format": "date-time"
}

```

All dates in the JSON payloads are represented in ISO 8601 date-time format. 
All date-time fields in responses must include the timezone. An example is below:
2017-04-05T10:43:07+00:00

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string(date-time)|false|none|All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|

<h2 id="tocS_Identification_0">Identification_0</h2>
<!-- backwards compatibility -->
<a id="schemaidentification_0"></a>
<a id="schema_Identification_0"></a>
<a id="tocSidentification_0"></a>
<a id="tocsidentification_0"></a>

```json
{
  "description": "Identification assigned by an institution to identify an account. This identification is known by the account owner.",
  "type": "string",
  "minLength": 1,
  "maxLength": 256
}

```

Identification assigned by an institution to identify an account. This identification is known by the account owner.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Identification assigned by an institution to identify an account. This identification is known by the account owner.|

<h2 id="tocS_Identification_1">Identification_1</h2>
<!-- backwards compatibility -->
<a id="schemaidentification_1"></a>
<a id="schema_Identification_1"></a>
<a id="tocSidentification_1"></a>
<a id="tocsidentification_1"></a>

```json
{
  "description": "Unique and unambiguous identification of the servicing institution.",
  "type": "string",
  "minLength": 1,
  "maxLength": 35
}

```

Unique and unambiguous identification of the servicing institution.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Unique and unambiguous identification of the servicing institution.|

<h2 id="tocS_Identification_2">Identification_2</h2>
<!-- backwards compatibility -->
<a id="schemaidentification_2"></a>
<a id="schema_Identification_2"></a>
<a id="tocSidentification_2"></a>
<a id="tocsidentification_2"></a>

```json
{
  "description": "Unique and unambiguous identification of a financial institution or a branch of a financial institution.",
  "type": "string",
  "minLength": 1,
  "maxLength": 35
}

```

Unique and unambiguous identification of a financial institution or a branch of a financial institution.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Unique and unambiguous identification of a financial institution or a branch of a financial institution.|

<h2 id="tocS_LastPaymentDateTime">LastPaymentDateTime</h2>
<!-- backwards compatibility -->
<a id="schemalastpaymentdatetime"></a>
<a id="schema_LastPaymentDateTime"></a>
<a id="tocSlastpaymentdatetime"></a>
<a id="tocslastpaymentdatetime"></a>

```json
{
  "description": "The date on which the last (most recent) payment for a Standing Order schedule was made.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
  "type": "string",
  "format": "date-time"
}

```

The date on which the last (most recent) payment for a Standing Order schedule was made.All dates in the JSON payloads are represented in ISO 8601 date-time format. 
All date-time fields in responses must include the timezone. An example is below:
2017-04-05T10:43:07+00:00

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string(date-time)|false|none|The date on which the last (most recent) payment for a Standing Order schedule was made.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|

<h2 id="tocS_Links">Links</h2>
<!-- backwards compatibility -->
<a id="schemalinks"></a>
<a id="schema_Links"></a>
<a id="tocSlinks"></a>
<a id="tocslinks"></a>

```json
{
  "type": "object",
  "description": "Links relevant to the payload",
  "properties": {
    "Self": {
      "type": "string",
      "format": "uri"
    },
    "First": {
      "type": "string",
      "format": "uri"
    },
    "Prev": {
      "type": "string",
      "format": "uri"
    },
    "Next": {
      "type": "string",
      "format": "uri"
    },
    "Last": {
      "type": "string",
      "format": "uri"
    }
  },
  "additionalProperties": false,
  "required": [
    "Self"
  ]
}

```

Links relevant to the payload

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Self|string(uri)|true|none|none|
|First|string(uri)|false|none|none|
|Prev|string(uri)|false|none|none|
|Next|string(uri)|false|none|none|
|Last|string(uri)|false|none|none|

<h2 id="tocS_MandateIdentification">MandateIdentification</h2>
<!-- backwards compatibility -->
<a id="schemamandateidentification"></a>
<a id="schema_MandateIdentification"></a>
<a id="tocSmandateidentification"></a>
<a id="tocsmandateidentification"></a>

```json
{
  "description": "Direct Debit reference. For AUDDIS service users provide Core Reference. For non AUDDIS service users provide Core reference if possible or last used reference.",
  "type": "string",
  "minLength": 1,
  "maxLength": 35
}

```

Direct Debit reference. For AUDDIS service users provide Core Reference. For non AUDDIS service users provide Core reference if possible or last used reference.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Direct Debit reference. For AUDDIS service users provide Core Reference. For non AUDDIS service users provide Core reference if possible or last used reference.|

<h2 id="tocS_MaturityDate">MaturityDate</h2>
<!-- backwards compatibility -->
<a id="schemamaturitydate"></a>
<a id="schema_MaturityDate"></a>
<a id="tocSmaturitydate"></a>
<a id="tocsmaturitydate"></a>

```json
{
  "description": "Maturity date of the account.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
  "type": "string",
  "format": "date-time"
}

```

Maturity date of the account.All dates in the JSON payloads are represented in ISO 8601 date-time format. 
All date-time fields in responses must include the timezone. An example is below:
2017-04-05T10:43:07+00:00

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string(date-time)|false|none|Maturity date of the account.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|

<h2 id="tocS_Meta">Meta</h2>
<!-- backwards compatibility -->
<a id="schemameta"></a>
<a id="schema_Meta"></a>
<a id="tocSmeta"></a>
<a id="tocsmeta"></a>

```json
{
  "title": "MetaData",
  "type": "object",
  "description": "Meta Data relevant to the payload",
  "properties": {
    "TotalPages": {
      "type": "integer",
      "format": "int32"
    },
    "FirstAvailableDateTime": {
      "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
      "type": "string",
      "format": "date-time"
    },
    "LastAvailableDateTime": {
      "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
      "type": "string",
      "format": "date-time"
    }
  },
  "additionalProperties": false
}

```

MetaData

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|TotalPages|integer(int32)|false|none|none|
|FirstAvailableDateTime|[ISODateTime](#schemaisodatetime)|false|none|All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|
|LastAvailableDateTime|[ISODateTime](#schemaisodatetime)|false|none|All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|

<h2 id="tocS_Name_0">Name_0</h2>
<!-- backwards compatibility -->
<a id="schemaname_0"></a>
<a id="schema_Name_0"></a>
<a id="tocSname_0"></a>
<a id="tocsname_0"></a>

```json
{
  "description": "The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.\nNote, the account name is not the product name or the nickname of the account.",
  "type": "string",
  "minLength": 1,
  "maxLength": 350
}

```

The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.
Note, the account name is not the product name or the nickname of the account.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.<br>Note, the account name is not the product name or the nickname of the account.|

<h2 id="tocS_Name_1">Name_1</h2>
<!-- backwards compatibility -->
<a id="schemaname_1"></a>
<a id="schema_Name_1"></a>
<a id="tocSname_1"></a>
<a id="tocsname_1"></a>

```json
{
  "description": "Name by which an agent is known and which is usually used to identify that agent.",
  "type": "string",
  "minLength": 1,
  "maxLength": 140
}

```

Name by which an agent is known and which is usually used to identify that agent.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Name by which an agent is known and which is usually used to identify that agent.|

<h2 id="tocS_Name_2">Name_2</h2>
<!-- backwards compatibility -->
<a id="schemaname_2"></a>
<a id="schema_Name_2"></a>
<a id="tocSname_2"></a>
<a id="tocsname_2"></a>

```json
{
  "description": "Name of Service User.",
  "type": "string",
  "minLength": 1,
  "maxLength": 70
}

```

Name of Service User.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Name of Service User.|

<h2 id="tocS_Name_3">Name_3</h2>
<!-- backwards compatibility -->
<a id="schemaname_3"></a>
<a id="schema_Name_3"></a>
<a id="tocSname_3"></a>
<a id="tocsname_3"></a>

```json
{
  "description": "Name by which a party is known and which is usually used to identify that party.",
  "type": "string",
  "minLength": 1,
  "maxLength": 350
}

```

Name by which a party is known and which is usually used to identify that party.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Name by which a party is known and which is usually used to identify that party.|

<h2 id="tocS_Name_4">Name_4</h2>
<!-- backwards compatibility -->
<a id="schemaname_4"></a>
<a id="schema_Name_4"></a>
<a id="tocSname_4"></a>
<a id="tocsname_4"></a>

```json
{
  "description": "Long name associated with the code",
  "type": "string",
  "minLength": 1,
  "maxLength": 70
}

```

Long name associated with the code

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Long name associated with the code|

<h2 id="tocS_NextPaymentDateTime">NextPaymentDateTime</h2>
<!-- backwards compatibility -->
<a id="schemanextpaymentdatetime"></a>
<a id="schema_NextPaymentDateTime"></a>
<a id="tocSnextpaymentdatetime"></a>
<a id="tocsnextpaymentdatetime"></a>

```json
{
  "description": "The date on which the next payment for a Standing Order schedule will be made.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
  "type": "string",
  "format": "date-time"
}

```

The date on which the next payment for a Standing Order schedule will be made.All dates in the JSON payloads are represented in ISO 8601 date-time format. 
All date-time fields in responses must include the timezone. An example is below:
2017-04-05T10:43:07+00:00

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string(date-time)|false|none|The date on which the next payment for a Standing Order schedule will be made.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|

<h2 id="tocS_Nickname">Nickname</h2>
<!-- backwards compatibility -->
<a id="schemanickname"></a>
<a id="schema_Nickname"></a>
<a id="tocSnickname"></a>
<a id="tocsnickname"></a>

```json
{
  "description": "The nickname of the account, assigned by the account owner in order to provide an additional means of identification of the account.",
  "type": "string",
  "minLength": 1,
  "maxLength": 70
}

```

The nickname of the account, assigned by the account owner in order to provide an additional means of identification of the account.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|The nickname of the account, assigned by the account owner in order to provide an additional means of identification of the account.|

<h2 id="tocS_NumberOfPayments">NumberOfPayments</h2>
<!-- backwards compatibility -->
<a id="schemanumberofpayments"></a>
<a id="schema_NumberOfPayments"></a>
<a id="tocSnumberofpayments"></a>
<a id="tocsnumberofpayments"></a>

```json
{
  "description": "Number of the payments that will be made in completing this frequency sequence including any executed since the sequence start date.",
  "type": "string",
  "minLength": 1,
  "maxLength": 35
}

```

Number of the payments that will be made in completing this frequency sequence including any executed since the sequence start date.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Number of the payments that will be made in completing this frequency sequence including any executed since the sequence start date.|

<h2 id="tocS_Number_0">Number_0</h2>
<!-- backwards compatibility -->
<a id="schemanumber_0"></a>
<a id="schema_Number_0"></a>
<a id="tocSnumber_0"></a>
<a id="tocsnumber_0"></a>

```json
{
  "description": "Indicates whether the advertised overdraft rate is guaranteed to be offered to a borrower by the bank e.g. if it�s part of a government scheme, or whether the rate may vary dependent on the applicant�s circumstances.",
  "type": "integer"
}

```

Indicates whether the advertised overdraft rate is guaranteed to be offered to a borrower by the bank e.g. if it�s part of a government scheme, or whether the rate may vary dependent on the applicant�s circumstances.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|integer|false|none|Indicates whether the advertised overdraft rate is guaranteed to be offered to a borrower by the bank e.g. if it�s part of a government scheme, or whether the rate may vary dependent on the applicant�s circumstances.|

<h2 id="tocS_Number_1">Number_1</h2>
<!-- backwards compatibility -->
<a id="schemanumber_1"></a>
<a id="schema_Number_1"></a>
<a id="tocSnumber_1"></a>
<a id="tocsnumber_1"></a>

```json
{
  "description": "fee/charges are captured dependent on the number of occurrences rather than capped at a particular amount",
  "type": "integer"
}

```

fee/charges are captured dependent on the number of occurrences rather than capped at a particular amount

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|integer|false|none|fee/charges are captured dependent on the number of occurrences rather than capped at a particular amount|

<h2 id="tocS_OBAccount4">OBAccount4</h2>
<!-- backwards compatibility -->
<a id="schemaobaccount4"></a>
<a id="schema_OBAccount4"></a>
<a id="tocSobaccount4"></a>
<a id="tocsobaccount4"></a>

```json
{
  "type": "object",
  "description": "Unambiguous identification of the account to which credit and debit entries are made.",
  "required": [
    "AccountId",
    "Currency",
    "AccountType",
    "AccountSubType"
  ],
  "properties": {
    "AccountId": {
      "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
      "type": "string",
      "minLength": 1,
      "maxLength": 40
    },
    "Status": {
      "description": "Specifies the status of account resource in code form.",
      "type": "string",
      "enum": [
        "Disabled",
        "Enabled"
      ]
    },
    "StatusUpdateDateTime": {
      "description": "Date and time at which the resource status was updated.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
      "type": "string",
      "format": "date-time"
    },
    "Currency": {
      "description": "Identification of the currency in which the account is held. \nUsage: Currency should only be used in case one and the same account number covers several currencies\nand the initiating party needs to identify which currency needs to be used for settlement on the account.",
      "type": "string",
      "pattern": "^[A-Z]{3,3}$"
    },
    "AccountType": {
      "description": "Specifies the type of account (personal or business).",
      "type": "string",
      "enum": [
        "Business",
        "Personal"
      ]
    },
    "AccountSubType": {
      "description": "Specifies the sub type of account (product family group).",
      "type": "string",
      "enum": [
        "CreditCard",
        "CurrentAccount",
        "Savings"
      ]
    },
    "Description": {
      "description": "Specifies the description of the account type.",
      "type": "string",
      "minLength": 1,
      "maxLength": 35
    },
    "Nickname": {
      "description": "The nickname of the account, assigned by the account owner in order to provide an additional means of identification of the account.",
      "type": "string",
      "minLength": 1,
      "maxLength": 70
    },
    "Account": {
      "type": "array",
      "items": {
        "type": "object",
        "description": "Provides the details to identify an account.",
        "required": [
          "SchemeName",
          "Identification"
        ],
        "properties": {
          "SchemeName": {
            "description": "Name of the identification scheme, in a coded form as published in an external list.",
            "type": "string",
            "x-namespaced-enum": [
              "UK.OBIE.SortCodeAccountNumber"
            ]
          },
          "Identification": {
            "description": "Identification assigned by an institution to identify an account. This identification is known by the account owner.",
            "type": "string",
            "minLength": 1,
            "maxLength": 256
          },
          "Name": {
            "description": "The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.\nNote, the account name is not the product name or the nickname of the account.",
            "type": "string",
            "minLength": 1,
            "maxLength": 350
          },
          "SecondaryIdentification": {
            "description": "This is secondary identification of the account, as assigned by the account servicing institution. \nThis can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).",
            "type": "string",
            "minLength": 1,
            "maxLength": 34
          }
        }
      }
    },
    "Servicer": {
      "type": "object",
      "required": [
        "SchemeName",
        "Identification"
      ],
      "description": "Party that manages the account on behalf of the account owner, that is manages the registration and booking of entries on the account, calculates balances on the account and provides information about the account.",
      "properties": {
        "SchemeName": {
          "description": "Name of the identification scheme, in a coded form as published in an external list.",
          "type": "string",
          "x-namespaced-enum": [
            "UK.OBIE.BICFI"
          ]
        },
        "Identification": {
          "description": "Unique and unambiguous identification of the servicing institution.",
          "type": "string",
          "minLength": 1,
          "maxLength": 35
        }
      }
    }
  }
}

```

Unambiguous identification of the account to which credit and debit entries are made.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|AccountId|[AccountId](#schemaaccountid)|true|none|A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.|
|Status|[OBAccountStatus1Code](#schemaobaccountstatus1code)|false|none|Specifies the status of account resource in code form.|
|StatusUpdateDateTime|[StatusUpdateDateTime](#schemastatusupdatedatetime)|false|none|Date and time at which the resource status was updated.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|
|Currency|[ActiveOrHistoricCurrencyCode_0](#schemaactiveorhistoriccurrencycode_0)|true|none|Identification of the currency in which the account is held. <br>Usage: Currency should only be used in case one and the same account number covers several currencies<br>and the initiating party needs to identify which currency needs to be used for settlement on the account.|
|AccountType|[OBExternalAccountType1Code](#schemaobexternalaccounttype1code)|true|none|Specifies the type of account (personal or business).|
|AccountSubType|[OBExternalAccountSubType1Code](#schemaobexternalaccountsubtype1code)|true|none|Specifies the sub type of account (product family group).|
|Description|[Description_0](#schemadescription_0)|false|none|Specifies the description of the account type.|
|Nickname|[Nickname](#schemanickname)|false|none|The nickname of the account, assigned by the account owner in order to provide an additional means of identification of the account.|
|Account|[object]|false|none|none|
|» SchemeName|[OBExternalAccountIdentification4Code](#schemaobexternalaccountidentification4code)|true|none|Name of the identification scheme, in a coded form as published in an external list.|
|» Identification|[Identification_0](#schemaidentification_0)|true|none|Identification assigned by an institution to identify an account. This identification is known by the account owner.|
|» Name|[Name_0](#schemaname_0)|false|none|The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.<br>Note, the account name is not the product name or the nickname of the account.|
|» SecondaryIdentification|[SecondaryIdentification](#schemasecondaryidentification)|false|none|This is secondary identification of the account, as assigned by the account servicing institution. <br>This can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).|
|Servicer|[OBBranchAndFinancialInstitutionIdentification5_0](#schemaobbranchandfinancialinstitutionidentification5_0)|false|none|Party that manages the account on behalf of the account owner, that is manages the registration and booking of entries on the account, calculates balances on the account and provides information about the account.|

<h2 id="tocS_OBAccount4Basic">OBAccount4Basic</h2>
<!-- backwards compatibility -->
<a id="schemaobaccount4basic"></a>
<a id="schema_OBAccount4Basic"></a>
<a id="tocSobaccount4basic"></a>
<a id="tocsobaccount4basic"></a>

```json
{
  "type": "object",
  "description": "Unambiguous identification of the account to which credit and debit entries are made.",
  "required": [
    "AccountId",
    "Currency",
    "AccountType",
    "AccountSubType"
  ],
  "properties": {
    "AccountId": {
      "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
      "type": "string",
      "minLength": 1,
      "maxLength": 40
    },
    "Status": {
      "description": "Specifies the status of account resource in code form.",
      "type": "string",
      "enum": [
        "Disabled",
        "Enabled"
      ]
    },
    "StatusUpdateDateTime": {
      "description": "Date and time at which the resource status was updated.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
      "type": "string",
      "format": "date-time"
    },
    "Currency": {
      "description": "Identification of the currency in which the account is held. \nUsage: Currency should only be used in case one and the same account number covers several currencies\nand the initiating party needs to identify which currency needs to be used for settlement on the account.",
      "type": "string",
      "pattern": "^[A-Z]{3,3}$"
    },
    "AccountType": {
      "description": "Specifies the type of account (personal or business).",
      "type": "string",
      "enum": [
        "Business",
        "Personal"
      ]
    },
    "AccountSubType": {
      "description": "Specifies the sub type of account (product family group).",
      "type": "string",
      "enum": [
        "CreditCard",
        "CurrentAccount",
        "Savings"
      ]
    },
    "Description": {
      "description": "Specifies the description of the account type.",
      "type": "string",
      "minLength": 1,
      "maxLength": 35
    },
    "Nickname": {
      "description": "The nickname of the account, assigned by the account owner in order to provide an additional means of identification of the account.",
      "type": "string",
      "minLength": 1,
      "maxLength": 70
    }
  }
}

```

Unambiguous identification of the account to which credit and debit entries are made.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|AccountId|[AccountId](#schemaaccountid)|true|none|A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.|
|Status|[OBAccountStatus1Code](#schemaobaccountstatus1code)|false|none|Specifies the status of account resource in code form.|
|StatusUpdateDateTime|[StatusUpdateDateTime](#schemastatusupdatedatetime)|false|none|Date and time at which the resource status was updated.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|
|Currency|[ActiveOrHistoricCurrencyCode_0](#schemaactiveorhistoriccurrencycode_0)|true|none|Identification of the currency in which the account is held. <br>Usage: Currency should only be used in case one and the same account number covers several currencies<br>and the initiating party needs to identify which currency needs to be used for settlement on the account.|
|AccountType|[OBExternalAccountType1Code](#schemaobexternalaccounttype1code)|true|none|Specifies the type of account (personal or business).|
|AccountSubType|[OBExternalAccountSubType1Code](#schemaobexternalaccountsubtype1code)|true|none|Specifies the sub type of account (product family group).|
|Description|[Description_0](#schemadescription_0)|false|none|Specifies the description of the account type.|
|Nickname|[Nickname](#schemanickname)|false|none|The nickname of the account, assigned by the account owner in order to provide an additional means of identification of the account.|

<h2 id="tocS_OBAccount4Detail">OBAccount4Detail</h2>
<!-- backwards compatibility -->
<a id="schemaobaccount4detail"></a>
<a id="schema_OBAccount4Detail"></a>
<a id="tocSobaccount4detail"></a>
<a id="tocsobaccount4detail"></a>

```json
{
  "type": "object",
  "description": "Unambiguous identification of the account to which credit and debit entries are made.",
  "required": [
    "AccountId",
    "Currency",
    "AccountType",
    "AccountSubType",
    "Account"
  ],
  "properties": {
    "AccountId": {
      "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
      "type": "string",
      "minLength": 1,
      "maxLength": 40
    },
    "Status": {
      "description": "Specifies the status of account resource in code form.",
      "type": "string",
      "enum": [
        "Disabled",
        "Enabled"
      ]
    },
    "StatusUpdateDateTime": {
      "description": "Date and time at which the resource status was updated.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
      "type": "string",
      "format": "date-time"
    },
    "Currency": {
      "description": "Identification of the currency in which the account is held. \nUsage: Currency should only be used in case one and the same account number covers several currencies\nand the initiating party needs to identify which currency needs to be used for settlement on the account.",
      "type": "string",
      "pattern": "^[A-Z]{3,3}$"
    },
    "AccountType": {
      "description": "Specifies the type of account (personal or business).",
      "type": "string",
      "enum": [
        "Business",
        "Personal"
      ]
    },
    "AccountSubType": {
      "description": "Specifies the sub type of account (product family group).",
      "type": "string",
      "enum": [
        "CreditCard",
        "CurrentAccount",
        "Savings"
      ]
    },
    "Description": {
      "description": "Specifies the description of the account type.",
      "type": "string",
      "minLength": 1,
      "maxLength": 35
    },
    "Nickname": {
      "description": "The nickname of the account, assigned by the account owner in order to provide an additional means of identification of the account.",
      "type": "string",
      "minLength": 1,
      "maxLength": 70
    },
    "Account": {
      "type": "array",
      "items": {
        "type": "object",
        "description": "Provides the details to identify an account.",
        "required": [
          "SchemeName",
          "Identification"
        ],
        "properties": {
          "SchemeName": {
            "description": "Name of the identification scheme, in a coded form as published in an external list.",
            "type": "string",
            "x-namespaced-enum": [
              "UK.OBIE.SortCodeAccountNumber"
            ]
          },
          "Identification": {
            "description": "Identification assigned by an institution to identify an account. This identification is known by the account owner.",
            "type": "string",
            "minLength": 1,
            "maxLength": 256
          },
          "Name": {
            "description": "The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.\nNote, the account name is not the product name or the nickname of the account.",
            "type": "string",
            "minLength": 1,
            "maxLength": 350
          },
          "SecondaryIdentification": {
            "description": "This is secondary identification of the account, as assigned by the account servicing institution. \nThis can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).",
            "type": "string",
            "minLength": 1,
            "maxLength": 34
          }
        }
      }
    },
    "Servicer": {
      "type": "object",
      "required": [
        "SchemeName",
        "Identification"
      ],
      "description": "Party that manages the account on behalf of the account owner, that is manages the registration and booking of entries on the account, calculates balances on the account and provides information about the account.",
      "properties": {
        "SchemeName": {
          "description": "Name of the identification scheme, in a coded form as published in an external list.",
          "type": "string",
          "x-namespaced-enum": [
            "UK.OBIE.BICFI"
          ]
        },
        "Identification": {
          "description": "Unique and unambiguous identification of the servicing institution.",
          "type": "string",
          "minLength": 1,
          "maxLength": 35
        }
      }
    }
  }
}

```

Unambiguous identification of the account to which credit and debit entries are made.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|AccountId|[AccountId](#schemaaccountid)|true|none|A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.|
|Status|[OBAccountStatus1Code](#schemaobaccountstatus1code)|false|none|Specifies the status of account resource in code form.|
|StatusUpdateDateTime|[StatusUpdateDateTime](#schemastatusupdatedatetime)|false|none|Date and time at which the resource status was updated.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|
|Currency|[ActiveOrHistoricCurrencyCode_0](#schemaactiveorhistoriccurrencycode_0)|true|none|Identification of the currency in which the account is held. <br>Usage: Currency should only be used in case one and the same account number covers several currencies<br>and the initiating party needs to identify which currency needs to be used for settlement on the account.|
|AccountType|[OBExternalAccountType1Code](#schemaobexternalaccounttype1code)|true|none|Specifies the type of account (personal or business).|
|AccountSubType|[OBExternalAccountSubType1Code](#schemaobexternalaccountsubtype1code)|true|none|Specifies the sub type of account (product family group).|
|Description|[Description_0](#schemadescription_0)|false|none|Specifies the description of the account type.|
|Nickname|[Nickname](#schemanickname)|false|none|The nickname of the account, assigned by the account owner in order to provide an additional means of identification of the account.|
|Account|[object]|true|none|none|
|» SchemeName|[OBExternalAccountIdentification4Code](#schemaobexternalaccountidentification4code)|true|none|Name of the identification scheme, in a coded form as published in an external list.|
|» Identification|[Identification_0](#schemaidentification_0)|true|none|Identification assigned by an institution to identify an account. This identification is known by the account owner.|
|» Name|[Name_0](#schemaname_0)|false|none|The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.<br>Note, the account name is not the product name or the nickname of the account.|
|» SecondaryIdentification|[SecondaryIdentification](#schemasecondaryidentification)|false|none|This is secondary identification of the account, as assigned by the account servicing institution. <br>This can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).|
|Servicer|[OBBranchAndFinancialInstitutionIdentification5_0](#schemaobbranchandfinancialinstitutionidentification5_0)|false|none|Party that manages the account on behalf of the account owner, that is manages the registration and booking of entries on the account, calculates balances on the account and provides information about the account.|

<h2 id="tocS_OBAccount6">OBAccount6</h2>
<!-- backwards compatibility -->
<a id="schemaobaccount6"></a>
<a id="schema_OBAccount6"></a>
<a id="tocSobaccount6"></a>
<a id="tocsobaccount6"></a>

```json
{
  "type": "object",
  "description": "Unambiguous identification of the account to which credit and debit entries are made. The following fields are optional only for accounts that are switched:\n\n  * Data.Currency  \n  * Data.AccountType  \n  * Data.AccountSubType\n\nFor all other accounts, the fields must be populated by the ASPSP.",
  "required": [
    "AccountId"
  ],
  "properties": {
    "AccountId": {
      "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
      "type": "string",
      "minLength": 1,
      "maxLength": 40
    },
    "Status": {
      "description": "Specifies the status of account resource in code form.",
      "type": "string",
      "enum": [
        "Disabled",
        "Enabled"
      ]
    },
    "StatusUpdateDateTime": {
      "description": "Date and time at which the resource status was updated.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
      "type": "string",
      "format": "date-time"
    },
    "Currency": {
      "description": "Identification of the currency in which the account is held. \nUsage: Currency should only be used in case one and the same account number covers several currencies\nand the initiating party needs to identify which currency needs to be used for settlement on the account.",
      "type": "string",
      "pattern": "^[A-Z]{3,3}$"
    },
    "AccountType": {
      "description": "Specifies the type of account (personal or business).",
      "type": "string",
      "enum": [
        "Business",
        "Personal"
      ]
    },
    "AccountSubType": {
      "description": "Specifies the sub type of account (product family group).",
      "type": "string",
      "enum": [
        "CreditCard",
        "CurrentAccount",
        "Savings"
      ]
    },
    "Description": {
      "description": "Specifies the description of the account type.",
      "type": "string",
      "minLength": 1,
      "maxLength": 35
    },
    "Nickname": {
      "description": "The nickname of the account, assigned by the account owner in order to provide an additional means of identification of the account.",
      "type": "string",
      "minLength": 1,
      "maxLength": 70
    },
    "SwitchStatus": {
      "description": "Specifies the switch status for the account, in a coded form.",
      "type": "string",
      "x-namespaced-enum": [
        "UK.CASS.NotSwitced",
        "UK.CASS.SwitchCompleted"
      ]
    },
    "Account": {
      "type": "array",
      "items": {
        "type": "object",
        "description": "Provides the details to identify an account.",
        "required": [
          "SchemeName",
          "Identification"
        ],
        "properties": {
          "SchemeName": {
            "description": "Name of the identification scheme, in a coded form as published in an external list.",
            "type": "string",
            "x-namespaced-enum": [
              "UK.OBIE.SortCodeAccountNumber"
            ]
          },
          "Identification": {
            "description": "Identification assigned by an institution to identify an account. This identification is known by the account owner.",
            "type": "string",
            "minLength": 1,
            "maxLength": 256
          },
          "Name": {
            "description": "The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.\nNote, the account name is not the product name or the nickname of the account.",
            "type": "string",
            "minLength": 1,
            "maxLength": 350
          },
          "SecondaryIdentification": {
            "description": "This is secondary identification of the account, as assigned by the account servicing institution. \nThis can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).",
            "type": "string",
            "minLength": 1,
            "maxLength": 34
          }
        }
      }
    },
    "Servicer": {
      "type": "object",
      "required": [
        "SchemeName",
        "Identification"
      ],
      "description": "Party that manages the account on behalf of the account owner, that is manages the registration and booking of entries on the account, calculates balances on the account and provides information about the account.",
      "properties": {
        "SchemeName": {
          "description": "Name of the identification scheme, in a coded form as published in an external list.",
          "type": "string",
          "x-namespaced-enum": [
            "UK.OBIE.BICFI"
          ]
        },
        "Identification": {
          "description": "Unique and unambiguous identification of the servicing institution.",
          "type": "string",
          "minLength": 1,
          "maxLength": 35
        }
      }
    }
  },
  "additionalProperties": false
}

```

Unambiguous identification of the account to which credit and debit entries are made. The following fields are optional only for accounts that are switched:

  * Data.Currency  
  * Data.AccountType  
  * Data.AccountSubType

For all other accounts, the fields must be populated by the ASPSP.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|AccountId|[AccountId](#schemaaccountid)|true|none|A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.|
|Status|[OBAccountStatus1Code](#schemaobaccountstatus1code)|false|none|Specifies the status of account resource in code form.|
|StatusUpdateDateTime|[StatusUpdateDateTime](#schemastatusupdatedatetime)|false|none|Date and time at which the resource status was updated.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|
|Currency|[ActiveOrHistoricCurrencyCode_0](#schemaactiveorhistoriccurrencycode_0)|false|none|Identification of the currency in which the account is held. <br>Usage: Currency should only be used in case one and the same account number covers several currencies<br>and the initiating party needs to identify which currency needs to be used for settlement on the account.|
|AccountType|[OBExternalAccountType1Code](#schemaobexternalaccounttype1code)|false|none|Specifies the type of account (personal or business).|
|AccountSubType|[OBExternalAccountSubType1Code](#schemaobexternalaccountsubtype1code)|false|none|Specifies the sub type of account (product family group).|
|Description|[Description_0](#schemadescription_0)|false|none|Specifies the description of the account type.|
|Nickname|[Nickname](#schemanickname)|false|none|The nickname of the account, assigned by the account owner in order to provide an additional means of identification of the account.|
|SwitchStatus|[OBExternalSwitchStatusCode](#schemaobexternalswitchstatuscode)|false|none|Specifies the switch status for the account, in a coded form.|
|Account|[object]|false|none|none|
|» SchemeName|[OBExternalAccountIdentification4Code](#schemaobexternalaccountidentification4code)|true|none|Name of the identification scheme, in a coded form as published in an external list.|
|» Identification|[Identification_0](#schemaidentification_0)|true|none|Identification assigned by an institution to identify an account. This identification is known by the account owner.|
|» Name|[Name_0](#schemaname_0)|false|none|The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.<br>Note, the account name is not the product name or the nickname of the account.|
|» SecondaryIdentification|[SecondaryIdentification](#schemasecondaryidentification)|false|none|This is secondary identification of the account, as assigned by the account servicing institution. <br>This can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).|
|Servicer|[OBBranchAndFinancialInstitutionIdentification5_0](#schemaobbranchandfinancialinstitutionidentification5_0)|false|none|Party that manages the account on behalf of the account owner, that is manages the registration and booking of entries on the account, calculates balances on the account and provides information about the account.|

<h2 id="tocS_OBAccount6Basic">OBAccount6Basic</h2>
<!-- backwards compatibility -->
<a id="schemaobaccount6basic"></a>
<a id="schema_OBAccount6Basic"></a>
<a id="tocSobaccount6basic"></a>
<a id="tocsobaccount6basic"></a>

```json
{
  "type": "object",
  "description": "Unambiguous identification of the account to which credit and debit entries are made.",
  "required": [
    "AccountId",
    "Currency",
    "AccountType",
    "AccountSubType"
  ],
  "properties": {
    "AccountId": {
      "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
      "type": "string",
      "minLength": 1,
      "maxLength": 40
    },
    "Status": {
      "description": "Specifies the status of account resource in code form.",
      "type": "string",
      "enum": [
        "Disabled",
        "Enabled"
      ]
    },
    "StatusUpdateDateTime": {
      "description": "Date and time at which the resource status was updated.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
      "type": "string",
      "format": "date-time"
    },
    "Currency": {
      "description": "Identification of the currency in which the account is held. \nUsage: Currency should only be used in case one and the same account number covers several currencies\nand the initiating party needs to identify which currency needs to be used for settlement on the account.",
      "type": "string",
      "pattern": "^[A-Z]{3,3}$"
    },
    "AccountType": {
      "description": "Specifies the type of account (personal or business).",
      "type": "string",
      "enum": [
        "Business",
        "Personal"
      ]
    },
    "AccountSubType": {
      "description": "Specifies the sub type of account (product family group).",
      "type": "string",
      "enum": [
        "CreditCard",
        "CurrentAccount",
        "Savings"
      ]
    },
    "Description": {
      "description": "Specifies the description of the account type.",
      "type": "string",
      "minLength": 1,
      "maxLength": 35
    },
    "Nickname": {
      "description": "The nickname of the account, assigned by the account owner in order to provide an additional means of identification of the account.",
      "type": "string",
      "minLength": 1,
      "maxLength": 70
    },
    "SwitchStatus": {
      "description": "Specifies the switch status for the account, in a coded form.",
      "type": "string",
      "x-namespaced-enum": [
        "UK.CASS.NotSwitced",
        "UK.CASS.SwitchCompleted"
      ]
    }
  },
  "additionalProperties": false
}

```

Unambiguous identification of the account to which credit and debit entries are made.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|AccountId|[AccountId](#schemaaccountid)|true|none|A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.|
|Status|[OBAccountStatus1Code](#schemaobaccountstatus1code)|false|none|Specifies the status of account resource in code form.|
|StatusUpdateDateTime|[StatusUpdateDateTime](#schemastatusupdatedatetime)|false|none|Date and time at which the resource status was updated.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|
|Currency|[ActiveOrHistoricCurrencyCode_0](#schemaactiveorhistoriccurrencycode_0)|true|none|Identification of the currency in which the account is held. <br>Usage: Currency should only be used in case one and the same account number covers several currencies<br>and the initiating party needs to identify which currency needs to be used for settlement on the account.|
|AccountType|[OBExternalAccountType1Code](#schemaobexternalaccounttype1code)|true|none|Specifies the type of account (personal or business).|
|AccountSubType|[OBExternalAccountSubType1Code](#schemaobexternalaccountsubtype1code)|true|none|Specifies the sub type of account (product family group).|
|Description|[Description_0](#schemadescription_0)|false|none|Specifies the description of the account type.|
|Nickname|[Nickname](#schemanickname)|false|none|The nickname of the account, assigned by the account owner in order to provide an additional means of identification of the account.|
|SwitchStatus|[OBExternalSwitchStatusCode](#schemaobexternalswitchstatuscode)|false|none|Specifies the switch status for the account, in a coded form.|

<h2 id="tocS_OBAccount6Detail">OBAccount6Detail</h2>
<!-- backwards compatibility -->
<a id="schemaobaccount6detail"></a>
<a id="schema_OBAccount6Detail"></a>
<a id="tocSobaccount6detail"></a>
<a id="tocsobaccount6detail"></a>

```json
{
  "type": "object",
  "description": "Unambiguous identification of the account to which credit and debit entries are made.",
  "required": [
    "AccountId",
    "Currency",
    "AccountType",
    "AccountSubType",
    "Account"
  ],
  "properties": {
    "AccountId": {
      "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
      "type": "string",
      "minLength": 1,
      "maxLength": 40
    },
    "Status": {
      "description": "Specifies the status of account resource in code form.",
      "type": "string",
      "enum": [
        "Disabled",
        "Enabled"
      ]
    },
    "StatusUpdateDateTime": {
      "description": "Date and time at which the resource status was updated.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
      "type": "string",
      "format": "date-time"
    },
    "Currency": {
      "description": "Identification of the currency in which the account is held. \nUsage: Currency should only be used in case one and the same account number covers several currencies\nand the initiating party needs to identify which currency needs to be used for settlement on the account.",
      "type": "string",
      "pattern": "^[A-Z]{3,3}$"
    },
    "AccountType": {
      "description": "Specifies the type of account (personal or business).",
      "type": "string",
      "enum": [
        "Business",
        "Personal"
      ]
    },
    "AccountSubType": {
      "description": "Specifies the sub type of account (product family group).",
      "type": "string",
      "enum": [
        "CreditCard",
        "CurrentAccount",
        "Savings"
      ]
    },
    "Description": {
      "description": "Specifies the description of the account type.",
      "type": "string",
      "minLength": 1,
      "maxLength": 35
    },
    "Nickname": {
      "description": "The nickname of the account, assigned by the account owner in order to provide an additional means of identification of the account.",
      "type": "string",
      "minLength": 1,
      "maxLength": 70
    },
    "SwitchStatus": {
      "description": "Specifies the switch status for the account, in a coded form.",
      "type": "string",
      "x-namespaced-enum": [
        "UK.CASS.NotSwitced",
        "UK.CASS.SwitchCompleted"
      ]
    },
    "Account": {
      "type": "array",
      "items": {
        "type": "object",
        "description": "Provides the details to identify an account.",
        "required": [
          "SchemeName",
          "Identification"
        ],
        "properties": {
          "SchemeName": {
            "description": "Name of the identification scheme, in a coded form as published in an external list.",
            "type": "string",
            "x-namespaced-enum": [
              "UK.OBIE.SortCodeAccountNumber"
            ]
          },
          "Identification": {
            "description": "Identification assigned by an institution to identify an account. This identification is known by the account owner.",
            "type": "string",
            "minLength": 1,
            "maxLength": 256
          },
          "Name": {
            "description": "The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.\nNote, the account name is not the product name or the nickname of the account.",
            "type": "string",
            "minLength": 1,
            "maxLength": 350
          },
          "SecondaryIdentification": {
            "description": "This is secondary identification of the account, as assigned by the account servicing institution. \nThis can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).",
            "type": "string",
            "minLength": 1,
            "maxLength": 34
          }
        }
      }
    },
    "Servicer": {
      "type": "object",
      "required": [
        "SchemeName",
        "Identification"
      ],
      "description": "Party that manages the account on behalf of the account owner, that is manages the registration and booking of entries on the account, calculates balances on the account and provides information about the account.",
      "properties": {
        "SchemeName": {
          "description": "Name of the identification scheme, in a coded form as published in an external list.",
          "type": "string",
          "x-namespaced-enum": [
            "UK.OBIE.BICFI"
          ]
        },
        "Identification": {
          "description": "Unique and unambiguous identification of the servicing institution.",
          "type": "string",
          "minLength": 1,
          "maxLength": 35
        }
      }
    }
  },
  "additionalProperties": false
}

```

Unambiguous identification of the account to which credit and debit entries are made.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|AccountId|[AccountId](#schemaaccountid)|true|none|A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.|
|Status|[OBAccountStatus1Code](#schemaobaccountstatus1code)|false|none|Specifies the status of account resource in code form.|
|StatusUpdateDateTime|[StatusUpdateDateTime](#schemastatusupdatedatetime)|false|none|Date and time at which the resource status was updated.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|
|Currency|[ActiveOrHistoricCurrencyCode_0](#schemaactiveorhistoriccurrencycode_0)|true|none|Identification of the currency in which the account is held. <br>Usage: Currency should only be used in case one and the same account number covers several currencies<br>and the initiating party needs to identify which currency needs to be used for settlement on the account.|
|AccountType|[OBExternalAccountType1Code](#schemaobexternalaccounttype1code)|true|none|Specifies the type of account (personal or business).|
|AccountSubType|[OBExternalAccountSubType1Code](#schemaobexternalaccountsubtype1code)|true|none|Specifies the sub type of account (product family group).|
|Description|[Description_0](#schemadescription_0)|false|none|Specifies the description of the account type.|
|Nickname|[Nickname](#schemanickname)|false|none|The nickname of the account, assigned by the account owner in order to provide an additional means of identification of the account.|
|SwitchStatus|[OBExternalSwitchStatusCode](#schemaobexternalswitchstatuscode)|false|none|Specifies the switch status for the account, in a coded form.|
|Account|[object]|true|none|none|
|» SchemeName|[OBExternalAccountIdentification4Code](#schemaobexternalaccountidentification4code)|true|none|Name of the identification scheme, in a coded form as published in an external list.|
|» Identification|[Identification_0](#schemaidentification_0)|true|none|Identification assigned by an institution to identify an account. This identification is known by the account owner.|
|» Name|[Name_0](#schemaname_0)|false|none|The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.<br>Note, the account name is not the product name or the nickname of the account.|
|» SecondaryIdentification|[SecondaryIdentification](#schemasecondaryidentification)|false|none|This is secondary identification of the account, as assigned by the account servicing institution. <br>This can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).|
|Servicer|[OBBranchAndFinancialInstitutionIdentification5_0](#schemaobbranchandfinancialinstitutionidentification5_0)|false|none|Party that manages the account on behalf of the account owner, that is manages the registration and booking of entries on the account, calculates balances on the account and provides information about the account.|

<h2 id="tocS_OBAccountStatus1Code">OBAccountStatus1Code</h2>
<!-- backwards compatibility -->
<a id="schemaobaccountstatus1code"></a>
<a id="schema_OBAccountStatus1Code"></a>
<a id="tocSobaccountstatus1code"></a>
<a id="tocsobaccountstatus1code"></a>

```json
{
  "description": "Specifies the status of account resource in code form.",
  "type": "string",
  "enum": [
    "Disabled",
    "Enabled"
  ]
}

```

Specifies the status of account resource in code form.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Specifies the status of account resource in code form.|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|Disabled|
|*anonymous*|Enabled|

<h2 id="tocS_OBActiveCurrencyAndAmount_SimpleType">OBActiveCurrencyAndAmount_SimpleType</h2>
<!-- backwards compatibility -->
<a id="schemaobactivecurrencyandamount_simpletype"></a>
<a id="schema_OBActiveCurrencyAndAmount_SimpleType"></a>
<a id="tocSobactivecurrencyandamount_simpletype"></a>
<a id="tocsobactivecurrencyandamount_simpletype"></a>

```json
{
  "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
  "type": "string",
  "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
}

```

A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.|

<h2 id="tocS_OBActiveOrHistoricCurrencyAndAmount_0">OBActiveOrHistoricCurrencyAndAmount_0</h2>
<!-- backwards compatibility -->
<a id="schemaobactiveorhistoriccurrencyandamount_0"></a>
<a id="schema_OBActiveOrHistoricCurrencyAndAmount_0"></a>
<a id="tocSobactiveorhistoriccurrencyandamount_0"></a>
<a id="tocsobactiveorhistoriccurrencyandamount_0"></a>

```json
{
  "type": "object",
  "required": [
    "Amount",
    "Currency"
  ],
  "description": "The amount of the most recent direct debit collection.",
  "properties": {
    "Amount": {
      "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
      "type": "string",
      "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
    },
    "Currency": {
      "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
      "type": "string",
      "pattern": "^[A-Z]{3,3}$"
    }
  }
}

```

The amount of the most recent direct debit collection.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Amount|[OBActiveCurrencyAndAmount_SimpleType](#schemaobactivecurrencyandamount_simpletype)|true|none|A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.|
|Currency|[ActiveOrHistoricCurrencyCode_1](#schemaactiveorhistoriccurrencycode_1)|true|none|A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 "Codes for the representation of currencies and funds".|

<h2 id="tocS_OBActiveOrHistoricCurrencyAndAmount_1">OBActiveOrHistoricCurrencyAndAmount_1</h2>
<!-- backwards compatibility -->
<a id="schemaobactiveorhistoriccurrencyandamount_1"></a>
<a id="schema_OBActiveOrHistoricCurrencyAndAmount_1"></a>
<a id="tocSobactiveorhistoriccurrencyandamount_1"></a>
<a id="tocsobactiveorhistoriccurrencyandamount_1"></a>

```json
{
  "type": "object",
  "required": [
    "Amount",
    "Currency"
  ],
  "description": "Amount of money to be moved between the debtor and creditor, before deduction of charges, expressed in the currency as ordered by the initiating party.\nUsage: This amount has to be transported unchanged through the transaction chain.",
  "properties": {
    "Amount": {
      "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
      "type": "string",
      "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
    },
    "Currency": {
      "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
      "type": "string",
      "pattern": "^[A-Z]{3,3}$"
    }
  }
}

```

Amount of money to be moved between the debtor and creditor, before deduction of charges, expressed in the currency as ordered by the initiating party.
Usage: This amount has to be transported unchanged through the transaction chain.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Amount|[OBActiveCurrencyAndAmount_SimpleType](#schemaobactivecurrencyandamount_simpletype)|true|none|A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.|
|Currency|[ActiveOrHistoricCurrencyCode_1](#schemaactiveorhistoriccurrencycode_1)|true|none|A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 "Codes for the representation of currencies and funds".|

<h2 id="tocS_OBActiveOrHistoricCurrencyAndAmount_10">OBActiveOrHistoricCurrencyAndAmount_10</h2>
<!-- backwards compatibility -->
<a id="schemaobactiveorhistoriccurrencyandamount_10"></a>
<a id="schema_OBActiveOrHistoricCurrencyAndAmount_10"></a>
<a id="tocSobactiveorhistoriccurrencyandamount_10"></a>
<a id="tocsobactiveorhistoriccurrencyandamount_10"></a>

```json
{
  "type": "object",
  "required": [
    "Amount",
    "Currency"
  ],
  "description": "Transaction charges to be paid by the charge bearer.",
  "properties": {
    "Amount": {
      "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
      "type": "string",
      "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
    },
    "Currency": {
      "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
      "type": "string",
      "pattern": "^[A-Z]{3,3}$"
    }
  }
}

```

Transaction charges to be paid by the charge bearer.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Amount|[OBActiveCurrencyAndAmount_SimpleType](#schemaobactivecurrencyandamount_simpletype)|true|none|A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.|
|Currency|[ActiveOrHistoricCurrencyCode_1](#schemaactiveorhistoriccurrencycode_1)|true|none|A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 "Codes for the representation of currencies and funds".|

<h2 id="tocS_OBActiveOrHistoricCurrencyAndAmount_11">OBActiveOrHistoricCurrencyAndAmount_11</h2>
<!-- backwards compatibility -->
<a id="schemaobactiveorhistoriccurrencyandamount_11"></a>
<a id="schema_OBActiveOrHistoricCurrencyAndAmount_11"></a>
<a id="tocSobactiveorhistoriccurrencyandamount_11"></a>
<a id="tocsobactiveorhistoriccurrencyandamount_11"></a>

```json
{
  "type": "object",
  "required": [
    "Amount",
    "Currency"
  ],
  "description": "The amount of the last (most recent) Standing Order instruction.",
  "properties": {
    "Amount": {
      "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
      "type": "string",
      "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
    },
    "Currency": {
      "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
      "type": "string",
      "pattern": "^[A-Z]{3,3}$"
    }
  }
}

```

The amount of the last (most recent) Standing Order instruction.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Amount|[OBActiveCurrencyAndAmount_SimpleType](#schemaobactivecurrencyandamount_simpletype)|true|none|A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.|
|Currency|[ActiveOrHistoricCurrencyCode_1](#schemaactiveorhistoriccurrencycode_1)|true|none|A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 "Codes for the representation of currencies and funds".|

<h2 id="tocS_OBActiveOrHistoricCurrencyAndAmount_2">OBActiveOrHistoricCurrencyAndAmount_2</h2>
<!-- backwards compatibility -->
<a id="schemaobactiveorhistoriccurrencyandamount_2"></a>
<a id="schema_OBActiveOrHistoricCurrencyAndAmount_2"></a>
<a id="tocSobactiveorhistoriccurrencyandamount_2"></a>
<a id="tocsobactiveorhistoriccurrencyandamount_2"></a>

```json
{
  "type": "object",
  "required": [
    "Amount",
    "Currency"
  ],
  "description": "The amount of the first Standing Order",
  "properties": {
    "Amount": {
      "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
      "type": "string",
      "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
    },
    "Currency": {
      "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
      "type": "string",
      "pattern": "^[A-Z]{3,3}$"
    }
  }
}

```

The amount of the first Standing Order

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Amount|[OBActiveCurrencyAndAmount_SimpleType](#schemaobactivecurrencyandamount_simpletype)|true|none|A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.|
|Currency|[ActiveOrHistoricCurrencyCode_1](#schemaactiveorhistoriccurrencycode_1)|true|none|A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 "Codes for the representation of currencies and funds".|

<h2 id="tocS_OBActiveOrHistoricCurrencyAndAmount_3">OBActiveOrHistoricCurrencyAndAmount_3</h2>
<!-- backwards compatibility -->
<a id="schemaobactiveorhistoriccurrencyandamount_3"></a>
<a id="schema_OBActiveOrHistoricCurrencyAndAmount_3"></a>
<a id="tocSobactiveorhistoriccurrencyandamount_3"></a>
<a id="tocsobactiveorhistoriccurrencyandamount_3"></a>

```json
{
  "type": "object",
  "required": [
    "Amount",
    "Currency"
  ],
  "description": "The amount of the next Standing Order.",
  "properties": {
    "Amount": {
      "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
      "type": "string",
      "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
    },
    "Currency": {
      "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
      "type": "string",
      "pattern": "^[A-Z]{3,3}$"
    }
  }
}

```

The amount of the next Standing Order.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Amount|[OBActiveCurrencyAndAmount_SimpleType](#schemaobactivecurrencyandamount_simpletype)|true|none|A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.|
|Currency|[ActiveOrHistoricCurrencyCode_1](#schemaactiveorhistoriccurrencycode_1)|true|none|A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 "Codes for the representation of currencies and funds".|

<h2 id="tocS_OBActiveOrHistoricCurrencyAndAmount_4">OBActiveOrHistoricCurrencyAndAmount_4</h2>
<!-- backwards compatibility -->
<a id="schemaobactiveorhistoriccurrencyandamount_4"></a>
<a id="schema_OBActiveOrHistoricCurrencyAndAmount_4"></a>
<a id="tocSobactiveorhistoriccurrencyandamount_4"></a>
<a id="tocsobactiveorhistoriccurrencyandamount_4"></a>

```json
{
  "type": "object",
  "required": [
    "Amount",
    "Currency"
  ],
  "description": "The amount of the final Standing Order",
  "properties": {
    "Amount": {
      "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
      "type": "string",
      "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
    },
    "Currency": {
      "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
      "type": "string",
      "pattern": "^[A-Z]{3,3}$"
    }
  }
}

```

The amount of the final Standing Order

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Amount|[OBActiveCurrencyAndAmount_SimpleType](#schemaobactivecurrencyandamount_simpletype)|true|none|A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.|
|Currency|[ActiveOrHistoricCurrencyCode_1](#schemaactiveorhistoriccurrencycode_1)|true|none|A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 "Codes for the representation of currencies and funds".|

<h2 id="tocS_OBActiveOrHistoricCurrencyAndAmount_5">OBActiveOrHistoricCurrencyAndAmount_5</h2>
<!-- backwards compatibility -->
<a id="schemaobactiveorhistoriccurrencyandamount_5"></a>
<a id="schema_OBActiveOrHistoricCurrencyAndAmount_5"></a>
<a id="tocSobactiveorhistoriccurrencyandamount_5"></a>
<a id="tocsobactiveorhistoriccurrencyandamount_5"></a>

```json
{
  "type": "object",
  "required": [
    "Amount",
    "Currency"
  ],
  "description": "Amount of money associated with the statement benefit type.",
  "properties": {
    "Amount": {
      "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
      "type": "string",
      "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
    },
    "Currency": {
      "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
      "type": "string",
      "pattern": "^[A-Z]{3,3}$"
    }
  }
}

```

Amount of money associated with the statement benefit type.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Amount|[OBActiveCurrencyAndAmount_SimpleType](#schemaobactivecurrencyandamount_simpletype)|true|none|A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.|
|Currency|[ActiveOrHistoricCurrencyCode_1](#schemaactiveorhistoriccurrencycode_1)|true|none|A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 "Codes for the representation of currencies and funds".|

<h2 id="tocS_OBActiveOrHistoricCurrencyAndAmount_6">OBActiveOrHistoricCurrencyAndAmount_6</h2>
<!-- backwards compatibility -->
<a id="schemaobactiveorhistoriccurrencyandamount_6"></a>
<a id="schema_OBActiveOrHistoricCurrencyAndAmount_6"></a>
<a id="tocSobactiveorhistoriccurrencyandamount_6"></a>
<a id="tocsobactiveorhistoriccurrencyandamount_6"></a>

```json
{
  "type": "object",
  "required": [
    "Amount",
    "Currency"
  ],
  "description": "Amount of money associated with the statement fee type.",
  "properties": {
    "Amount": {
      "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
      "type": "string",
      "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
    },
    "Currency": {
      "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
      "type": "string",
      "pattern": "^[A-Z]{3,3}$"
    }
  }
}

```

Amount of money associated with the statement fee type.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Amount|[OBActiveCurrencyAndAmount_SimpleType](#schemaobactivecurrencyandamount_simpletype)|true|none|A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.|
|Currency|[ActiveOrHistoricCurrencyCode_1](#schemaactiveorhistoriccurrencycode_1)|true|none|A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 "Codes for the representation of currencies and funds".|

<h2 id="tocS_OBActiveOrHistoricCurrencyAndAmount_7">OBActiveOrHistoricCurrencyAndAmount_7</h2>
<!-- backwards compatibility -->
<a id="schemaobactiveorhistoriccurrencyandamount_7"></a>
<a id="schema_OBActiveOrHistoricCurrencyAndAmount_7"></a>
<a id="tocSobactiveorhistoriccurrencyandamount_7"></a>
<a id="tocsobactiveorhistoriccurrencyandamount_7"></a>

```json
{
  "type": "object",
  "required": [
    "Amount",
    "Currency"
  ],
  "description": "Amount of money associated with the statement interest amount type.",
  "properties": {
    "Amount": {
      "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
      "type": "string",
      "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
    },
    "Currency": {
      "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
      "type": "string",
      "pattern": "^[A-Z]{3,3}$"
    }
  }
}

```

Amount of money associated with the statement interest amount type.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Amount|[OBActiveCurrencyAndAmount_SimpleType](#schemaobactivecurrencyandamount_simpletype)|true|none|A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.|
|Currency|[ActiveOrHistoricCurrencyCode_1](#schemaactiveorhistoriccurrencycode_1)|true|none|A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 "Codes for the representation of currencies and funds".|

<h2 id="tocS_OBActiveOrHistoricCurrencyAndAmount_8">OBActiveOrHistoricCurrencyAndAmount_8</h2>
<!-- backwards compatibility -->
<a id="schemaobactiveorhistoriccurrencyandamount_8"></a>
<a id="schema_OBActiveOrHistoricCurrencyAndAmount_8"></a>
<a id="tocSobactiveorhistoriccurrencyandamount_8"></a>
<a id="tocsobactiveorhistoriccurrencyandamount_8"></a>

```json
{
  "type": "object",
  "required": [
    "Amount",
    "Currency"
  ],
  "description": "Amount of money associated with the amount type.",
  "properties": {
    "Amount": {
      "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
      "type": "string",
      "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
    },
    "Currency": {
      "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
      "type": "string",
      "pattern": "^[A-Z]{3,3}$"
    }
  }
}

```

Amount of money associated with the amount type.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Amount|[OBActiveCurrencyAndAmount_SimpleType](#schemaobactivecurrencyandamount_simpletype)|true|none|A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.|
|Currency|[ActiveOrHistoricCurrencyCode_1](#schemaactiveorhistoriccurrencycode_1)|true|none|A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 "Codes for the representation of currencies and funds".|

<h2 id="tocS_OBActiveOrHistoricCurrencyAndAmount_9">OBActiveOrHistoricCurrencyAndAmount_9</h2>
<!-- backwards compatibility -->
<a id="schemaobactiveorhistoriccurrencyandamount_9"></a>
<a id="schema_OBActiveOrHistoricCurrencyAndAmount_9"></a>
<a id="tocSobactiveorhistoriccurrencyandamount_9"></a>
<a id="tocsobactiveorhistoriccurrencyandamount_9"></a>

```json
{
  "type": "object",
  "required": [
    "Amount",
    "Currency"
  ],
  "description": "Amount of money in the cash transaction entry.",
  "properties": {
    "Amount": {
      "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
      "type": "string",
      "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
    },
    "Currency": {
      "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
      "type": "string",
      "pattern": "^[A-Z]{3,3}$"
    }
  }
}

```

Amount of money in the cash transaction entry.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Amount|[OBActiveCurrencyAndAmount_SimpleType](#schemaobactivecurrencyandamount_simpletype)|true|none|A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.|
|Currency|[ActiveOrHistoricCurrencyCode_1](#schemaactiveorhistoriccurrencycode_1)|true|none|A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 "Codes for the representation of currencies and funds".|

<h2 id="tocS_OBAddressTypeCode">OBAddressTypeCode</h2>
<!-- backwards compatibility -->
<a id="schemaobaddresstypecode"></a>
<a id="schema_OBAddressTypeCode"></a>
<a id="tocSobaddresstypecode"></a>
<a id="tocsobaddresstypecode"></a>

```json
{
  "description": "Identifies the nature of the postal address.",
  "type": "string",
  "enum": [
    "Business",
    "Correspondence",
    "DeliveryTo",
    "MailTo",
    "POBox",
    "Postal",
    "Residential",
    "Statement"
  ]
}

```

Identifies the nature of the postal address.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Identifies the nature of the postal address.|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|Business|
|*anonymous*|Correspondence|
|*anonymous*|DeliveryTo|
|*anonymous*|MailTo|
|*anonymous*|POBox|
|*anonymous*|Postal|
|*anonymous*|Residential|
|*anonymous*|Statement|

<h2 id="tocS_OBBCAData1">OBBCAData1</h2>
<!-- backwards compatibility -->
<a id="schemaobbcadata1"></a>
<a id="schema_OBBCAData1"></a>
<a id="tocSobbcadata1"></a>
<a id="tocsobbcadata1"></a>

```json
{
  "type": "object",
  "title": "BCA",
  "properties": {
    "ProductDetails": {
      "type": "object",
      "title": "ProductDetails",
      "properties": {
        "Segment": {
          "description": "Market segmentation is a marketing term referring to the aggregating of prospective buyers into groups, or segments, that have common needs and respond similarly to a marketing action. Market segmentation enables companies to target different categories of consumers who perceive the full value of certain products and services differently from one another.\n\nRead more: Market Segmentation http://www.investopedia.com/terms/m/marketsegmentation.asp#ixzz4gfEEalTd \nWith respect to BCA products, they are segmented in relation to different markets that they wish to focus on. ",
          "title": "Segment",
          "type": "array",
          "items": {
            "description": "Market segmentation is a marketing term referring to the aggregating of prospective buyers into groups, or segments, that have common needs and respond similarly to a marketing action. Market segmentation enables companies to target different categories of consumers who perceive the full value of certain products and services differently from one another.\n\nRead more: Market Segmentation http://www.investopedia.com/terms/m/marketsegmentation.asp#ixzz4gfEEalTd \nWith respect to BCA products, they are segmented in relation to different markets that they wish to focus on. ",
            "type": "string",
            "enum": [
              "ClientAccount",
              "Standard",
              "NonCommercialChaitiesClbSoc",
              "NonCommercialPublicAuthGovt",
              "Religious",
              "SectorSpecific",
              "Startup",
              "Switcher"
            ]
          }
        },
        "FeeFreeLength": {
          "description": "The length/duration of the fee free period",
          "title": "FeeFreeLength",
          "type": "number",
          "format": "float"
        },
        "FeeFreeLengthPeriod": {
          "description": "The unit of period (days, weeks, months etc.) of the promotional length",
          "title": "FeeFreeLengthPeriod",
          "type": "string",
          "enum": [
            "Day",
            "Half Year",
            "Month",
            "Quarter",
            "Week",
            "Year"
          ]
        },
        "Notes": {
          "description": "Optional additional notes to supplement the Core product details",
          "title": "Notes",
          "type": "array",
          "items": {
            "description": "maxLength 2000 text",
            "type": "string",
            "minLength": 1,
            "maxLength": 2000
          }
        }
      },
      "additionalProperties": false
    },
    "CreditInterest": {
      "description": "Details about the interest that may be payable to the BCA account holders",
      "type": "object",
      "title": "CreditInterest",
      "properties": {
        "TierBandSet": {
          "description": "The group of tiers or bands for which credit interest can be applied.",
          "type": "array",
          "title": "TierBandSet",
          "items": {
            "description": "The group of tiers or bands for which credit interest can be applied.",
            "type": "object",
            "properties": {
              "TierBandMethod": {
                "description": "The methodology of how credit interest is paid/applied. It can be:-\n\n1. Banded\nInterest rates are banded. i.e. Increasing rate on whole balance as balance increases.\n\n2. Tiered\nInterest rates are tiered. i.e. increasing rate for each tier as balance increases, but interest paid on tier fixed for that tier and not on whole balance.\n\n3. Whole\nThe same interest rate is applied irrespective of the BCA balance",
                "title": "TierBandMethod",
                "type": "string",
                "enum": [
                  "Banded",
                  "Tiered",
                  "Whole"
                ]
              },
              "CalculationMethod": {
                "description": "Methods of calculating interest",
                "title": "CalculationMethod",
                "type": "string",
                "enum": [
                  "Compound",
                  "SimpleInterest"
                ]
              },
              "Destination": {
                "description": "Describes whether accrued interest is payable only to the BCA or to another bank account",
                "title": "Destination",
                "type": "string",
                "enum": [
                  "PayAway",
                  "SelfCredit"
                ]
              },
              "Notes": {
                "description": "Optional additional notes to supplement the Tier Band Set details",
                "title": "Notes",
                "type": "array",
                "items": {
                  "description": "maxLength 2000 text",
                  "type": "string",
                  "minLength": 1,
                  "maxLength": 2000
                }
              },
              "TierBand": {
                "description": "Tier Band Details",
                "type": "array",
                "title": "TierBand",
                "items": {
                  "description": "Tier Band Details",
                  "type": "object",
                  "properties": {
                    "Identification": {
                      "description": "Unique and unambiguous identification of a  Tier Band for a BCA.",
                      "title": "Identification",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 35
                    },
                    "TierValueMinimum": {
                      "description": "Minimum deposit value for which the credit interest tier applies.",
                      "title": "TierValueMinimum",
                      "type": "string",
                      "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                    },
                    "TierValueMaximum": {
                      "description": "Maximum deposit value for which the credit interest tier applies.",
                      "title": "TierValueMaximum",
                      "type": "string",
                      "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                    },
                    "CalculationFrequency": {
                      "description": "How often is credit interest calculated for the account.",
                      "title": "CalculationFrequency",
                      "type": "string",
                      "enum": [
                        "Daily",
                        "HalfYearly",
                        "Monthly",
                        "Other",
                        "Quarterly",
                        "PerStatementDate",
                        "Weekly",
                        "Yearly"
                      ]
                    },
                    "ApplicationFrequency": {
                      "description": "How often is interest applied to the BCA for this tier/band i.e. how often the financial institution pays accumulated interest to the customer's BCA.",
                      "title": "ApplicationFrequency",
                      "type": "string",
                      "enum": [
                        "Daily",
                        "HalfYearly",
                        "Monthly",
                        "Other",
                        "Quarterly",
                        "PerStatementDate",
                        "Weekly",
                        "Yearly"
                      ]
                    },
                    "DepositInterestAppliedCoverage": {
                      "description": "Amount on which Interest applied.",
                      "title": "DepositInterestAppliedCoverage",
                      "type": "string",
                      "enum": [
                        "Banded",
                        "Tiered",
                        "Whole"
                      ]
                    },
                    "FixedVariableInterestRateType": {
                      "description": "Type of interest rate, Fixed or Variable",
                      "title": "FixedVariableInterestRateType",
                      "type": "string",
                      "enum": [
                        "Fixed",
                        "Variable"
                      ]
                    },
                    "AER": {
                      "description": "The annual equivalent rate (AER) is interest that is calculated under the assumption that any interest paid is combined with the original balance and the next interest payment will be based on the slightly higher account balance. Overall, this means that interest can be compounded several times in a year depending on the number of times that interest payments are made. \n\nRead more: Annual Equivalent Rate (AER) http://www.investopedia.com/terms/a/aer.asp#ixzz4gfR7IO1A",
                      "title": "AER",
                      "type": "string",
                      "pattern": "^(-?\\d{1,3}){1}(\\.\\d{1,4}){0,1}$"
                    },
                    "BankInterestRateType": {
                      "description": "Interest rate types, other than AER, which financial institutions may use to describe the annual interest rate payable to the BCA.",
                      "title": "BankInterestRateType",
                      "type": "string",
                      "enum": [
                        "Gross",
                        "Other"
                      ]
                    },
                    "BankInterestRate": {
                      "description": "Bank Interest for the BCA product",
                      "title": "BankInterestRate",
                      "type": "string",
                      "pattern": "^(-?\\d{1,3}){1}(\\.\\d{1,4}){0,1}$"
                    },
                    "Notes": {
                      "description": "Optional additional notes to supplement the Tier Band details",
                      "title": "Notes",
                      "type": "array",
                      "items": {
                        "description": "maxLength 2000 text",
                        "type": "string",
                        "minLength": 1,
                        "maxLength": 2000
                      }
                    },
                    "OtherBankInterestType": {
                      "description": "Other interest rate types which are not available in the standard code list",
                      "type": "object",
                      "title": "OtherBankInterestType",
                      "properties": {
                        "Code": {
                          "description": "The four letter Mnemonic used within an XML file to identify a code",
                          "title": "Code",
                          "type": "string",
                          "pattern": "^\\w{0,4}$",
                          "minLength": 0,
                          "maxLength": 4
                        },
                        "Name": {
                          "description": "Long name associated with the code",
                          "title": "Name",
                          "type": "string",
                          "minLength": 1,
                          "maxLength": 70
                        },
                        "Description": {
                          "description": "Description to describe the purpose of the code",
                          "title": "Description",
                          "type": "string",
                          "minLength": 1,
                          "maxLength": 350
                        }
                      },
                      "additionalProperties": false,
                      "required": [
                        "Name",
                        "Description"
                      ]
                    },
                    "OtherApplicationFrequency": {
                      "description": "Other application frequencies that are not available in the standard code list",
                      "type": "object",
                      "title": "OtherApplicationFrequency",
                      "properties": {
                        "Code": {
                          "description": "The four letter Mnemonic used within an XML file to identify a code",
                          "title": "Code",
                          "type": "string",
                          "pattern": "^\\w{0,4}$",
                          "minLength": 0,
                          "maxLength": 4
                        },
                        "Name": {
                          "description": "Long name associated with the code",
                          "title": "Name",
                          "type": "string",
                          "minLength": 1,
                          "maxLength": 70
                        },
                        "Description": {
                          "description": "Description to describe the purpose of the code",
                          "title": "Description",
                          "type": "string",
                          "minLength": 1,
                          "maxLength": 350
                        }
                      },
                      "additionalProperties": false,
                      "required": [
                        "Name",
                        "Description"
                      ]
                    },
                    "OtherCalculationFrequency": {
                      "description": "Other calculation frequency which is not available in the standard code set.",
                      "type": "object",
                      "title": "OtherCalculationFrequency",
                      "properties": {
                        "Code": {
                          "description": "The four letter Mnemonic used within an XML file to identify a code",
                          "title": "Code",
                          "type": "string",
                          "pattern": "^\\w{0,4}$",
                          "minLength": 0,
                          "maxLength": 4
                        },
                        "Name": {
                          "description": "Long name associated with the code",
                          "title": "Name",
                          "type": "string",
                          "minLength": 1,
                          "maxLength": 70
                        },
                        "Description": {
                          "description": "Description to describe the purpose of the code",
                          "title": "Description",
                          "type": "string",
                          "minLength": 1,
                          "maxLength": 350
                        }
                      },
                      "additionalProperties": false,
                      "required": [
                        "Name",
                        "Description"
                      ]
                    }
                  },
                  "required": [
                    "TierValueMinimum",
                    "ApplicationFrequency",
                    "FixedVariableInterestRateType",
                    "AER"
                  ]
                },
                "minItems": 1
              }
            },
            "required": [
              "TierBandMethod",
              "Destination",
              "TierBand"
            ]
          },
          "minItems": 1
        }
      },
      "additionalProperties": false,
      "required": [
        "TierBandSet"
      ]
    },
    "Overdraft": {
      "description": "Borrowing details",
      "type": "object",
      "title": "Overdraft",
      "properties": {
        "Notes": {
          "description": "Associated Notes about the overdraft rates",
          "title": "Notes",
          "type": "array",
          "items": {
            "description": "maxLength 2000 text",
            "type": "string",
            "minLength": 1,
            "maxLength": 2000
          }
        },
        "OverdraftTierBandSet": {
          "description": "Tier band set details",
          "type": "array",
          "title": "OverdraftTierBandSet",
          "items": {
            "description": "Tier band set details",
            "type": "object",
            "properties": {
              "TierBandMethod": {
                "description": "The methodology of how overdraft is charged. It can be:\n'Whole'  Where the same charge/rate is applied to the entirety of the overdraft balance (where charges are applicable). \n'Tiered' Where different charges/rates are applied dependent on overdraft maximum and minimum balance amount tiers defined by the lending financial organisation\n'Banded' Where different charges/rates are applied dependent on overdraft maximum and minimum balance amount bands defined by a government organisation.",
                "title": "TierBandMethod",
                "type": "string",
                "enum": [
                  "Banded",
                  "Tiered",
                  "Whole"
                ]
              },
              "OverdraftType": {
                "description": "An overdraft can either be 'committed' which means that the facility cannot be withdrawn without reasonable notification before it's agreed end date, or 'on demand' which means that the financial institution can demand repayment at any point in time.",
                "title": "OverdraftType",
                "type": "string",
                "enum": [
                  "Committed",
                  "OnDemand"
                ]
              },
              "Identification": {
                "description": "Unique and unambiguous identification of a  Tier Band for a overdraft product.",
                "title": "Identification",
                "type": "string",
                "minLength": 1,
                "maxLength": 35
              },
              "AuthorisedIndicator": {
                "description": "Indicates if the Overdraft is authorised (Y) or unauthorised (N)",
                "title": "AuthorisedIndicator",
                "type": "boolean"
              },
              "BufferAmount": {
                "description": "When a customer exceeds their credit limit, a financial institution will not charge the customer unauthorised overdraft charges if they do not exceed by more than the buffer amount. Note: Authorised overdraft charges may still apply.",
                "title": "BufferAmount",
                "type": "string",
                "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
              },
              "Notes": {
                "description": "Optional additional notes to supplement the overdraft Tier Band Set details",
                "title": "Notes",
                "type": "array",
                "items": {
                  "description": "maxLength 2000 text",
                  "type": "string",
                  "minLength": 1,
                  "maxLength": 2000
                }
              },
              "OverdraftTierBand": {
                "description": "Provides overdraft details for a specific tier or band",
                "type": "array",
                "title": "OverdraftTierBand",
                "items": {
                  "description": "Provides overdraft details for a specific tier or band",
                  "type": "object",
                  "properties": {
                    "Identification": {
                      "description": "Unique and unambiguous identification of a  Tier Band for a overdraft.",
                      "title": "Identification",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 35
                    },
                    "TierValueMin": {
                      "description": "Minimum value of Overdraft Tier/Band",
                      "title": "TierValueMin",
                      "type": "string",
                      "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                    },
                    "TierValueMax": {
                      "description": "Maximum value of Overdraft Tier/Band",
                      "title": "TierValueMax",
                      "type": "string",
                      "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                    },
                    "EAR": {
                      "description": "EAR means Effective Annual Rate and/or Equivalent Annual Rate (frequently\nused interchangeably), being the actual annual interest rate of an Overdraft.",
                      "title": "EAR",
                      "type": "string",
                      "pattern": "^(-?\\d{1,3}){1}(\\.\\d{1,4}){0,1}$"
                    },
                    "RepresentativeAPR": {
                      "description": "An annual percentage rate (APR) is the annual rate charged for borrowing or earned through an investment. APR is expressed as a percentage that represents the actual yearly cost of funds over the term of a loan. This includes any fees or additional costs associated with the transaction but does not take compounding into account.",
                      "title": "RepresentativeAPR",
                      "type": "string",
                      "pattern": "^(-?\\d{1,3}){1}(\\.\\d{1,4}){0,1}$"
                    },
                    "AgreementLengthMin": {
                      "description": "Specifies the minimum length of a band for a fixed overdraft agreement",
                      "title": "AgreementLengthMin",
                      "type": "number",
                      "format": "float"
                    },
                    "AgreementLengthMax": {
                      "description": "Specifies the maximum length of a band for a fixed overdraft agreement",
                      "title": "AgreementLengthMax",
                      "type": "number",
                      "format": "float"
                    },
                    "AgreementPeriod": {
                      "description": "Specifies the period of a fixed length overdraft agreement",
                      "title": "AgreementPeriod",
                      "type": "string",
                      "enum": [
                        "Day",
                        "Half Year",
                        "Month",
                        "Quarter",
                        "Week",
                        "Year"
                      ]
                    },
                    "OverdraftInterestChargingCoverage": {
                      "description": "Refers to which interest rate is applied when interests are tiered. For example, if an overdraft balance is £2k and the interest tiers are:- 0-£500 0.1%, 500-1000 0.2%, 1000-10000 0.5%, then the applicable interest rate could either be 0.5% of the entire balance (since the account balance sits in the top interest tier) or (0.1%*500)+(0.2%*500)+(0.5%*1000). In the 1st situation, we say the interest is applied to the ‘Whole’ of the account balance,  and in the 2nd that it is ‘Tiered’.",
                      "title": "OverdraftInterestChargingCoverage",
                      "type": "string",
                      "enum": [
                        "Banded",
                        "Tiered",
                        "Whole"
                      ]
                    },
                    "BankGuaranteedIndicator": {
                      "description": "Indicates whether the advertised overdraft rate is guaranteed to be offered to a borrower by the bank e.g. if it’s part of a government scheme, or whether the rate may vary dependent on the applicant’s circumstances.",
                      "title": "BankGuaranteedIndicator",
                      "type": "boolean"
                    },
                    "Notes": {
                      "description": "Optional additional notes to supplement the Tier/band details",
                      "title": "Notes",
                      "type": "array",
                      "items": {
                        "description": "maxLength 2000 text",
                        "type": "string",
                        "minLength": 1,
                        "maxLength": 2000
                      }
                    },
                    "OverdraftFeesCharges": {
                      "description": "Overdraft fees and charges",
                      "type": "array",
                      "title": "OverdraftFeesCharges",
                      "items": {
                        "description": "Overdraft fees and charges",
                        "type": "object",
                        "properties": {
                          "OverdraftFeeChargeCap": {
                            "description": "Details about any caps (maximum charges) that apply to a particular fee/charge. Capping can either be based on an amount (in gbp), an amount (in items) or a rate.",
                            "type": "array",
                            "title": "OverdraftFeeChargeCap",
                            "items": {
                              "description": "Details about any caps (maximum charges) that apply to a particular fee/charge. Capping can either be based on an amount (in gbp), an amount (in items) or a rate.",
                              "type": "object",
                              "properties": {
                                "FeeType": {
                                  "description": "Fee/charge type which is being capped",
                                  "title": "FeeType",
                                  "type": "array",
                                  "items": {
                                    "description": "Overdraft fee type",
                                    "type": "string",
                                    "enum": [
                                      "ArrangedOverdraft",
                                      "AnnualReview",
                                      "EmergencyBorrowing",
                                      "BorrowingItem",
                                      "OverdraftRenewal",
                                      "OverdraftSetup",
                                      "Surcharge",
                                      "TempOverdraft",
                                      "UnauthorisedBorrowing",
                                      "UnauthorisedPaidTrans",
                                      "Other",
                                      "UnauthorisedUnpaidTrans"
                                    ]
                                  },
                                  "minItems": 1
                                },
                                "MinMaxType": {
                                  "description": "Min Max type",
                                  "title": "MinMaxType",
                                  "type": "string",
                                  "enum": [
                                    "Minimum",
                                    "Maximum"
                                  ]
                                },
                                "FeeCapOccurrence": {
                                  "description": "Indicates whether the advertised overdraft rate is guaranteed to be offered to a borrower by the bank e.g. if it’s part of a government scheme, or whether the rate may vary dependent on the applicant’s circumstances.",
                                  "title": "FeeCapOccurrence",
                                  "type": "number",
                                  "format": "float"
                                },
                                "FeeCapAmount": {
                                  "description": "Cap amount charged for a fee/charge",
                                  "title": "FeeCapAmount",
                                  "type": "string",
                                  "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                                },
                                "CappingPeriod": {
                                  "description": "Period e.g. day, week, month etc. for which the fee/charge is capped",
                                  "title": "CappingPeriod",
                                  "type": "string",
                                  "enum": [
                                    "Day",
                                    "Half Year",
                                    "Month",
                                    "Quarter",
                                    "Week",
                                    "Year"
                                  ]
                                },
                                "Notes": {
                                  "description": "Notes related to Overdraft fee charge cap",
                                  "title": "Notes",
                                  "type": "array",
                                  "items": {
                                    "description": "maxLength 2000 text",
                                    "type": "string",
                                    "minLength": 1,
                                    "maxLength": 2000
                                  }
                                },
                                "OtherFeeType": {
                                  "description": "Other fee type code which is not available in the standard code set",
                                  "type": "array",
                                  "title": "OtherFeeType",
                                  "items": {
                                    "description": "Other fee type code which is not available in the standard code set",
                                    "type": "object",
                                    "properties": {
                                      "Code": {
                                        "description": "The four letter Mnemonic used within an XML file to identify a code",
                                        "title": "Code",
                                        "type": "string",
                                        "pattern": "^\\w{0,4}$",
                                        "minLength": 0,
                                        "maxLength": 4
                                      },
                                      "Name": {
                                        "description": "Long name associated with the code",
                                        "title": "Name",
                                        "type": "string",
                                        "minLength": 1,
                                        "maxLength": 70
                                      },
                                      "Description": {
                                        "description": "Description to describe the purpose of the code",
                                        "title": "Description",
                                        "type": "string",
                                        "minLength": 1,
                                        "maxLength": 350
                                      }
                                    },
                                    "required": [
                                      "Name",
                                      "Description"
                                    ]
                                  }
                                }
                              },
                              "required": [
                                "FeeType",
                                "MinMaxType"
                              ]
                            }
                          },
                          "OverdraftFeeChargeDetail": {
                            "description": "Details about the fees/charges",
                            "type": "array",
                            "title": "OverdraftFeeChargeDetail",
                            "items": {
                              "description": "Details about the fees/charges",
                              "type": "object",
                              "properties": {
                                "FeeType": {
                                  "description": "Overdraft fee type",
                                  "title": "FeeType",
                                  "type": "string",
                                  "enum": [
                                    "ArrangedOverdraft",
                                    "AnnualReview",
                                    "EmergencyBorrowing",
                                    "BorrowingItem",
                                    "OverdraftRenewal",
                                    "OverdraftSetup",
                                    "Surcharge",
                                    "TempOverdraft",
                                    "UnauthorisedBorrowing",
                                    "UnauthorisedPaidTrans",
                                    "Other",
                                    "UnauthorisedUnpaidTrans"
                                  ]
                                },
                                "NegotiableIndicator": {
                                  "description": "Indicates whether fee and charges are negotiable",
                                  "title": "NegotiableIndicator",
                                  "type": "boolean"
                                },
                                "OverdraftControlIndicator": {
                                  "description": "Indicates if the fee/charge is already covered by an 'Overdraft Control' fee or not.",
                                  "title": "OverdraftControlIndicator",
                                  "type": "boolean"
                                },
                                "IncrementalBorrowingAmount": {
                                  "description": "Every additional tranche of an overdraft balance to which an overdraft fee is applied",
                                  "title": "IncrementalBorrowingAmount",
                                  "type": "string",
                                  "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                                },
                                "FeeAmount": {
                                  "description": "Amount charged for an overdraft fee/charge (where it is charged in terms of an amount rather than a rate)",
                                  "title": "FeeAmount",
                                  "type": "string",
                                  "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                                },
                                "FeeRate": {
                                  "description": "Rate charged for overdraft fee/charge (where it is charged in terms of a rate rather than an amount)",
                                  "title": "FeeRate",
                                  "type": "string",
                                  "pattern": "^(-?\\d{1,3}){1}(\\.\\d{1,4}){0,1}$"
                                },
                                "FeeRateType": {
                                  "description": "Rate type for overdraft fee/charge (where it is charged in terms of a rate rather than an amount)",
                                  "title": "FeeRateType",
                                  "type": "string",
                                  "enum": [
                                    "Gross",
                                    "Other"
                                  ]
                                },
                                "ApplicationFrequency": {
                                  "description": "Frequency at which the overdraft charge is applied to the account",
                                  "title": "ApplicationFrequency",
                                  "type": "string",
                                  "enum": [
                                    "OnClosing",
                                    "OnOpening",
                                    "ChargingPeriod",
                                    "Daily",
                                    "PerItem",
                                    "Monthly",
                                    "OnAnniversary",
                                    "Other",
                                    "PerHundredPounds",
                                    "PerHour",
                                    "PerOccurrence",
                                    "PerSheet",
                                    "PerTransaction",
                                    "PerTransactionAmount",
                                    "PerTransactionPercentage",
                                    "Quarterly",
                                    "SixMonthly",
                                    "StatementMonthly",
                                    "Weekly",
                                    "Yearly"
                                  ]
                                },
                                "CalculationFrequency": {
                                  "description": "How often is the overdraft fee/charge calculated for the account.",
                                  "title": "CalculationFrequency",
                                  "type": "string",
                                  "enum": [
                                    "OnClosing",
                                    "OnOpening",
                                    "ChargingPeriod",
                                    "Daily",
                                    "PerItem",
                                    "Monthly",
                                    "OnAnniversary",
                                    "Other",
                                    "PerHundredPounds",
                                    "PerHour",
                                    "PerOccurrence",
                                    "PerSheet",
                                    "PerTransaction",
                                    "PerTransactionAmount",
                                    "PerTransactionPercentage",
                                    "Quarterly",
                                    "SixMonthly",
                                    "StatementMonthly",
                                    "Weekly",
                                    "Yearly"
                                  ]
                                },
                                "Notes": {
                                  "description": "Free text for capturing any other info related to Overdraft Fees Charge Details",
                                  "title": "Notes",
                                  "type": "array",
                                  "items": {
                                    "description": "maxLength 2000 text",
                                    "type": "string",
                                    "minLength": 1,
                                    "maxLength": 2000
                                  }
                                },
                                "OverdraftFeeChargeCap": {
                                  "description": "Details about any caps (maximum charges) that apply to a particular fee/charge. Capping can either be based on an amount (in gbp), an amount (in items) or a rate.",
                                  "type": "array",
                                  "title": "OverdraftFeeChargeCap",
                                  "items": {
                                    "description": "Details about any caps (maximum charges) that apply to a particular fee/charge. Capping can either be based on an amount (in gbp), an amount (in items) or a rate.",
                                    "type": "object",
                                    "properties": {
                                      "FeeType": {
                                        "description": "Fee/charge type which is being capped",
                                        "title": "FeeType",
                                        "type": "array",
                                        "items": {
                                          "description": "Overdraft fee type",
                                          "type": "string",
                                          "enum": [
                                            "ArrangedOverdraft",
                                            "AnnualReview",
                                            "EmergencyBorrowing",
                                            "BorrowingItem",
                                            "OverdraftRenewal",
                                            "OverdraftSetup",
                                            "Surcharge",
                                            "TempOverdraft",
                                            "UnauthorisedBorrowing",
                                            "UnauthorisedPaidTrans",
                                            "Other",
                                            "UnauthorisedUnpaidTrans"
                                          ]
                                        },
                                        "minItems": 1
                                      },
                                      "MinMaxType": {
                                        "description": "Min Max type",
                                        "title": "MinMaxType",
                                        "type": "string",
                                        "enum": [
                                          "Minimum",
                                          "Maximum"
                                        ]
                                      },
                                      "FeeCapOccurrence": {
                                        "description": "Indicates whether the advertised overdraft rate is guaranteed to be offered to a borrower by the bank e.g. if it’s part of a government scheme, or whether the rate may vary dependent on the applicant’s circumstances.",
                                        "title": "FeeCapOccurrence",
                                        "type": "number",
                                        "format": "float"
                                      },
                                      "FeeCapAmount": {
                                        "description": "Cap amount charged for a fee/charge",
                                        "title": "FeeCapAmount",
                                        "type": "string",
                                        "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                                      },
                                      "CappingPeriod": {
                                        "description": "Period e.g. day, week, month etc. for which the fee/charge is capped",
                                        "title": "CappingPeriod",
                                        "type": "string",
                                        "enum": [
                                          "Day",
                                          "Half Year",
                                          "Month",
                                          "Quarter",
                                          "Week",
                                          "Year"
                                        ]
                                      },
                                      "Notes": {
                                        "description": "Notes related to Overdraft fee charge cap",
                                        "title": "Notes",
                                        "type": "array",
                                        "items": {
                                          "description": "maxLength 2000 text",
                                          "type": "string",
                                          "minLength": 1,
                                          "maxLength": 2000
                                        }
                                      },
                                      "OtherFeeType": {
                                        "description": "Other fee type code which is not available in the standard code set",
                                        "type": "array",
                                        "title": "OtherFeeType",
                                        "items": {
                                          "description": "Other fee type code which is not available in the standard code set",
                                          "type": "object",
                                          "properties": {
                                            "Code": {
                                              "description": "The four letter Mnemonic used within an XML file to identify a code",
                                              "title": "Code",
                                              "type": "string",
                                              "pattern": "^\\w{0,4}$",
                                              "minLength": 0,
                                              "maxLength": 4
                                            },
                                            "Name": {
                                              "description": "Long name associated with the code",
                                              "title": "Name",
                                              "type": "string",
                                              "minLength": 1,
                                              "maxLength": 70
                                            },
                                            "Description": {
                                              "description": "Description to describe the purpose of the code",
                                              "title": "Description",
                                              "type": "string",
                                              "minLength": 1,
                                              "maxLength": 350
                                            }
                                          },
                                          "required": [
                                            "Name",
                                            "Description"
                                          ]
                                        }
                                      }
                                    },
                                    "required": [
                                      "FeeType",
                                      "MinMaxType"
                                    ]
                                  }
                                },
                                "OtherFeeType": {
                                  "description": "Other Fee type which is not available in the standard code set",
                                  "type": "object",
                                  "title": "OtherFeeType",
                                  "properties": {
                                    "Code": {
                                      "description": "The four letter Mnemonic used within an XML file to identify a code",
                                      "title": "Code",
                                      "type": "string",
                                      "pattern": "^\\w{0,4}$",
                                      "minLength": 0,
                                      "maxLength": 4
                                    },
                                    "Name": {
                                      "description": "Long name associated with the code",
                                      "title": "Name",
                                      "type": "string",
                                      "minLength": 1,
                                      "maxLength": 70
                                    },
                                    "Description": {
                                      "description": "Description to describe the purpose of the code",
                                      "title": "Description",
                                      "type": "string",
                                      "minLength": 1,
                                      "maxLength": 350
                                    }
                                  },
                                  "additionalProperties": false,
                                  "required": [
                                    "Name",
                                    "Description"
                                  ]
                                },
                                "OtherFeeRateType": {
                                  "description": "Other fee rate type code which is not available in the standard code set",
                                  "type": "object",
                                  "title": "OtherFeeRateType",
                                  "properties": {
                                    "Code": {
                                      "description": "The four letter Mnemonic used within an XML file to identify a code",
                                      "title": "Code",
                                      "type": "string",
                                      "pattern": "^\\w{0,4}$",
                                      "minLength": 0,
                                      "maxLength": 4
                                    },
                                    "Name": {
                                      "description": "Long name associated with the code",
                                      "title": "Name",
                                      "type": "string",
                                      "minLength": 1,
                                      "maxLength": 70
                                    },
                                    "Description": {
                                      "description": "Description to describe the purpose of the code",
                                      "title": "Description",
                                      "type": "string",
                                      "minLength": 1,
                                      "maxLength": 350
                                    }
                                  },
                                  "additionalProperties": false,
                                  "required": [
                                    "Name",
                                    "Description"
                                  ]
                                },
                                "OtherApplicationFrequency": {
                                  "description": "Other application frequencies that are not available in the standard code list",
                                  "type": "object",
                                  "title": "OtherApplicationFrequency",
                                  "properties": {
                                    "Code": {
                                      "description": "The four letter Mnemonic used within an XML file to identify a code",
                                      "title": "Code",
                                      "type": "string",
                                      "pattern": "^\\w{0,4}$",
                                      "minLength": 0,
                                      "maxLength": 4
                                    },
                                    "Name": {
                                      "description": "Long name associated with the code",
                                      "title": "Name",
                                      "type": "string",
                                      "minLength": 1,
                                      "maxLength": 70
                                    },
                                    "Description": {
                                      "description": "Description to describe the purpose of the code",
                                      "title": "Description",
                                      "type": "string",
                                      "minLength": 1,
                                      "maxLength": 350
                                    }
                                  },
                                  "additionalProperties": false,
                                  "required": [
                                    "Name",
                                    "Description"
                                  ]
                                },
                                "OtherCalculationFrequency": {
                                  "description": "Other calculation frequency which is not available in the standard code set.",
                                  "type": "object",
                                  "title": "OtherCalculationFrequency",
                                  "properties": {
                                    "Code": {
                                      "description": "The four letter Mnemonic used within an XML file to identify a code",
                                      "title": "Code",
                                      "type": "string",
                                      "pattern": "^\\w{0,4}$",
                                      "minLength": 0,
                                      "maxLength": 4
                                    },
                                    "Name": {
                                      "description": "Long name associated with the code",
                                      "title": "Name",
                                      "type": "string",
                                      "minLength": 1,
                                      "maxLength": 70
                                    },
                                    "Description": {
                                      "description": "Description to describe the purpose of the code",
                                      "title": "Description",
                                      "type": "string",
                                      "minLength": 1,
                                      "maxLength": 350
                                    }
                                  },
                                  "additionalProperties": false,
                                  "required": [
                                    "Name",
                                    "Description"
                                  ]
                                }
                              },
                              "required": [
                                "FeeType",
                                "ApplicationFrequency"
                              ]
                            },
                            "minItems": 1
                          }
                        },
                        "required": [
                          "OverdraftFeeChargeDetail"
                        ]
                      }
                    }
                  },
                  "required": [
                    "TierValueMin"
                  ]
                },
                "minItems": 1
              },
              "OverdraftFeesCharges": {
                "description": "Overdraft fees and charges details",
                "type": "array",
                "title": "OverdraftFeesCharges",
                "items": {
                  "description": "Overdraft fees and charges details",
                  "type": "object",
                  "properties": {
                    "OverdraftFeeChargeCap": {
                      "description": "Details about any caps (maximum charges) that apply to a particular fee/charge. Capping can either be based on an amount (in gbp), an amount (in items) or a rate.",
                      "type": "array",
                      "title": "OverdraftFeeChargeCap",
                      "items": {
                        "description": "Details about any caps (maximum charges) that apply to a particular fee/charge. Capping can either be based on an amount (in gbp), an amount (in items) or a rate.",
                        "type": "object",
                        "properties": {
                          "FeeType": {
                            "description": "Fee/charge type which is being capped",
                            "title": "FeeType",
                            "type": "array",
                            "items": {
                              "description": "Overdraft fee type",
                              "type": "string",
                              "enum": [
                                "ArrangedOverdraft",
                                "AnnualReview",
                                "EmergencyBorrowing",
                                "BorrowingItem",
                                "OverdraftRenewal",
                                "OverdraftSetup",
                                "Surcharge",
                                "TempOverdraft",
                                "UnauthorisedBorrowing",
                                "UnauthorisedPaidTrans",
                                "Other",
                                "UnauthorisedUnpaidTrans"
                              ]
                            },
                            "minItems": 1
                          },
                          "MinMaxType": {
                            "description": "Min Max type",
                            "title": "MinMaxType",
                            "type": "string",
                            "enum": [
                              "Minimum",
                              "Maximum"
                            ]
                          },
                          "FeeCapOccurrence": {
                            "description": "Indicates whether the advertised overdraft rate is guaranteed to be offered to a borrower by the bank e.g. if it’s part of a government scheme, or whether the rate may vary dependent on the applicant’s circumstances.",
                            "title": "FeeCapOccurrence",
                            "type": "number",
                            "format": "float"
                          },
                          "FeeCapAmount": {
                            "description": "Cap amount charged for a fee/charge",
                            "title": "FeeCapAmount",
                            "type": "string",
                            "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                          },
                          "CappingPeriod": {
                            "description": "Period e.g. day, week, month etc. for which the fee/charge is capped",
                            "title": "CappingPeriod",
                            "type": "string",
                            "enum": [
                              "Day",
                              "Half Year",
                              "Month",
                              "Quarter",
                              "Week",
                              "Year"
                            ]
                          },
                          "Notes": {
                            "description": "Notes related to Overdraft fee charge cap",
                            "title": "Notes",
                            "type": "array",
                            "items": {
                              "description": "maxLength 2000 text",
                              "type": "string",
                              "minLength": 1,
                              "maxLength": 2000
                            }
                          },
                          "OtherFeeType": {
                            "description": "Other fee type code which is not available in the standard code set",
                            "type": "array",
                            "title": "OtherFeeType",
                            "items": {
                              "description": "Other fee type code which is not available in the standard code set",
                              "type": "object",
                              "properties": {
                                "Code": {
                                  "description": "The four letter Mnemonic used within an XML file to identify a code",
                                  "title": "Code",
                                  "type": "string",
                                  "pattern": "^\\w{0,4}$",
                                  "minLength": 0,
                                  "maxLength": 4
                                },
                                "Name": {
                                  "description": "Long name associated with the code",
                                  "title": "Name",
                                  "type": "string",
                                  "minLength": 1,
                                  "maxLength": 70
                                },
                                "Description": {
                                  "description": "Description to describe the purpose of the code",
                                  "title": "Description",
                                  "type": "string",
                                  "minLength": 1,
                                  "maxLength": 350
                                }
                              },
                              "required": [
                                "Name",
                                "Description"
                              ]
                            }
                          }
                        },
                        "required": [
                          "FeeType",
                          "MinMaxType"
                        ]
                      }
                    },
                    "OverdraftFeeChargeDetail": {
                      "description": "Details about the fees/charges",
                      "type": "array",
                      "title": "OverdraftFeeChargeDetail",
                      "items": {
                        "description": "Details about the fees/charges",
                        "type": "object",
                        "properties": {
                          "FeeType": {
                            "description": "Overdraft fee type",
                            "title": "FeeType",
                            "type": "string",
                            "enum": [
                              "ArrangedOverdraft",
                              "AnnualReview",
                              "EmergencyBorrowing",
                              "BorrowingItem",
                              "OverdraftRenewal",
                              "OverdraftSetup",
                              "Surcharge",
                              "TempOverdraft",
                              "UnauthorisedBorrowing",
                              "UnauthorisedPaidTrans",
                              "Other",
                              "UnauthorisedUnpaidTrans"
                            ]
                          },
                          "NegotiableIndicator": {
                            "description": "Indicates whether fee and charges are negotiable",
                            "title": "NegotiableIndicator",
                            "type": "boolean"
                          },
                          "OverdraftControlIndicator": {
                            "description": "Indicates if the fee/charge is already covered by an 'Overdraft Control' fee or not.",
                            "title": "OverdraftControlIndicator",
                            "type": "boolean"
                          },
                          "IncrementalBorrowingAmount": {
                            "description": "Every additional tranche of an overdraft balance to which an overdraft fee is applied",
                            "title": "IncrementalBorrowingAmount",
                            "type": "string",
                            "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                          },
                          "FeeAmount": {
                            "description": "Amount charged for an overdraft fee/charge (where it is charged in terms of an amount rather than a rate)",
                            "title": "FeeAmount",
                            "type": "string",
                            "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                          },
                          "FeeRate": {
                            "description": "Rate charged for overdraft fee/charge (where it is charged in terms of a rate rather than an amount)",
                            "title": "FeeRate",
                            "type": "string",
                            "pattern": "^(-?\\d{1,3}){1}(\\.\\d{1,4}){0,1}$"
                          },
                          "FeeRateType": {
                            "description": "Rate type for overdraft fee/charge (where it is charged in terms of a rate rather than an amount)",
                            "title": "FeeRateType",
                            "type": "string",
                            "enum": [
                              "Gross",
                              "Other"
                            ]
                          },
                          "ApplicationFrequency": {
                            "description": "Frequency at which the overdraft charge is applied to the account",
                            "title": "ApplicationFrequency",
                            "type": "string",
                            "enum": [
                              "OnClosing",
                              "OnOpening",
                              "ChargingPeriod",
                              "Daily",
                              "PerItem",
                              "Monthly",
                              "OnAnniversary",
                              "Other",
                              "PerHundredPounds",
                              "PerHour",
                              "PerOccurrence",
                              "PerSheet",
                              "PerTransaction",
                              "PerTransactionAmount",
                              "PerTransactionPercentage",
                              "Quarterly",
                              "SixMonthly",
                              "StatementMonthly",
                              "Weekly",
                              "Yearly"
                            ]
                          },
                          "CalculationFrequency": {
                            "description": "How often is the overdraft fee/charge calculated for the account.",
                            "title": "CalculationFrequency",
                            "type": "string",
                            "enum": [
                              "OnClosing",
                              "OnOpening",
                              "ChargingPeriod",
                              "Daily",
                              "PerItem",
                              "Monthly",
                              "OnAnniversary",
                              "Other",
                              "PerHundredPounds",
                              "PerHour",
                              "PerOccurrence",
                              "PerSheet",
                              "PerTransaction",
                              "PerTransactionAmount",
                              "PerTransactionPercentage",
                              "Quarterly",
                              "SixMonthly",
                              "StatementMonthly",
                              "Weekly",
                              "Yearly"
                            ]
                          },
                          "Notes": {
                            "description": "Free text for capturing any other info related to Overdraft Fees Charge Details",
                            "title": "Notes",
                            "type": "array",
                            "items": {
                              "description": "maxLength 2000 text",
                              "type": "string",
                              "minLength": 1,
                              "maxLength": 2000
                            }
                          },
                          "OverdraftFeeChargeCap": {
                            "description": "Details about any caps (maximum charges) that apply to a particular fee/charge. Capping can either be based on an amount (in gbp), an amount (in items) or a rate.",
                            "type": "array",
                            "title": "OverdraftFeeChargeCap",
                            "items": {
                              "description": "Details about any caps (maximum charges) that apply to a particular fee/charge. Capping can either be based on an amount (in gbp), an amount (in items) or a rate.",
                              "type": "object",
                              "properties": {
                                "FeeType": {
                                  "description": "Fee/charge type which is being capped",
                                  "title": "FeeType",
                                  "type": "array",
                                  "items": {
                                    "description": "Overdraft fee type",
                                    "type": "string",
                                    "enum": [
                                      "ArrangedOverdraft",
                                      "AnnualReview",
                                      "EmergencyBorrowing",
                                      "BorrowingItem",
                                      "OverdraftRenewal",
                                      "OverdraftSetup",
                                      "Surcharge",
                                      "TempOverdraft",
                                      "UnauthorisedBorrowing",
                                      "UnauthorisedPaidTrans",
                                      "Other",
                                      "UnauthorisedUnpaidTrans"
                                    ]
                                  },
                                  "minItems": 1
                                },
                                "MinMaxType": {
                                  "description": "Min Max type",
                                  "title": "MinMaxType",
                                  "type": "string",
                                  "enum": [
                                    "Minimum",
                                    "Maximum"
                                  ]
                                },
                                "FeeCapOccurrence": {
                                  "description": "Indicates whether the advertised overdraft rate is guaranteed to be offered to a borrower by the bank e.g. if it’s part of a government scheme, or whether the rate may vary dependent on the applicant’s circumstances.",
                                  "title": "FeeCapOccurrence",
                                  "type": "number",
                                  "format": "float"
                                },
                                "FeeCapAmount": {
                                  "description": "Cap amount charged for a fee/charge",
                                  "title": "FeeCapAmount",
                                  "type": "string",
                                  "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                                },
                                "CappingPeriod": {
                                  "description": "Period e.g. day, week, month etc. for which the fee/charge is capped",
                                  "title": "CappingPeriod",
                                  "type": "string",
                                  "enum": [
                                    "Day",
                                    "Half Year",
                                    "Month",
                                    "Quarter",
                                    "Week",
                                    "Year"
                                  ]
                                },
                                "Notes": {
                                  "description": "Notes related to Overdraft fee charge cap",
                                  "title": "Notes",
                                  "type": "array",
                                  "items": {
                                    "description": "maxLength 2000 text",
                                    "type": "string",
                                    "minLength": 1,
                                    "maxLength": 2000
                                  }
                                },
                                "OtherFeeType": {
                                  "description": "Other fee type code which is not available in the standard code set",
                                  "type": "array",
                                  "title": "OtherFeeType",
                                  "items": {
                                    "description": "Other fee type code which is not available in the standard code set",
                                    "type": "object",
                                    "properties": {
                                      "Code": {
                                        "description": "The four letter Mnemonic used within an XML file to identify a code",
                                        "title": "Code",
                                        "type": "string",
                                        "pattern": "^\\w{0,4}$",
                                        "minLength": 0,
                                        "maxLength": 4
                                      },
                                      "Name": {
                                        "description": "Long name associated with the code",
                                        "title": "Name",
                                        "type": "string",
                                        "minLength": 1,
                                        "maxLength": 70
                                      },
                                      "Description": {
                                        "description": "Description to describe the purpose of the code",
                                        "title": "Description",
                                        "type": "string",
                                        "minLength": 1,
                                        "maxLength": 350
                                      }
                                    },
                                    "required": [
                                      "Name",
                                      "Description"
                                    ]
                                  }
                                }
                              },
                              "required": [
                                "FeeType",
                                "MinMaxType"
                              ]
                            }
                          },
                          "OtherFeeType": {
                            "description": "Other Fee type which is not available in the standard code set",
                            "type": "object",
                            "title": "OtherFeeType",
                            "properties": {
                              "Code": {
                                "description": "The four letter Mnemonic used within an XML file to identify a code",
                                "title": "Code",
                                "type": "string",
                                "pattern": "^\\w{0,4}$",
                                "minLength": 0,
                                "maxLength": 4
                              },
                              "Name": {
                                "description": "Long name associated with the code",
                                "title": "Name",
                                "type": "string",
                                "minLength": 1,
                                "maxLength": 70
                              },
                              "Description": {
                                "description": "Description to describe the purpose of the code",
                                "title": "Description",
                                "type": "string",
                                "minLength": 1,
                                "maxLength": 350
                              }
                            },
                            "additionalProperties": false,
                            "required": [
                              "Name",
                              "Description"
                            ]
                          },
                          "OtherFeeRateType": {
                            "description": "Other fee rate type code which is not available in the standard code set",
                            "type": "object",
                            "title": "OtherFeeRateType",
                            "properties": {
                              "Code": {
                                "description": "The four letter Mnemonic used within an XML file to identify a code",
                                "title": "Code",
                                "type": "string",
                                "pattern": "^\\w{0,4}$",
                                "minLength": 0,
                                "maxLength": 4
                              },
                              "Name": {
                                "description": "Long name associated with the code",
                                "title": "Name",
                                "type": "string",
                                "minLength": 1,
                                "maxLength": 70
                              },
                              "Description": {
                                "description": "Description to describe the purpose of the code",
                                "title": "Description",
                                "type": "string",
                                "minLength": 1,
                                "maxLength": 350
                              }
                            },
                            "additionalProperties": false,
                            "required": [
                              "Name",
                              "Description"
                            ]
                          },
                          "OtherApplicationFrequency": {
                            "description": "Other application frequencies that are not available in the standard code list",
                            "type": "object",
                            "title": "OtherApplicationFrequency",
                            "properties": {
                              "Code": {
                                "description": "The four letter Mnemonic used within an XML file to identify a code",
                                "title": "Code",
                                "type": "string",
                                "pattern": "^\\w{0,4}$",
                                "minLength": 0,
                                "maxLength": 4
                              },
                              "Name": {
                                "description": "Long name associated with the code",
                                "title": "Name",
                                "type": "string",
                                "minLength": 1,
                                "maxLength": 70
                              },
                              "Description": {
                                "description": "Description to describe the purpose of the code",
                                "title": "Description",
                                "type": "string",
                                "minLength": 1,
                                "maxLength": 350
                              }
                            },
                            "additionalProperties": false,
                            "required": [
                              "Name",
                              "Description"
                            ]
                          },
                          "OtherCalculationFrequency": {
                            "description": "Other calculation frequency which is not available in the standard code set.",
                            "type": "object",
                            "title": "OtherCalculationFrequency",
                            "properties": {
                              "Code": {
                                "description": "The four letter Mnemonic used within an XML file to identify a code",
                                "title": "Code",
                                "type": "string",
                                "pattern": "^\\w{0,4}$",
                                "minLength": 0,
                                "maxLength": 4
                              },
                              "Name": {
                                "description": "Long name associated with the code",
                                "title": "Name",
                                "type": "string",
                                "minLength": 1,
                                "maxLength": 70
                              },
                              "Description": {
                                "description": "Description to describe the purpose of the code",
                                "title": "Description",
                                "type": "string",
                                "minLength": 1,
                                "maxLength": 350
                              }
                            },
                            "additionalProperties": false,
                            "required": [
                              "Name",
                              "Description"
                            ]
                          }
                        },
                        "required": [
                          "FeeType",
                          "ApplicationFrequency"
                        ]
                      },
                      "minItems": 1
                    }
                  },
                  "required": [
                    "OverdraftFeeChargeDetail"
                  ]
                }
              }
            },
            "required": [
              "TierBandMethod",
              "OverdraftTierBand"
            ]
          },
          "minItems": 1
        }
      },
      "additionalProperties": false,
      "required": [
        "OverdraftTierBandSet"
      ]
    },
    "OtherFeesCharges": {
      "description": "Contains details of fees and charges which are not associated with either Overdraft or features/benefits",
      "type": "array",
      "title": "OtherFeesCharges",
      "items": {
        "description": "Contains details of fees and charges which are not associated with either Overdraft or features/benefits",
        "type": "object",
        "properties": {
          "TariffType": {
            "description": "TariffType which defines the fee and charges.",
            "title": "TariffType",
            "type": "string",
            "enum": [
              "Electronic",
              "Mixed",
              "Other"
            ]
          },
          "TariffName": {
            "description": "Name of the tariff",
            "title": "TariffName",
            "type": "string",
            "minLength": 1,
            "maxLength": 350
          },
          "OtherTariffType": {
            "description": "Other tariff type which is not in the standard list.",
            "type": "object",
            "title": "OtherTariffType",
            "properties": {
              "Code": {
                "description": "The four letter Mnemonic used within an XML file to identify a code",
                "title": "Code",
                "type": "string",
                "pattern": "^\\w{0,4}$",
                "minLength": 0,
                "maxLength": 4
              },
              "Name": {
                "description": "Long name associated with the code",
                "title": "Name",
                "type": "string",
                "minLength": 1,
                "maxLength": 70
              },
              "Description": {
                "description": "Description to describe the purpose of the code",
                "title": "Description",
                "type": "string",
                "minLength": 1,
                "maxLength": 350
              }
            },
            "additionalProperties": false,
            "required": [
              "Name",
              "Description"
            ]
          },
          "FeeChargeDetail": {
            "description": "Other fees/charges details",
            "type": "array",
            "title": "FeeChargeDetail",
            "items": {
              "description": "Other fees/charges details",
              "type": "object",
              "properties": {
                "FeeCategory": {
                  "description": "Categorisation of fees and charges into standard categories.",
                  "title": "FeeCategory",
                  "type": "string",
                  "enum": [
                    "Other",
                    "Servicing"
                  ]
                },
                "FeeType": {
                  "description": "Fee/Charge Type",
                  "title": "FeeType",
                  "type": "string",
                  "enum": [
                    "Other",
                    "ServiceCAccountFee",
                    "ServiceCAccountFeeMonthly",
                    "ServiceCAccountFeeQuarterly",
                    "ServiceCFixedTariff",
                    "ServiceCBusiDepAccBreakage",
                    "ServiceCMinimumMonthlyFee",
                    "ServiceCOther"
                  ]
                },
                "NegotiableIndicator": {
                  "description": "Fee/charge which is usually negotiable rather than a fixed amount",
                  "title": "NegotiableIndicator",
                  "type": "boolean"
                },
                "FeeAmount": {
                  "description": "Fee Amount charged for a fee/charge (where it is charged in terms of an amount rather than a rate)",
                  "title": "FeeAmount",
                  "type": "string",
                  "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                },
                "FeeRate": {
                  "description": "Rate charged for Fee/Charge (where it is charged in terms of a rate rather than an amount)",
                  "title": "FeeRate",
                  "type": "string",
                  "pattern": "^(-?\\d{1,3}){1}(\\.\\d{1,4}){0,1}$"
                },
                "FeeRateType": {
                  "description": "Rate type for Fee/Charge (where it is charged in terms of a rate rather than an amount)",
                  "title": "FeeRateType",
                  "type": "string",
                  "enum": [
                    "Gross",
                    "Other"
                  ]
                },
                "ApplicationFrequency": {
                  "description": "How frequently the fee/charge is applied to the account",
                  "title": "ApplicationFrequency",
                  "type": "string",
                  "enum": [
                    "OnClosing",
                    "OnOpening",
                    "ChargingPeriod",
                    "Daily",
                    "PerItem",
                    "Monthly",
                    "OnAnniversary",
                    "Other",
                    "PerHundredPounds",
                    "PerHour",
                    "PerOccurrence",
                    "PerSheet",
                    "PerTransaction",
                    "PerTransactionAmount",
                    "PerTransactionPercentage",
                    "Quarterly",
                    "SixMonthly",
                    "StatementMonthly",
                    "Weekly",
                    "Yearly"
                  ]
                },
                "CalculationFrequency": {
                  "description": "How frequently the fee/charge is calculated",
                  "title": "CalculationFrequency",
                  "type": "string",
                  "enum": [
                    "OnClosing",
                    "OnOpening",
                    "ChargingPeriod",
                    "Daily",
                    "PerItem",
                    "Monthly",
                    "OnAnniversary",
                    "Other",
                    "PerHundredPounds",
                    "PerHour",
                    "PerOccurrence",
                    "PerSheet",
                    "PerTransaction",
                    "PerTransactionAmount",
                    "PerTransactionPercentage",
                    "Quarterly",
                    "SixMonthly",
                    "StatementMonthly",
                    "Weekly",
                    "Yearly"
                  ]
                },
                "Notes": {
                  "description": "Optional additional notes to supplement the fee/charge details.",
                  "title": "Notes",
                  "type": "array",
                  "items": {
                    "description": "maxLength 2000 text",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 2000
                  }
                },
                "FeeChargeCap": {
                  "description": "Details about any caps (maximum charges) that apply to a particular or group of fee/charge",
                  "type": "array",
                  "title": "FeeChargeCap",
                  "items": {
                    "description": "Details about any caps (maximum charges) that apply to a particular or group of fee/charge",
                    "type": "object",
                    "properties": {
                      "FeeType": {
                        "description": "Fee/charge type which is being capped",
                        "title": "FeeType",
                        "type": "array",
                        "items": {
                          "description": "Fee/charge type which is being capped",
                          "type": "string",
                          "enum": [
                            "Other",
                            "ServiceCAccountFee",
                            "ServiceCAccountFeeMonthly",
                            "ServiceCAccountFeeQuarterly",
                            "ServiceCFixedTariff",
                            "ServiceCBusiDepAccBreakage",
                            "ServiceCMinimumMonthlyFee",
                            "ServiceCOther"
                          ]
                        },
                        "minItems": 1
                      },
                      "MinMaxType": {
                        "description": "Min Max type",
                        "title": "MinMaxType",
                        "type": "string",
                        "enum": [
                          "Minimum",
                          "Maximum"
                        ]
                      },
                      "FeeCapOccurrence": {
                        "description": "fee/charges are captured dependent on the number of occurrences rather than capped at a particular amount",
                        "title": "FeeCapOccurrence",
                        "type": "number",
                        "format": "float"
                      },
                      "FeeCapAmount": {
                        "description": "Cap amount charged for a fee/charge (where it is charged in terms of an amount rather than a rate)",
                        "title": "FeeCapAmount",
                        "type": "string",
                        "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                      },
                      "CappingPeriod": {
                        "description": "Period e.g. day, week, month etc. for which the fee/charge is capped",
                        "title": "CappingPeriod",
                        "type": "string",
                        "enum": [
                          "Day",
                          "Half Year",
                          "Month",
                          "Quarter",
                          "Week",
                          "Year"
                        ]
                      },
                      "Notes": {
                        "description": "Free text for adding  extra details for fee charge cap",
                        "title": "Notes",
                        "type": "array",
                        "items": {
                          "description": "maxLength 2000 text",
                          "type": "string",
                          "minLength": 1,
                          "maxLength": 2000
                        }
                      },
                      "OtherFeeType": {
                        "description": "Other fee type code which is not available in the standard code set",
                        "type": "array",
                        "title": "OtherFeeType",
                        "items": {
                          "description": "Other fee type code which is not available in the standard code set",
                          "type": "object",
                          "properties": {
                            "Code": {
                              "description": "The four letter Mnemonic used within an XML file to identify a code",
                              "title": "Code",
                              "type": "string",
                              "pattern": "^\\w{0,4}$",
                              "minLength": 0,
                              "maxLength": 4
                            },
                            "Name": {
                              "description": "Long name associated with the code",
                              "title": "Name",
                              "type": "string",
                              "minLength": 1,
                              "maxLength": 70
                            },
                            "Description": {
                              "description": "Description to describe the purpose of the code",
                              "title": "Description",
                              "type": "string",
                              "minLength": 1,
                              "maxLength": 350
                            }
                          },
                          "required": [
                            "Name",
                            "Description"
                          ]
                        }
                      }
                    },
                    "required": [
                      "FeeType",
                      "MinMaxType"
                    ]
                  }
                },
                "OtherFeeCategoryType": {
                  "type": "object",
                  "title": "OtherFeeCategoryType",
                  "properties": {
                    "Code": {
                      "description": "The four letter Mnemonic used within an XML file to identify a code",
                      "title": "Code",
                      "type": "string",
                      "pattern": "^\\w{0,4}$",
                      "minLength": 0,
                      "maxLength": 4
                    },
                    "Name": {
                      "description": "Long name associated with the code",
                      "title": "Name",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 70
                    },
                    "Description": {
                      "description": "Description to describe the purpose of the code",
                      "title": "Description",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 350
                    }
                  },
                  "additionalProperties": false,
                  "required": [
                    "Name",
                    "Description"
                  ]
                },
                "OtherFeeType": {
                  "description": "Other Fee/charge type which is not available in the standard code set",
                  "type": "object",
                  "title": "OtherFeeType",
                  "properties": {
                    "Code": {
                      "description": "The four letter Mnemonic used within an XML file to identify a code",
                      "title": "Code",
                      "type": "string",
                      "pattern": "^\\w{0,4}$",
                      "minLength": 0,
                      "maxLength": 4
                    },
                    "FeeCategory": {
                      "description": "Categorisation of fees and charges into standard categories.",
                      "title": "FeeCategory",
                      "type": "string",
                      "enum": [
                        "Other",
                        "Servicing"
                      ]
                    },
                    "Name": {
                      "description": "Long name associated with the code",
                      "title": "Name",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 70
                    },
                    "Description": {
                      "description": "Description to describe the purpose of the code",
                      "title": "Description",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 350
                    }
                  },
                  "additionalProperties": false,
                  "required": [
                    "FeeCategory",
                    "Name",
                    "Description"
                  ]
                },
                "OtherFeeRateType": {
                  "description": "Other fee rate type which is not available in the standard code set",
                  "type": "object",
                  "title": "OtherFeeRateType",
                  "properties": {
                    "Code": {
                      "description": "The four letter Mnemonic used within an XML file to identify a code",
                      "title": "Code",
                      "type": "string",
                      "pattern": "^\\w{0,4}$",
                      "minLength": 0,
                      "maxLength": 4
                    },
                    "Name": {
                      "description": "Long name associated with the code",
                      "title": "Name",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 70
                    },
                    "Description": {
                      "description": "Description to describe the purpose of the code",
                      "title": "Description",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 350
                    }
                  },
                  "additionalProperties": false,
                  "required": [
                    "Name",
                    "Description"
                  ]
                },
                "OtherApplicationFrequency": {
                  "description": "Other application frequencies not covered in the standard code list",
                  "type": "object",
                  "title": "OtherApplicationFrequency",
                  "properties": {
                    "Code": {
                      "description": "The four letter Mnemonic used within an XML file to identify a code",
                      "title": "Code",
                      "type": "string",
                      "pattern": "^\\w{0,4}$",
                      "minLength": 0,
                      "maxLength": 4
                    },
                    "Name": {
                      "description": "Long name associated with the code",
                      "title": "Name",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 70
                    },
                    "Description": {
                      "description": "Description to describe the purpose of the code",
                      "title": "Description",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 350
                    }
                  },
                  "additionalProperties": false,
                  "required": [
                    "Name",
                    "Description"
                  ]
                },
                "OtherCalculationFrequency": {
                  "description": "Other calculation frequency which is not available in standard code set.",
                  "type": "object",
                  "title": "OtherCalculationFrequency",
                  "properties": {
                    "Code": {
                      "description": "The four letter Mnemonic used within an XML file to identify a code",
                      "title": "Code",
                      "type": "string",
                      "pattern": "^\\w{0,4}$",
                      "minLength": 0,
                      "maxLength": 4
                    },
                    "Name": {
                      "description": "Long name associated with the code",
                      "title": "Name",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 70
                    },
                    "Description": {
                      "description": "Description to describe the purpose of the code",
                      "title": "Description",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 350
                    }
                  },
                  "additionalProperties": false,
                  "required": [
                    "Name",
                    "Description"
                  ]
                },
                "FeeApplicableRange": {
                  "description": "Range or amounts or rates for which the fee/charge applies",
                  "type": "object",
                  "title": "FeeApplicableRange",
                  "properties": {
                    "MinimumAmount": {
                      "description": "Minimum Amount on which fee/charge is applicable (where it is expressed as an amount)",
                      "title": "MinimumAmount",
                      "type": "string",
                      "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                    },
                    "MaximumAmount": {
                      "description": "Maximum Amount on which fee is applicable (where it is expressed as an amount)",
                      "title": "MaximumAmount",
                      "type": "string",
                      "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                    },
                    "MinimumRate": {
                      "description": "Minimum rate on which fee/charge is applicable(where it is expressed as an rate)",
                      "title": "MinimumRate",
                      "type": "string",
                      "pattern": "^(-?\\d{1,3}){1}(\\.\\d{1,4}){0,1}$"
                    },
                    "MaximumRate": {
                      "description": "Maximum rate on which fee/charge is applicable(where it is expressed as an rate)",
                      "title": "MaximumRate",
                      "type": "string",
                      "pattern": "^(-?\\d{1,3}){1}(\\.\\d{1,4}){0,1}$"
                    }
                  },
                  "additionalProperties": false
                }
              },
              "required": [
                "FeeCategory",
                "FeeType",
                "ApplicationFrequency"
              ]
            },
            "minItems": 1
          },
          "FeeChargeCap": {
            "description": "Details about any caps (maximum charges) that apply to a particular or group of fee/charge",
            "type": "array",
            "title": "FeeChargeCap",
            "items": {
              "description": "Details about any caps (maximum charges) that apply to a particular or group of fee/charge",
              "type": "object",
              "properties": {
                "FeeType": {
                  "description": "Fee/charge type which is being capped",
                  "title": "FeeType",
                  "type": "array",
                  "items": {
                    "description": "Fee/charge type which is being capped",
                    "type": "string",
                    "enum": [
                      "Other",
                      "ServiceCAccountFee",
                      "ServiceCAccountFeeMonthly",
                      "ServiceCAccountFeeQuarterly",
                      "ServiceCFixedTariff",
                      "ServiceCBusiDepAccBreakage",
                      "ServiceCMinimumMonthlyFee",
                      "ServiceCOther"
                    ]
                  },
                  "minItems": 1
                },
                "MinMaxType": {
                  "description": "Min Max type",
                  "title": "MinMaxType",
                  "type": "string",
                  "enum": [
                    "Minimum",
                    "Maximum"
                  ]
                },
                "FeeCapOccurrence": {
                  "description": "fee/charges are captured dependent on the number of occurrences rather than capped at a particular amount",
                  "title": "FeeCapOccurrence",
                  "type": "number",
                  "format": "float"
                },
                "FeeCapAmount": {
                  "description": "Cap amount charged for a fee/charge (where it is charged in terms of an amount rather than a rate)",
                  "title": "FeeCapAmount",
                  "type": "string",
                  "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                },
                "CappingPeriod": {
                  "description": "Period e.g. day, week, month etc. for which the fee/charge is capped",
                  "title": "CappingPeriod",
                  "type": "string",
                  "enum": [
                    "Day",
                    "Half Year",
                    "Month",
                    "Quarter",
                    "Week",
                    "Year"
                  ]
                },
                "Notes": {
                  "description": "Free text for adding  extra details for fee charge cap",
                  "title": "Notes",
                  "type": "array",
                  "items": {
                    "description": "maxLength 2000 text",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 2000
                  }
                },
                "OtherFeeType": {
                  "description": "Other fee type code which is not available in the standard code set",
                  "type": "array",
                  "title": "OtherFeeType",
                  "items": {
                    "description": "Other fee type code which is not available in the standard code set",
                    "type": "object",
                    "properties": {
                      "Code": {
                        "description": "The four letter Mnemonic used within an XML file to identify a code",
                        "title": "Code",
                        "type": "string",
                        "pattern": "^\\w{0,4}$",
                        "minLength": 0,
                        "maxLength": 4
                      },
                      "Name": {
                        "description": "Long name associated with the code",
                        "title": "Name",
                        "type": "string",
                        "minLength": 1,
                        "maxLength": 70
                      },
                      "Description": {
                        "description": "Description to describe the purpose of the code",
                        "title": "Description",
                        "type": "string",
                        "minLength": 1,
                        "maxLength": 350
                      }
                    },
                    "required": [
                      "Name",
                      "Description"
                    ]
                  }
                }
              },
              "required": [
                "FeeType",
                "MinMaxType"
              ]
            }
          }
        },
        "required": [
          "FeeChargeDetail"
        ]
      }
    }
  },
  "additionalProperties": false
}

```

BCA

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|ProductDetails|object|false|none|none|
|» Segment|[string]|false|none|Market segmentation is a marketing term referring to the aggregating of prospective buyers into groups, or segments, that have common needs and respond similarly to a marketing action. Market segmentation enables companies to target different categories of consumers who perceive the full value of certain products and services differently from one another.<br><br>Read more: Market Segmentation http://www.investopedia.com/terms/m/marketsegmentation.asp#ixzz4gfEEalTd <br>With respect to BCA products, they are segmented in relation to different markets that they wish to focus on.|
|» FeeFreeLength|number(float)|false|none|The length/duration of the fee free period|
|» FeeFreeLengthPeriod|string|false|none|The unit of period (days, weeks, months etc.) of the promotional length|
|» Notes|[string]|false|none|Optional additional notes to supplement the Core product details|
|CreditInterest|object|false|none|Details about the interest that may be payable to the BCA account holders|
|» TierBandSet|[object]|true|none|The group of tiers or bands for which credit interest can be applied.|
|»» TierBandMethod|string|true|none|The methodology of how credit interest is paid/applied. It can be:-<br><br>1. Banded<br>Interest rates are banded. i.e. Increasing rate on whole balance as balance increases.<br><br>2. Tiered<br>Interest rates are tiered. i.e. increasing rate for each tier as balance increases, but interest paid on tier fixed for that tier and not on whole balance.<br><br>3. Whole<br>The same interest rate is applied irrespective of the BCA balance|
|»» CalculationMethod|string|false|none|Methods of calculating interest|
|»» Destination|string|true|none|Describes whether accrued interest is payable only to the BCA or to another bank account|
|»» Notes|[string]|false|none|Optional additional notes to supplement the Tier Band Set details|
|»» TierBand|[object]|true|none|Tier Band Details|
|»»» Identification|string|false|none|Unique and unambiguous identification of a  Tier Band for a BCA.|
|»»» TierValueMinimum|string|true|none|Minimum deposit value for which the credit interest tier applies.|
|»»» TierValueMaximum|string|false|none|Maximum deposit value for which the credit interest tier applies.|
|»»» CalculationFrequency|string|false|none|How often is credit interest calculated for the account.|
|»»» ApplicationFrequency|string|true|none|How often is interest applied to the BCA for this tier/band i.e. how often the financial institution pays accumulated interest to the customer's BCA.|
|»»» DepositInterestAppliedCoverage|string|false|none|Amount on which Interest applied.|
|»»» FixedVariableInterestRateType|string|true|none|Type of interest rate, Fixed or Variable|
|»»» AER|string|true|none|The annual equivalent rate (AER) is interest that is calculated under the assumption that any interest paid is combined with the original balance and the next interest payment will be based on the slightly higher account balance. Overall, this means that interest can be compounded several times in a year depending on the number of times that interest payments are made. <br><br>Read more: Annual Equivalent Rate (AER) http://www.investopedia.com/terms/a/aer.asp#ixzz4gfR7IO1A|
|»»» BankInterestRateType|string|false|none|Interest rate types, other than AER, which financial institutions may use to describe the annual interest rate payable to the BCA.|
|»»» BankInterestRate|string|false|none|Bank Interest for the BCA product|
|»»» Notes|[string]|false|none|Optional additional notes to supplement the Tier Band details|
|»»» OtherBankInterestType|object|false|none|Other interest rate types which are not available in the standard code list|
|»»»» Code|string|false|none|The four letter Mnemonic used within an XML file to identify a code|
|»»»» Name|string|true|none|Long name associated with the code|
|»»»» Description|string|true|none|Description to describe the purpose of the code|
|»»» OtherApplicationFrequency|object|false|none|Other application frequencies that are not available in the standard code list|
|»»»» Code|string|false|none|The four letter Mnemonic used within an XML file to identify a code|
|»»»» Name|string|true|none|Long name associated with the code|
|»»»» Description|string|true|none|Description to describe the purpose of the code|
|»»» OtherCalculationFrequency|object|false|none|Other calculation frequency which is not available in the standard code set.|
|»»»» Code|string|false|none|The four letter Mnemonic used within an XML file to identify a code|
|»»»» Name|string|true|none|Long name associated with the code|
|»»»» Description|string|true|none|Description to describe the purpose of the code|
|Overdraft|object|false|none|Borrowing details|
|» Notes|[string]|false|none|Associated Notes about the overdraft rates|
|» OverdraftTierBandSet|[object]|true|none|Tier band set details|
|»» TierBandMethod|string|true|none|The methodology of how overdraft is charged. It can be:<br>'Whole'  Where the same charge/rate is applied to the entirety of the overdraft balance (where charges are applicable). <br>'Tiered' Where different charges/rates are applied dependent on overdraft maximum and minimum balance amount tiers defined by the lending financial organisation<br>'Banded' Where different charges/rates are applied dependent on overdraft maximum and minimum balance amount bands defined by a government organisation.|
|»» OverdraftType|string|false|none|An overdraft can either be 'committed' which means that the facility cannot be withdrawn without reasonable notification before it's agreed end date, or 'on demand' which means that the financial institution can demand repayment at any point in time.|
|»» Identification|string|false|none|Unique and unambiguous identification of a  Tier Band for a overdraft product.|
|»» AuthorisedIndicator|boolean|false|none|Indicates if the Overdraft is authorised (Y) or unauthorised (N)|
|»» BufferAmount|string|false|none|When a customer exceeds their credit limit, a financial institution will not charge the customer unauthorised overdraft charges if they do not exceed by more than the buffer amount. Note: Authorised overdraft charges may still apply.|
|»» Notes|[string]|false|none|Optional additional notes to supplement the overdraft Tier Band Set details|
|»» OverdraftTierBand|[object]|true|none|Provides overdraft details for a specific tier or band|
|»»» Identification|string|false|none|Unique and unambiguous identification of a  Tier Band for a overdraft.|
|»»» TierValueMin|string|true|none|Minimum value of Overdraft Tier/Band|
|»»» TierValueMax|string|false|none|Maximum value of Overdraft Tier/Band|
|»»» EAR|string|false|none|EAR means Effective Annual Rate and/or Equivalent Annual Rate (frequently<br>used interchangeably), being the actual annual interest rate of an Overdraft.|
|»»» RepresentativeAPR|string|false|none|An annual percentage rate (APR) is the annual rate charged for borrowing or earned through an investment. APR is expressed as a percentage that represents the actual yearly cost of funds over the term of a loan. This includes any fees or additional costs associated with the transaction but does not take compounding into account.|
|»»» AgreementLengthMin|number(float)|false|none|Specifies the minimum length of a band for a fixed overdraft agreement|
|»»» AgreementLengthMax|number(float)|false|none|Specifies the maximum length of a band for a fixed overdraft agreement|
|»»» AgreementPeriod|string|false|none|Specifies the period of a fixed length overdraft agreement|
|»»» OverdraftInterestChargingCoverage|string|false|none|Refers to which interest rate is applied when interests are tiered. For example, if an overdraft balance is £2k and the interest tiers are:- 0-£500 0.1%, 500-1000 0.2%, 1000-10000 0.5%, then the applicable interest rate could either be 0.5% of the entire balance (since the account balance sits in the top interest tier) or (0.1%*500)+(0.2%*500)+(0.5%*1000). In the 1st situation, we say the interest is applied to the ‘Whole’ of the account balance,  and in the 2nd that it is ‘Tiered’.|
|»»» BankGuaranteedIndicator|boolean|false|none|Indicates whether the advertised overdraft rate is guaranteed to be offered to a borrower by the bank e.g. if it’s part of a government scheme, or whether the rate may vary dependent on the applicant’s circumstances.|
|»»» Notes|[string]|false|none|Optional additional notes to supplement the Tier/band details|
|»»» OverdraftFeesCharges|[object]|false|none|Overdraft fees and charges|
|»»»» OverdraftFeeChargeCap|[object]|false|none|Details about any caps (maximum charges) that apply to a particular fee/charge. Capping can either be based on an amount (in gbp), an amount (in items) or a rate.|
|»»»»» FeeType|[string]|true|none|Fee/charge type which is being capped|
|»»»»» MinMaxType|string|true|none|Min Max type|
|»»»»» FeeCapOccurrence|number(float)|false|none|Indicates whether the advertised overdraft rate is guaranteed to be offered to a borrower by the bank e.g. if it’s part of a government scheme, or whether the rate may vary dependent on the applicant’s circumstances.|
|»»»»» FeeCapAmount|string|false|none|Cap amount charged for a fee/charge|
|»»»»» CappingPeriod|string|false|none|Period e.g. day, week, month etc. for which the fee/charge is capped|
|»»»»» Notes|[string]|false|none|Notes related to Overdraft fee charge cap|
|»»»»» OtherFeeType|[object]|false|none|Other fee type code which is not available in the standard code set|
|»»»»»» Code|string|false|none|The four letter Mnemonic used within an XML file to identify a code|
|»»»»»» Name|string|true|none|Long name associated with the code|
|»»»»»» Description|string|true|none|Description to describe the purpose of the code|
|»»»» OverdraftFeeChargeDetail|[object]|true|none|Details about the fees/charges|
|»»»»» FeeType|string|true|none|Overdraft fee type|
|»»»»» NegotiableIndicator|boolean|false|none|Indicates whether fee and charges are negotiable|
|»»»»» OverdraftControlIndicator|boolean|false|none|Indicates if the fee/charge is already covered by an 'Overdraft Control' fee or not.|
|»»»»» IncrementalBorrowingAmount|string|false|none|Every additional tranche of an overdraft balance to which an overdraft fee is applied|
|»»»»» FeeAmount|string|false|none|Amount charged for an overdraft fee/charge (where it is charged in terms of an amount rather than a rate)|
|»»»»» FeeRate|string|false|none|Rate charged for overdraft fee/charge (where it is charged in terms of a rate rather than an amount)|
|»»»»» FeeRateType|string|false|none|Rate type for overdraft fee/charge (where it is charged in terms of a rate rather than an amount)|
|»»»»» ApplicationFrequency|string|true|none|Frequency at which the overdraft charge is applied to the account|
|»»»»» CalculationFrequency|string|false|none|How often is the overdraft fee/charge calculated for the account.|
|»»»»» Notes|[string]|false|none|Free text for capturing any other info related to Overdraft Fees Charge Details|
|»»»»» OverdraftFeeChargeCap|[object]|false|none|Details about any caps (maximum charges) that apply to a particular fee/charge. Capping can either be based on an amount (in gbp), an amount (in items) or a rate.|
|»»»»»» FeeType|[string]|true|none|Fee/charge type which is being capped|
|»»»»»» MinMaxType|string|true|none|Min Max type|
|»»»»»» FeeCapOccurrence|number(float)|false|none|Indicates whether the advertised overdraft rate is guaranteed to be offered to a borrower by the bank e.g. if it’s part of a government scheme, or whether the rate may vary dependent on the applicant’s circumstances.|
|»»»»»» FeeCapAmount|string|false|none|Cap amount charged for a fee/charge|
|»»»»»» CappingPeriod|string|false|none|Period e.g. day, week, month etc. for which the fee/charge is capped|
|»»»»»» Notes|[string]|false|none|Notes related to Overdraft fee charge cap|
|»»»»»» OtherFeeType|[object]|false|none|Other fee type code which is not available in the standard code set|
|»»»»»»» Code|string|false|none|The four letter Mnemonic used within an XML file to identify a code|
|»»»»»»» Name|string|true|none|Long name associated with the code|
|»»»»»»» Description|string|true|none|Description to describe the purpose of the code|
|»»»»» OtherFeeType|object|false|none|Other Fee type which is not available in the standard code set|
|»»»»»» Code|string|false|none|The four letter Mnemonic used within an XML file to identify a code|
|»»»»»» Name|string|true|none|Long name associated with the code|
|»»»»»» Description|string|true|none|Description to describe the purpose of the code|
|»»»»» OtherFeeRateType|object|false|none|Other fee rate type code which is not available in the standard code set|
|»»»»»» Code|string|false|none|The four letter Mnemonic used within an XML file to identify a code|
|»»»»»» Name|string|true|none|Long name associated with the code|
|»»»»»» Description|string|true|none|Description to describe the purpose of the code|
|»»»»» OtherApplicationFrequency|object|false|none|Other application frequencies that are not available in the standard code list|
|»»»»»» Code|string|false|none|The four letter Mnemonic used within an XML file to identify a code|
|»»»»»» Name|string|true|none|Long name associated with the code|
|»»»»»» Description|string|true|none|Description to describe the purpose of the code|
|»»»»» OtherCalculationFrequency|object|false|none|Other calculation frequency which is not available in the standard code set.|
|»»»»»» Code|string|false|none|The four letter Mnemonic used within an XML file to identify a code|
|»»»»»» Name|string|true|none|Long name associated with the code|
|»»»»»» Description|string|true|none|Description to describe the purpose of the code|
|»» OverdraftFeesCharges|[object]|false|none|Overdraft fees and charges details|
|»»» OverdraftFeeChargeCap|[object]|false|none|Details about any caps (maximum charges) that apply to a particular fee/charge. Capping can either be based on an amount (in gbp), an amount (in items) or a rate.|
|»»»» FeeType|[string]|true|none|Fee/charge type which is being capped|
|»»»» MinMaxType|string|true|none|Min Max type|
|»»»» FeeCapOccurrence|number(float)|false|none|Indicates whether the advertised overdraft rate is guaranteed to be offered to a borrower by the bank e.g. if it’s part of a government scheme, or whether the rate may vary dependent on the applicant’s circumstances.|
|»»»» FeeCapAmount|string|false|none|Cap amount charged for a fee/charge|
|»»»» CappingPeriod|string|false|none|Period e.g. day, week, month etc. for which the fee/charge is capped|
|»»»» Notes|[string]|false|none|Notes related to Overdraft fee charge cap|
|»»»» OtherFeeType|[object]|false|none|Other fee type code which is not available in the standard code set|
|»»»»» Code|string|false|none|The four letter Mnemonic used within an XML file to identify a code|
|»»»»» Name|string|true|none|Long name associated with the code|
|»»»»» Description|string|true|none|Description to describe the purpose of the code|
|»»» OverdraftFeeChargeDetail|[object]|true|none|Details about the fees/charges|
|»»»» FeeType|string|true|none|Overdraft fee type|
|»»»» NegotiableIndicator|boolean|false|none|Indicates whether fee and charges are negotiable|
|»»»» OverdraftControlIndicator|boolean|false|none|Indicates if the fee/charge is already covered by an 'Overdraft Control' fee or not.|
|»»»» IncrementalBorrowingAmount|string|false|none|Every additional tranche of an overdraft balance to which an overdraft fee is applied|
|»»»» FeeAmount|string|false|none|Amount charged for an overdraft fee/charge (where it is charged in terms of an amount rather than a rate)|
|»»»» FeeRate|string|false|none|Rate charged for overdraft fee/charge (where it is charged in terms of a rate rather than an amount)|
|»»»» FeeRateType|string|false|none|Rate type for overdraft fee/charge (where it is charged in terms of a rate rather than an amount)|
|»»»» ApplicationFrequency|string|true|none|Frequency at which the overdraft charge is applied to the account|
|»»»» CalculationFrequency|string|false|none|How often is the overdraft fee/charge calculated for the account.|
|»»»» Notes|[string]|false|none|Free text for capturing any other info related to Overdraft Fees Charge Details|
|»»»» OverdraftFeeChargeCap|[object]|false|none|Details about any caps (maximum charges) that apply to a particular fee/charge. Capping can either be based on an amount (in gbp), an amount (in items) or a rate.|
|»»»»» FeeType|[string]|true|none|Fee/charge type which is being capped|
|»»»»» MinMaxType|string|true|none|Min Max type|
|»»»»» FeeCapOccurrence|number(float)|false|none|Indicates whether the advertised overdraft rate is guaranteed to be offered to a borrower by the bank e.g. if it’s part of a government scheme, or whether the rate may vary dependent on the applicant’s circumstances.|
|»»»»» FeeCapAmount|string|false|none|Cap amount charged for a fee/charge|
|»»»»» CappingPeriod|string|false|none|Period e.g. day, week, month etc. for which the fee/charge is capped|
|»»»»» Notes|[string]|false|none|Notes related to Overdraft fee charge cap|
|»»»»» OtherFeeType|[object]|false|none|Other fee type code which is not available in the standard code set|
|»»»»»» Code|string|false|none|The four letter Mnemonic used within an XML file to identify a code|
|»»»»»» Name|string|true|none|Long name associated with the code|
|»»»»»» Description|string|true|none|Description to describe the purpose of the code|
|»»»» OtherFeeType|object|false|none|Other Fee type which is not available in the standard code set|
|»»»»» Code|string|false|none|The four letter Mnemonic used within an XML file to identify a code|
|»»»»» Name|string|true|none|Long name associated with the code|
|»»»»» Description|string|true|none|Description to describe the purpose of the code|
|»»»» OtherFeeRateType|object|false|none|Other fee rate type code which is not available in the standard code set|
|»»»»» Code|string|false|none|The four letter Mnemonic used within an XML file to identify a code|
|»»»»» Name|string|true|none|Long name associated with the code|
|»»»»» Description|string|true|none|Description to describe the purpose of the code|
|»»»» OtherApplicationFrequency|object|false|none|Other application frequencies that are not available in the standard code list|
|»»»»» Code|string|false|none|The four letter Mnemonic used within an XML file to identify a code|
|»»»»» Name|string|true|none|Long name associated with the code|
|»»»»» Description|string|true|none|Description to describe the purpose of the code|
|»»»» OtherCalculationFrequency|object|false|none|Other calculation frequency which is not available in the standard code set.|
|»»»»» Code|string|false|none|The four letter Mnemonic used within an XML file to identify a code|
|»»»»» Name|string|true|none|Long name associated with the code|
|»»»»» Description|string|true|none|Description to describe the purpose of the code|
|OtherFeesCharges|[object]|false|none|Contains details of fees and charges which are not associated with either Overdraft or features/benefits|
|» TariffType|string|false|none|TariffType which defines the fee and charges.|
|» TariffName|string|false|none|Name of the tariff|
|» OtherTariffType|object|false|none|Other tariff type which is not in the standard list.|
|»» Code|string|false|none|The four letter Mnemonic used within an XML file to identify a code|
|»» Name|string|true|none|Long name associated with the code|
|»» Description|string|true|none|Description to describe the purpose of the code|
|» FeeChargeDetail|[object]|true|none|Other fees/charges details|
|»» FeeCategory|string|true|none|Categorisation of fees and charges into standard categories.|
|»» FeeType|string|true|none|Fee/Charge Type|
|»» NegotiableIndicator|boolean|false|none|Fee/charge which is usually negotiable rather than a fixed amount|
|»» FeeAmount|string|false|none|Fee Amount charged for a fee/charge (where it is charged in terms of an amount rather than a rate)|
|»» FeeRate|string|false|none|Rate charged for Fee/Charge (where it is charged in terms of a rate rather than an amount)|
|»» FeeRateType|string|false|none|Rate type for Fee/Charge (where it is charged in terms of a rate rather than an amount)|
|»» ApplicationFrequency|string|true|none|How frequently the fee/charge is applied to the account|
|»» CalculationFrequency|string|false|none|How frequently the fee/charge is calculated|
|»» Notes|[string]|false|none|Optional additional notes to supplement the fee/charge details.|
|»» FeeChargeCap|[object]|false|none|Details about any caps (maximum charges) that apply to a particular or group of fee/charge|
|»»» FeeType|[string]|true|none|Fee/charge type which is being capped|
|»»» MinMaxType|string|true|none|Min Max type|
|»»» FeeCapOccurrence|number(float)|false|none|fee/charges are captured dependent on the number of occurrences rather than capped at a particular amount|
|»»» FeeCapAmount|string|false|none|Cap amount charged for a fee/charge (where it is charged in terms of an amount rather than a rate)|
|»»» CappingPeriod|string|false|none|Period e.g. day, week, month etc. for which the fee/charge is capped|
|»»» Notes|[string]|false|none|Free text for adding  extra details for fee charge cap|
|»»» OtherFeeType|[object]|false|none|Other fee type code which is not available in the standard code set|
|»»»» Code|string|false|none|The four letter Mnemonic used within an XML file to identify a code|
|»»»» Name|string|true|none|Long name associated with the code|
|»»»» Description|string|true|none|Description to describe the purpose of the code|
|»» OtherFeeCategoryType|object|false|none|none|
|»»» Code|string|false|none|The four letter Mnemonic used within an XML file to identify a code|
|»»» Name|string|true|none|Long name associated with the code|
|»»» Description|string|true|none|Description to describe the purpose of the code|
|»» OtherFeeType|object|false|none|Other Fee/charge type which is not available in the standard code set|
|»»» Code|string|false|none|The four letter Mnemonic used within an XML file to identify a code|
|»»» FeeCategory|string|true|none|Categorisation of fees and charges into standard categories.|
|»»» Name|string|true|none|Long name associated with the code|
|»»» Description|string|true|none|Description to describe the purpose of the code|
|»» OtherFeeRateType|object|false|none|Other fee rate type which is not available in the standard code set|
|»»» Code|string|false|none|The four letter Mnemonic used within an XML file to identify a code|
|»»» Name|string|true|none|Long name associated with the code|
|»»» Description|string|true|none|Description to describe the purpose of the code|
|»» OtherApplicationFrequency|object|false|none|Other application frequencies not covered in the standard code list|
|»»» Code|string|false|none|The four letter Mnemonic used within an XML file to identify a code|
|»»» Name|string|true|none|Long name associated with the code|
|»»» Description|string|true|none|Description to describe the purpose of the code|
|»» OtherCalculationFrequency|object|false|none|Other calculation frequency which is not available in standard code set.|
|»»» Code|string|false|none|The four letter Mnemonic used within an XML file to identify a code|
|»»» Name|string|true|none|Long name associated with the code|
|»»» Description|string|true|none|Description to describe the purpose of the code|
|»» FeeApplicableRange|object|false|none|Range or amounts or rates for which the fee/charge applies|
|»»» MinimumAmount|string|false|none|Minimum Amount on which fee/charge is applicable (where it is expressed as an amount)|
|»»» MaximumAmount|string|false|none|Maximum Amount on which fee is applicable (where it is expressed as an amount)|
|»»» MinimumRate|string|false|none|Minimum rate on which fee/charge is applicable(where it is expressed as an rate)|
|»»» MaximumRate|string|false|none|Maximum rate on which fee/charge is applicable(where it is expressed as an rate)|
|» FeeChargeCap|[object]|false|none|Details about any caps (maximum charges) that apply to a particular or group of fee/charge|
|»» FeeType|[string]|true|none|Fee/charge type which is being capped|
|»» MinMaxType|string|true|none|Min Max type|
|»» FeeCapOccurrence|number(float)|false|none|fee/charges are captured dependent on the number of occurrences rather than capped at a particular amount|
|»» FeeCapAmount|string|false|none|Cap amount charged for a fee/charge (where it is charged in terms of an amount rather than a rate)|
|»» CappingPeriod|string|false|none|Period e.g. day, week, month etc. for which the fee/charge is capped|
|»» Notes|[string]|false|none|Free text for adding  extra details for fee charge cap|
|»» OtherFeeType|[object]|false|none|Other fee type code which is not available in the standard code set|
|»»» Code|string|false|none|The four letter Mnemonic used within an XML file to identify a code|
|»»» Name|string|true|none|Long name associated with the code|
|»»» Description|string|true|none|Description to describe the purpose of the code|

#### Enumerated Values

|Property|Value|
|---|---|
|FeeFreeLengthPeriod|Day|
|FeeFreeLengthPeriod|Half Year|
|FeeFreeLengthPeriod|Month|
|FeeFreeLengthPeriod|Quarter|
|FeeFreeLengthPeriod|Week|
|FeeFreeLengthPeriod|Year|
|TierBandMethod|Banded|
|TierBandMethod|Tiered|
|TierBandMethod|Whole|
|CalculationMethod|Compound|
|CalculationMethod|SimpleInterest|
|Destination|PayAway|
|Destination|SelfCredit|
|CalculationFrequency|Daily|
|CalculationFrequency|HalfYearly|
|CalculationFrequency|Monthly|
|CalculationFrequency|Other|
|CalculationFrequency|Quarterly|
|CalculationFrequency|PerStatementDate|
|CalculationFrequency|Weekly|
|CalculationFrequency|Yearly|
|ApplicationFrequency|Daily|
|ApplicationFrequency|HalfYearly|
|ApplicationFrequency|Monthly|
|ApplicationFrequency|Other|
|ApplicationFrequency|Quarterly|
|ApplicationFrequency|PerStatementDate|
|ApplicationFrequency|Weekly|
|ApplicationFrequency|Yearly|
|DepositInterestAppliedCoverage|Banded|
|DepositInterestAppliedCoverage|Tiered|
|DepositInterestAppliedCoverage|Whole|
|FixedVariableInterestRateType|Fixed|
|FixedVariableInterestRateType|Variable|
|BankInterestRateType|Gross|
|BankInterestRateType|Other|
|TierBandMethod|Banded|
|TierBandMethod|Tiered|
|TierBandMethod|Whole|
|OverdraftType|Committed|
|OverdraftType|OnDemand|
|AgreementPeriod|Day|
|AgreementPeriod|Half Year|
|AgreementPeriod|Month|
|AgreementPeriod|Quarter|
|AgreementPeriod|Week|
|AgreementPeriod|Year|
|OverdraftInterestChargingCoverage|Banded|
|OverdraftInterestChargingCoverage|Tiered|
|OverdraftInterestChargingCoverage|Whole|
|MinMaxType|Minimum|
|MinMaxType|Maximum|
|CappingPeriod|Day|
|CappingPeriod|Half Year|
|CappingPeriod|Month|
|CappingPeriod|Quarter|
|CappingPeriod|Week|
|CappingPeriod|Year|
|FeeType|ArrangedOverdraft|
|FeeType|AnnualReview|
|FeeType|EmergencyBorrowing|
|FeeType|BorrowingItem|
|FeeType|OverdraftRenewal|
|FeeType|OverdraftSetup|
|FeeType|Surcharge|
|FeeType|TempOverdraft|
|FeeType|UnauthorisedBorrowing|
|FeeType|UnauthorisedPaidTrans|
|FeeType|Other|
|FeeType|UnauthorisedUnpaidTrans|
|FeeRateType|Gross|
|FeeRateType|Other|
|ApplicationFrequency|OnClosing|
|ApplicationFrequency|OnOpening|
|ApplicationFrequency|ChargingPeriod|
|ApplicationFrequency|Daily|
|ApplicationFrequency|PerItem|
|ApplicationFrequency|Monthly|
|ApplicationFrequency|OnAnniversary|
|ApplicationFrequency|Other|
|ApplicationFrequency|PerHundredPounds|
|ApplicationFrequency|PerHour|
|ApplicationFrequency|PerOccurrence|
|ApplicationFrequency|PerSheet|
|ApplicationFrequency|PerTransaction|
|ApplicationFrequency|PerTransactionAmount|
|ApplicationFrequency|PerTransactionPercentage|
|ApplicationFrequency|Quarterly|
|ApplicationFrequency|SixMonthly|
|ApplicationFrequency|StatementMonthly|
|ApplicationFrequency|Weekly|
|ApplicationFrequency|Yearly|
|CalculationFrequency|OnClosing|
|CalculationFrequency|OnOpening|
|CalculationFrequency|ChargingPeriod|
|CalculationFrequency|Daily|
|CalculationFrequency|PerItem|
|CalculationFrequency|Monthly|
|CalculationFrequency|OnAnniversary|
|CalculationFrequency|Other|
|CalculationFrequency|PerHundredPounds|
|CalculationFrequency|PerHour|
|CalculationFrequency|PerOccurrence|
|CalculationFrequency|PerSheet|
|CalculationFrequency|PerTransaction|
|CalculationFrequency|PerTransactionAmount|
|CalculationFrequency|PerTransactionPercentage|
|CalculationFrequency|Quarterly|
|CalculationFrequency|SixMonthly|
|CalculationFrequency|StatementMonthly|
|CalculationFrequency|Weekly|
|CalculationFrequency|Yearly|
|MinMaxType|Minimum|
|MinMaxType|Maximum|
|CappingPeriod|Day|
|CappingPeriod|Half Year|
|CappingPeriod|Month|
|CappingPeriod|Quarter|
|CappingPeriod|Week|
|CappingPeriod|Year|
|MinMaxType|Minimum|
|MinMaxType|Maximum|
|CappingPeriod|Day|
|CappingPeriod|Half Year|
|CappingPeriod|Month|
|CappingPeriod|Quarter|
|CappingPeriod|Week|
|CappingPeriod|Year|
|FeeType|ArrangedOverdraft|
|FeeType|AnnualReview|
|FeeType|EmergencyBorrowing|
|FeeType|BorrowingItem|
|FeeType|OverdraftRenewal|
|FeeType|OverdraftSetup|
|FeeType|Surcharge|
|FeeType|TempOverdraft|
|FeeType|UnauthorisedBorrowing|
|FeeType|UnauthorisedPaidTrans|
|FeeType|Other|
|FeeType|UnauthorisedUnpaidTrans|
|FeeRateType|Gross|
|FeeRateType|Other|
|ApplicationFrequency|OnClosing|
|ApplicationFrequency|OnOpening|
|ApplicationFrequency|ChargingPeriod|
|ApplicationFrequency|Daily|
|ApplicationFrequency|PerItem|
|ApplicationFrequency|Monthly|
|ApplicationFrequency|OnAnniversary|
|ApplicationFrequency|Other|
|ApplicationFrequency|PerHundredPounds|
|ApplicationFrequency|PerHour|
|ApplicationFrequency|PerOccurrence|
|ApplicationFrequency|PerSheet|
|ApplicationFrequency|PerTransaction|
|ApplicationFrequency|PerTransactionAmount|
|ApplicationFrequency|PerTransactionPercentage|
|ApplicationFrequency|Quarterly|
|ApplicationFrequency|SixMonthly|
|ApplicationFrequency|StatementMonthly|
|ApplicationFrequency|Weekly|
|ApplicationFrequency|Yearly|
|CalculationFrequency|OnClosing|
|CalculationFrequency|OnOpening|
|CalculationFrequency|ChargingPeriod|
|CalculationFrequency|Daily|
|CalculationFrequency|PerItem|
|CalculationFrequency|Monthly|
|CalculationFrequency|OnAnniversary|
|CalculationFrequency|Other|
|CalculationFrequency|PerHundredPounds|
|CalculationFrequency|PerHour|
|CalculationFrequency|PerOccurrence|
|CalculationFrequency|PerSheet|
|CalculationFrequency|PerTransaction|
|CalculationFrequency|PerTransactionAmount|
|CalculationFrequency|PerTransactionPercentage|
|CalculationFrequency|Quarterly|
|CalculationFrequency|SixMonthly|
|CalculationFrequency|StatementMonthly|
|CalculationFrequency|Weekly|
|CalculationFrequency|Yearly|
|MinMaxType|Minimum|
|MinMaxType|Maximum|
|CappingPeriod|Day|
|CappingPeriod|Half Year|
|CappingPeriod|Month|
|CappingPeriod|Quarter|
|CappingPeriod|Week|
|CappingPeriod|Year|
|TariffType|Electronic|
|TariffType|Mixed|
|TariffType|Other|
|FeeCategory|Other|
|FeeCategory|Servicing|
|FeeType|Other|
|FeeType|ServiceCAccountFee|
|FeeType|ServiceCAccountFeeMonthly|
|FeeType|ServiceCAccountFeeQuarterly|
|FeeType|ServiceCFixedTariff|
|FeeType|ServiceCBusiDepAccBreakage|
|FeeType|ServiceCMinimumMonthlyFee|
|FeeType|ServiceCOther|
|FeeRateType|Gross|
|FeeRateType|Other|
|ApplicationFrequency|OnClosing|
|ApplicationFrequency|OnOpening|
|ApplicationFrequency|ChargingPeriod|
|ApplicationFrequency|Daily|
|ApplicationFrequency|PerItem|
|ApplicationFrequency|Monthly|
|ApplicationFrequency|OnAnniversary|
|ApplicationFrequency|Other|
|ApplicationFrequency|PerHundredPounds|
|ApplicationFrequency|PerHour|
|ApplicationFrequency|PerOccurrence|
|ApplicationFrequency|PerSheet|
|ApplicationFrequency|PerTransaction|
|ApplicationFrequency|PerTransactionAmount|
|ApplicationFrequency|PerTransactionPercentage|
|ApplicationFrequency|Quarterly|
|ApplicationFrequency|SixMonthly|
|ApplicationFrequency|StatementMonthly|
|ApplicationFrequency|Weekly|
|ApplicationFrequency|Yearly|
|CalculationFrequency|OnClosing|
|CalculationFrequency|OnOpening|
|CalculationFrequency|ChargingPeriod|
|CalculationFrequency|Daily|
|CalculationFrequency|PerItem|
|CalculationFrequency|Monthly|
|CalculationFrequency|OnAnniversary|
|CalculationFrequency|Other|
|CalculationFrequency|PerHundredPounds|
|CalculationFrequency|PerHour|
|CalculationFrequency|PerOccurrence|
|CalculationFrequency|PerSheet|
|CalculationFrequency|PerTransaction|
|CalculationFrequency|PerTransactionAmount|
|CalculationFrequency|PerTransactionPercentage|
|CalculationFrequency|Quarterly|
|CalculationFrequency|SixMonthly|
|CalculationFrequency|StatementMonthly|
|CalculationFrequency|Weekly|
|CalculationFrequency|Yearly|
|MinMaxType|Minimum|
|MinMaxType|Maximum|
|CappingPeriod|Day|
|CappingPeriod|Half Year|
|CappingPeriod|Month|
|CappingPeriod|Quarter|
|CappingPeriod|Week|
|CappingPeriod|Year|
|FeeCategory|Other|
|FeeCategory|Servicing|
|MinMaxType|Minimum|
|MinMaxType|Maximum|
|CappingPeriod|Day|
|CappingPeriod|Half Year|
|CappingPeriod|Month|
|CappingPeriod|Quarter|
|CappingPeriod|Week|
|CappingPeriod|Year|

<h2 id="tocS_OBBalanceType1Code">OBBalanceType1Code</h2>
<!-- backwards compatibility -->
<a id="schemaobbalancetype1code"></a>
<a id="schema_OBBalanceType1Code"></a>
<a id="tocSobbalancetype1code"></a>
<a id="tocsobbalancetype1code"></a>

```json
{
  "description": "Balance type, in a coded form.",
  "type": "string",
  "enum": [
    "Expected",
    "InterimBooked"
  ]
}

```

Balance type, in a coded form.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Balance type, in a coded form.|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|Expected|
|*anonymous*|InterimBooked|

<h2 id="tocS_OBBankTransactionCodeStructure1">OBBankTransactionCodeStructure1</h2>
<!-- backwards compatibility -->
<a id="schemaobbanktransactioncodestructure1"></a>
<a id="schema_OBBankTransactionCodeStructure1"></a>
<a id="tocSobbanktransactioncodestructure1"></a>
<a id="tocsobbanktransactioncodestructure1"></a>

```json
{
  "type": "object",
  "required": [
    "Code",
    "SubCode"
  ],
  "description": "Set of elements used to fully identify the type of underlying transaction resulting in an entry.",
  "properties": {
    "Code": {
      "description": "Specifies the family within a domain.",
      "type": "string"
    },
    "SubCode": {
      "description": "Specifies the sub-product family within a specific family.",
      "type": "string"
    }
  }
}

```

Set of elements used to fully identify the type of underlying transaction resulting in an entry.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Code|string|true|none|Specifies the family within a domain.|
|SubCode|string|true|none|Specifies the sub-product family within a specific family.|

<h2 id="tocS_OBBeneficiary5">OBBeneficiary5</h2>
<!-- backwards compatibility -->
<a id="schemaobbeneficiary5"></a>
<a id="schema_OBBeneficiary5"></a>
<a id="tocSobbeneficiary5"></a>
<a id="tocsobbeneficiary5"></a>

```json
{
  "type": "object",
  "properties": {
    "AccountId": {
      "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
      "type": "string",
      "minLength": 1,
      "maxLength": 40
    },
    "BeneficiaryId": {
      "description": "A unique and immutable identifier used to identify the beneficiary resource. This identifier has no meaning to the account owner.",
      "type": "string",
      "minLength": 1,
      "maxLength": 40
    },
    "Reference": {
      "description": "Unique reference, as assigned by the creditor, to unambiguously refer to the payment transaction.\nUsage: If available, the initiating party should provide this reference in the structured remittance information, to enable reconciliation by the creditor upon receipt of the amount of money.\nIf the business context requires the use of a creditor reference or a payment remit identification, and only one identifier can be passed through the end-to-end chain, the creditor's reference or payment remittance identification should be quoted in the end-to-end transaction identification.",
      "type": "string",
      "minLength": 1,
      "maxLength": 35
    },
    "CreditorAccount": {
      "type": "object",
      "required": [
        "SchemeName",
        "Identification"
      ],
      "description": "Provides the details to identify the beneficiary account.",
      "properties": {
        "SchemeName": {
          "description": "Name of the identification scheme, in a coded form as published in an external list.",
          "type": "string",
          "x-namespaced-enum": [
            "UK.OBIE.SortCodeAccountNumber"
          ]
        },
        "Identification": {
          "description": "Identification assigned by an institution to identify an account. This identification is known by the account owner.",
          "type": "string",
          "minLength": 1,
          "maxLength": 256
        },
        "Name": {
          "description": "The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.\nNote, the account name is not the product name or the nickname of the account.",
          "type": "string",
          "minLength": 1,
          "maxLength": 350
        },
        "SecondaryIdentification": {
          "description": "This is secondary identification of the account, as assigned by the account servicing institution. \nThis can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).",
          "type": "string",
          "minLength": 1,
          "maxLength": 34
        }
      }
    }
  },
  "additionalProperties": false
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|AccountId|[AccountId](#schemaaccountid)|false|none|A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.|
|BeneficiaryId|[BeneficiaryId](#schemabeneficiaryid)|false|none|A unique and immutable identifier used to identify the beneficiary resource. This identifier has no meaning to the account owner.|
|Reference|[Reference](#schemareference)|false|none|Unique reference, as assigned by the creditor, to unambiguously refer to the payment transaction.<br>Usage: If available, the initiating party should provide this reference in the structured remittance information, to enable reconciliation by the creditor upon receipt of the amount of money.<br>If the business context requires the use of a creditor reference or a payment remit identification, and only one identifier can be passed through the end-to-end chain, the creditor's reference or payment remittance identification should be quoted in the end-to-end transaction identification.|
|CreditorAccount|[OBCashAccount5_0](#schemaobcashaccount5_0)|false|none|Provides the details to identify the beneficiary account.|

<h2 id="tocS_OBBeneficiary5Basic">OBBeneficiary5Basic</h2>
<!-- backwards compatibility -->
<a id="schemaobbeneficiary5basic"></a>
<a id="schema_OBBeneficiary5Basic"></a>
<a id="tocSobbeneficiary5basic"></a>
<a id="tocsobbeneficiary5basic"></a>

```json
{
  "type": "object",
  "properties": {
    "AccountId": {
      "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
      "type": "string",
      "minLength": 1,
      "maxLength": 40
    },
    "BeneficiaryId": {
      "description": "A unique and immutable identifier used to identify the beneficiary resource. This identifier has no meaning to the account owner.",
      "type": "string",
      "minLength": 1,
      "maxLength": 40
    },
    "Reference": {
      "description": "Unique reference, as assigned by the creditor, to unambiguously refer to the payment transaction.\nUsage: If available, the initiating party should provide this reference in the structured remittance information, to enable reconciliation by the creditor upon receipt of the amount of money.\nIf the business context requires the use of a creditor reference or a payment remit identification, and only one identifier can be passed through the end-to-end chain, the creditor's reference or payment remittance identification should be quoted in the end-to-end transaction identification.",
      "type": "string",
      "minLength": 1,
      "maxLength": 35
    }
  },
  "additionalProperties": false
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|AccountId|[AccountId](#schemaaccountid)|false|none|A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.|
|BeneficiaryId|[BeneficiaryId](#schemabeneficiaryid)|false|none|A unique and immutable identifier used to identify the beneficiary resource. This identifier has no meaning to the account owner.|
|Reference|[Reference](#schemareference)|false|none|Unique reference, as assigned by the creditor, to unambiguously refer to the payment transaction.<br>Usage: If available, the initiating party should provide this reference in the structured remittance information, to enable reconciliation by the creditor upon receipt of the amount of money.<br>If the business context requires the use of a creditor reference or a payment remit identification, and only one identifier can be passed through the end-to-end chain, the creditor's reference or payment remittance identification should be quoted in the end-to-end transaction identification.|

<h2 id="tocS_OBBeneficiary5Detail">OBBeneficiary5Detail</h2>
<!-- backwards compatibility -->
<a id="schemaobbeneficiary5detail"></a>
<a id="schema_OBBeneficiary5Detail"></a>
<a id="tocSobbeneficiary5detail"></a>
<a id="tocsobbeneficiary5detail"></a>

```json
{
  "type": "object",
  "properties": {
    "AccountId": {
      "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
      "type": "string",
      "minLength": 1,
      "maxLength": 40
    },
    "BeneficiaryId": {
      "description": "A unique and immutable identifier used to identify the beneficiary resource. This identifier has no meaning to the account owner.",
      "type": "string",
      "minLength": 1,
      "maxLength": 40
    },
    "Reference": {
      "description": "Unique reference, as assigned by the creditor, to unambiguously refer to the payment transaction.\nUsage: If available, the initiating party should provide this reference in the structured remittance information, to enable reconciliation by the creditor upon receipt of the amount of money.\nIf the business context requires the use of a creditor reference or a payment remit identification, and only one identifier can be passed through the end-to-end chain, the creditor's reference or payment remittance identification should be quoted in the end-to-end transaction identification.",
      "type": "string",
      "minLength": 1,
      "maxLength": 35
    },
    "CreditorAccount": {
      "type": "object",
      "required": [
        "SchemeName",
        "Identification"
      ],
      "description": "Provides the details to identify the beneficiary account.",
      "properties": {
        "SchemeName": {
          "description": "Name of the identification scheme, in a coded form as published in an external list.",
          "type": "string",
          "x-namespaced-enum": [
            "UK.OBIE.SortCodeAccountNumber"
          ]
        },
        "Identification": {
          "description": "Identification assigned by an institution to identify an account. This identification is known by the account owner.",
          "type": "string",
          "minLength": 1,
          "maxLength": 256
        },
        "Name": {
          "description": "The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.\nNote, the account name is not the product name or the nickname of the account.",
          "type": "string",
          "minLength": 1,
          "maxLength": 350
        },
        "SecondaryIdentification": {
          "description": "This is secondary identification of the account, as assigned by the account servicing institution. \nThis can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).",
          "type": "string",
          "minLength": 1,
          "maxLength": 34
        }
      }
    }
  },
  "additionalProperties": false,
  "required": [
    "CreditorAccount"
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|AccountId|[AccountId](#schemaaccountid)|false|none|A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.|
|BeneficiaryId|[BeneficiaryId](#schemabeneficiaryid)|false|none|A unique and immutable identifier used to identify the beneficiary resource. This identifier has no meaning to the account owner.|
|Reference|[Reference](#schemareference)|false|none|Unique reference, as assigned by the creditor, to unambiguously refer to the payment transaction.<br>Usage: If available, the initiating party should provide this reference in the structured remittance information, to enable reconciliation by the creditor upon receipt of the amount of money.<br>If the business context requires the use of a creditor reference or a payment remit identification, and only one identifier can be passed through the end-to-end chain, the creditor's reference or payment remittance identification should be quoted in the end-to-end transaction identification.|
|CreditorAccount|[OBCashAccount5_0](#schemaobcashaccount5_0)|true|none|Provides the details to identify the beneficiary account.|

<h2 id="tocS_OBBranchAndFinancialInstitutionIdentification5_0">OBBranchAndFinancialInstitutionIdentification5_0</h2>
<!-- backwards compatibility -->
<a id="schemaobbranchandfinancialinstitutionidentification5_0"></a>
<a id="schema_OBBranchAndFinancialInstitutionIdentification5_0"></a>
<a id="tocSobbranchandfinancialinstitutionidentification5_0"></a>
<a id="tocsobbranchandfinancialinstitutionidentification5_0"></a>

```json
{
  "type": "object",
  "required": [
    "SchemeName",
    "Identification"
  ],
  "description": "Party that manages the account on behalf of the account owner, that is manages the registration and booking of entries on the account, calculates balances on the account and provides information about the account.",
  "properties": {
    "SchemeName": {
      "description": "Name of the identification scheme, in a coded form as published in an external list.",
      "type": "string",
      "x-namespaced-enum": [
        "UK.OBIE.BICFI"
      ]
    },
    "Identification": {
      "description": "Unique and unambiguous identification of the servicing institution.",
      "type": "string",
      "minLength": 1,
      "maxLength": 35
    }
  }
}

```

Party that manages the account on behalf of the account owner, that is manages the registration and booking of entries on the account, calculates balances on the account and provides information about the account.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|SchemeName|[OBExternalFinancialInstitutionIdentification4Code](#schemaobexternalfinancialinstitutionidentification4code)|true|none|Name of the identification scheme, in a coded form as published in an external list.|
|Identification|[Identification_1](#schemaidentification_1)|true|none|Unique and unambiguous identification of the servicing institution.|

<h2 id="tocS_OBBranchAndFinancialInstitutionIdentification5_1">OBBranchAndFinancialInstitutionIdentification5_1</h2>
<!-- backwards compatibility -->
<a id="schemaobbranchandfinancialinstitutionidentification5_1"></a>
<a id="schema_OBBranchAndFinancialInstitutionIdentification5_1"></a>
<a id="tocSobbranchandfinancialinstitutionidentification5_1"></a>
<a id="tocsobbranchandfinancialinstitutionidentification5_1"></a>

```json
{
  "type": "object",
  "required": [
    "SchemeName",
    "Identification"
  ],
  "description": "Party that manages the account on behalf of the account owner, that is manages the registration and booking of entries on the account, calculates balances on the account and provides information about the account.\nThis is the servicer of the beneficiary account.",
  "properties": {
    "SchemeName": {
      "description": "Name of the identification scheme, in a coded form as published in an external list.",
      "type": "string",
      "x-namespaced-enum": [
        "UK.OBIE.BICFI"
      ]
    },
    "Identification": {
      "description": "Unique and unambiguous identification of the servicing institution.",
      "type": "string",
      "minLength": 1,
      "maxLength": 35
    }
  }
}

```

Party that manages the account on behalf of the account owner, that is manages the registration and booking of entries on the account, calculates balances on the account and provides information about the account.
This is the servicer of the beneficiary account.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|SchemeName|[OBExternalFinancialInstitutionIdentification4Code](#schemaobexternalfinancialinstitutionidentification4code)|true|none|Name of the identification scheme, in a coded form as published in an external list.|
|Identification|[Identification_1](#schemaidentification_1)|true|none|Unique and unambiguous identification of the servicing institution.|

<h2 id="tocS_OBBranchAndFinancialInstitutionIdentification6_0">OBBranchAndFinancialInstitutionIdentification6_0</h2>
<!-- backwards compatibility -->
<a id="schemaobbranchandfinancialinstitutionidentification6_0"></a>
<a id="schema_OBBranchAndFinancialInstitutionIdentification6_0"></a>
<a id="tocSobbranchandfinancialinstitutionidentification6_0"></a>
<a id="tocsobbranchandfinancialinstitutionidentification6_0"></a>

```json
{
  "type": "object",
  "description": "Party that manages the account on behalf of the account owner, that is manages the registration and booking of entries on the account, calculates balances on the account and provides information about the account.\nThis is the servicer of the beneficiary account.",
  "properties": {
    "SchemeName": {
      "description": "Name of the identification scheme, in a coded form as published in an external list.",
      "type": "string",
      "x-namespaced-enum": [
        "UK.OBIE.BICFI"
      ]
    },
    "Identification": {
      "description": "Unique and unambiguous identification of the servicing institution.",
      "type": "string",
      "minLength": 1,
      "maxLength": 35
    },
    "Name": {
      "description": "Name by which an agent is known and which is usually used to identify that agent.",
      "type": "string",
      "minLength": 1,
      "maxLength": 140
    },
    "PostalAddress": {
      "type": "object",
      "description": "Information that locates and identifies a specific address, as defined by postal services.",
      "properties": {
        "AddressType": {
          "description": "Identifies the nature of the postal address.",
          "type": "string",
          "enum": [
            "Business",
            "Correspondence",
            "DeliveryTo",
            "MailTo",
            "POBox",
            "Postal",
            "Residential",
            "Statement"
          ]
        },
        "Department": {
          "description": "Identification of a division of a large organisation or building.",
          "type": "string",
          "minLength": 1,
          "maxLength": 70
        },
        "SubDepartment": {
          "description": "Identification of a sub-division of a large organisation or building.",
          "type": "string",
          "minLength": 1,
          "maxLength": 70
        },
        "StreetName": {
          "description": "Name of a street or thoroughfare.",
          "type": "string",
          "minLength": 1,
          "maxLength": 70
        },
        "BuildingNumber": {
          "description": "Number that identifies the position of a building on a street.",
          "type": "string",
          "minLength": 1,
          "maxLength": 16
        },
        "PostCode": {
          "description": "Identifier consisting of a group of letters and/or numbers that is added to a postal address to assist the sorting of mail.",
          "type": "string",
          "minLength": 1,
          "maxLength": 16
        },
        "TownName": {
          "description": "Name of a built-up area, with defined boundaries, and a local government.",
          "type": "string",
          "minLength": 1,
          "maxLength": 35
        },
        "CountrySubDivision": {
          "description": "Identifies a subdivision of a country such as state, region, county.",
          "type": "string",
          "minLength": 1,
          "maxLength": 35
        },
        "Country": {
          "description": "Nation with its own government.",
          "type": "string",
          "pattern": "^[A-Z]{2,2}$"
        },
        "AddressLine": {
          "type": "array",
          "items": {
            "description": "Information that locates and identifies a specific address, as defined by postal services, presented in free format text.",
            "type": "string",
            "minLength": 1,
            "maxLength": 70
          },
          "minItems": 0,
          "maxItems": 7
        }
      }
    }
  }
}

```

Party that manages the account on behalf of the account owner, that is manages the registration and booking of entries on the account, calculates balances on the account and provides information about the account.
This is the servicer of the beneficiary account.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|SchemeName|[OBExternalFinancialInstitutionIdentification4Code](#schemaobexternalfinancialinstitutionidentification4code)|false|none|Name of the identification scheme, in a coded form as published in an external list.|
|Identification|[Identification_1](#schemaidentification_1)|false|none|Unique and unambiguous identification of the servicing institution.|
|Name|[Name_1](#schemaname_1)|false|none|Name by which an agent is known and which is usually used to identify that agent.|
|PostalAddress|[OBPostalAddress6](#schemaobpostaladdress6)|false|none|Information that locates and identifies a specific address, as defined by postal services.|

<h2 id="tocS_OBBranchAndFinancialInstitutionIdentification6_1">OBBranchAndFinancialInstitutionIdentification6_1</h2>
<!-- backwards compatibility -->
<a id="schemaobbranchandfinancialinstitutionidentification6_1"></a>
<a id="schema_OBBranchAndFinancialInstitutionIdentification6_1"></a>
<a id="tocSobbranchandfinancialinstitutionidentification6_1"></a>
<a id="tocsobbranchandfinancialinstitutionidentification6_1"></a>

```json
{
  "type": "object",
  "description": "Financial institution servicing an account for the creditor.",
  "properties": {
    "SchemeName": {
      "description": "Name of the identification scheme, in a coded form as published in an external list.",
      "type": "string",
      "x-namespaced-enum": [
        "UK.OBIE.BICFI"
      ]
    },
    "Identification": {
      "description": "Unique and unambiguous identification of a financial institution or a branch of a financial institution.",
      "type": "string",
      "minLength": 1,
      "maxLength": 35
    },
    "Name": {
      "description": "Name by which an agent is known and which is usually used to identify that agent.",
      "type": "string",
      "minLength": 1,
      "maxLength": 140
    },
    "PostalAddress": {
      "type": "object",
      "description": "Information that locates and identifies a specific address, as defined by postal services.",
      "properties": {
        "AddressType": {
          "description": "Identifies the nature of the postal address.",
          "type": "string",
          "enum": [
            "Business",
            "Correspondence",
            "DeliveryTo",
            "MailTo",
            "POBox",
            "Postal",
            "Residential",
            "Statement"
          ]
        },
        "Department": {
          "description": "Identification of a division of a large organisation or building.",
          "type": "string",
          "minLength": 1,
          "maxLength": 70
        },
        "SubDepartment": {
          "description": "Identification of a sub-division of a large organisation or building.",
          "type": "string",
          "minLength": 1,
          "maxLength": 70
        },
        "StreetName": {
          "description": "Name of a street or thoroughfare.",
          "type": "string",
          "minLength": 1,
          "maxLength": 70
        },
        "BuildingNumber": {
          "description": "Number that identifies the position of a building on a street.",
          "type": "string",
          "minLength": 1,
          "maxLength": 16
        },
        "PostCode": {
          "description": "Identifier consisting of a group of letters and/or numbers that is added to a postal address to assist the sorting of mail.",
          "type": "string",
          "minLength": 1,
          "maxLength": 16
        },
        "TownName": {
          "description": "Name of a built-up area, with defined boundaries, and a local government.",
          "type": "string",
          "minLength": 1,
          "maxLength": 35
        },
        "CountrySubDivision": {
          "description": "Identifies a subdivision of a country such as state, region, county.",
          "type": "string",
          "minLength": 1,
          "maxLength": 35
        },
        "Country": {
          "description": "Nation with its own government.",
          "type": "string",
          "pattern": "^[A-Z]{2,2}$"
        },
        "AddressLine": {
          "type": "array",
          "items": {
            "description": "Information that locates and identifies a specific address, as defined by postal services, presented in free format text.",
            "type": "string",
            "minLength": 1,
            "maxLength": 70
          },
          "minItems": 0,
          "maxItems": 7
        }
      }
    }
  }
}

```

Financial institution servicing an account for the creditor.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|SchemeName|[OBExternalFinancialInstitutionIdentification4Code](#schemaobexternalfinancialinstitutionidentification4code)|false|none|Name of the identification scheme, in a coded form as published in an external list.|
|Identification|[Identification_2](#schemaidentification_2)|false|none|Unique and unambiguous identification of a financial institution or a branch of a financial institution.|
|Name|[Name_1](#schemaname_1)|false|none|Name by which an agent is known and which is usually used to identify that agent.|
|PostalAddress|[OBPostalAddress6](#schemaobpostaladdress6)|false|none|Information that locates and identifies a specific address, as defined by postal services.|

<h2 id="tocS_OBBranchAndFinancialInstitutionIdentification6_2">OBBranchAndFinancialInstitutionIdentification6_2</h2>
<!-- backwards compatibility -->
<a id="schemaobbranchandfinancialinstitutionidentification6_2"></a>
<a id="schema_OBBranchAndFinancialInstitutionIdentification6_2"></a>
<a id="tocSobbranchandfinancialinstitutionidentification6_2"></a>
<a id="tocsobbranchandfinancialinstitutionidentification6_2"></a>

```json
{
  "type": "object",
  "description": "Financial institution servicing an account for the debtor.",
  "properties": {
    "SchemeName": {
      "description": "Name of the identification scheme, in a coded form as published in an external list.",
      "type": "string",
      "x-namespaced-enum": [
        "UK.OBIE.BICFI"
      ]
    },
    "Identification": {
      "description": "Unique and unambiguous identification of a financial institution or a branch of a financial institution.",
      "type": "string",
      "minLength": 1,
      "maxLength": 35
    },
    "Name": {
      "description": "Name by which an agent is known and which is usually used to identify that agent.",
      "type": "string",
      "minLength": 1,
      "maxLength": 140
    },
    "PostalAddress": {
      "type": "object",
      "description": "Information that locates and identifies a specific address, as defined by postal services.",
      "properties": {
        "AddressType": {
          "description": "Identifies the nature of the postal address.",
          "type": "string",
          "enum": [
            "Business",
            "Correspondence",
            "DeliveryTo",
            "MailTo",
            "POBox",
            "Postal",
            "Residential",
            "Statement"
          ]
        },
        "Department": {
          "description": "Identification of a division of a large organisation or building.",
          "type": "string",
          "minLength": 1,
          "maxLength": 70
        },
        "SubDepartment": {
          "description": "Identification of a sub-division of a large organisation or building.",
          "type": "string",
          "minLength": 1,
          "maxLength": 70
        },
        "StreetName": {
          "description": "Name of a street or thoroughfare.",
          "type": "string",
          "minLength": 1,
          "maxLength": 70
        },
        "BuildingNumber": {
          "description": "Number that identifies the position of a building on a street.",
          "type": "string",
          "minLength": 1,
          "maxLength": 16
        },
        "PostCode": {
          "description": "Identifier consisting of a group of letters and/or numbers that is added to a postal address to assist the sorting of mail.",
          "type": "string",
          "minLength": 1,
          "maxLength": 16
        },
        "TownName": {
          "description": "Name of a built-up area, with defined boundaries, and a local government.",
          "type": "string",
          "minLength": 1,
          "maxLength": 35
        },
        "CountrySubDivision": {
          "description": "Identifies a subdivision of a country such as state, region, county.",
          "type": "string",
          "minLength": 1,
          "maxLength": 35
        },
        "Country": {
          "description": "Nation with its own government.",
          "type": "string",
          "pattern": "^[A-Z]{2,2}$"
        },
        "AddressLine": {
          "type": "array",
          "items": {
            "description": "Information that locates and identifies a specific address, as defined by postal services, presented in free format text.",
            "type": "string",
            "minLength": 1,
            "maxLength": 70
          },
          "minItems": 0,
          "maxItems": 7
        }
      }
    }
  }
}

```

Financial institution servicing an account for the debtor.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|SchemeName|[OBExternalFinancialInstitutionIdentification4Code](#schemaobexternalfinancialinstitutionidentification4code)|false|none|Name of the identification scheme, in a coded form as published in an external list.|
|Identification|[Identification_2](#schemaidentification_2)|false|none|Unique and unambiguous identification of a financial institution or a branch of a financial institution.|
|Name|[Name_1](#schemaname_1)|false|none|Name by which an agent is known and which is usually used to identify that agent.|
|PostalAddress|[OBPostalAddress6](#schemaobpostaladdress6)|false|none|Information that locates and identifies a specific address, as defined by postal services.|

<h2 id="tocS_OBCashAccount5_0">OBCashAccount5_0</h2>
<!-- backwards compatibility -->
<a id="schemaobcashaccount5_0"></a>
<a id="schema_OBCashAccount5_0"></a>
<a id="tocSobcashaccount5_0"></a>
<a id="tocsobcashaccount5_0"></a>

```json
{
  "type": "object",
  "required": [
    "SchemeName",
    "Identification"
  ],
  "description": "Provides the details to identify the beneficiary account.",
  "properties": {
    "SchemeName": {
      "description": "Name of the identification scheme, in a coded form as published in an external list.",
      "type": "string",
      "x-namespaced-enum": [
        "UK.OBIE.SortCodeAccountNumber"
      ]
    },
    "Identification": {
      "description": "Identification assigned by an institution to identify an account. This identification is known by the account owner.",
      "type": "string",
      "minLength": 1,
      "maxLength": 256
    },
    "Name": {
      "description": "The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.\nNote, the account name is not the product name or the nickname of the account.",
      "type": "string",
      "minLength": 1,
      "maxLength": 350
    },
    "SecondaryIdentification": {
      "description": "This is secondary identification of the account, as assigned by the account servicing institution. \nThis can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).",
      "type": "string",
      "minLength": 1,
      "maxLength": 34
    }
  }
}

```

Provides the details to identify the beneficiary account.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|SchemeName|[OBExternalAccountIdentification4Code](#schemaobexternalaccountidentification4code)|true|none|Name of the identification scheme, in a coded form as published in an external list.|
|Identification|[Identification_0](#schemaidentification_0)|true|none|Identification assigned by an institution to identify an account. This identification is known by the account owner.|
|Name|[Name_0](#schemaname_0)|false|none|The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.<br>Note, the account name is not the product name or the nickname of the account.|
|SecondaryIdentification|[SecondaryIdentification](#schemasecondaryidentification)|false|none|This is secondary identification of the account, as assigned by the account servicing institution. <br>This can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).|

<h2 id="tocS_OBCashAccount5_1">OBCashAccount5_1</h2>
<!-- backwards compatibility -->
<a id="schemaobcashaccount5_1"></a>
<a id="schema_OBCashAccount5_1"></a>
<a id="tocSobcashaccount5_1"></a>
<a id="tocsobcashaccount5_1"></a>

```json
{
  "type": "object",
  "required": [
    "SchemeName",
    "Identification"
  ],
  "description": "Provides the details to identify the beneficiary account.",
  "properties": {
    "SchemeName": {
      "description": "Name of the identification scheme, in a coded form as published in an external list.",
      "type": "string",
      "x-namespaced-enum": [
        "UK.OBIE.SortCodeAccountNumber"
      ]
    },
    "Identification": {
      "description": "Beneficiary account identification.",
      "type": "string",
      "minLength": 1,
      "maxLength": 256
    },
    "Name": {
      "description": "The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.\nNote, the account name is not the product name or the nickname of the account.",
      "type": "string",
      "minLength": 1,
      "maxLength": 350
    },
    "SecondaryIdentification": {
      "description": "This is secondary identification of the account, as assigned by the account servicing institution. \nThis can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).",
      "type": "string",
      "minLength": 1,
      "maxLength": 34
    }
  }
}

```

Provides the details to identify the beneficiary account.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|SchemeName|[OBExternalAccountIdentification4Code](#schemaobexternalaccountidentification4code)|true|none|Name of the identification scheme, in a coded form as published in an external list.|
|Identification|string|true|none|Beneficiary account identification.|
|Name|[Name_0](#schemaname_0)|false|none|The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.<br>Note, the account name is not the product name or the nickname of the account.|
|SecondaryIdentification|[SecondaryIdentification](#schemasecondaryidentification)|false|none|This is secondary identification of the account, as assigned by the account servicing institution. <br>This can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).|

<h2 id="tocS_OBCashAccount6_0">OBCashAccount6_0</h2>
<!-- backwards compatibility -->
<a id="schemaobcashaccount6_0"></a>
<a id="schema_OBCashAccount6_0"></a>
<a id="tocSobcashaccount6_0"></a>
<a id="tocsobcashaccount6_0"></a>

```json
{
  "type": "object",
  "description": "Unambiguous identification of the account of the creditor, in the case of a debit transaction.",
  "properties": {
    "SchemeName": {
      "description": "Name of the identification scheme, in a coded form as published in an external list.",
      "type": "string",
      "x-namespaced-enum": [
        "UK.OBIE.SortCodeAccountNumber"
      ]
    },
    "Identification": {
      "description": "Identification assigned by an institution to identify an account. This identification is known by the account owner.",
      "type": "string",
      "minLength": 1,
      "maxLength": 256
    },
    "Name": {
      "description": "The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.\nNote, the account name is not the product name or the nickname of the account.",
      "type": "string",
      "minLength": 1,
      "maxLength": 350
    },
    "SecondaryIdentification": {
      "description": "This is secondary identification of the account, as assigned by the account servicing institution. \nThis can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).",
      "type": "string",
      "minLength": 1,
      "maxLength": 34
    }
  }
}

```

Unambiguous identification of the account of the creditor, in the case of a debit transaction.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|SchemeName|[OBExternalAccountIdentification4Code](#schemaobexternalaccountidentification4code)|false|none|Name of the identification scheme, in a coded form as published in an external list.|
|Identification|[Identification_0](#schemaidentification_0)|false|none|Identification assigned by an institution to identify an account. This identification is known by the account owner.|
|Name|[Name_0](#schemaname_0)|false|none|The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.<br>Note, the account name is not the product name or the nickname of the account.|
|SecondaryIdentification|[SecondaryIdentification](#schemasecondaryidentification)|false|none|This is secondary identification of the account, as assigned by the account servicing institution. <br>This can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).|

<h2 id="tocS_OBCashAccount6_1">OBCashAccount6_1</h2>
<!-- backwards compatibility -->
<a id="schemaobcashaccount6_1"></a>
<a id="schema_OBCashAccount6_1"></a>
<a id="tocSobcashaccount6_1"></a>
<a id="tocsobcashaccount6_1"></a>

```json
{
  "type": "object",
  "description": "Unambiguous identification of the account of the debtor, in the case of a crebit transaction.",
  "properties": {
    "SchemeName": {
      "description": "Name of the identification scheme, in a coded form as published in an external list.",
      "type": "string",
      "x-namespaced-enum": [
        "UK.OBIE.SortCodeAccountNumber"
      ]
    },
    "Identification": {
      "description": "Identification assigned by an institution to identify an account. This identification is known by the account owner.",
      "type": "string",
      "minLength": 1,
      "maxLength": 256
    },
    "Name": {
      "description": "The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.\nNote, the account name is not the product name or the nickname of the account.",
      "type": "string",
      "minLength": 1,
      "maxLength": 350
    },
    "SecondaryIdentification": {
      "description": "This is secondary identification of the account, as assigned by the account servicing institution. \nThis can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).",
      "type": "string",
      "minLength": 1,
      "maxLength": 34
    }
  }
}

```

Unambiguous identification of the account of the debtor, in the case of a crebit transaction.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|SchemeName|[OBExternalAccountIdentification4Code](#schemaobexternalaccountidentification4code)|false|none|Name of the identification scheme, in a coded form as published in an external list.|
|Identification|[Identification_0](#schemaidentification_0)|false|none|Identification assigned by an institution to identify an account. This identification is known by the account owner.|
|Name|[Name_0](#schemaname_0)|false|none|The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.<br>Note, the account name is not the product name or the nickname of the account.|
|SecondaryIdentification|[SecondaryIdentification](#schemasecondaryidentification)|false|none|This is secondary identification of the account, as assigned by the account servicing institution. <br>This can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).|

<h2 id="tocS_OBCreditDebitCode_0">OBCreditDebitCode_0</h2>
<!-- backwards compatibility -->
<a id="schemaobcreditdebitcode_0"></a>
<a id="schema_OBCreditDebitCode_0"></a>
<a id="tocSobcreditdebitcode_0"></a>
<a id="tocsobcreditdebitcode_0"></a>

```json
{
  "description": "Indicates whether the amount is a credit or a debit. \nUsage: A zero amount is considered to be a credit amount.",
  "type": "string",
  "enum": [
    "Credit",
    "Debit"
  ]
}

```

Indicates whether the amount is a credit or a debit. 
Usage: A zero amount is considered to be a credit amount.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Indicates whether the amount is a credit or a debit. <br>Usage: A zero amount is considered to be a credit amount.|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|Credit|
|*anonymous*|Debit|

<h2 id="tocS_OBCreditDebitCode_1">OBCreditDebitCode_1</h2>
<!-- backwards compatibility -->
<a id="schemaobcreditdebitcode_1"></a>
<a id="schema_OBCreditDebitCode_1"></a>
<a id="tocSobcreditdebitcode_1"></a>
<a id="tocsobcreditdebitcode_1"></a>

```json
{
  "description": "Indicates whether the transaction is a credit or a debit entry.",
  "type": "string",
  "enum": [
    "Credit",
    "Debit"
  ]
}

```

Indicates whether the transaction is a credit or a debit entry.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Indicates whether the transaction is a credit or a debit entry.|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|Credit|
|*anonymous*|Debit|

<h2 id="tocS_OBCreditDebitCode_2">OBCreditDebitCode_2</h2>
<!-- backwards compatibility -->
<a id="schemaobcreditdebitcode_2"></a>
<a id="schema_OBCreditDebitCode_2"></a>
<a id="tocSobcreditdebitcode_2"></a>
<a id="tocsobcreditdebitcode_2"></a>

```json
{
  "description": "Indicates whether the balance is a credit or a debit balance. \nUsage: A zero balance is considered to be a credit balance.",
  "type": "string",
  "enum": [
    "Credit",
    "Debit"
  ]
}

```

Indicates whether the balance is a credit or a debit balance. 
Usage: A zero balance is considered to be a credit balance.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Indicates whether the balance is a credit or a debit balance. <br>Usage: A zero balance is considered to be a credit balance.|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|Credit|
|*anonymous*|Debit|

<h2 id="tocS_OBCurrencyExchange5">OBCurrencyExchange5</h2>
<!-- backwards compatibility -->
<a id="schemaobcurrencyexchange5"></a>
<a id="schema_OBCurrencyExchange5"></a>
<a id="tocSobcurrencyexchange5"></a>
<a id="tocsobcurrencyexchange5"></a>

```json
{
  "type": "object",
  "required": [
    "SourceCurrency",
    "ExchangeRate"
  ],
  "description": "Set of elements used to provide details on the currency exchange.",
  "properties": {
    "SourceCurrency": {
      "description": "Currency from which an amount is to be converted in a currency conversion.",
      "type": "string",
      "pattern": "^[A-Z]{3,3}$"
    },
    "TargetCurrency": {
      "description": "Currency into which an amount is to be converted in a currency conversion.",
      "type": "string",
      "pattern": "^[A-Z]{3,3}$"
    },
    "UnitCurrency": {
      "description": "Currency in which the rate of exchange is expressed in a currency exchange. In the example 1GBP = xxxCUR, the unit currency is GBP.",
      "type": "string",
      "pattern": "^[A-Z]{3,3}$"
    },
    "ExchangeRate": {
      "description": "Factor used to convert an amount from one currency into another. This reflects the price at which one currency was bought with another currency.\nUsage: ExchangeRate expresses the ratio between UnitCurrency and QuotedCurrency (ExchangeRate = UnitCurrency/QuotedCurrency).",
      "type": "number"
    },
    "InstructedAmount": {
      "type": "object",
      "required": [
        "Amount",
        "Currency"
      ],
      "description": "Amount of money to be moved between the debtor and creditor, before deduction of charges, expressed in the currency as ordered by the initiating party.",
      "properties": {
        "Amount": {
          "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
          "type": "string",
          "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
        },
        "Currency": {
          "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
          "type": "string",
          "pattern": "^[A-Z]{3,3}$"
        }
      }
    }
  }
}

```

Set of elements used to provide details on the currency exchange.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|SourceCurrency|string|true|none|Currency from which an amount is to be converted in a currency conversion.|
|TargetCurrency|string|false|none|Currency into which an amount is to be converted in a currency conversion.|
|UnitCurrency|string|false|none|Currency in which the rate of exchange is expressed in a currency exchange. In the example 1GBP = xxxCUR, the unit currency is GBP.|
|ExchangeRate|number|true|none|Factor used to convert an amount from one currency into another. This reflects the price at which one currency was bought with another currency.<br>Usage: ExchangeRate expresses the ratio between UnitCurrency and QuotedCurrency (ExchangeRate = UnitCurrency/QuotedCurrency).|
|InstructedAmount|object|false|none|Amount of money to be moved between the debtor and creditor, before deduction of charges, expressed in the currency as ordered by the initiating party.|
|» Amount|[OBActiveCurrencyAndAmount_SimpleType](#schemaobactivecurrencyandamount_simpletype)|true|none|A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.|
|» Currency|[ActiveOrHistoricCurrencyCode_1](#schemaactiveorhistoriccurrencycode_1)|true|none|A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 "Codes for the representation of currencies and funds".|

<h2 id="tocS_OBEntryStatus1Code">OBEntryStatus1Code</h2>
<!-- backwards compatibility -->
<a id="schemaobentrystatus1code"></a>
<a id="schema_OBEntryStatus1Code"></a>
<a id="tocSobentrystatus1code"></a>
<a id="tocsobentrystatus1code"></a>

```json
{
  "description": "Status of a transaction entry on the books of the account servicer.",
  "type": "string",
  "enum": [
    "Booked",
    "Pending"
  ]
}

```

Status of a transaction entry on the books of the account servicer.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Status of a transaction entry on the books of the account servicer.|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|Booked|
|*anonymous*|Pending|

<h2 id="tocS_OBTransactionMutability1Code">OBTransactionMutability1Code</h2>
<!-- backwards compatibility -->
<a id="schemaobtransactionmutability1code"></a>
<a id="schema_OBTransactionMutability1Code"></a>
<a id="tocSobtransactionmutability1code"></a>
<a id="tocsobtransactionmutability1code"></a>

```json
{
  "description": "Specifies the Mutability of the Transaction record.",
  "type": "string",
  "enum": [
    "Mutable",
    "Immutable"
  ]
}

```

Specifies the Mutability of the Transaction record.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Specifies the Mutability of the Transaction record.|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|Mutable|
|*anonymous*|Immutable|

<h2 id="tocS_OBError1">OBError1</h2>
<!-- backwards compatibility -->
<a id="schemaoberror1"></a>
<a id="schema_OBError1"></a>
<a id="tocSoberror1"></a>
<a id="tocsoberror1"></a>

```json
{
  "type": "object",
  "properties": {
    "ErrorCode": {
      "description": "Low level textual error code, e.g., UK.OBIE.Field.Missing",
      "type": "string",
      "x-namespaced-enum": [
        "UK.OBIE.Field.Expected",
        "UK.OBIE.Field.Invalid",
        "UK.OBIE.Field.InvalidDate",
        "UK.OBIE.Field.Missing",
        "UK.OBIE.Field.Unexpected",
        "UK.OBIE.Header.Invalid",
        "UK.OBIE.Header.Missing",
        "UK.OBIE.Reauthenticate",
        "UK.OBIE.Resource.ConsentMismatch",
        "UK.OBIE.Resource.InvalidConsentStatus",
        "UK.OBIE.Resource.InvalidFormat",
        "UK.OBIE.Resource.NotFound",
        "UK.OBIE.Rules.AfterCutOffDateTime",
        "UK.OBIE.Rules.DuplicateReference",
        "UK.OBIE.Signature.Invalid",
        "UK.OBIE.Signature.InvalidClaim",
        "UK.OBIE.Signature.Malformed",
        "UK.OBIE.Signature.Missing",
        "UK.OBIE.Signature.MissingClaim",
        "UK.OBIE.Signature.Unexpected",
        "UK.OBIE.UnexpectedError",
        "UK.OBIE.Unsupported.AccountIdentifier",
        "UK.OBIE.Unsupported.AccountSecondaryIdentifier",
        "UK.OBIE.Unsupported.Currency",
        "UK.OBIE.Unsupported.Frequency",
        "UK.OBIE.Unsupported.LocalInstrument",
        "UK.OBIE.Unsupported.Scheme"
      ]
    },
    "Message": {
      "description": "A description of the error that occurred. e.g., 'A mandatory field isn't supplied' or 'RequestedExecutionDateTime must be in future'\nOBIE doesn't standardise this field",
      "type": "string",
      "minLength": 1,
      "maxLength": 500
    },
    "Path": {
      "description": "Recommended but optional reference to the JSON Path of the field with error, e.g., Data.Initiation.InstructedAmount.Currency",
      "type": "string",
      "minLength": 1,
      "maxLength": 500
    },
    "Url": {
      "description": "URL to help remediate the problem, or provide more information, or to API Reference, or help etc",
      "type": "string"
    }
  },
  "required": [
    "ErrorCode",
    "Message"
  ],
  "additionalProperties": false,
  "minProperties": 1
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|ErrorCode|string|true|none|Low level textual error code, e.g., UK.OBIE.Field.Missing|
|Message|string|true|none|A description of the error that occurred. e.g., 'A mandatory field isn't supplied' or 'RequestedExecutionDateTime must be in future'<br>OBIE doesn't standardise this field|
|Path|string|false|none|Recommended but optional reference to the JSON Path of the field with error, e.g., Data.Initiation.InstructedAmount.Currency|
|Url|string|false|none|URL to help remediate the problem, or provide more information, or to API Reference, or help etc|

<h2 id="tocS_OBErrorResponse1">OBErrorResponse1</h2>
<!-- backwards compatibility -->
<a id="schemaoberrorresponse1"></a>
<a id="schema_OBErrorResponse1"></a>
<a id="tocSoberrorresponse1"></a>
<a id="tocsoberrorresponse1"></a>

```json
{
  "description": "An array of detail error codes, and messages, and URLs to documentation to help remediation.",
  "type": "object",
  "properties": {
    "Code": {
      "description": "High level textual error code, to help categorize the errors.",
      "type": "string",
      "minLength": 1,
      "maxLength": 40
    },
    "Id": {
      "description": "A unique reference for the error instance, for audit purposes, in case of unknown/unclassified errors.",
      "type": "string",
      "minLength": 1,
      "maxLength": 40
    },
    "Message": {
      "description": "Brief Error message, e.g., 'There is something wrong with the request parameters provided'",
      "type": "string",
      "minLength": 1,
      "maxLength": 500
    },
    "Errors": {
      "items": {
        "type": "object",
        "properties": {
          "ErrorCode": {
            "description": "Low level textual error code, e.g., UK.OBIE.Field.Missing",
            "type": "string",
            "x-namespaced-enum": [
              "UK.OBIE.Field.Expected",
              "UK.OBIE.Field.Invalid",
              "UK.OBIE.Field.InvalidDate",
              "UK.OBIE.Field.Missing",
              "UK.OBIE.Field.Unexpected",
              "UK.OBIE.Header.Invalid",
              "UK.OBIE.Header.Missing",
              "UK.OBIE.Reauthenticate",
              "UK.OBIE.Resource.ConsentMismatch",
              "UK.OBIE.Resource.InvalidConsentStatus",
              "UK.OBIE.Resource.InvalidFormat",
              "UK.OBIE.Resource.NotFound",
              "UK.OBIE.Rules.AfterCutOffDateTime",
              "UK.OBIE.Rules.DuplicateReference",
              "UK.OBIE.Signature.Invalid",
              "UK.OBIE.Signature.InvalidClaim",
              "UK.OBIE.Signature.Malformed",
              "UK.OBIE.Signature.Missing",
              "UK.OBIE.Signature.MissingClaim",
              "UK.OBIE.Signature.Unexpected",
              "UK.OBIE.UnexpectedError",
              "UK.OBIE.Unsupported.AccountIdentifier",
              "UK.OBIE.Unsupported.AccountSecondaryIdentifier",
              "UK.OBIE.Unsupported.Currency",
              "UK.OBIE.Unsupported.Frequency",
              "UK.OBIE.Unsupported.LocalInstrument",
              "UK.OBIE.Unsupported.Scheme"
            ]
          },
          "Message": {
            "description": "A description of the error that occurred. e.g., 'A mandatory field isn't supplied' or 'RequestedExecutionDateTime must be in future'\nOBIE doesn't standardise this field",
            "type": "string",
            "minLength": 1,
            "maxLength": 500
          },
          "Path": {
            "description": "Recommended but optional reference to the JSON Path of the field with error, e.g., Data.Initiation.InstructedAmount.Currency",
            "type": "string",
            "minLength": 1,
            "maxLength": 500
          },
          "Url": {
            "description": "URL to help remediate the problem, or provide more information, or to API Reference, or help etc",
            "type": "string"
          }
        },
        "required": [
          "ErrorCode",
          "Message"
        ],
        "additionalProperties": false,
        "minProperties": 1
      },
      "type": "array",
      "minItems": 1
    }
  },
  "required": [
    "Code",
    "Message",
    "Errors"
  ],
  "additionalProperties": false
}

```

An array of detail error codes, and messages, and URLs to documentation to help remediation.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Code|string|true|none|High level textual error code, to help categorize the errors.|
|Id|string|false|none|A unique reference for the error instance, for audit purposes, in case of unknown/unclassified errors.|
|Message|string|true|none|Brief Error message, e.g., 'There is something wrong with the request parameters provided'|
|Errors|[[OBError1](#schemaoberror1)]|true|none|none|

<h2 id="tocS_OBExternalAccountIdentification4Code">OBExternalAccountIdentification4Code</h2>
<!-- backwards compatibility -->
<a id="schemaobexternalaccountidentification4code"></a>
<a id="schema_OBExternalAccountIdentification4Code"></a>
<a id="tocSobexternalaccountidentification4code"></a>
<a id="tocsobexternalaccountidentification4code"></a>

```json
{
  "description": "Name of the identification scheme, in a coded form as published in an external list.",
  "type": "string",
  "x-namespaced-enum": [
    "UK.OBIE.SortCodeAccountNumber"
  ]
}

```

Name of the identification scheme, in a coded form as published in an external list.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Name of the identification scheme, in a coded form as published in an external list.|

<h2 id="tocS_OBExternalAccountRole1Code">OBExternalAccountRole1Code</h2>
<!-- backwards compatibility -->
<a id="schemaobexternalaccountrole1code"></a>
<a id="schema_OBExternalAccountRole1Code"></a>
<a id="tocSobexternalaccountrole1code"></a>
<a id="tocsobexternalaccountrole1code"></a>

```json
{
  "description": "A party’s role with respect to the related account.",
  "type": "string",
  "x-namespaced-enum": [
    "UK.OBIE.Administrator",
    "UK.OBIE.Beneficiary",
    "UK.OBIE.CustodianForMinor",
    "UK.OBIE.Granter",
    "UK.OBIE.LegalGuardian",
    "UK.OBIE.OtherParty",
    "UK.OBIE.PowerOfAttorney",
    "UK.OBIE.Principal",
    "UK.OBIE.Protector",
    "UK.OBIE.RegisteredShareholderName",
    "UK.OBIE.SecondaryOwner",
    "UK.OBIE.SeniorManagingOfficial",
    "UK.OBIE.Settlor",
    "UK.OBIE.SuccessorOnDeath"
  ]
}

```

A party’s role with respect to the related account.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|A party’s role with respect to the related account.|

<h2 id="tocS_OBExternalAccountSubType1Code">OBExternalAccountSubType1Code</h2>
<!-- backwards compatibility -->
<a id="schemaobexternalaccountsubtype1code"></a>
<a id="schema_OBExternalAccountSubType1Code"></a>
<a id="tocSobexternalaccountsubtype1code"></a>
<a id="tocsobexternalaccountsubtype1code"></a>

```json
{
  "description": "Specifies the sub type of account (product family group).",
  "type": "string",
  "enum": [
    "CreditCard",
    "CurrentAccount",
    "Savings"
  ]
}

```

Specifies the sub type of account (product family group).

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Specifies the sub type of account (product family group).|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|CreditCard|
|*anonymous*|CurrentAccount|
|*anonymous*|Savings|

<h2 id="tocS_OBExternalAccountType1Code">OBExternalAccountType1Code</h2>
<!-- backwards compatibility -->
<a id="schemaobexternalaccounttype1code"></a>
<a id="schema_OBExternalAccountType1Code"></a>
<a id="tocSobexternalaccounttype1code"></a>
<a id="tocsobexternalaccounttype1code"></a>

```json
{
  "description": "Specifies the type of account (personal or business).",
  "type": "string",
  "enum": [
    "Business",
    "Personal"
  ]
}

```

Specifies the type of account (personal or business).

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Specifies the type of account (personal or business).|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|Business|
|*anonymous*|Personal|

<h2 id="tocS_OBExternalDirectDebitStatus1Code">OBExternalDirectDebitStatus1Code</h2>
<!-- backwards compatibility -->
<a id="schemaobexternaldirectdebitstatus1code"></a>
<a id="schema_OBExternalDirectDebitStatus1Code"></a>
<a id="tocSobexternaldirectdebitstatus1code"></a>
<a id="tocsobexternaldirectdebitstatus1code"></a>

```json
{
  "description": "Specifies the status of the direct debit in code form.",
  "type": "string",
  "enum": [
    "Active",
    "Inactive"
  ]
}

```

Specifies the status of the direct debit in code form.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Specifies the status of the direct debit in code form.|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|Active|
|*anonymous*|Inactive|

<h2 id="tocS_OBExternalFinancialInstitutionIdentification4Code">OBExternalFinancialInstitutionIdentification4Code</h2>
<!-- backwards compatibility -->
<a id="schemaobexternalfinancialinstitutionidentification4code"></a>
<a id="schema_OBExternalFinancialInstitutionIdentification4Code"></a>
<a id="tocSobexternalfinancialinstitutionidentification4code"></a>
<a id="tocsobexternalfinancialinstitutionidentification4code"></a>

```json
{
  "description": "Name of the identification scheme, in a coded form as published in an external list.",
  "type": "string",
  "x-namespaced-enum": [
    "UK.OBIE.BICFI"
  ]
}

```

Name of the identification scheme, in a coded form as published in an external list.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Name of the identification scheme, in a coded form as published in an external list.|

<h2 id="tocS_OBExternalSwitchStatusCode">OBExternalSwitchStatusCode</h2>
<!-- backwards compatibility -->
<a id="schemaobexternalswitchstatuscode"></a>
<a id="schema_OBExternalSwitchStatusCode"></a>
<a id="tocSobexternalswitchstatuscode"></a>
<a id="tocsobexternalswitchstatuscode"></a>

```json
{
  "description": "Specifies the switch status for the account, in a coded form.",
  "type": "string",
  "x-namespaced-enum": [
    "UK.CASS.NotSwitced",
    "UK.CASS.SwitchCompleted"
  ]
}

```

Specifies the switch status for the account, in a coded form.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Specifies the switch status for the account, in a coded form.|

<h2 id="tocS_OBExternalLegalStructureType1Code">OBExternalLegalStructureType1Code</h2>
<!-- backwards compatibility -->
<a id="schemaobexternallegalstructuretype1code"></a>
<a id="schema_OBExternalLegalStructureType1Code"></a>
<a id="tocSobexternallegalstructuretype1code"></a>
<a id="tocsobexternallegalstructuretype1code"></a>

```json
{
  "description": "Legal standing of the party.",
  "type": "string",
  "x-namespaced-enum": [
    "UK.OBIE.CIC",
    "UK.OBIE.CIO",
    "UK.OBIE.Charity",
    "UK.OBIE.CoOp",
    "UK.OBIE.GeneralPartnership",
    "UK.OBIE.Individual",
    "UK.OBIE.LimitedLiabilityPartnership",
    "UK.OBIE.LimitedPartnership",
    "UK.OBIE.PrivateLimitedCompany",
    "UK.OBIE.PublicLimitedCompany",
    "UK.OBIE.ScottishLimitedPartnership",
    "UK.OBIE.Sole"
  ]
}

```

Legal standing of the party.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Legal standing of the party.|

<h2 id="tocS_OBExternalPartyType1Code">OBExternalPartyType1Code</h2>
<!-- backwards compatibility -->
<a id="schemaobexternalpartytype1code"></a>
<a id="schema_OBExternalPartyType1Code"></a>
<a id="tocSobexternalpartytype1code"></a>
<a id="tocsobexternalpartytype1code"></a>

```json
{
  "description": "Party type, in a coded form.",
  "type": "string",
  "enum": [
    "Delegate",
    "Joint",
    "Sole"
  ]
}

```

Party type, in a coded form.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Party type, in a coded form.|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|Delegate|
|*anonymous*|Joint|
|*anonymous*|Sole|

<h2 id="tocS_OBExternalScheduleType1Code">OBExternalScheduleType1Code</h2>
<!-- backwards compatibility -->
<a id="schemaobexternalscheduletype1code"></a>
<a id="schema_OBExternalScheduleType1Code"></a>
<a id="tocSobexternalscheduletype1code"></a>
<a id="tocsobexternalscheduletype1code"></a>

```json
{
  "description": "Specifies the scheduled payment date type requested",
  "type": "string",
  "enum": [
    "Arrival",
    "Execution"
  ]
}

```

Specifies the scheduled payment date type requested

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Specifies the scheduled payment date type requested|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|Arrival|
|*anonymous*|Execution|

<h2 id="tocS_OBExternalStandingOrderStatus1Code">OBExternalStandingOrderStatus1Code</h2>
<!-- backwards compatibility -->
<a id="schemaobexternalstandingorderstatus1code"></a>
<a id="schema_OBExternalStandingOrderStatus1Code"></a>
<a id="tocSobexternalstandingorderstatus1code"></a>
<a id="tocsobexternalstandingorderstatus1code"></a>

```json
{
  "description": "Specifies the status of the standing order in code form.",
  "type": "string",
  "enum": [
    "Active",
    "Inactive"
  ]
}

```

Specifies the status of the standing order in code form.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Specifies the status of the standing order in code form.|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|Active|
|*anonymous*|Inactive|

<h2 id="tocS_OBExternalStatementAmountType1Code">OBExternalStatementAmountType1Code</h2>
<!-- backwards compatibility -->
<a id="schemaobexternalstatementamounttype1code"></a>
<a id="schema_OBExternalStatementAmountType1Code"></a>
<a id="tocSobexternalstatementamounttype1code"></a>
<a id="tocsobexternalstatementamounttype1code"></a>

```json
{
  "description": "Amount type, in a coded form.",
  "type": "string",
  "x-namespaced-enum": [
    "UK.OBIE.ArrearsClosingBalance",
    "UK.OBIE.AvailableBalance",
    "UK.OBIE.AverageBalanceWhenInCredit",
    "UK.OBIE.AverageBalanceWhenInDebit",
    "UK.OBIE.AverageDailyBalance",
    "UK.OBIE.BalanceTransferClosingBalance",
    "UK.OBIE.CashClosingBalance",
    "UK.OBIE.ClosingBalance",
    "UK.OBIE.CreditLimit",
    "UK.OBIE.CurrentPayment",
    "UK.OBIE.DirectDebitPaymentDue",
    "UK.OBIE.FSCSInsurance",
    "UK.OBIE.MinimumPaymentDue",
    "UK.OBIE.PendingTransactionsBalance",
    "UK.OBIE.PreviousClosingBalance",
    "UK.OBIE.PreviousPayment",
    "UK.OBIE.PurchaseClosingBalance",
    "UK.OBIE.StartingBalance",
    "UK.OBIE.TotalAdjustments",
    "UK.OBIE.TotalCashAdvances",
    "UK.OBIE.TotalCharges",
    "UK.OBIE.TotalCredits",
    "UK.OBIE.TotalDebits",
    "UK.OBIE.TotalPurchases"
  ]
}

```

Amount type, in a coded form.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Amount type, in a coded form.|

<h2 id="tocS_OBExternalStatementBenefitType1Code">OBExternalStatementBenefitType1Code</h2>
<!-- backwards compatibility -->
<a id="schemaobexternalstatementbenefittype1code"></a>
<a id="schema_OBExternalStatementBenefitType1Code"></a>
<a id="tocSobexternalstatementbenefittype1code"></a>
<a id="tocsobexternalstatementbenefittype1code"></a>

```json
{
  "description": "Benefit type, in a coded form.",
  "type": "string",
  "x-namespaced-enum": [
    "UK.OBIE.Cashback",
    "UK.OBIE.Insurance",
    "UK.OBIE.TravelDiscount",
    "UK.OBIE.TravelInsurance"
  ]
}

```

Benefit type, in a coded form.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Benefit type, in a coded form.|

<h2 id="tocS_OBExternalStatementDateTimeType1Code">OBExternalStatementDateTimeType1Code</h2>
<!-- backwards compatibility -->
<a id="schemaobexternalstatementdatetimetype1code"></a>
<a id="schema_OBExternalStatementDateTimeType1Code"></a>
<a id="tocSobexternalstatementdatetimetype1code"></a>
<a id="tocsobexternalstatementdatetimetype1code"></a>

```json
{
  "description": "Date time type, in a coded form.",
  "type": "string",
  "x-namespaced-enum": [
    "UK.OBIE.BalanceTransferPromoEnd",
    "UK.OBIE.DirectDebitDue",
    "UK.OBIE.LastPayment",
    "UK.OBIE.LastStatement",
    "UK.OBIE.NextStatement",
    "UK.OBIE.PaymentDue",
    "UK.OBIE.PurchasePromoEnd",
    "UK.OBIE.StatementAvailable"
  ]
}

```

Date time type, in a coded form.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Date time type, in a coded form.|

<h2 id="tocS_OBExternalStatementFeeFrequency1Code">OBExternalStatementFeeFrequency1Code</h2>
<!-- backwards compatibility -->
<a id="schemaobexternalstatementfeefrequency1code"></a>
<a id="schema_OBExternalStatementFeeFrequency1Code"></a>
<a id="tocSobexternalstatementfeefrequency1code"></a>
<a id="tocsobexternalstatementfeefrequency1code"></a>

```json
{
  "description": "How frequently the fee is applied to the Account.",
  "type": "string",
  "x-namespaced-enum": [
    "UK.OBIE.ChargingPeriod",
    "UK.OBIE.PerTransactionAmount",
    "UK.OBIE.PerTransactionPercentage",
    "UK.OBIE.Quarterly",
    "UK.OBIE.StatementMonthly",
    "UK.OBIE.Weekly"
  ]
}

```

How frequently the fee is applied to the Account.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|How frequently the fee is applied to the Account.|

<h2 id="tocS_OBExternalStatementFeeRateType1Code">OBExternalStatementFeeRateType1Code</h2>
<!-- backwards compatibility -->
<a id="schemaobexternalstatementfeeratetype1code"></a>
<a id="schema_OBExternalStatementFeeRateType1Code"></a>
<a id="tocSobexternalstatementfeeratetype1code"></a>
<a id="tocsobexternalstatementfeeratetype1code"></a>

```json
{
  "description": "Description that may be available for the statement fee rate type.",
  "type": "string",
  "x-namespaced-enum": [
    "UK.OBIE.AER",
    "UK.OBIE.EAR"
  ]
}

```

Description that may be available for the statement fee rate type.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Description that may be available for the statement fee rate type.|

<h2 id="tocS_OBExternalStatementFeeType1Code">OBExternalStatementFeeType1Code</h2>
<!-- backwards compatibility -->
<a id="schemaobexternalstatementfeetype1code"></a>
<a id="schema_OBExternalStatementFeeType1Code"></a>
<a id="tocSobexternalstatementfeetype1code"></a>
<a id="tocsobexternalstatementfeetype1code"></a>

```json
{
  "description": "Fee type, in a coded form.",
  "type": "string",
  "x-namespaced-enum": [
    "UK.OBIE.Annual",
    "UK.OBIE.BalanceTransfer",
    "UK.OBIE.CashAdvance",
    "UK.OBIE.CashTransaction",
    "UK.OBIE.ForeignCashTransaction",
    "UK.OBIE.ForeignTransaction",
    "UK.OBIE.Gambling",
    "UK.OBIE.LatePayment",
    "UK.OBIE.MoneyTransfer",
    "UK.OBIE.Monthly",
    "UK.OBIE.Overlimit",
    "UK.OBIE.PostalOrder",
    "UK.OBIE.PrizeEntry",
    "UK.OBIE.StatementCopy",
    "UK.OBIE.Total"
  ]
}

```

Fee type, in a coded form.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Fee type, in a coded form.|

<h2 id="tocS_OBExternalStatementInterestFrequency1Code">OBExternalStatementInterestFrequency1Code</h2>
<!-- backwards compatibility -->
<a id="schemaobexternalstatementinterestfrequency1code"></a>
<a id="schema_OBExternalStatementInterestFrequency1Code"></a>
<a id="tocSobexternalstatementinterestfrequency1code"></a>
<a id="tocsobexternalstatementinterestfrequency1code"></a>

```json
{
  "description": "Specifies the statement fee type requested",
  "type": "string",
  "x-namespaced-enum": [
    "UK.OBIE.Daily",
    "UK.OBIE.HalfYearly",
    "UK.OBIE.Monthly",
    "UK.OBIE.PerStatementDate",
    "UK.OBIE.Quarterly",
    "UK.OBIE.Weekly",
    "UK.OBIE.Yearly"
  ]
}

```

Specifies the statement fee type requested

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Specifies the statement fee type requested|

<h2 id="tocS_OBExternalStatementInterestRateType1Code">OBExternalStatementInterestRateType1Code</h2>
<!-- backwards compatibility -->
<a id="schemaobexternalstatementinterestratetype1code"></a>
<a id="schema_OBExternalStatementInterestRateType1Code"></a>
<a id="tocSobexternalstatementinterestratetype1code"></a>
<a id="tocsobexternalstatementinterestratetype1code"></a>

```json
{
  "description": "Description that may be available for the statement Interest rate type.",
  "type": "string",
  "x-namespaced-enum": [
    "UK.OBIE.BOEBaseRate",
    "UK.OBIE.FixedRate",
    "UK.OBIE.Gross",
    "UK.OBIE.LoanProviderBaseRate",
    "UK.OBIE.Net"
  ]
}

```

Description that may be available for the statement Interest rate type.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Description that may be available for the statement Interest rate type.|

<h2 id="tocS_OBExternalStatementInterestType1Code">OBExternalStatementInterestType1Code</h2>
<!-- backwards compatibility -->
<a id="schemaobexternalstatementinteresttype1code"></a>
<a id="schema_OBExternalStatementInterestType1Code"></a>
<a id="tocSobexternalstatementinteresttype1code"></a>
<a id="tocsobexternalstatementinteresttype1code"></a>

```json
{
  "description": "Interest amount type, in a coded form.",
  "type": "string",
  "x-namespaced-enum": [
    "UK.OBIE.BalanceTransfer",
    "UK.OBIE.Cash",
    "UK.OBIE.EstimatedNext",
    "UK.OBIE.Purchase",
    "UK.OBIE.Total"
  ]
}

```

Interest amount type, in a coded form.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Interest amount type, in a coded form.|

<h2 id="tocS_OBExternalStatementRateType1Code">OBExternalStatementRateType1Code</h2>
<!-- backwards compatibility -->
<a id="schemaobexternalstatementratetype1code"></a>
<a id="schema_OBExternalStatementRateType1Code"></a>
<a id="tocSobexternalstatementratetype1code"></a>
<a id="tocsobexternalstatementratetype1code"></a>

```json
{
  "description": "Statement rate type, in a coded form.",
  "type": "string",
  "x-namespaced-enum": [
    "UK.OBIE.AnnualBalanceTransfer",
    "UK.OBIE.AnnualBalanceTransferAfterPromo",
    "UK.OBIE.AnnualBalanceTransferPromo",
    "UK.OBIE.AnnualCash",
    "UK.OBIE.AnnualPurchase",
    "UK.OBIE.AnnualPurchaseAfterPromo",
    "UK.OBIE.AnnualPurchasePromo",
    "UK.OBIE.MonthlyBalanceTransfer",
    "UK.OBIE.MonthlyCash",
    "UK.OBIE.MonthlyPurchase"
  ]
}

```

Statement rate type, in a coded form.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Statement rate type, in a coded form.|

<h2 id="tocS_OBExternalStatementType1Code">OBExternalStatementType1Code</h2>
<!-- backwards compatibility -->
<a id="schemaobexternalstatementtype1code"></a>
<a id="schema_OBExternalStatementType1Code"></a>
<a id="tocSobexternalstatementtype1code"></a>
<a id="tocsobexternalstatementtype1code"></a>

```json
{
  "description": "Statement type, in a coded form.",
  "type": "string",
  "enum": [
    "AccountClosure",
    "AccountOpening",
    "Annual",
    "Interim",
    "RegularPeriodic"
  ]
}

```

Statement type, in a coded form.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Statement type, in a coded form.|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|AccountClosure|
|*anonymous*|AccountOpening|
|*anonymous*|Annual|
|*anonymous*|Interim|
|*anonymous*|RegularPeriodic|

<h2 id="tocS_OBExternalStatementValueType1Code">OBExternalStatementValueType1Code</h2>
<!-- backwards compatibility -->
<a id="schemaobexternalstatementvaluetype1code"></a>
<a id="schema_OBExternalStatementValueType1Code"></a>
<a id="tocSobexternalstatementvaluetype1code"></a>
<a id="tocsobexternalstatementvaluetype1code"></a>

```json
{
  "description": "Statement value type, in a coded form.",
  "type": "string",
  "x-namespaced-enum": [
    "UK.OBIE.AirMilesPoints",
    "UK.OBIE.AirMilesPointsBalance",
    "UK.OBIE.Credits",
    "UK.OBIE.Debits",
    "UK.OBIE.HotelPoints",
    "UK.OBIE.HotelPointsBalance",
    "UK.OBIE.RetailShoppingPoints",
    "UK.OBIE.RetailShoppingPointsBalance"
  ]
}

```

Statement value type, in a coded form.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Statement value type, in a coded form.|

<h2 id="tocS_OBMerchantDetails1">OBMerchantDetails1</h2>
<!-- backwards compatibility -->
<a id="schemaobmerchantdetails1"></a>
<a id="schema_OBMerchantDetails1"></a>
<a id="tocSobmerchantdetails1"></a>
<a id="tocsobmerchantdetails1"></a>

```json
{
  "type": "object",
  "description": "Details of the merchant involved in the transaction.",
  "properties": {
    "MerchantCategoryCode": {
      "description": "Category code conform to ISO 18245, related to the type of services or goods the merchant provides for the transaction.",
      "type": "string",
      "minLength": 3,
      "maxLength": 4
    }
  }
}

```

Details of the merchant involved in the transaction.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|MerchantCategoryCode|string|false|none|Category code conform to ISO 18245, related to the type of services or goods the merchant provides for the transaction.|

<h2 id="tocS_OBPCAData1">OBPCAData1</h2>
<!-- backwards compatibility -->
<a id="schemaobpcadata1"></a>
<a id="schema_OBPCAData1"></a>
<a id="tocSobpcadata1"></a>
<a id="tocsobpcadata1"></a>

```json
{
  "type": "object",
  "title": "PCA",
  "properties": {
    "ProductDetails": {
      "type": "object",
      "title": "ProductDetails",
      "properties": {
        "Segment": {
          "description": "Market segmentation is a marketing term referring to the aggregating of prospective buyers into groups, or segments, that have common needs and respond similarly to a marketing action. Market segmentation enables companies to target different categories of consumers who perceive the full value of certain products and services differently from one another.\n\nRead more: Market Segmentation http://www.investopedia.com/terms/m/marketsegmentation.asp#ixzz4gfEEalTd \nWith respect to PCA products, they are segmented in relation to different markets that they wish to focus on. ",
          "title": "Segment",
          "type": "array",
          "items": {
            "description": "Market segmentation is a marketing term referring to the aggregating of prospective buyers into groups, or segments, that have common needs and respond similarly to a marketing action. Market segmentation enables companies to target different categories of consumers who perceive the full value of certain products and services differently from one another.\n\nRead more: Market Segmentation http://www.investopedia.com/terms/m/marketsegmentation.asp#ixzz4gfEEalTd \nWith respect to PCA products, they are segmented in relation to different markets that they wish to focus on. ",
            "type": "string",
            "enum": [
              "Basic",
              "General",
              "Graduate",
              "Other",
              "Premium",
              "Student",
              "Youth"
            ]
          }
        },
        "MonthlyMaximumCharge": {
          "description": "The maximum relevant charges that could accrue as defined fully in Part 7 of the CMA order",
          "title": "MonthlyMaximumCharge",
          "type": "string",
          "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
        }
      },
      "additionalProperties": false
    },
    "CreditInterest": {
      "description": "Details about the interest that may be payable to the PCA account holders",
      "type": "object",
      "title": "CreditInterest",
      "properties": {
        "TierBandSet": {
          "description": "The group of tiers or bands for which credit interest can be applied.",
          "type": "array",
          "title": "TierBandSet",
          "items": {
            "description": "The group of tiers or bands for which credit interest can be applied.",
            "type": "object",
            "properties": {
              "TierBandMethod": {
                "description": "The methodology of how credit interest is charged. It can be:-\n\n1. Banded\nInterest rates are banded. i.e. Increasing rate on whole balance as balance increases.\n\n2. Tiered\nInterest rates are tiered. i.e. increasing rate for each tier as balance increases, but interest paid on tier fixed for that tier and not on whole balance.\n\n3. Whole\nThe same interest rate is applied irrespective of the PCA balance",
                "title": "TierBandMethod",
                "type": "string",
                "enum": [
                  "Tiered",
                  "Whole"
                ]
              },
              "CalculationMethod": {
                "description": "Methods of calculating interest",
                "title": "CalculationMethod",
                "type": "string",
                "enum": [
                  "SimpleInterest"
                ]
              },
              "Destination": {
                "description": "Describes whether accrued interest is payable only to the PCA or to another bank account",
                "title": "Destination",
                "type": "string",
                "enum": [
                  "PayAway",
                  "SelfCredit"
                ]
              },
              "TierBand": {
                "description": "Tier Band Details",
                "type": "array",
                "title": "TierBand",
                "items": {
                  "description": "Tier Band Details",
                  "type": "object",
                  "properties": {
                    "Identification": {
                      "description": "Unique and unambiguous identification of a  Tier Band for a PCA.",
                      "title": "Identification",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 35
                    },
                    "TierValueMinimum": {
                      "description": "Minimum deposit value for which the credit interest tier applies.",
                      "title": "TierValueMinimum",
                      "type": "string",
                      "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                    },
                    "TierValueMaximum": {
                      "description": "Maximum deposit value for which the credit interest tier applies.",
                      "title": "TierValueMaximum",
                      "type": "string",
                      "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                    },
                    "CalculationFrequency": {
                      "description": "How often is credit interest calculated for the account.",
                      "title": "CalculationFrequency",
                      "type": "string",
                      "enum": [
                        "Daily",
                        "HalfYearly",
                        "Monthly",
                        "Quarterly",
                        "Weekly",
                        "Yearly"
                      ]
                    },
                    "ApplicationFrequency": {
                      "description": "How often is interest applied to the PCA for this tier/band i.e. how often the financial institution pays accumulated interest to the customer's PCA.",
                      "title": "ApplicationFrequency",
                      "type": "string",
                      "enum": [
                        "Daily",
                        "HalfYearly",
                        "Monthly",
                        "Quarterly",
                        "Weekly",
                        "Yearly"
                      ]
                    },
                    "FixedVariableInterestRateType": {
                      "description": "Type of interest rate, Fixed or Variable",
                      "title": "FixedVariableInterestRateType",
                      "type": "string",
                      "enum": [
                        "Variable"
                      ]
                    },
                    "AER": {
                      "description": "The annual equivalent rate (AER) is interest that is calculated under the assumption that any interest paid is combined with the original balance and the next interest payment will be based on the slightly higher account balance. Overall, this means that interest can be compounded several times in a year depending on the number of times that interest payments are made. \n\nRead more: Annual Equivalent Rate (AER) http://www.investopedia.com/terms/a/aer.asp#ixzz4gfR7IO1A",
                      "title": "AER",
                      "type": "string",
                      "pattern": "^(-?\\d{1,3}){1}(\\.\\d{1,4}){0,1}$"
                    }
                  },
                  "required": [
                    "TierValueMinimum",
                    "ApplicationFrequency",
                    "FixedVariableInterestRateType",
                    "AER"
                  ]
                },
                "minItems": 1
              }
            },
            "required": [
              "TierBandMethod",
              "TierBand"
            ]
          },
          "minItems": 1
        }
      },
      "additionalProperties": false,
      "required": [
        "TierBandSet"
      ]
    },
    "Overdraft": {
      "description": "Details about Overdraft rates, fees & charges",
      "type": "object",
      "title": "Overdraft",
      "properties": {
        "OverdraftTierBandSet": {
          "description": "Tier band set details",
          "type": "array",
          "title": "OverdraftTierBandSet",
          "items": {
            "description": "Tier band set details",
            "type": "object",
            "properties": {
              "TierBandMethod": {
                "description": "The methodology of how overdraft is charged. It can be:\n'Whole'  Where the same charge/rate is applied to the entirety of the overdraft balance (where charges are applicable). \n'Tiered' Where different charges/rates are applied dependent on overdraft maximum and minimum balance amount tiers defined by the lending financial organisation\n'Banded' Where different charges/rates are applied dependent on overdraft maximum and minimum balance amount bands defined by a government organisation.",
                "title": "TierBandMethod",
                "type": "string",
                "enum": [
                  "Tiered",
                  "Whole"
                ]
              },
              "Identification": {
                "description": "Unique and unambiguous identification of a  Tier Band for a overdraft product.",
                "title": "Identification",
                "type": "string",
                "minLength": 1,
                "maxLength": 35
              },
              "OverdraftTierBand": {
                "description": "Provides overdraft details for a specific tier or band",
                "type": "array",
                "title": "OverdraftTierBand",
                "items": {
                  "description": "Provides overdraft details for a specific tier or band",
                  "type": "object",
                  "properties": {
                    "Identification": {
                      "description": "Unique and unambiguous identification of a  Tier Band for a overdraft.",
                      "title": "Identification",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 35
                    },
                    "TierValueMin": {
                      "description": "Minimum value of Overdraft Tier/Band",
                      "title": "TierValueMin",
                      "type": "string",
                      "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                    },
                    "TierValueMax": {
                      "description": "Maximum value of Overdraft Tier/Band",
                      "title": "TierValueMax",
                      "type": "string",
                      "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                    },
                    "EAR": {
                      "description": "EAR means Effective Annual Rate and/or Equivalent Annual Rate (frequently\nused interchangeably), being the actual annual interest rate of an Overdraft.",
                      "title": "EAR",
                      "type": "string",
                      "pattern": "^(-?\\d{1,3}){1}(\\.\\d{1,4}){0,1}$"
                    },
                    "OverdraftFeesCharges": {
                      "description": "Overdraft fees and charges",
                      "type": "array",
                      "title": "OverdraftFeesCharges",
                      "items": {
                        "description": "Overdraft fees and charges",
                        "type": "object",
                        "properties": {
                          "OverdraftFeeChargeCap": {
                            "description": "Details about any caps (maximum charges) that apply to a particular fee/charge",
                            "type": "array",
                            "title": "OverdraftFeeChargeCap",
                            "items": {
                              "description": "Details about any caps (maximum charges) that apply to a particular fee/charge",
                              "type": "object",
                              "properties": {
                                "FeeType": {
                                  "description": "Fee/charge type which is being capped",
                                  "title": "FeeType",
                                  "type": "array",
                                  "items": {
                                    "description": "Overdraft fee type",
                                    "type": "string",
                                    "enum": [
                                      "ArrangedOverdraft"
                                    ]
                                  },
                                  "minItems": 1
                                },
                                "MinMaxType": {
                                  "description": "Indicates that this is the minimum/ maximum fee/charge that can be applied by the financial institution",
                                  "title": "MinMaxType",
                                  "type": "string",
                                  "enum": [
                                    "Minimum",
                                    "Maximum"
                                  ]
                                },
                                "FeeCapAmount": {
                                  "description": "Cap amount charged for a fee/charge",
                                  "title": "FeeCapAmount",
                                  "type": "string",
                                  "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                                },
                                "CappingPeriod": {
                                  "description": "Period e.g. day, week, month etc. for which the fee/charge is capped",
                                  "title": "CappingPeriod",
                                  "type": "string",
                                  "enum": [
                                    "Day",
                                    "Half Year",
                                    "Month",
                                    "Quarter",
                                    "Week",
                                    "Year"
                                  ]
                                }
                              },
                              "required": [
                                "FeeType",
                                "MinMaxType"
                              ]
                            }
                          },
                          "OverdraftFeeChargeDetail": {
                            "description": "Details about the fees/charges",
                            "type": "array",
                            "title": "OverdraftFeeChargeDetail",
                            "items": {
                              "description": "Details about the fees/charges",
                              "type": "object",
                              "properties": {
                                "FeeType": {
                                  "description": "Overdraft fee type",
                                  "title": "FeeType",
                                  "type": "string",
                                  "enum": [
                                    "ArrangedOverdraft"
                                  ]
                                },
                                "FeeRate": {
                                  "description": "Rate charged for overdraft fee/charge (where it is charged in terms of a rate rather than an amount)",
                                  "title": "FeeRate",
                                  "type": "string",
                                  "pattern": "^(-?\\d{1,3}){1}(\\.\\d{1,4}){0,1}$"
                                },
                                "ApplicationFrequency": {
                                  "description": "Frequency at which the overdraft charge is applied to the account",
                                  "title": "ApplicationFrequency",
                                  "type": "string",
                                  "enum": [
                                    "Daily",
                                    "Monthly",
                                    "Quarterly",
                                    "SixMonthly",
                                    "Weekly",
                                    "Yearly"
                                  ]
                                },
                                "CalculationFrequency": {
                                  "description": "How often is the overdraft fee/charge calculated for the account.",
                                  "title": "CalculationFrequency",
                                  "type": "string",
                                  "enum": [
                                    "Daily",
                                    "Monthly",
                                    "Quarterly",
                                    "SixMonthly",
                                    "Weekly",
                                    "Yearly"
                                  ]
                                }
                              },
                              "required": [
                                "FeeType",
                                "ApplicationFrequency"
                              ]
                            },
                            "minItems": 1
                          }
                        },
                        "required": [
                          "OverdraftFeeChargeDetail"
                        ]
                      }
                    }
                  },
                  "required": [
                    "TierValueMin"
                  ]
                },
                "minItems": 1
              }
            },
            "required": [
              "TierBandMethod",
              "OverdraftTierBand"
            ]
          },
          "minItems": 1
        }
      },
      "additionalProperties": false,
      "required": [
        "OverdraftTierBandSet"
      ]
    }
  },
  "additionalProperties": false
}

```

PCA

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|ProductDetails|object|false|none|none|
|» Segment|[string]|false|none|Market segmentation is a marketing term referring to the aggregating of prospective buyers into groups, or segments, that have common needs and respond similarly to a marketing action. Market segmentation enables companies to target different categories of consumers who perceive the full value of certain products and services differently from one another.<br><br>Read more: Market Segmentation http://www.investopedia.com/terms/m/marketsegmentation.asp#ixzz4gfEEalTd <br>With respect to PCA products, they are segmented in relation to different markets that they wish to focus on.|
|» MonthlyMaximumCharge|string|false|none|The maximum relevant charges that could accrue as defined fully in Part 7 of the CMA order|
|CreditInterest|object|false|none|Details about the interest that may be payable to the PCA account holders|
|» TierBandSet|[object]|true|none|The group of tiers or bands for which credit interest can be applied.|
|»» TierBandMethod|string|true|none|The methodology of how credit interest is charged. It can be:-<br><br>1. Banded<br>Interest rates are banded. i.e. Increasing rate on whole balance as balance increases.<br><br>2. Tiered<br>Interest rates are tiered. i.e. increasing rate for each tier as balance increases, but interest paid on tier fixed for that tier and not on whole balance.<br><br>3. Whole<br>The same interest rate is applied irrespective of the PCA balance|
|»» CalculationMethod|string|false|none|Methods of calculating interest|
|»» Destination|string|false|none|Describes whether accrued interest is payable only to the PCA or to another bank account|
|»» TierBand|[object]|true|none|Tier Band Details|
|»»» Identification|string|false|none|Unique and unambiguous identification of a  Tier Band for a PCA.|
|»»» TierValueMinimum|string|true|none|Minimum deposit value for which the credit interest tier applies.|
|»»» TierValueMaximum|string|false|none|Maximum deposit value for which the credit interest tier applies.|
|»»» CalculationFrequency|string|false|none|How often is credit interest calculated for the account.|
|»»» ApplicationFrequency|string|true|none|How often is interest applied to the PCA for this tier/band i.e. how often the financial institution pays accumulated interest to the customer's PCA.|
|»»» FixedVariableInterestRateType|string|true|none|Type of interest rate, Fixed or Variable|
|»»» AER|string|true|none|The annual equivalent rate (AER) is interest that is calculated under the assumption that any interest paid is combined with the original balance and the next interest payment will be based on the slightly higher account balance. Overall, this means that interest can be compounded several times in a year depending on the number of times that interest payments are made. <br><br>Read more: Annual Equivalent Rate (AER) http://www.investopedia.com/terms/a/aer.asp#ixzz4gfR7IO1A|
|Overdraft|object|false|none|Details about Overdraft rates, fees & charges|
|» OverdraftTierBandSet|[object]|true|none|Tier band set details|
|»» TierBandMethod|string|true|none|The methodology of how overdraft is charged. It can be:<br>'Whole'  Where the same charge/rate is applied to the entirety of the overdraft balance (where charges are applicable). <br>'Tiered' Where different charges/rates are applied dependent on overdraft maximum and minimum balance amount tiers defined by the lending financial organisation<br>'Banded' Where different charges/rates are applied dependent on overdraft maximum and minimum balance amount bands defined by a government organisation.|
|»» Identification|string|false|none|Unique and unambiguous identification of a  Tier Band for a overdraft product.|
|»» OverdraftTierBand|[object]|true|none|Provides overdraft details for a specific tier or band|
|»»» Identification|string|false|none|Unique and unambiguous identification of a  Tier Band for a overdraft.|
|»»» TierValueMin|string|true|none|Minimum value of Overdraft Tier/Band|
|»»» TierValueMax|string|false|none|Maximum value of Overdraft Tier/Band|
|»»» EAR|string|false|none|EAR means Effective Annual Rate and/or Equivalent Annual Rate (frequently<br>used interchangeably), being the actual annual interest rate of an Overdraft.|
|»»» OverdraftFeesCharges|[object]|false|none|Overdraft fees and charges|
|»»»» OverdraftFeeChargeCap|[object]|false|none|Details about any caps (maximum charges) that apply to a particular fee/charge|
|»»»»» FeeType|[string]|true|none|Fee/charge type which is being capped|
|»»»»» MinMaxType|string|true|none|Indicates that this is the minimum/ maximum fee/charge that can be applied by the financial institution|
|»»»»» FeeCapAmount|string|false|none|Cap amount charged for a fee/charge|
|»»»»» CappingPeriod|string|false|none|Period e.g. day, week, month etc. for which the fee/charge is capped|
|»»»» OverdraftFeeChargeDetail|[object]|true|none|Details about the fees/charges|
|»»»»» FeeType|string|true|none|Overdraft fee type|
|»»»»» FeeRate|string|false|none|Rate charged for overdraft fee/charge (where it is charged in terms of a rate rather than an amount)|
|»»»»» ApplicationFrequency|string|true|none|Frequency at which the overdraft charge is applied to the account|
|»»»»» CalculationFrequency|string|false|none|How often is the overdraft fee/charge calculated for the account.|

#### Enumerated Values

|Property|Value|
|---|---|
|TierBandMethod|Tiered|
|TierBandMethod|Whole|
|CalculationMethod|SimpleInterest|
|Destination|PayAway|
|Destination|SelfCredit|
|CalculationFrequency|Daily|
|CalculationFrequency|HalfYearly|
|CalculationFrequency|Monthly|
|CalculationFrequency|Quarterly|
|CalculationFrequency|Weekly|
|CalculationFrequency|Yearly|
|ApplicationFrequency|Daily|
|ApplicationFrequency|HalfYearly|
|ApplicationFrequency|Monthly|
|ApplicationFrequency|Quarterly|
|ApplicationFrequency|Weekly|
|ApplicationFrequency|Yearly|
|FixedVariableInterestRateType|Variable|
|TierBandMethod|Tiered|
|TierBandMethod|Whole|
|MinMaxType|Minimum|
|MinMaxType|Maximum|
|CappingPeriod|Day|
|CappingPeriod|Half Year|
|CappingPeriod|Month|
|CappingPeriod|Quarter|
|CappingPeriod|Week|
|CappingPeriod|Year|
|FeeType|ArrangedOverdraft|
|ApplicationFrequency|Daily|
|ApplicationFrequency|Monthly|
|ApplicationFrequency|Quarterly|
|ApplicationFrequency|SixMonthly|
|ApplicationFrequency|Weekly|
|ApplicationFrequency|Yearly|
|CalculationFrequency|Daily|
|CalculationFrequency|Monthly|
|CalculationFrequency|Quarterly|
|CalculationFrequency|SixMonthly|
|CalculationFrequency|Weekly|
|CalculationFrequency|Yearly|

<h2 id="tocS_OBParty2">OBParty2</h2>
<!-- backwards compatibility -->
<a id="schemaobparty2"></a>
<a id="schema_OBParty2"></a>
<a id="tocSobparty2"></a>
<a id="tocsobparty2"></a>

```json
{
  "type": "object",
  "required": [
    "PartyId"
  ],
  "properties": {
    "PartyId": {
      "description": "A unique and immutable identifier used to identify the customer resource. This identifier has no meaning to the account owner.",
      "type": "string",
      "minLength": 1,
      "maxLength": 40
    },
    "PartyType": {
      "description": "Party type, in a coded form.",
      "type": "string",
      "enum": [
        "Delegate",
        "Joint",
        "Sole"
      ]
    },
    "Name": {
      "description": "Name by which a party is known and which is usually used to identify that party.",
      "type": "string",
      "minLength": 1,
      "maxLength": 350
    },
    "FullLegalName": {
      "description": "Specifies a character string with a maximum length of 350 characters.",
      "type": "string",
      "minLength": 1,
      "maxLength": 350
    },
    "EmailAddress": {
      "description": "Address for electronic mail (e-mail).",
      "type": "string",
      "minLength": 1,
      "maxLength": 256
    },
    "Relationships": {
      "type": "object",
      "description": "The Party's relationships with other resources.",
      "properties": {
        "Account": {
          "type": "object",
          "required": [
            "Related",
            "Id"
          ],
          "description": "Relationship to the Account resource.",
          "properties": {
            "Related": {
              "description": "Absolute URI to the related resource.",
              "type": "string",
              "format": "uri"
            },
            "Id": {
              "description": "Unique identification as assigned by the ASPSP to uniquely identify the related resource.",
              "type": "string",
              "minLength": 1,
              "maxLength": 40
            }
          }
        }
      }
    },
    "Address": {
      "type": "array",
      "items": {
        "type": "object",
        "description": "Postal address of a party.",
        "required": [
          "Country"
        ],
        "properties": {
          "AddressType": {
            "description": "Identifies the nature of the postal address.",
            "type": "string",
            "enum": [
              "Business",
              "Correspondence",
              "DeliveryTo",
              "MailTo",
              "POBox",
              "Postal",
              "Residential",
              "Statement"
            ]
          },
          "AddressLine": {
            "type": "array",
            "items": {
              "description": "Information that locates and identifies a specific address, as defined by postal services, that is presented in free format text.",
              "type": "string",
              "minLength": 1,
              "maxLength": 70
            },
            "minItems": 0,
            "maxItems": 5
          },
          "StreetName": {
            "description": "Name of a street or thoroughfare.",
            "type": "string",
            "minLength": 1,
            "maxLength": 70
          },
          "BuildingNumber": {
            "description": "Number that identifies the position of a building on a street.",
            "type": "string",
            "minLength": 1,
            "maxLength": 16
          },
          "PostCode": {
            "description": "Identifier consisting of a group of letters and/or numbers that is added to a postal address to assist the sorting of mail.",
            "type": "string",
            "minLength": 1,
            "maxLength": 16
          },
          "TownName": {
            "description": "Name of a built-up area, with defined boundaries, and a local government.",
            "type": "string",
            "minLength": 1,
            "maxLength": 35
          },
          "CountrySubDivision": {
            "description": "Identifies a subdivision of a country eg, state, region, county.",
            "type": "string",
            "minLength": 1,
            "maxLength": 35
          },
          "Country": {
            "description": "Nation with its own government, occupying a particular territory.",
            "type": "string",
            "pattern": "^[A-Z]{2,2}$"
          }
        }
      }
    }
  },
  "additionalProperties": false
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|PartyId|[PartyId](#schemapartyid)|true|none|A unique and immutable identifier used to identify the customer resource. This identifier has no meaning to the account owner.|
|PartyType|[OBExternalPartyType1Code](#schemaobexternalpartytype1code)|false|none|Party type, in a coded form.|
|Name|[Name_3](#schemaname_3)|false|none|Name by which a party is known and which is usually used to identify that party.|
|FullLegalName|[FullLegalName](#schemafulllegalname)|false|none|Specifies a character string with a maximum length of 350 characters.|
|EmailAddress|[EmailAddress](#schemaemailaddress)|false|none|Address for electronic mail (e-mail).|
|Relationships|[OBPartyRelationships1](#schemaobpartyrelationships1)|false|none|The Party's relationships with other resources.|
|Address|[object]|false|none|none|
|» AddressType|[OBAddressTypeCode](#schemaobaddresstypecode)|false|none|Identifies the nature of the postal address.|
|» AddressLine|[string]|false|none|none|
|» StreetName|[StreetName](#schemastreetname)|false|none|Name of a street or thoroughfare.|
|» BuildingNumber|[BuildingNumber](#schemabuildingnumber)|false|none|Number that identifies the position of a building on a street.|
|» PostCode|[PostCode](#schemapostcode)|false|none|Identifier consisting of a group of letters and/or numbers that is added to a postal address to assist the sorting of mail.|
|» TownName|[TownName](#schematownname)|false|none|Name of a built-up area, with defined boundaries, and a local government.|
|» CountrySubDivision|[CountrySubDivision](#schemacountrysubdivision)|false|none|Identifies a subdivision of a country eg, state, region, county.|
|» Country|[CountryCode](#schemacountrycode)|true|none|Nation with its own government, occupying a particular territory.|

<h2 id="tocS_OBPartyRelationships1">OBPartyRelationships1</h2>
<!-- backwards compatibility -->
<a id="schemaobpartyrelationships1"></a>
<a id="schema_OBPartyRelationships1"></a>
<a id="tocSobpartyrelationships1"></a>
<a id="tocsobpartyrelationships1"></a>

```json
{
  "type": "object",
  "description": "The Party's relationships with other resources.",
  "properties": {
    "Account": {
      "type": "object",
      "required": [
        "Related",
        "Id"
      ],
      "description": "Relationship to the Account resource.",
      "properties": {
        "Related": {
          "description": "Absolute URI to the related resource.",
          "type": "string",
          "format": "uri"
        },
        "Id": {
          "description": "Unique identification as assigned by the ASPSP to uniquely identify the related resource.",
          "type": "string",
          "minLength": 1,
          "maxLength": 40
        }
      }
    }
  }
}

```

The Party's relationships with other resources.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Account|object|false|none|Relationship to the Account resource.|
|» Related|string(uri)|true|none|Absolute URI to the related resource.|
|» Id|string|true|none|Unique identification as assigned by the ASPSP to uniquely identify the related resource.|

<h2 id="tocS_OBPostalAddress6">OBPostalAddress6</h2>
<!-- backwards compatibility -->
<a id="schemaobpostaladdress6"></a>
<a id="schema_OBPostalAddress6"></a>
<a id="tocSobpostaladdress6"></a>
<a id="tocsobpostaladdress6"></a>

```json
{
  "type": "object",
  "description": "Information that locates and identifies a specific address, as defined by postal services.",
  "properties": {
    "AddressType": {
      "description": "Identifies the nature of the postal address.",
      "type": "string",
      "enum": [
        "Business",
        "Correspondence",
        "DeliveryTo",
        "MailTo",
        "POBox",
        "Postal",
        "Residential",
        "Statement"
      ]
    },
    "Department": {
      "description": "Identification of a division of a large organisation or building.",
      "type": "string",
      "minLength": 1,
      "maxLength": 70
    },
    "SubDepartment": {
      "description": "Identification of a sub-division of a large organisation or building.",
      "type": "string",
      "minLength": 1,
      "maxLength": 70
    },
    "StreetName": {
      "description": "Name of a street or thoroughfare.",
      "type": "string",
      "minLength": 1,
      "maxLength": 70
    },
    "BuildingNumber": {
      "description": "Number that identifies the position of a building on a street.",
      "type": "string",
      "minLength": 1,
      "maxLength": 16
    },
    "PostCode": {
      "description": "Identifier consisting of a group of letters and/or numbers that is added to a postal address to assist the sorting of mail.",
      "type": "string",
      "minLength": 1,
      "maxLength": 16
    },
    "TownName": {
      "description": "Name of a built-up area, with defined boundaries, and a local government.",
      "type": "string",
      "minLength": 1,
      "maxLength": 35
    },
    "CountrySubDivision": {
      "description": "Identifies a subdivision of a country such as state, region, county.",
      "type": "string",
      "minLength": 1,
      "maxLength": 35
    },
    "Country": {
      "description": "Nation with its own government.",
      "type": "string",
      "pattern": "^[A-Z]{2,2}$"
    },
    "AddressLine": {
      "type": "array",
      "items": {
        "description": "Information that locates and identifies a specific address, as defined by postal services, presented in free format text.",
        "type": "string",
        "minLength": 1,
        "maxLength": 70
      },
      "minItems": 0,
      "maxItems": 7
    }
  }
}

```

Information that locates and identifies a specific address, as defined by postal services.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|AddressType|[OBAddressTypeCode](#schemaobaddresstypecode)|false|none|Identifies the nature of the postal address.|
|Department|string|false|none|Identification of a division of a large organisation or building.|
|SubDepartment|string|false|none|Identification of a sub-division of a large organisation or building.|
|StreetName|[StreetName](#schemastreetname)|false|none|Name of a street or thoroughfare.|
|BuildingNumber|[BuildingNumber](#schemabuildingnumber)|false|none|Number that identifies the position of a building on a street.|
|PostCode|[PostCode](#schemapostcode)|false|none|Identifier consisting of a group of letters and/or numbers that is added to a postal address to assist the sorting of mail.|
|TownName|[TownName](#schematownname)|false|none|Name of a built-up area, with defined boundaries, and a local government.|
|CountrySubDivision|string|false|none|Identifies a subdivision of a country such as state, region, county.|
|Country|string|false|none|Nation with its own government.|
|AddressLine|[string]|false|none|none|

<h2 id="tocS_OBRate1_0">OBRate1_0</h2>
<!-- backwards compatibility -->
<a id="schemaobrate1_0"></a>
<a id="schema_OBRate1_0"></a>
<a id="tocSobrate1_0"></a>
<a id="tocsobrate1_0"></a>

```json
{
  "description": "Rate charged for Statement Fee (where it is charged in terms of a rate rather than an amount)",
  "type": "number"
}

```

Rate charged for Statement Fee (where it is charged in terms of a rate rather than an amount)

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|number|false|none|Rate charged for Statement Fee (where it is charged in terms of a rate rather than an amount)|

<h2 id="tocS_OBRate1_1">OBRate1_1</h2>
<!-- backwards compatibility -->
<a id="schemaobrate1_1"></a>
<a id="schema_OBRate1_1"></a>
<a id="tocSobrate1_1"></a>
<a id="tocsobrate1_1"></a>

```json
{
  "description": "field representing a percentage (e.g. 0.05 represents 5% and 0.9525 represents 95.25%). Note the number of decimal places may vary.",
  "type": "number"
}

```

field representing a percentage (e.g. 0.05 represents 5% and 0.9525 represents 95.25%). Note the number of decimal places may vary.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|number|false|none|field representing a percentage (e.g. 0.05 represents 5% and 0.9525 represents 95.25%). Note the number of decimal places may vary.|

<h2 id="tocS_OBReadAccount6">OBReadAccount6</h2>
<!-- backwards compatibility -->
<a id="schemaobreadaccount6"></a>
<a id="schema_OBReadAccount6"></a>
<a id="tocSobreadaccount6"></a>
<a id="tocsobreadaccount6"></a>

```json
{
  "type": "object",
  "required": [
    "Data"
  ],
  "properties": {
    "Data": {
      "type": "object",
      "properties": {
        "Account": {
          "type": "array",
          "items": {
            "type": "object",
            "description": "Unambiguous identification of the account to which credit and debit entries are made. The following fields are optional only for accounts that are switched:\n\n  * Data.Currency  \n  * Data.AccountType  \n  * Data.AccountSubType\n\nFor all other accounts, the fields must be populated by the ASPSP.",
            "required": [
              "AccountId"
            ],
            "properties": {
              "AccountId": {
                "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
                "type": "string",
                "minLength": 1,
                "maxLength": 40
              },
              "Status": {
                "description": "Specifies the status of account resource in code form.",
                "type": "string",
                "enum": [
                  "Disabled",
                  "Enabled"
                ]
              },
              "StatusUpdateDateTime": {
                "description": "Date and time at which the resource status was updated.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
                "type": "string",
                "format": "date-time"
              },
              "Currency": {
                "description": "Identification of the currency in which the account is held. \nUsage: Currency should only be used in case one and the same account number covers several currencies\nand the initiating party needs to identify which currency needs to be used for settlement on the account.",
                "type": "string",
                "pattern": "^[A-Z]{3,3}$"
              },
              "AccountType": {
                "description": "Specifies the type of account (personal or business).",
                "type": "string",
                "enum": [
                  "Business",
                  "Personal"
                ]
              },
              "AccountSubType": {
                "description": "Specifies the sub type of account (product family group).",
                "type": "string",
                "enum": [
                  "CreditCard",
                  "CurrentAccount",
                  "Savings"
                ]
              },
              "Description": {
                "description": "Specifies the description of the account type.",
                "type": "string",
                "minLength": 1,
                "maxLength": 35
              },
              "Nickname": {
                "description": "The nickname of the account, assigned by the account owner in order to provide an additional means of identification of the account.",
                "type": "string",
                "minLength": 1,
                "maxLength": 70
              },
              "SwitchStatus": {
                "description": "Specifies the switch status for the account, in a coded form.",
                "type": "string",
                "x-namespaced-enum": [
                  "UK.CASS.NotSwitced",
                  "UK.CASS.SwitchCompleted"
                ]
              },
              "Account": {
                "type": "array",
                "items": {
                  "type": "object",
                  "description": "Provides the details to identify an account.",
                  "required": [
                    "SchemeName",
                    "Identification"
                  ],
                  "properties": {
                    "SchemeName": {
                      "description": "Name of the identification scheme, in a coded form as published in an external list.",
                      "type": "string",
                      "x-namespaced-enum": [
                        "UK.OBIE.SortCodeAccountNumber"
                      ]
                    },
                    "Identification": {
                      "description": "Identification assigned by an institution to identify an account. This identification is known by the account owner.",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 256
                    },
                    "Name": {
                      "description": "The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.\nNote, the account name is not the product name or the nickname of the account.",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 350
                    },
                    "SecondaryIdentification": {
                      "description": "This is secondary identification of the account, as assigned by the account servicing institution. \nThis can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 34
                    }
                  }
                }
              },
              "Servicer": {
                "type": "object",
                "required": [
                  "SchemeName",
                  "Identification"
                ],
                "description": "Party that manages the account on behalf of the account owner, that is manages the registration and booking of entries on the account, calculates balances on the account and provides information about the account.",
                "properties": {
                  "SchemeName": {
                    "description": "Name of the identification scheme, in a coded form as published in an external list.",
                    "type": "string",
                    "x-namespaced-enum": [
                      "UK.OBIE.BICFI"
                    ]
                  },
                  "Identification": {
                    "description": "Unique and unambiguous identification of the servicing institution.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 35
                  }
                }
              }
            },
            "additionalProperties": false
          }
        }
      }
    },
    "Links": {
      "type": "object",
      "description": "Links relevant to the payload",
      "properties": {
        "Self": {
          "type": "string",
          "format": "uri"
        },
        "First": {
          "type": "string",
          "format": "uri"
        },
        "Prev": {
          "type": "string",
          "format": "uri"
        },
        "Next": {
          "type": "string",
          "format": "uri"
        },
        "Last": {
          "type": "string",
          "format": "uri"
        }
      },
      "additionalProperties": false,
      "required": [
        "Self"
      ]
    },
    "Meta": {
      "title": "MetaData",
      "type": "object",
      "description": "Meta Data relevant to the payload",
      "properties": {
        "TotalPages": {
          "type": "integer",
          "format": "int32"
        },
        "FirstAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "LastAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        }
      },
      "additionalProperties": false
    }
  },
  "additionalProperties": false
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Data|object|true|none|none|
|» Account|[[OBAccount6](#schemaobaccount6)]|false|none|[Unambiguous identification of the account to which credit and debit entries are made. The following fields are optional only for accounts that are switched:<br><br>  * Data.Currency  <br>  * Data.AccountType  <br>  * Data.AccountSubType<br><br>For all other accounts, the fields must be populated by the ASPSP.]|
|Links|[Links](#schemalinks)|false|none|Links relevant to the payload|
|Meta|[Meta](#schemameta)|false|none|Meta Data relevant to the payload|

<h2 id="tocS_OBReadBalance1">OBReadBalance1</h2>
<!-- backwards compatibility -->
<a id="schemaobreadbalance1"></a>
<a id="schema_OBReadBalance1"></a>
<a id="tocSobreadbalance1"></a>
<a id="tocsobreadbalance1"></a>

```json
{
  "type": "object",
  "required": [
    "Data"
  ],
  "properties": {
    "Data": {
      "type": "object",
      "required": [
        "Balance"
      ],
      "properties": {
        "Balance": {
          "type": "array",
          "items": {
            "type": "object",
            "description": "Set of elements used to define the balance details.",
            "required": [
              "AccountId",
              "CreditDebitIndicator",
              "Type",
              "DateTime",
              "Amount"
            ],
            "properties": {
              "AccountId": {
                "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
                "type": "string",
                "minLength": 1,
                "maxLength": 40
              },
              "CreditDebitIndicator": {
                "description": "Indicates whether the balance is a credit or a debit balance. \nUsage: A zero balance is considered to be a credit balance.",
                "type": "string",
                "enum": [
                  "Credit",
                  "Debit"
                ]
              },
              "Type": {
                "description": "Balance type, in a coded form.",
                "type": "string",
                "enum": [
                  "Expected",
                  "InterimBooked"
                ]
              },
              "DateTime": {
                "description": "Indicates the date (and time) of the balance.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
                "type": "string",
                "format": "date-time"
              },
              "Amount": {
                "type": "object",
                "required": [
                  "Amount",
                  "Currency"
                ],
                "description": "Amount of money of the cash balance.",
                "properties": {
                  "Amount": {
                    "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
                    "type": "string",
                    "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
                  },
                  "Currency": {
                    "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
                    "type": "string",
                    "pattern": "^[A-Z]{3,3}$"
                  }
                }
              },
              "CreditLine": {
                "type": "array",
                "items": {
                  "type": "object",
                  "description": "Set of elements used to provide details on the credit line.",
                  "required": [
                    "Included"
                  ],
                  "properties": {
                    "Included": {
                      "description": "Indicates whether or not the credit line is included in the balance of the account.\nUsage: If not present, credit line is not included in the balance amount of the account.",
                      "type": "boolean"
                    },
                    "Type": {
                      "description": "Limit type, in a coded form.",
                      "type": "string",
                      "enum": [
                        "Pre-Agreed",
                        "Temporary"
                      ]
                    },
                    "Amount": {
                      "type": "object",
                      "required": [
                        "Amount",
                        "Currency"
                      ],
                      "description": "Amount of money of the credit line.",
                      "properties": {
                        "Amount": {
                          "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
                          "type": "string",
                          "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
                        },
                        "Currency": {
                          "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
                          "type": "string",
                          "pattern": "^[A-Z]{3,3}$"
                        }
                      }
                    }
                  }
                }
              }
            }
          },
          "minItems": 1
        }
      }
    },
    "Links": {
      "type": "object",
      "description": "Links relevant to the payload",
      "properties": {
        "Self": {
          "type": "string",
          "format": "uri"
        },
        "First": {
          "type": "string",
          "format": "uri"
        },
        "Prev": {
          "type": "string",
          "format": "uri"
        },
        "Next": {
          "type": "string",
          "format": "uri"
        },
        "Last": {
          "type": "string",
          "format": "uri"
        }
      },
      "additionalProperties": false,
      "required": [
        "Self"
      ]
    },
    "Meta": {
      "title": "MetaData",
      "type": "object",
      "description": "Meta Data relevant to the payload",
      "properties": {
        "TotalPages": {
          "type": "integer",
          "format": "int32"
        },
        "FirstAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "LastAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        }
      },
      "additionalProperties": false
    }
  },
  "additionalProperties": false
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Data|object|true|none|none|
|» Balance|[object]|true|none|none|
|»» AccountId|[AccountId](#schemaaccountid)|true|none|A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.|
|»» CreditDebitIndicator|[OBCreditDebitCode_2](#schemaobcreditdebitcode_2)|true|none|Indicates whether the balance is a credit or a debit balance. <br>Usage: A zero balance is considered to be a credit balance.|
|»» Type|[OBBalanceType1Code](#schemaobbalancetype1code)|true|none|Balance type, in a coded form.|
|»» DateTime|string(date-time)|true|none|Indicates the date (and time) of the balance.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|
|»» Amount|object|true|none|Amount of money of the cash balance.|
|»»» Amount|[OBActiveCurrencyAndAmount_SimpleType](#schemaobactivecurrencyandamount_simpletype)|true|none|A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.|
|»»» Currency|[ActiveOrHistoricCurrencyCode_1](#schemaactiveorhistoriccurrencycode_1)|true|none|A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 "Codes for the representation of currencies and funds".|
|»» CreditLine|[object]|false|none|none|
|»»» Included|boolean|true|none|Indicates whether or not the credit line is included in the balance of the account.<br>Usage: If not present, credit line is not included in the balance amount of the account.|
|»»» Type|string|false|none|Limit type, in a coded form.|
|»»» Amount|object|false|none|Amount of money of the credit line.|
|»»»» Amount|[OBActiveCurrencyAndAmount_SimpleType](#schemaobactivecurrencyandamount_simpletype)|true|none|A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.|
|»»»» Currency|[ActiveOrHistoricCurrencyCode_1](#schemaactiveorhistoriccurrencycode_1)|true|none|A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 "Codes for the representation of currencies and funds".|
|Links|[Links](#schemalinks)|false|none|Links relevant to the payload|
|Meta|[Meta](#schemameta)|false|none|Meta Data relevant to the payload|

#### Enumerated Values

|Property|Value|
|---|---|
|Type|Pre-Agreed|
|Type|Temporary|

<h2 id="tocS_OBReadBeneficiary5">OBReadBeneficiary5</h2>
<!-- backwards compatibility -->
<a id="schemaobreadbeneficiary5"></a>
<a id="schema_OBReadBeneficiary5"></a>
<a id="tocSobreadbeneficiary5"></a>
<a id="tocsobreadbeneficiary5"></a>

```json
{
  "type": "object",
  "required": [
    "Data"
  ],
  "properties": {
    "Data": {
      "type": "object",
      "properties": {
        "Beneficiary": {
          "type": "array",
          "items": {
            "type": "object",
            "properties": {
              "AccountId": {
                "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
                "type": "string",
                "minLength": 1,
                "maxLength": 40
              },
              "BeneficiaryId": {
                "description": "A unique and immutable identifier used to identify the beneficiary resource. This identifier has no meaning to the account owner.",
                "type": "string",
                "minLength": 1,
                "maxLength": 40
              },
              "Reference": {
                "description": "Unique reference, as assigned by the creditor, to unambiguously refer to the payment transaction.\nUsage: If available, the initiating party should provide this reference in the structured remittance information, to enable reconciliation by the creditor upon receipt of the amount of money.\nIf the business context requires the use of a creditor reference or a payment remit identification, and only one identifier can be passed through the end-to-end chain, the creditor's reference or payment remittance identification should be quoted in the end-to-end transaction identification.",
                "type": "string",
                "minLength": 1,
                "maxLength": 35
              },
              "CreditorAccount": {
                "type": "object",
                "required": [
                  "SchemeName",
                  "Identification"
                ],
                "description": "Provides the details to identify the beneficiary account.",
                "properties": {
                  "SchemeName": {
                    "description": "Name of the identification scheme, in a coded form as published in an external list.",
                    "type": "string",
                    "x-namespaced-enum": [
                      "UK.OBIE.SortCodeAccountNumber"
                    ]
                  },
                  "Identification": {
                    "description": "Identification assigned by an institution to identify an account. This identification is known by the account owner.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 256
                  },
                  "Name": {
                    "description": "The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.\nNote, the account name is not the product name or the nickname of the account.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 350
                  },
                  "SecondaryIdentification": {
                    "description": "This is secondary identification of the account, as assigned by the account servicing institution. \nThis can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 34
                  }
                }
              }
            },
            "additionalProperties": false
          }
        }
      }
    },
    "Links": {
      "type": "object",
      "description": "Links relevant to the payload",
      "properties": {
        "Self": {
          "type": "string",
          "format": "uri"
        },
        "First": {
          "type": "string",
          "format": "uri"
        },
        "Prev": {
          "type": "string",
          "format": "uri"
        },
        "Next": {
          "type": "string",
          "format": "uri"
        },
        "Last": {
          "type": "string",
          "format": "uri"
        }
      },
      "additionalProperties": false,
      "required": [
        "Self"
      ]
    },
    "Meta": {
      "title": "MetaData",
      "type": "object",
      "description": "Meta Data relevant to the payload",
      "properties": {
        "TotalPages": {
          "type": "integer",
          "format": "int32"
        },
        "FirstAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "LastAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        }
      },
      "additionalProperties": false
    }
  },
  "additionalProperties": false
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Data|object|true|none|none|
|» Beneficiary|[[OBBeneficiary5](#schemaobbeneficiary5)]|false|none|none|
|Links|[Links](#schemalinks)|false|none|Links relevant to the payload|
|Meta|[Meta](#schemameta)|false|none|Meta Data relevant to the payload|

<h2 id="tocS_OBReadConsent1">OBReadConsent1</h2>
<!-- backwards compatibility -->
<a id="schemaobreadconsent1"></a>
<a id="schema_OBReadConsent1"></a>
<a id="tocSobreadconsent1"></a>
<a id="tocsobreadconsent1"></a>

```json
{
  "type": "object",
  "required": [
    "Data",
    "Risk"
  ],
  "properties": {
    "Data": {
      "type": "object",
      "required": [
        "Permissions"
      ],
      "properties": {
        "Permissions": {
          "type": "array",
          "items": {
            "description": "Specifies the Open Banking account access data types. This is a list of the data clusters being consented by the PSU, and requested for authorisation with the ASPSP.",
            "type": "string",
            "enum": [
              "ReadAccountsBasic",
              "ReadAccountsDetail",
              "ReadBalances",
              "ReadBeneficiariesBasic",
              "ReadBeneficiariesDetail",
              "ReadDirectDebits",
              "ReadPAN",
              "ReadParty",
              "ReadPartyPSU",
              "ReadProducts",
              "ReadScheduledPaymentsBasic",
              "ReadScheduledPaymentsDetail",
              "ReadStandingOrdersBasic",
              "ReadStandingOrdersDetail",
              "ReadTransactionsBasic",
              "ReadTransactionsCredits",
              "ReadTransactionsDebits",
              "ReadTransactionsDetail"
            ]
          },
          "minItems": 1
        },
        "ExpirationDateTime": {
          "description": "Specified date and time the permissions will expire.\nIf this is not populated, the permissions will be open ended.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "TransactionFromDateTime": {
          "description": "Specified start date and time for the transaction query period.\nIf this is not populated, the start date will be open ended, and data will be returned from the earliest available transaction.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "TransactionToDateTime": {
          "description": "Specified end date and time for the transaction query period.\nIf this is not populated, the end date will be open ended, and data will be returned to the latest available transaction.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        }
      }
    },
    "Risk": {
      "description": "The Risk section is sent by the initiating party to the ASPSP. It is used to specify additional details for risk scoring for Account Info.",
      "type": "object",
      "properties": {},
      "additionalProperties": false
    }
  },
  "additionalProperties": false
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Data|object|true|none|none|
|» Permissions|[string]|true|none|none|
|» ExpirationDateTime|string(date-time)|false|none|Specified date and time the permissions will expire.<br>If this is not populated, the permissions will be open ended.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|
|» TransactionFromDateTime|string(date-time)|false|none|Specified start date and time for the transaction query period.<br>If this is not populated, the start date will be open ended, and data will be returned from the earliest available transaction.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|
|» TransactionToDateTime|string(date-time)|false|none|Specified end date and time for the transaction query period.<br>If this is not populated, the end date will be open ended, and data will be returned to the latest available transaction.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|
|Risk|[OBRisk2](#schemaobrisk2)|true|none|The Risk section is sent by the initiating party to the ASPSP. It is used to specify additional details for risk scoring for Account Info.|

<h2 id="tocS_OBReadConsentResponse1">OBReadConsentResponse1</h2>
<!-- backwards compatibility -->
<a id="schemaobreadconsentresponse1"></a>
<a id="schema_OBReadConsentResponse1"></a>
<a id="tocSobreadconsentresponse1"></a>
<a id="tocsobreadconsentresponse1"></a>

```json
{
  "type": "object",
  "required": [
    "Data",
    "Risk"
  ],
  "properties": {
    "Data": {
      "type": "object",
      "required": [
        "ConsentId",
        "CreationDateTime",
        "Status",
        "StatusUpdateDateTime",
        "Permissions"
      ],
      "properties": {
        "ConsentId": {
          "description": "Unique identification as assigned to identify the account access consent resource.",
          "type": "string",
          "minLength": 1,
          "maxLength": 128
        },
        "CreationDateTime": {
          "description": "Date and time at which the resource was created.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "Status": {
          "description": "Specifies the status of consent resource in code form.",
          "type": "string",
          "enum": [
            "Authorised",
            "AwaitingAuthorisation",
            "Rejected",
            "Revoked"
          ]
        },
        "StatusUpdateDateTime": {
          "description": "Date and time at which the resource status was updated.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "Permissions": {
          "type": "array",
          "items": {
            "description": "Specifies the Open Banking account access data types. This is a list of the data clusters being consented by the PSU, and requested for authorisation with the ASPSP.",
            "type": "string",
            "enum": [
              "ReadAccountsBasic",
              "ReadAccountsDetail",
              "ReadBalances",
              "ReadBeneficiariesBasic",
              "ReadBeneficiariesDetail",
              "ReadDirectDebits",
              "ReadPAN",
              "ReadParty",
              "ReadPartyPSU",
              "ReadProducts",
              "ReadScheduledPaymentsBasic",
              "ReadScheduledPaymentsDetail",
              "ReadStandingOrdersBasic",
              "ReadStandingOrdersDetail",
              "ReadTransactionsBasic",
              "ReadTransactionsCredits",
              "ReadTransactionsDebits",
              "ReadTransactionsDetail"
            ]
          },
          "minItems": 1
        },
        "ExpirationDateTime": {
          "description": "Specified date and time the permissions will expire.\nIf this is not populated, the permissions will be open ended.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "TransactionFromDateTime": {
          "description": "Specified start date and time for the transaction query period.\nIf this is not populated, the start date will be open ended, and data will be returned from the earliest available transaction.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "TransactionToDateTime": {
          "description": "Specified end date and time for the transaction query period.\nIf this is not populated, the end date will be open ended, and data will be returned to the latest available transaction.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        }
      }
    },
    "Risk": {
      "description": "The Risk section is sent by the initiating party to the ASPSP. It is used to specify additional details for risk scoring for Account Info.",
      "type": "object",
      "properties": {},
      "additionalProperties": false
    },
    "Links": {
      "type": "object",
      "description": "Links relevant to the payload",
      "properties": {
        "Self": {
          "type": "string",
          "format": "uri"
        },
        "First": {
          "type": "string",
          "format": "uri"
        },
        "Prev": {
          "type": "string",
          "format": "uri"
        },
        "Next": {
          "type": "string",
          "format": "uri"
        },
        "Last": {
          "type": "string",
          "format": "uri"
        }
      },
      "additionalProperties": false,
      "required": [
        "Self"
      ]
    },
    "Meta": {
      "title": "MetaData",
      "type": "object",
      "description": "Meta Data relevant to the payload",
      "properties": {
        "TotalPages": {
          "type": "integer",
          "format": "int32"
        },
        "FirstAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "LastAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        }
      },
      "additionalProperties": false
    }
  },
  "additionalProperties": false
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Data|object|true|none|none|
|» ConsentId|string|true|none|Unique identification as assigned to identify the account access consent resource.|
|» CreationDateTime|[CreationDateTime](#schemacreationdatetime)|true|none|Date and time at which the resource was created.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|
|» Status|string|true|none|Specifies the status of consent resource in code form.|
|» StatusUpdateDateTime|[StatusUpdateDateTime](#schemastatusupdatedatetime)|true|none|Date and time at which the resource status was updated.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|
|» Permissions|[string]|true|none|none|
|» ExpirationDateTime|string(date-time)|false|none|Specified date and time the permissions will expire.<br>If this is not populated, the permissions will be open ended.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|
|» TransactionFromDateTime|string(date-time)|false|none|Specified start date and time for the transaction query period.<br>If this is not populated, the start date will be open ended, and data will be returned from the earliest available transaction.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|
|» TransactionToDateTime|string(date-time)|false|none|Specified end date and time for the transaction query period.<br>If this is not populated, the end date will be open ended, and data will be returned to the latest available transaction.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|
|Risk|[OBRisk2](#schemaobrisk2)|true|none|The Risk section is sent by the initiating party to the ASPSP. It is used to specify additional details for risk scoring for Account Info.|
|Links|[Links](#schemalinks)|false|none|Links relevant to the payload|
|Meta|[Meta](#schemameta)|false|none|Meta Data relevant to the payload|

#### Enumerated Values

|Property|Value|
|---|---|
|Status|Authorised|
|Status|AwaitingAuthorisation|
|Status|Rejected|
|Status|Revoked|

<h2 id="tocS_OBReadDataTransaction6">OBReadDataTransaction6</h2>
<!-- backwards compatibility -->
<a id="schemaobreaddatatransaction6"></a>
<a id="schema_OBReadDataTransaction6"></a>
<a id="tocSobreaddatatransaction6"></a>
<a id="tocsobreaddatatransaction6"></a>

```json
{
  "type": "object",
  "properties": {
    "Transaction": {
      "type": "array",
      "items": {
        "type": "object",
        "description": "Provides further details on an entry in the report.",
        "required": [
          "AccountId",
          "CreditDebitIndicator",
          "Status",
          "BookingDateTime",
          "Amount"
        ],
        "properties": {
          "AccountId": {
            "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
            "type": "string",
            "minLength": 1,
            "maxLength": 40
          },
          "TransactionId": {
            "description": "Unique identifier for the transaction within an servicing institution. This identifier is both unique and immutable.",
            "type": "string",
            "minLength": 1,
            "maxLength": 210
          },
          "TransactionReference": {
            "description": "Unique reference for the transaction. This reference is optionally populated, and may as an example be the FPID in the Faster Payments context.",
            "type": "string",
            "minLength": 1,
            "maxLength": 210
          },
          "CreditDebitIndicator": {
            "description": "Indicates whether the transaction is a credit or a debit entry.",
            "type": "string",
            "enum": [
              "Credit",
              "Debit"
            ]
          },
          "Status": {
            "description": "Status of a transaction entry on the books of the account servicer.",
            "type": "string",
            "enum": [
              "Booked",
              "Pending"
            ]
          },
          "BookingDateTime": {
            "description": "Date and time when a transaction entry is posted to an account on the account servicer's books.\nUsage: Booking date is the expected booking date, unless the status is booked, in which case it is the actual booking date.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
            "type": "string",
            "format": "date-time"
          },
          "ValueDateTime": {
            "description": "Date and time at which assets become available to the account owner in case of a credit entry, or cease to be available to the account owner in case of a debit transaction entry.\nUsage: If transaction entry status is pending and value date is present, then the value date refers to an expected/requested value date.\nFor transaction entries subject to availability/float and for which availability information is provided, the value date must not be used. In this case the availability component identifies the number of availability days.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
            "type": "string",
            "format": "date-time"
          },
          "TransactionInformation": {
            "description": "Further details of the transaction. \nThis is the transaction narrative, which is unstructured text.",
            "type": "string",
            "minLength": 1,
            "maxLength": 500
          },
          "AddressLine": {
            "description": "Information that locates and identifies a specific address for a transaction entry, that is presented in free format text.",
            "type": "string",
            "minLength": 1,
            "maxLength": 70
          },
          "Amount": {
            "type": "object",
            "required": [
              "Amount",
              "Currency"
            ],
            "description": "Amount of money in the cash transaction entry.",
            "properties": {
              "Amount": {
                "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
                "type": "string",
                "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
              },
              "Currency": {
                "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
                "type": "string",
                "pattern": "^[A-Z]{3,3}$"
              }
            }
          },
          "CurrencyExchange": {
            "type": "object",
            "required": [
              "SourceCurrency",
              "ExchangeRate"
            ],
            "description": "Set of elements used to provide details on the currency exchange.",
            "properties": {
              "SourceCurrency": {
                "description": "Currency from which an amount is to be converted in a currency conversion.",
                "type": "string",
                "pattern": "^[A-Z]{3,3}$"
              },
              "TargetCurrency": {
                "description": "Currency into which an amount is to be converted in a currency conversion.",
                "type": "string",
                "pattern": "^[A-Z]{3,3}$"
              },
              "UnitCurrency": {
                "description": "Currency in which the rate of exchange is expressed in a currency exchange. In the example 1GBP = xxxCUR, the unit currency is GBP.",
                "type": "string",
                "pattern": "^[A-Z]{3,3}$"
              },
              "ExchangeRate": {
                "description": "Factor used to convert an amount from one currency into another. This reflects the price at which one currency was bought with another currency.\nUsage: ExchangeRate expresses the ratio between UnitCurrency and QuotedCurrency (ExchangeRate = UnitCurrency/QuotedCurrency).",
                "type": "number"
              },
              "InstructedAmount": {
                "type": "object",
                "required": [
                  "Amount",
                  "Currency"
                ],
                "description": "Amount of money to be moved between the debtor and creditor, before deduction of charges, expressed in the currency as ordered by the initiating party.",
                "properties": {
                  "Amount": {
                    "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
                    "type": "string",
                    "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
                  },
                  "Currency": {
                    "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
                    "type": "string",
                    "pattern": "^[A-Z]{3,3}$"
                  }
                }
              }
            }
          },
          "ProprietaryBankTransactionCode": {
            "type": "object",
            "required": [
              "Code"
            ],
            "description": "Set of elements to fully identify a proprietary bank transaction code.",
            "properties": {
              "Code": {
                "description": "Proprietary bank transaction code to identify the underlying transaction.",
                "type": "string",
                "minLength": 1,
                "maxLength": 35
              }
            },
            "additionalProperties": false
          },
          "Balance": {
            "type": "object",
            "required": [
              "CreditDebitIndicator",
              "Type",
              "Amount"
            ],
            "description": "Set of elements used to define the balance as a numerical representation of the net increases and decreases in an account after a transaction entry is applied to the account.",
            "properties": {
              "CreditDebitIndicator": {
                "description": "Indicates whether the balance is a credit or a debit balance. \nUsage: A zero balance is considered to be a credit balance.",
                "type": "string",
                "enum": [
                  "Credit",
                  "Debit"
                ]
              },
              "Type": {
                "description": "Balance type, in a coded form.",
                "type": "string",
                "enum": [
                  "Expected",
                  "InterimBooked"
                ]
              },
              "Amount": {
                "type": "object",
                "required": [
                  "Amount",
                  "Currency"
                ],
                "description": "Amount of money of the cash balance after a transaction entry is applied to the account..",
                "properties": {
                  "Amount": {
                    "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
                    "type": "string",
                    "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
                  },
                  "Currency": {
                    "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
                    "type": "string",
                    "pattern": "^[A-Z]{3,3}$"
                  }
                }
              }
            },
            "additionalProperties": false
          },
          "MerchantDetails": {
            "type": "object",
            "description": "Details of the merchant involved in the transaction.",
            "properties": {
              "MerchantCategoryCode": {
                "description": "Category code conform to ISO 18245, related to the type of services or goods the merchant provides for the transaction.",
                "type": "string",
                "minLength": 3,
                "maxLength": 4
              }
            }
          },
          "CreditorAccount": {
            "type": "object",
            "description": "Unambiguous identification of the account of the creditor, in the case of a debit transaction.",
            "properties": {
              "SchemeName": {
                "description": "Name of the identification scheme, in a coded form as published in an external list.",
                "type": "string",
                "x-namespaced-enum": [
                  "UK.OBIE.SortCodeAccountNumber"
                ]
              },
              "Identification": {
                "description": "Identification assigned by an institution to identify an account. This identification is known by the account owner.",
                "type": "string",
                "minLength": 1,
                "maxLength": 256
              },
              "Name": {
                "description": "The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.\nNote, the account name is not the product name or the nickname of the account.",
                "type": "string",
                "minLength": 1,
                "maxLength": 350
              },
              "SecondaryIdentification": {
                "description": "This is secondary identification of the account, as assigned by the account servicing institution. \nThis can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).",
                "type": "string",
                "minLength": 1,
                "maxLength": 34
              }
            }
          },
          "DebtorAccount": {
            "type": "object",
            "description": "Unambiguous identification of the account of the debtor, in the case of a crebit transaction.",
            "properties": {
              "SchemeName": {
                "description": "Name of the identification scheme, in a coded form as published in an external list.",
                "type": "string",
                "x-namespaced-enum": [
                  "UK.OBIE.SortCodeAccountNumber"
                ]
              },
              "Identification": {
                "description": "Identification assigned by an institution to identify an account. This identification is known by the account owner.",
                "type": "string",
                "minLength": 1,
                "maxLength": 256
              },
              "Name": {
                "description": "The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.\nNote, the account name is not the product name or the nickname of the account.",
                "type": "string",
                "minLength": 1,
                "maxLength": 350
              },
              "SecondaryIdentification": {
                "description": "This is secondary identification of the account, as assigned by the account servicing institution. \nThis can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).",
                "type": "string",
                "minLength": 1,
                "maxLength": 34
              }
            }
          },
          "CardInstrument": {
            "type": "object",
            "required": [
              "CardSchemeName"
            ],
            "description": "Set of elements to describe the card instrument used in the transaction.",
            "properties": {
              "CardSchemeName": {
                "description": "Name of the card scheme.",
                "type": "string",
                "enum": [
                  "AmericanExpress",
                  "Diners",
                  "Discover",
                  "MasterCard",
                  "VISA"
                ]
              },
              "AuthorisationType": {
                "description": "The card authorisation type.",
                "type": "string",
                "enum": [
                  "ConsumerDevice",
                  "Contactless",
                  "None",
                  "PIN"
                ]
              },
              "Name": {
                "description": "Name of the cardholder using the card instrument.",
                "type": "string",
                "minLength": 1,
                "maxLength": 70
              },
              "Identification": {
                "description": "Identification assigned by an institution to identify the card instrument used in the transaction. This identification is known by the account owner, and may be masked.",
                "type": "string",
                "minLength": 1,
                "maxLength": 34
              }
            },
            "additionalProperties": false
          }
        },
        "additionalProperties": false
      }
    }
  },
  "additionalProperties": false
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Transaction|[[OBTransaction6](#schemaobtransaction6)]|false|none|[Provides further details on an entry in the report.]|

<h2 id="tocS_OBReadDirectDebit2">OBReadDirectDebit2</h2>
<!-- backwards compatibility -->
<a id="schemaobreaddirectdebit2"></a>
<a id="schema_OBReadDirectDebit2"></a>
<a id="tocSobreaddirectdebit2"></a>
<a id="tocsobreaddirectdebit2"></a>

```json
{
  "type": "object",
  "required": [
    "Data"
  ],
  "properties": {
    "Data": {
      "type": "object",
      "properties": {
        "DirectDebit": {
          "type": "array",
          "items": {
            "type": "object",
            "description": "Account to or from which a cash entry is made.",
            "required": [
              "AccountId",
              "MandateIdentification",
              "Name"
            ],
            "properties": {
              "AccountId": {
                "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
                "type": "string",
                "minLength": 1,
                "maxLength": 40
              },
              "DirectDebitId": {
                "description": "A unique and immutable identifier used to identify the direct debit resource. This identifier has no meaning to the account owner.",
                "type": "string",
                "minLength": 1,
                "maxLength": 40
              },
              "MandateIdentification": {
                "description": "Direct Debit reference. For AUDDIS service users provide Core Reference. For non AUDDIS service users provide Core reference if possible or last used reference.",
                "type": "string",
                "minLength": 1,
                "maxLength": 35
              },
              "DirectDebitStatusCode": {
                "description": "Specifies the status of the direct debit in code form.",
                "type": "string",
                "enum": [
                  "Active",
                  "Inactive"
                ]
              },
              "Name": {
                "description": "Name of Service User.",
                "type": "string",
                "minLength": 1,
                "maxLength": 70
              },
              "PreviousPaymentDateTime": {
                "description": "Date of most recent direct debit collection.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
                "type": "string",
                "format": "date-time"
              },
              "PreviousPaymentAmount": {
                "type": "object",
                "required": [
                  "Amount",
                  "Currency"
                ],
                "description": "The amount of the most recent direct debit collection.",
                "properties": {
                  "Amount": {
                    "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
                    "type": "string",
                    "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
                  },
                  "Currency": {
                    "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
                    "type": "string",
                    "pattern": "^[A-Z]{3,3}$"
                  }
                }
              }
            }
          }
        }
      }
    },
    "Links": {
      "type": "object",
      "description": "Links relevant to the payload",
      "properties": {
        "Self": {
          "type": "string",
          "format": "uri"
        },
        "First": {
          "type": "string",
          "format": "uri"
        },
        "Prev": {
          "type": "string",
          "format": "uri"
        },
        "Next": {
          "type": "string",
          "format": "uri"
        },
        "Last": {
          "type": "string",
          "format": "uri"
        }
      },
      "additionalProperties": false,
      "required": [
        "Self"
      ]
    },
    "Meta": {
      "title": "MetaData",
      "type": "object",
      "description": "Meta Data relevant to the payload",
      "properties": {
        "TotalPages": {
          "type": "integer",
          "format": "int32"
        },
        "FirstAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "LastAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        }
      },
      "additionalProperties": false
    }
  },
  "additionalProperties": false
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Data|object|true|none|none|
|» DirectDebit|[object]|false|none|none|
|»» AccountId|[AccountId](#schemaaccountid)|true|none|A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.|
|»» DirectDebitId|[DirectDebitId](#schemadirectdebitid)|false|none|A unique and immutable identifier used to identify the direct debit resource. This identifier has no meaning to the account owner.|
|»» MandateIdentification|[MandateIdentification](#schemamandateidentification)|true|none|Direct Debit reference. For AUDDIS service users provide Core Reference. For non AUDDIS service users provide Core reference if possible or last used reference.|
|»» DirectDebitStatusCode|[OBExternalDirectDebitStatus1Code](#schemaobexternaldirectdebitstatus1code)|false|none|Specifies the status of the direct debit in code form.|
|»» Name|[Name_2](#schemaname_2)|true|none|Name of Service User.|
|»» PreviousPaymentDateTime|[PreviousPaymentDateTime](#schemapreviouspaymentdatetime)|false|none|Date of most recent direct debit collection.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|
|»» PreviousPaymentAmount|[OBActiveOrHistoricCurrencyAndAmount_0](#schemaobactiveorhistoriccurrencyandamount_0)|false|none|The amount of the most recent direct debit collection.|
|Links|[Links](#schemalinks)|false|none|Links relevant to the payload|
|Meta|[Meta](#schemameta)|false|none|Meta Data relevant to the payload|

<h2 id="tocS_OBReadParty2">OBReadParty2</h2>
<!-- backwards compatibility -->
<a id="schemaobreadparty2"></a>
<a id="schema_OBReadParty2"></a>
<a id="tocSobreadparty2"></a>
<a id="tocsobreadparty2"></a>

```json
{
  "type": "object",
  "required": [
    "Data"
  ],
  "properties": {
    "Data": {
      "type": "object",
      "properties": {
        "Party": {
          "type": "object",
          "required": [
            "PartyId"
          ],
          "properties": {
            "PartyId": {
              "description": "A unique and immutable identifier used to identify the customer resource. This identifier has no meaning to the account owner.",
              "type": "string",
              "minLength": 1,
              "maxLength": 40
            },
            "PartyType": {
              "description": "Party type, in a coded form.",
              "type": "string",
              "enum": [
                "Delegate",
                "Joint",
                "Sole"
              ]
            },
            "Name": {
              "description": "Name by which a party is known and which is usually used to identify that party.",
              "type": "string",
              "minLength": 1,
              "maxLength": 350
            },
            "FullLegalName": {
              "description": "Specifies a character string with a maximum length of 350 characters.",
              "type": "string",
              "minLength": 1,
              "maxLength": 350
            },
            "EmailAddress": {
              "description": "Address for electronic mail (e-mail).",
              "type": "string",
              "minLength": 1,
              "maxLength": 256
            },
            "Relationships": {
              "type": "object",
              "description": "The Party's relationships with other resources.",
              "properties": {
                "Account": {
                  "type": "object",
                  "required": [
                    "Related",
                    "Id"
                  ],
                  "description": "Relationship to the Account resource.",
                  "properties": {
                    "Related": {
                      "description": "Absolute URI to the related resource.",
                      "type": "string",
                      "format": "uri"
                    },
                    "Id": {
                      "description": "Unique identification as assigned by the ASPSP to uniquely identify the related resource.",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 40
                    }
                  }
                }
              }
            },
            "Address": {
              "type": "array",
              "items": {
                "type": "object",
                "description": "Postal address of a party.",
                "required": [
                  "Country"
                ],
                "properties": {
                  "AddressType": {
                    "description": "Identifies the nature of the postal address.",
                    "type": "string",
                    "enum": [
                      "Business",
                      "Correspondence",
                      "DeliveryTo",
                      "MailTo",
                      "POBox",
                      "Postal",
                      "Residential",
                      "Statement"
                    ]
                  },
                  "AddressLine": {
                    "type": "array",
                    "items": {
                      "description": "Information that locates and identifies a specific address, as defined by postal services, that is presented in free format text.",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 70
                    },
                    "minItems": 0,
                    "maxItems": 5
                  },
                  "StreetName": {
                    "description": "Name of a street or thoroughfare.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 70
                  },
                  "BuildingNumber": {
                    "description": "Number that identifies the position of a building on a street.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 16
                  },
                  "PostCode": {
                    "description": "Identifier consisting of a group of letters and/or numbers that is added to a postal address to assist the sorting of mail.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 16
                  },
                  "TownName": {
                    "description": "Name of a built-up area, with defined boundaries, and a local government.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 35
                  },
                  "CountrySubDivision": {
                    "description": "Identifies a subdivision of a country eg, state, region, county.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 35
                  },
                  "Country": {
                    "description": "Nation with its own government, occupying a particular territory.",
                    "type": "string",
                    "pattern": "^[A-Z]{2,2}$"
                  }
                }
              }
            }
          },
          "additionalProperties": false
        }
      }
    },
    "Links": {
      "type": "object",
      "description": "Links relevant to the payload",
      "properties": {
        "Self": {
          "type": "string",
          "format": "uri"
        },
        "First": {
          "type": "string",
          "format": "uri"
        },
        "Prev": {
          "type": "string",
          "format": "uri"
        },
        "Next": {
          "type": "string",
          "format": "uri"
        },
        "Last": {
          "type": "string",
          "format": "uri"
        }
      },
      "additionalProperties": false,
      "required": [
        "Self"
      ]
    },
    "Meta": {
      "title": "MetaData",
      "type": "object",
      "description": "Meta Data relevant to the payload",
      "properties": {
        "TotalPages": {
          "type": "integer",
          "format": "int32"
        },
        "FirstAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "LastAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        }
      },
      "additionalProperties": false
    }
  },
  "additionalProperties": false
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Data|object|true|none|none|
|» Party|[OBParty2](#schemaobparty2)|false|none|none|
|Links|[Links](#schemalinks)|false|none|Links relevant to the payload|
|Meta|[Meta](#schemameta)|false|none|Meta Data relevant to the payload|

<h2 id="tocS_OBReadParty3">OBReadParty3</h2>
<!-- backwards compatibility -->
<a id="schemaobreadparty3"></a>
<a id="schema_OBReadParty3"></a>
<a id="tocSobreadparty3"></a>
<a id="tocsobreadparty3"></a>

```json
{
  "type": "object",
  "required": [
    "Data"
  ],
  "properties": {
    "Data": {
      "type": "object",
      "properties": {
        "Party": {
          "type": "array",
          "items": {
            "type": "object",
            "required": [
              "PartyId"
            ],
            "properties": {
              "PartyId": {
                "description": "A unique and immutable identifier used to identify the customer resource. This identifier has no meaning to the account owner.",
                "type": "string",
                "minLength": 1,
                "maxLength": 40
              },
              "PartyType": {
                "description": "Party type, in a coded form.",
                "type": "string",
                "enum": [
                  "Delegate",
                  "Joint",
                  "Sole"
                ]
              },
              "Name": {
                "description": "Name by which a party is known and which is usually used to identify that party.",
                "type": "string",
                "minLength": 1,
                "maxLength": 350
              },
              "FullLegalName": {
                "description": "Specifies a character string with a maximum length of 350 characters.",
                "type": "string",
                "minLength": 1,
                "maxLength": 350
              },
              "EmailAddress": {
                "description": "Address for electronic mail (e-mail).",
                "type": "string",
                "minLength": 1,
                "maxLength": 256
              },
              "Relationships": {
                "type": "object",
                "description": "The Party's relationships with other resources.",
                "properties": {
                  "Account": {
                    "type": "object",
                    "required": [
                      "Related",
                      "Id"
                    ],
                    "description": "Relationship to the Account resource.",
                    "properties": {
                      "Related": {
                        "description": "Absolute URI to the related resource.",
                        "type": "string",
                        "format": "uri"
                      },
                      "Id": {
                        "description": "Unique identification as assigned by the ASPSP to uniquely identify the related resource.",
                        "type": "string",
                        "minLength": 1,
                        "maxLength": 40
                      }
                    }
                  }
                }
              },
              "Address": {
                "type": "array",
                "items": {
                  "type": "object",
                  "description": "Postal address of a party.",
                  "required": [
                    "Country"
                  ],
                  "properties": {
                    "AddressType": {
                      "description": "Identifies the nature of the postal address.",
                      "type": "string",
                      "enum": [
                        "Business",
                        "Correspondence",
                        "DeliveryTo",
                        "MailTo",
                        "POBox",
                        "Postal",
                        "Residential",
                        "Statement"
                      ]
                    },
                    "AddressLine": {
                      "type": "array",
                      "items": {
                        "description": "Information that locates and identifies a specific address, as defined by postal services, that is presented in free format text.",
                        "type": "string",
                        "minLength": 1,
                        "maxLength": 70
                      },
                      "minItems": 0,
                      "maxItems": 5
                    },
                    "StreetName": {
                      "description": "Name of a street or thoroughfare.",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 70
                    },
                    "BuildingNumber": {
                      "description": "Number that identifies the position of a building on a street.",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 16
                    },
                    "PostCode": {
                      "description": "Identifier consisting of a group of letters and/or numbers that is added to a postal address to assist the sorting of mail.",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 16
                    },
                    "TownName": {
                      "description": "Name of a built-up area, with defined boundaries, and a local government.",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 35
                    },
                    "CountrySubDivision": {
                      "description": "Identifies a subdivision of a country eg, state, region, county.",
                      "type": "string",
                      "minLength": 1,
                      "maxLength": 35
                    },
                    "Country": {
                      "description": "Nation with its own government, occupying a particular territory.",
                      "type": "string",
                      "pattern": "^[A-Z]{2,2}$"
                    }
                  }
                }
              }
            },
            "additionalProperties": false
          }
        }
      }
    },
    "Links": {
      "type": "object",
      "description": "Links relevant to the payload",
      "properties": {
        "Self": {
          "type": "string",
          "format": "uri"
        },
        "First": {
          "type": "string",
          "format": "uri"
        },
        "Prev": {
          "type": "string",
          "format": "uri"
        },
        "Next": {
          "type": "string",
          "format": "uri"
        },
        "Last": {
          "type": "string",
          "format": "uri"
        }
      },
      "additionalProperties": false,
      "required": [
        "Self"
      ]
    },
    "Meta": {
      "title": "MetaData",
      "type": "object",
      "description": "Meta Data relevant to the payload",
      "properties": {
        "TotalPages": {
          "type": "integer",
          "format": "int32"
        },
        "FirstAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "LastAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        }
      },
      "additionalProperties": false
    }
  },
  "additionalProperties": false
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Data|object|true|none|none|
|» Party|[[OBParty2](#schemaobparty2)]|false|none|none|
|Links|[Links](#schemalinks)|false|none|Links relevant to the payload|
|Meta|[Meta](#schemameta)|false|none|Meta Data relevant to the payload|

<h2 id="tocS_OBReadProduct2">OBReadProduct2</h2>
<!-- backwards compatibility -->
<a id="schemaobreadproduct2"></a>
<a id="schema_OBReadProduct2"></a>
<a id="tocSobreadproduct2"></a>
<a id="tocsobreadproduct2"></a>

```json
{
  "type": "object",
  "required": [
    "Data"
  ],
  "description": "Product details of Other Product which is not avaiable in the standard list",
  "properties": {
    "Data": {
      "type": "object",
      "description": "Aligning with the read write specs structure.",
      "properties": {
        "Product": {
          "type": "array",
          "items": {
            "type": "object",
            "description": "Product details associated with the Account",
            "required": [
              "AccountId",
              "ProductType"
            ],
            "properties": {
              "ProductName": {
                "description": "The name of the Product used for marketing purposes from a customer perspective. I.e. what the customer would recognise.",
                "type": "string",
                "minLength": 1,
                "maxLength": 350
              },
              "ProductId": {
                "description": "The unique ID that has been internally assigned by the financial institution to each of the current account banking products they market to their retail and/or small to medium enterprise (SME) customers.",
                "type": "string",
                "minLength": 1,
                "maxLength": 40
              },
              "AccountId": {
                "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
                "type": "string",
                "minLength": 1,
                "maxLength": 40
              },
              "ProductType": {
                "description": "Product type : Personal Current Account, Business Current Account",
                "type": "string",
                "enum": [
                  "CommercialCreditCard",
                  "PersonalCurrentAccount"
                ]
              },
              "MarketingStateId": {
                "description": "Unique and unambiguous identification of a  Product Marketing State.",
                "type": "string",
                "minLength": 1,
                "maxLength": 35
              },
              "OtherProductType": {
                "type": "object",
                "required": [
                  "Name",
                  "Description"
                ],
                "description": "Other product type details associated with the account.",
                "properties": {
                  "Name": {
                    "description": "Long name associated with the product",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 350
                  },
                  "Description": {
                    "description": "Description of the Product associated with the account",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 350
                  },
                  "CreditInterest": {
                    "type": "object",
                    "required": [
                      "TierBandSet"
                    ],
                    "description": "Details about the interest that may be payable to the Account holders",
                    "properties": {
                      "TierBandSet": {
                        "type": "array",
                        "items": {
                          "type": "object",
                          "description": "The group of tiers or bands for which credit interest can be applied.",
                          "required": [
                            "TierBandMethod",
                            "Destination",
                            "TierBand"
                          ],
                          "properties": {
                            "TierBandMethod": {
                              "description": "The methodology of how credit interest is paid/applied. It can be:-\n1. Banded\nInterest rates are banded. i.e. Increasing rate on whole balance as balance increases.\n2. Tiered\nInterest rates are tiered. i.e. increasing rate for each tier as balance increases, but interest paid on tier fixed for that tier and not on whole balance.\n3. Whole\nThe same interest rate is applied irrespective of the product holder's account balance",
                              "type": "string",
                              "enum": [
                                "INWH"
                              ]
                            },
                            "Destination": {
                              "description": "Describes whether accrued interest is payable only to the BCA or to another bank account",
                              "type": "string",
                              "enum": [
                                "INSC"
                              ]
                            },
                            "TierBand": {
                              "type": "array",
                              "items": {
                                "type": "object",
                                "description": "Tier Band Details",
                                "required": [
                                  "TierValueMinimum",
                                  "ApplicationFrequency",
                                  "FixedVariableInterestRateType",
                                  "AER"
                                ],
                                "properties": {
                                  "TierValueMinimum": {
                                    "description": "Minimum deposit value for which the credit interest tier applies.",
                                    "type": "string",
                                    "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                                  },
                                  "ApplicationFrequency": {
                                    "description": "How often is interest applied to the Product for this tier/band i.e. how often the financial institution pays accumulated interest to the customer's account.",
                                    "type": "string",
                                    "enum": [
                                      "FQDY",
                                      "FQWY",
                                      "FQMY",
                                      "FQQY",
                                      "FQHY",
                                      "FQYY"
                                    ]
                                  },
                                  "FixedVariableInterestRateType": {
                                    "description": "Type of interest rate, Fixed or Variable",
                                    "type": "string",
                                    "enum": [
                                      "INVA"
                                    ]
                                  },
                                  "AER": {
                                    "description": "The annual equivalent rate (AER) is interest that is calculated under the assumption that any interest paid is combined with the original balance and the next interest payment will be based on the slightly higher account balance. Overall, this means that interest can be compounded several times in a year depending on the number of times that interest payments are made. \nRead more: Annual Equivalent Rate (AER) http://www.investopedia.com/terms/a/aer.asp#ixzz4gfR7IO1A",
                                    "type": "string",
                                    "pattern": "^(-?\\d{1,3}){1}(\\.\\d{1,4}){0,1}$"
                                  }
                                }
                              }
                            }
                          }
                        }
                      }
                    }
                  }
                }
              },
              "PCA": {
                "type": "object",
                "title": "PCA",
                "properties": {
                  "ProductDetails": {
                    "type": "object",
                    "title": "ProductDetails",
                    "properties": {
                      "Segment": {
                        "description": "Market segmentation is a marketing term referring to the aggregating of prospective buyers into groups, or segments, that have common needs and respond similarly to a marketing action. Market segmentation enables companies to target different categories of consumers who perceive the full value of certain products and services differently from one another.\n\nRead more: Market Segmentation http://www.investopedia.com/terms/m/marketsegmentation.asp#ixzz4gfEEalTd \nWith respect to PCA products, they are segmented in relation to different markets that they wish to focus on. ",
                        "title": "Segment",
                        "type": "array",
                        "items": {
                          "description": "Market segmentation is a marketing term referring to the aggregating of prospective buyers into groups, or segments, that have common needs and respond similarly to a marketing action. Market segmentation enables companies to target different categories of consumers who perceive the full value of certain products and services differently from one another.\n\nRead more: Market Segmentation http://www.investopedia.com/terms/m/marketsegmentation.asp#ixzz4gfEEalTd \nWith respect to PCA products, they are segmented in relation to different markets that they wish to focus on. ",
                          "type": "string",
                          "enum": [
                            "Basic",
                            "General",
                            "Graduate",
                            "Other",
                            "Premium",
                            "Student",
                            "Youth"
                          ]
                        }
                      },
                      "MonthlyMaximumCharge": {
                        "description": "The maximum relevant charges that could accrue as defined fully in Part 7 of the CMA order",
                        "title": "MonthlyMaximumCharge",
                        "type": "string",
                        "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                      }
                    },
                    "additionalProperties": false
                  },
                  "CreditInterest": {
                    "description": "Details about the interest that may be payable to the PCA account holders",
                    "type": "object",
                    "title": "CreditInterest",
                    "properties": {
                      "TierBandSet": {
                        "description": "The group of tiers or bands for which credit interest can be applied.",
                        "type": "array",
                        "title": "TierBandSet",
                        "items": {
                          "description": "The group of tiers or bands for which credit interest can be applied.",
                          "type": "object",
                          "properties": {
                            "TierBandMethod": {
                              "description": "The methodology of how credit interest is charged. It can be:-\n\n1. Banded\nInterest rates are banded. i.e. Increasing rate on whole balance as balance increases.\n\n2. Tiered\nInterest rates are tiered. i.e. increasing rate for each tier as balance increases, but interest paid on tier fixed for that tier and not on whole balance.\n\n3. Whole\nThe same interest rate is applied irrespective of the PCA balance",
                              "title": "TierBandMethod",
                              "type": "string",
                              "enum": [
                                "Tiered",
                                "Whole"
                              ]
                            },
                            "CalculationMethod": {
                              "description": "Methods of calculating interest",
                              "title": "CalculationMethod",
                              "type": "string",
                              "enum": [
                                "SimpleInterest"
                              ]
                            },
                            "Destination": {
                              "description": "Describes whether accrued interest is payable only to the PCA or to another bank account",
                              "title": "Destination",
                              "type": "string",
                              "enum": [
                                "PayAway",
                                "SelfCredit"
                              ]
                            },
                            "TierBand": {
                              "description": "Tier Band Details",
                              "type": "array",
                              "title": "TierBand",
                              "items": {
                                "description": "Tier Band Details",
                                "type": "object",
                                "properties": {
                                  "Identification": {
                                    "description": "Unique and unambiguous identification of a  Tier Band for a PCA.",
                                    "title": "Identification",
                                    "type": "string",
                                    "minLength": 1,
                                    "maxLength": 35
                                  },
                                  "TierValueMinimum": {
                                    "description": "Minimum deposit value for which the credit interest tier applies.",
                                    "title": "TierValueMinimum",
                                    "type": "string",
                                    "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                                  },
                                  "TierValueMaximum": {
                                    "description": "Maximum deposit value for which the credit interest tier applies.",
                                    "title": "TierValueMaximum",
                                    "type": "string",
                                    "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                                  },
                                  "CalculationFrequency": {
                                    "description": "How often is credit interest calculated for the account.",
                                    "title": "CalculationFrequency",
                                    "type": "string",
                                    "enum": [
                                      "Daily",
                                      "HalfYearly",
                                      "Monthly",
                                      "Quarterly",
                                      "Weekly",
                                      "Yearly"
                                    ]
                                  },
                                  "ApplicationFrequency": {
                                    "description": "How often is interest applied to the PCA for this tier/band i.e. how often the financial institution pays accumulated interest to the customer's PCA.",
                                    "title": "ApplicationFrequency",
                                    "type": "string",
                                    "enum": [
                                      "Daily",
                                      "HalfYearly",
                                      "Monthly",
                                      "Quarterly",
                                      "Weekly",
                                      "Yearly"
                                    ]
                                  },
                                  "FixedVariableInterestRateType": {
                                    "description": "Type of interest rate, Fixed or Variable",
                                    "title": "FixedVariableInterestRateType",
                                    "type": "string",
                                    "enum": [
                                      "Variable"
                                    ]
                                  },
                                  "AER": {
                                    "description": "The annual equivalent rate (AER) is interest that is calculated under the assumption that any interest paid is combined with the original balance and the next interest payment will be based on the slightly higher account balance. Overall, this means that interest can be compounded several times in a year depending on the number of times that interest payments are made. \n\nRead more: Annual Equivalent Rate (AER) http://www.investopedia.com/terms/a/aer.asp#ixzz4gfR7IO1A",
                                    "title": "AER",
                                    "type": "string",
                                    "pattern": "^(-?\\d{1,3}){1}(\\.\\d{1,4}){0,1}$"
                                  }
                                },
                                "required": [
                                  "TierValueMinimum",
                                  "ApplicationFrequency",
                                  "FixedVariableInterestRateType",
                                  "AER"
                                ]
                              },
                              "minItems": 1
                            }
                          },
                          "required": [
                            "TierBandMethod",
                            "TierBand"
                          ]
                        },
                        "minItems": 1
                      }
                    },
                    "additionalProperties": false,
                    "required": [
                      "TierBandSet"
                    ]
                  },
                  "Overdraft": {
                    "description": "Details about Overdraft rates, fees & charges",
                    "type": "object",
                    "title": "Overdraft",
                    "properties": {
                      "OverdraftTierBandSet": {
                        "description": "Tier band set details",
                        "type": "array",
                        "title": "OverdraftTierBandSet",
                        "items": {
                          "description": "Tier band set details",
                          "type": "object",
                          "properties": {
                            "TierBandMethod": {
                              "description": "The methodology of how overdraft is charged. It can be:\n'Whole'  Where the same charge/rate is applied to the entirety of the overdraft balance (where charges are applicable). \n'Tiered' Where different charges/rates are applied dependent on overdraft maximum and minimum balance amount tiers defined by the lending financial organisation\n'Banded' Where different charges/rates are applied dependent on overdraft maximum and minimum balance amount bands defined by a government organisation.",
                              "title": "TierBandMethod",
                              "type": "string",
                              "enum": [
                                "Tiered",
                                "Whole"
                              ]
                            },
                            "Identification": {
                              "description": "Unique and unambiguous identification of a  Tier Band for a overdraft product.",
                              "title": "Identification",
                              "type": "string",
                              "minLength": 1,
                              "maxLength": 35
                            },
                            "OverdraftTierBand": {
                              "description": "Provides overdraft details for a specific tier or band",
                              "type": "array",
                              "title": "OverdraftTierBand",
                              "items": {
                                "description": "Provides overdraft details for a specific tier or band",
                                "type": "object",
                                "properties": {
                                  "Identification": {
                                    "description": "Unique and unambiguous identification of a  Tier Band for a overdraft.",
                                    "title": "Identification",
                                    "type": "string",
                                    "minLength": 1,
                                    "maxLength": 35
                                  },
                                  "TierValueMin": {
                                    "description": "Minimum value of Overdraft Tier/Band",
                                    "title": "TierValueMin",
                                    "type": "string",
                                    "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                                  },
                                  "TierValueMax": {
                                    "description": "Maximum value of Overdraft Tier/Band",
                                    "title": "TierValueMax",
                                    "type": "string",
                                    "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                                  },
                                  "EAR": {
                                    "description": "EAR means Effective Annual Rate and/or Equivalent Annual Rate (frequently\nused interchangeably), being the actual annual interest rate of an Overdraft.",
                                    "title": "EAR",
                                    "type": "string",
                                    "pattern": "^(-?\\d{1,3}){1}(\\.\\d{1,4}){0,1}$"
                                  },
                                  "OverdraftFeesCharges": {
                                    "description": "Overdraft fees and charges",
                                    "type": "array",
                                    "title": "OverdraftFeesCharges",
                                    "items": {
                                      "description": "Overdraft fees and charges",
                                      "type": "object",
                                      "properties": {
                                        "OverdraftFeeChargeCap": {
                                          "description": "Details about any caps (maximum charges) that apply to a particular fee/charge",
                                          "type": "array",
                                          "title": "OverdraftFeeChargeCap",
                                          "items": {
                                            "description": "Details about any caps (maximum charges) that apply to a particular fee/charge",
                                            "type": "object",
                                            "properties": {
                                              "FeeType": {
                                                "description": "Fee/charge type which is being capped",
                                                "title": "FeeType",
                                                "type": "array",
                                                "items": {
                                                  "description": "Overdraft fee type",
                                                  "type": "string",
                                                  "enum": [
                                                    "ArrangedOverdraft"
                                                  ]
                                                },
                                                "minItems": 1
                                              },
                                              "MinMaxType": {
                                                "description": "Indicates that this is the minimum/ maximum fee/charge that can be applied by the financial institution",
                                                "title": "MinMaxType",
                                                "type": "string",
                                                "enum": [
                                                  "Minimum",
                                                  "Maximum"
                                                ]
                                              },
                                              "FeeCapAmount": {
                                                "description": "Cap amount charged for a fee/charge",
                                                "title": "FeeCapAmount",
                                                "type": "string",
                                                "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
                                              },
                                              "CappingPeriod": {
                                                "description": "Period e.g. day, week, month etc. for which the fee/charge is capped",
                                                "title": "CappingPeriod",
                                                "type": "string",
                                                "enum": [
                                                  "Day",
                                                  "Half Year",
                                                  "Month",
                                                  "Quarter",
                                                  "Week",
                                                  "Year"
                                                ]
                                              }
                                            },
                                            "required": [
                                              "FeeType",
                                              "MinMaxType"
                                            ]
                                          }
                                        },
                                        "OverdraftFeeChargeDetail": {
                                          "description": "Details about the fees/charges",
                                          "type": "array",
                                          "title": "OverdraftFeeChargeDetail",
                                          "items": {
                                            "description": "Details about the fees/charges",
                                            "type": "object",
                                            "properties": {
                                              "FeeType": {
                                                "description": "Overdraft fee type",
                                                "title": "FeeType",
                                                "type": "string",
                                                "enum": [
                                                  "ArrangedOverdraft"
                                                ]
                                              },
                                              "FeeRate": {
                                                "description": "Rate charged for overdraft fee/charge (where it is charged in terms of a rate rather than an amount)",
                                                "title": "FeeRate",
                                                "type": "string",
                                                "pattern": "^(-?\\d{1,3}){1}(\\.\\d{1,4}){0,1}$"
                                              },
                                              "ApplicationFrequency": {
                                                "description": "Frequency at which the overdraft charge is applied to the account",
                                                "title": "ApplicationFrequency",
                                                "type": "string",
                                                "enum": [
                                                  "Daily",
                                                  "Monthly",
                                                  "Quarterly",
                                                  "SixMonthly",
                                                  "Weekly",
                                                  "Yearly"
                                                ]
                                              },
                                              "CalculationFrequency": {
                                                "description": "How often is the overdraft fee/charge calculated for the account.",
                                                "title": "CalculationFrequency",
                                                "type": "string",
                                                "enum": [
                                                  "Daily",
                                                  "Monthly",
                                                  "Quarterly",
                                                  "SixMonthly",
                                                  "Weekly",
                                                  "Yearly"
                                                ]
                                              }
                                            },
                                            "required": [
                                              "FeeType",
                                              "ApplicationFrequency"
                                            ]
                                          },
                                          "minItems": 1
                                        }
                                      },
                                      "required": [
                                        "OverdraftFeeChargeDetail"
                                      ]
                                    }
                                  }
                                },
                                "required": [
                                  "TierValueMin"
                                ]
                              },
                              "minItems": 1
                            }
                          },
                          "required": [
                            "TierBandMethod",
                            "OverdraftTierBand"
                          ]
                        },
                        "minItems": 1
                      }
                    },
                    "additionalProperties": false,
                    "required": [
                      "OverdraftTierBandSet"
                    ]
                  }
                },
                "additionalProperties": false
              }
            }
          }
        }
      }
    },
    "Links": {
      "type": "object",
      "description": "Links relevant to the payload",
      "properties": {
        "Self": {
          "type": "string",
          "format": "uri"
        },
        "First": {
          "type": "string",
          "format": "uri"
        },
        "Prev": {
          "type": "string",
          "format": "uri"
        },
        "Next": {
          "type": "string",
          "format": "uri"
        },
        "Last": {
          "type": "string",
          "format": "uri"
        }
      },
      "additionalProperties": false,
      "required": [
        "Self"
      ]
    },
    "Meta": {
      "title": "MetaData",
      "type": "object",
      "description": "Meta Data relevant to the payload",
      "properties": {
        "TotalPages": {
          "type": "integer",
          "format": "int32"
        },
        "FirstAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "LastAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        }
      },
      "additionalProperties": false
    }
  },
  "additionalProperties": false
}

```

Product details of Other Product which is not avaiable in the standard list

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Data|object|true|none|Aligning with the read write specs structure.|
|» Product|[object]|false|none|none|
|»» ProductName|string|false|none|The name of the Product used for marketing purposes from a customer perspective. I.e. what the customer would recognise.|
|»» ProductId|string|false|none|The unique ID that has been internally assigned by the financial institution to each of the current account banking products they market to their retail and/or small to medium enterprise (SME) customers.|
|»» AccountId|[AccountId](#schemaaccountid)|true|none|A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.|
|»» ProductType|string|true|none|Product type : Personal Current Account, Business Current Account|
|»» MarketingStateId|string|false|none|Unique and unambiguous identification of a  Product Marketing State.|
|»» OtherProductType|object|false|none|Other product type details associated with the account.|
|»»» Name|string|true|none|Long name associated with the product|
|»»» Description|string|true|none|Description of the Product associated with the account|
|»»» CreditInterest|object|false|none|Details about the interest that may be payable to the Account holders|
|»»»» TierBandSet|[object]|true|none|none|
|»»»»» TierBandMethod|string|true|none|The methodology of how credit interest is paid/applied. It can be:-<br>1. Banded<br>Interest rates are banded. i.e. Increasing rate on whole balance as balance increases.<br>2. Tiered<br>Interest rates are tiered. i.e. increasing rate for each tier as balance increases, but interest paid on tier fixed for that tier and not on whole balance.<br>3. Whole<br>The same interest rate is applied irrespective of the product holder's account balance|
|»»»»» Destination|string|true|none|Describes whether accrued interest is payable only to the BCA or to another bank account|
|»»»»» TierBand|[object]|true|none|none|
|»»»»»» TierValueMinimum|string|true|none|Minimum deposit value for which the credit interest tier applies.|
|»»»»»» ApplicationFrequency|string|true|none|How often is interest applied to the Product for this tier/band i.e. how often the financial institution pays accumulated interest to the customer's account.|
|»»»»»» FixedVariableInterestRateType|[OB_InterestFixedVariableType1Code](#schemaob_interestfixedvariabletype1code)|true|none|Type of interest rate, Fixed or Variable|
|»»»»»» AER|string|true|none|The annual equivalent rate (AER) is interest that is calculated under the assumption that any interest paid is combined with the original balance and the next interest payment will be based on the slightly higher account balance. Overall, this means that interest can be compounded several times in a year depending on the number of times that interest payments are made. <br>Read more: Annual Equivalent Rate (AER) http://www.investopedia.com/terms/a/aer.asp#ixzz4gfR7IO1A|
|»» PCA|[OBPCAData1](#schemaobpcadata1)|false|none|none|
|Links|[Links](#schemalinks)|false|none|Links relevant to the payload|
|Meta|[Meta](#schemameta)|false|none|Meta Data relevant to the payload|

#### Enumerated Values

|Property|Value|
|---|---|
|ProductType|CommercialCreditCard|
|ProductType|PersonalCurrentAccount|
|TierBandMethod|INWH|
|Destination|INSC|
|ApplicationFrequency|FQDY|
|ApplicationFrequency|FQWY|
|ApplicationFrequency|FQMY|
|ApplicationFrequency|FQQY|
|ApplicationFrequency|FQHY|
|ApplicationFrequency|FQYY|

<h2 id="tocS_OBReadScheduledPayment3">OBReadScheduledPayment3</h2>
<!-- backwards compatibility -->
<a id="schemaobreadscheduledpayment3"></a>
<a id="schema_OBReadScheduledPayment3"></a>
<a id="tocSobreadscheduledpayment3"></a>
<a id="tocsobreadscheduledpayment3"></a>

```json
{
  "type": "object",
  "required": [
    "Data"
  ],
  "properties": {
    "Data": {
      "type": "object",
      "properties": {
        "ScheduledPayment": {
          "type": "array",
          "items": {
            "type": "object",
            "required": [
              "AccountId",
              "ScheduledPaymentDateTime",
              "ScheduledType",
              "InstructedAmount"
            ],
            "properties": {
              "AccountId": {
                "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
                "type": "string",
                "minLength": 1,
                "maxLength": 40
              },
              "ScheduledPaymentId": {
                "description": "A unique and immutable identifier used to identify the scheduled payment resource. This identifier has no meaning to the account owner.",
                "type": "string",
                "minLength": 1,
                "maxLength": 40
              },
              "ScheduledPaymentDateTime": {
                "description": "The date on which the scheduled payment will be made.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
                "type": "string",
                "format": "date-time"
              },
              "ScheduledType": {
                "description": "Specifies the scheduled payment date type requested",
                "type": "string",
                "enum": [
                  "Arrival",
                  "Execution"
                ]
              },
              "Reference": {
                "description": "Unique reference, as assigned by the creditor, to unambiguously refer to the payment transaction.\nUsage: If available, the initiating party should provide this reference in the structured remittance information, to enable reconciliation by the creditor upon receipt of the amount of money.\nIf the business context requires the use of a creditor reference or a payment remit identification, and only one identifier can be passed through the end-to-end chain, the creditor's reference or payment remittance identification should be quoted in the end-to-end transaction identification.",
                "type": "string",
                "minLength": 1,
                "maxLength": 35
              },
              "InstructedAmount": {
                "type": "object",
                "required": [
                  "Amount",
                  "Currency"
                ],
                "description": "Amount of money to be moved between the debtor and creditor, before deduction of charges, expressed in the currency as ordered by the initiating party.\nUsage: This amount has to be transported unchanged through the transaction chain.",
                "properties": {
                  "Amount": {
                    "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
                    "type": "string",
                    "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
                  },
                  "Currency": {
                    "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
                    "type": "string",
                    "pattern": "^[A-Z]{3,3}$"
                  }
                }
              },
              "CreditorAccount": {
                "type": "object",
                "required": [
                  "SchemeName",
                  "Identification"
                ],
                "description": "Provides the details to identify the beneficiary account.",
                "properties": {
                  "SchemeName": {
                    "description": "Name of the identification scheme, in a coded form as published in an external list.",
                    "type": "string",
                    "x-namespaced-enum": [
                      "UK.OBIE.SortCodeAccountNumber"
                    ]
                  },
                  "Identification": {
                    "description": "Beneficiary account identification.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 256
                  },
                  "Name": {
                    "description": "The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.\nNote, the account name is not the product name or the nickname of the account.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 350
                  },
                  "SecondaryIdentification": {
                    "description": "This is secondary identification of the account, as assigned by the account servicing institution. \nThis can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 34
                  }
                }
              }
            },
            "additionalProperties": false
          }
        }
      }
    },
    "Links": {
      "type": "object",
      "description": "Links relevant to the payload",
      "properties": {
        "Self": {
          "type": "string",
          "format": "uri"
        },
        "First": {
          "type": "string",
          "format": "uri"
        },
        "Prev": {
          "type": "string",
          "format": "uri"
        },
        "Next": {
          "type": "string",
          "format": "uri"
        },
        "Last": {
          "type": "string",
          "format": "uri"
        }
      },
      "additionalProperties": false,
      "required": [
        "Self"
      ]
    },
    "Meta": {
      "title": "MetaData",
      "type": "object",
      "description": "Meta Data relevant to the payload",
      "properties": {
        "TotalPages": {
          "type": "integer",
          "format": "int32"
        },
        "FirstAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "LastAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        }
      },
      "additionalProperties": false
    }
  },
  "additionalProperties": false
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Data|object|true|none|none|
|» ScheduledPayment|[[OBScheduledPayment3](#schemaobscheduledpayment3)]|false|none|none|
|Links|[Links](#schemalinks)|false|none|Links relevant to the payload|
|Meta|[Meta](#schemameta)|false|none|Meta Data relevant to the payload|

<h2 id="tocS_OBReadStandingOrder6">OBReadStandingOrder6</h2>
<!-- backwards compatibility -->
<a id="schemaobreadstandingorder6"></a>
<a id="schema_OBReadStandingOrder6"></a>
<a id="tocSobreadstandingorder6"></a>
<a id="tocsobreadstandingorder6"></a>

```json
{
  "type": "object",
  "required": [
    "Data"
  ],
  "properties": {
    "Data": {
      "type": "object",
      "properties": {
        "StandingOrder": {
          "type": "array",
          "items": {
            "type": "object",
            "required": [
              "AccountId",
              "Frequency"
            ],
            "properties": {
              "AccountId": {
                "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
                "type": "string",
                "minLength": 1,
                "maxLength": 40
              },
              "StandingOrderId": {
                "description": "A unique and immutable identifier used to identify the standing order resource. This identifier has no meaning to the account owner.",
                "type": "string",
                "minLength": 1,
                "maxLength": 40
              },
              "Frequency": {
                "description": "Individual Definitions:\nNotKnown - Not Known\nEvryDay - Every day\nEvryWorkgDay - Every working day\nIntrvlDay - An interval specified in number of calendar days (02 to 31)\nIntrvlWkDay - An interval specified in weeks (01 to 09), and the day within the week (01 to 07)\nWkInMnthDay - A monthly interval, specifying the week of the month (01 to 05) and day within the week (01 to 07)\nIntrvlMnthDay - An interval specified in months (between 01 to 06, 12, 24), specifying the day within the month (-05 to -01, 01 to 31)\nQtrDay - Quarterly (either ENGLISH, SCOTTISH, or RECEIVED)\nENGLISH = Paid on the 25th March, 24th June, 29th September and 25th December.\nSCOTTISH = Paid on the 2nd February, 15th May, 1st August and 11th November.\nRECEIVED = Paid on the 20th March, 19th June, 24th September and 20th December.\nIndividual Patterns:\nNotKnown (ScheduleCode)\nEvryDay (ScheduleCode)\nEvryWorkgDay (ScheduleCode)\nIntrvlDay:NoOfDay (ScheduleCode + NoOfDay)\nIntrvlWkDay:IntervalInWeeks:DayInWeek (ScheduleCode + IntervalInWeeks + DayInWeek)\nWkInMnthDay:WeekInMonth:DayInWeek (ScheduleCode + WeekInMonth + DayInWeek)\nIntrvlMnthDay:IntervalInMonths:DayInMonth (ScheduleCode + IntervalInMonths + DayInMonth)\nQtrDay: + either (ENGLISH, SCOTTISH or RECEIVED) ScheduleCode + QuarterDay\nThe regular expression for this element combines five smaller versions for each permitted pattern. To aid legibility - the components are presented individually here:\nNotKnown\nEvryDay\nEvryWorkgDay\nIntrvlDay:((0[2-9])|([1-2][0-9])|3[0-1])\nIntrvlWkDay:0[1-9]:0[1-7]\nWkInMnthDay:0[1-5]:0[1-7]\nIntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01])\nQtrDay:(ENGLISH|SCOTTISH|RECEIVED)\nFull Regular Expression:\n^(NotKnown)$|^(EvryDay)$|^(EvryWorkgDay)$|^(IntrvlDay:((0[2-9])|([1-2][0-9])|3[0-1]))$|^(IntrvlWkDay:0[1-9]:0[1-7])$|^(WkInMnthDay:0[1-5]:0[1-7])$|^(IntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01]))$|^(QtrDay:(ENGLISH|SCOTTISH|RECEIVED))$",
                "type": "string",
                "pattern": "^(NotKnown)$|^(EvryDay)$|^(EvryWorkgDay)$|^(IntrvlDay:((0[2-9])|([1-2][0-9])|3[0-1]))$|^(IntrvlWkDay:0[1-9]:0[1-7])$|^(WkInMnthDay:0[1-5]:0[1-7])$|^(IntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01]))$|^(QtrDay:(ENGLISH|SCOTTISH|RECEIVED))$"
              },
              "Reference": {
                "description": "Unique reference, as assigned by the creditor, to unambiguously refer to the payment transaction.\nUsage: If available, the initiating party should provide this reference in the structured remittance information, to enable reconciliation by the creditor upon receipt of the amount of money.\nIf the business context requires the use of a creditor reference or a payment remit identification, and only one identifier can be passed through the end-to-end chain, the creditor's reference or payment remittance identification should be quoted in the end-to-end transaction identification.",
                "type": "string",
                "minLength": 1,
                "maxLength": 35
              },
              "NextPaymentDateTime": {
                "description": "The date on which the next payment for a Standing Order schedule will be made.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
                "type": "string",
                "format": "date-time"
              },
              "StandingOrderStatusCode": {
                "description": "Specifies the status of the standing order in code form.",
                "type": "string",
                "enum": [
                  "Active",
                  "Inactive"
                ]
              },
              "NextPaymentAmount": {
                "type": "object",
                "required": [
                  "Amount",
                  "Currency"
                ],
                "description": "The amount of the next Standing Order.",
                "properties": {
                  "Amount": {
                    "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
                    "type": "string",
                    "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
                  },
                  "Currency": {
                    "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
                    "type": "string",
                    "pattern": "^[A-Z]{3,3}$"
                  }
                }
              },
              "CreditorAccount": {
                "type": "object",
                "required": [
                  "SchemeName",
                  "Identification"
                ],
                "description": "Provides the details to identify the beneficiary account.",
                "properties": {
                  "SchemeName": {
                    "description": "Name of the identification scheme, in a coded form as published in an external list.",
                    "type": "string",
                    "x-namespaced-enum": [
                      "UK.OBIE.SortCodeAccountNumber"
                    ]
                  },
                  "Identification": {
                    "description": "Beneficiary account identification.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 256
                  },
                  "Name": {
                    "description": "The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.\nNote, the account name is not the product name or the nickname of the account.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 350
                  },
                  "SecondaryIdentification": {
                    "description": "This is secondary identification of the account, as assigned by the account servicing institution. \nThis can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 34
                  }
                }
              }
            },
            "additionalProperties": false
          }
        }
      }
    },
    "Links": {
      "type": "object",
      "description": "Links relevant to the payload",
      "properties": {
        "Self": {
          "type": "string",
          "format": "uri"
        },
        "First": {
          "type": "string",
          "format": "uri"
        },
        "Prev": {
          "type": "string",
          "format": "uri"
        },
        "Next": {
          "type": "string",
          "format": "uri"
        },
        "Last": {
          "type": "string",
          "format": "uri"
        }
      },
      "additionalProperties": false,
      "required": [
        "Self"
      ]
    },
    "Meta": {
      "title": "MetaData",
      "type": "object",
      "description": "Meta Data relevant to the payload",
      "properties": {
        "TotalPages": {
          "type": "integer",
          "format": "int32"
        },
        "FirstAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "LastAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        }
      },
      "additionalProperties": false
    }
  },
  "additionalProperties": false
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Data|object|true|none|none|
|» StandingOrder|[[OBStandingOrder6](#schemaobstandingorder6)]|false|none|none|
|Links|[Links](#schemalinks)|false|none|Links relevant to the payload|
|Meta|[Meta](#schemameta)|false|none|Meta Data relevant to the payload|

<h2 id="tocS_OBReadTransaction6">OBReadTransaction6</h2>
<!-- backwards compatibility -->
<a id="schemaobreadtransaction6"></a>
<a id="schema_OBReadTransaction6"></a>
<a id="tocSobreadtransaction6"></a>
<a id="tocsobreadtransaction6"></a>

```json
{
  "type": "object",
  "required": [
    "Data"
  ],
  "properties": {
    "Data": {
      "type": "object",
      "properties": {
        "Transaction": {
          "type": "array",
          "items": {
            "type": "object",
            "description": "Provides further details on an entry in the report.",
            "required": [
              "AccountId",
              "CreditDebitIndicator",
              "Status",
              "BookingDateTime",
              "Amount"
            ],
            "properties": {
              "AccountId": {
                "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
                "type": "string",
                "minLength": 1,
                "maxLength": 40
              },
              "TransactionId": {
                "description": "Unique identifier for the transaction within an servicing institution. This identifier is both unique and immutable.",
                "type": "string",
                "minLength": 1,
                "maxLength": 210
              },
              "TransactionReference": {
                "description": "Unique reference for the transaction. This reference is optionally populated, and may as an example be the FPID in the Faster Payments context.",
                "type": "string",
                "minLength": 1,
                "maxLength": 210
              },
              "CreditDebitIndicator": {
                "description": "Indicates whether the transaction is a credit or a debit entry.",
                "type": "string",
                "enum": [
                  "Credit",
                  "Debit"
                ]
              },
              "Status": {
                "description": "Status of a transaction entry on the books of the account servicer.",
                "type": "string",
                "enum": [
                  "Booked",
                  "Pending"
                ]
              },
              "BookingDateTime": {
                "description": "Date and time when a transaction entry is posted to an account on the account servicer's books.\nUsage: Booking date is the expected booking date, unless the status is booked, in which case it is the actual booking date.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
                "type": "string",
                "format": "date-time"
              },
              "ValueDateTime": {
                "description": "Date and time at which assets become available to the account owner in case of a credit entry, or cease to be available to the account owner in case of a debit transaction entry.\nUsage: If transaction entry status is pending and value date is present, then the value date refers to an expected/requested value date.\nFor transaction entries subject to availability/float and for which availability information is provided, the value date must not be used. In this case the availability component identifies the number of availability days.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
                "type": "string",
                "format": "date-time"
              },
              "TransactionInformation": {
                "description": "Further details of the transaction. \nThis is the transaction narrative, which is unstructured text.",
                "type": "string",
                "minLength": 1,
                "maxLength": 500
              },
              "AddressLine": {
                "description": "Information that locates and identifies a specific address for a transaction entry, that is presented in free format text.",
                "type": "string",
                "minLength": 1,
                "maxLength": 70
              },
              "Amount": {
                "type": "object",
                "required": [
                  "Amount",
                  "Currency"
                ],
                "description": "Amount of money in the cash transaction entry.",
                "properties": {
                  "Amount": {
                    "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
                    "type": "string",
                    "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
                  },
                  "Currency": {
                    "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
                    "type": "string",
                    "pattern": "^[A-Z]{3,3}$"
                  }
                }
              },
              "CurrencyExchange": {
                "type": "object",
                "required": [
                  "SourceCurrency",
                  "ExchangeRate"
                ],
                "description": "Set of elements used to provide details on the currency exchange.",
                "properties": {
                  "SourceCurrency": {
                    "description": "Currency from which an amount is to be converted in a currency conversion.",
                    "type": "string",
                    "pattern": "^[A-Z]{3,3}$"
                  },
                  "TargetCurrency": {
                    "description": "Currency into which an amount is to be converted in a currency conversion.",
                    "type": "string",
                    "pattern": "^[A-Z]{3,3}$"
                  },
                  "UnitCurrency": {
                    "description": "Currency in which the rate of exchange is expressed in a currency exchange. In the example 1GBP = xxxCUR, the unit currency is GBP.",
                    "type": "string",
                    "pattern": "^[A-Z]{3,3}$"
                  },
                  "ExchangeRate": {
                    "description": "Factor used to convert an amount from one currency into another. This reflects the price at which one currency was bought with another currency.\nUsage: ExchangeRate expresses the ratio between UnitCurrency and QuotedCurrency (ExchangeRate = UnitCurrency/QuotedCurrency).",
                    "type": "number"
                  },
                  "InstructedAmount": {
                    "type": "object",
                    "required": [
                      "Amount",
                      "Currency"
                    ],
                    "description": "Amount of money to be moved between the debtor and creditor, before deduction of charges, expressed in the currency as ordered by the initiating party.",
                    "properties": {
                      "Amount": {
                        "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
                        "type": "string",
                        "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
                      },
                      "Currency": {
                        "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
                        "type": "string",
                        "pattern": "^[A-Z]{3,3}$"
                      }
                    }
                  }
                }
              },
              "ProprietaryBankTransactionCode": {
                "type": "object",
                "required": [
                  "Code"
                ],
                "description": "Set of elements to fully identify a proprietary bank transaction code.",
                "properties": {
                  "Code": {
                    "description": "Proprietary bank transaction code to identify the underlying transaction.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 35
                  }
                },
                "additionalProperties": false
              },
              "Balance": {
                "type": "object",
                "required": [
                  "CreditDebitIndicator",
                  "Type",
                  "Amount"
                ],
                "description": "Set of elements used to define the balance as a numerical representation of the net increases and decreases in an account after a transaction entry is applied to the account.",
                "properties": {
                  "CreditDebitIndicator": {
                    "description": "Indicates whether the balance is a credit or a debit balance. \nUsage: A zero balance is considered to be a credit balance.",
                    "type": "string",
                    "enum": [
                      "Credit",
                      "Debit"
                    ]
                  },
                  "Type": {
                    "description": "Balance type, in a coded form.",
                    "type": "string",
                    "enum": [
                      "Expected",
                      "InterimBooked"
                    ]
                  },
                  "Amount": {
                    "type": "object",
                    "required": [
                      "Amount",
                      "Currency"
                    ],
                    "description": "Amount of money of the cash balance after a transaction entry is applied to the account..",
                    "properties": {
                      "Amount": {
                        "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
                        "type": "string",
                        "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
                      },
                      "Currency": {
                        "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
                        "type": "string",
                        "pattern": "^[A-Z]{3,3}$"
                      }
                    }
                  }
                },
                "additionalProperties": false
              },
              "MerchantDetails": {
                "type": "object",
                "description": "Details of the merchant involved in the transaction.",
                "properties": {
                  "MerchantCategoryCode": {
                    "description": "Category code conform to ISO 18245, related to the type of services or goods the merchant provides for the transaction.",
                    "type": "string",
                    "minLength": 3,
                    "maxLength": 4
                  }
                }
              },
              "CreditorAccount": {
                "type": "object",
                "description": "Unambiguous identification of the account of the creditor, in the case of a debit transaction.",
                "properties": {
                  "SchemeName": {
                    "description": "Name of the identification scheme, in a coded form as published in an external list.",
                    "type": "string",
                    "x-namespaced-enum": [
                      "UK.OBIE.SortCodeAccountNumber"
                    ]
                  },
                  "Identification": {
                    "description": "Identification assigned by an institution to identify an account. This identification is known by the account owner.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 256
                  },
                  "Name": {
                    "description": "The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.\nNote, the account name is not the product name or the nickname of the account.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 350
                  },
                  "SecondaryIdentification": {
                    "description": "This is secondary identification of the account, as assigned by the account servicing institution. \nThis can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 34
                  }
                }
              },
              "DebtorAccount": {
                "type": "object",
                "description": "Unambiguous identification of the account of the debtor, in the case of a crebit transaction.",
                "properties": {
                  "SchemeName": {
                    "description": "Name of the identification scheme, in a coded form as published in an external list.",
                    "type": "string",
                    "x-namespaced-enum": [
                      "UK.OBIE.SortCodeAccountNumber"
                    ]
                  },
                  "Identification": {
                    "description": "Identification assigned by an institution to identify an account. This identification is known by the account owner.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 256
                  },
                  "Name": {
                    "description": "The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.\nNote, the account name is not the product name or the nickname of the account.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 350
                  },
                  "SecondaryIdentification": {
                    "description": "This is secondary identification of the account, as assigned by the account servicing institution. \nThis can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 34
                  }
                }
              },
              "CardInstrument": {
                "type": "object",
                "required": [
                  "CardSchemeName"
                ],
                "description": "Set of elements to describe the card instrument used in the transaction.",
                "properties": {
                  "CardSchemeName": {
                    "description": "Name of the card scheme.",
                    "type": "string",
                    "enum": [
                      "AmericanExpress",
                      "Diners",
                      "Discover",
                      "MasterCard",
                      "VISA"
                    ]
                  },
                  "AuthorisationType": {
                    "description": "The card authorisation type.",
                    "type": "string",
                    "enum": [
                      "ConsumerDevice",
                      "Contactless",
                      "None",
                      "PIN"
                    ]
                  },
                  "Name": {
                    "description": "Name of the cardholder using the card instrument.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 70
                  },
                  "Identification": {
                    "description": "Identification assigned by an institution to identify the card instrument used in the transaction. This identification is known by the account owner, and may be masked.",
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 34
                  }
                },
                "additionalProperties": false
              }
            },
            "additionalProperties": false
          }
        }
      },
      "additionalProperties": false
    },
    "Links": {
      "type": "object",
      "description": "Links relevant to the payload",
      "properties": {
        "Self": {
          "type": "string",
          "format": "uri"
        },
        "First": {
          "type": "string",
          "format": "uri"
        },
        "Prev": {
          "type": "string",
          "format": "uri"
        },
        "Next": {
          "type": "string",
          "format": "uri"
        },
        "Last": {
          "type": "string",
          "format": "uri"
        }
      },
      "additionalProperties": false,
      "required": [
        "Self"
      ]
    },
    "Meta": {
      "title": "MetaData",
      "type": "object",
      "description": "Meta Data relevant to the payload",
      "properties": {
        "TotalPages": {
          "type": "integer",
          "format": "int32"
        },
        "FirstAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        },
        "LastAvailableDateTime": {
          "description": "All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
          "type": "string",
          "format": "date-time"
        }
      },
      "additionalProperties": false
    }
  },
  "additionalProperties": false
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Data|[OBReadDataTransaction6](#schemaobreaddatatransaction6)|true|none|none|
|Links|[Links](#schemalinks)|false|none|Links relevant to the payload|
|Meta|[Meta](#schemameta)|false|none|Meta Data relevant to the payload|

<h2 id="tocS_OBRisk2">OBRisk2</h2>
<!-- backwards compatibility -->
<a id="schemaobrisk2"></a>
<a id="schema_OBRisk2"></a>
<a id="tocSobrisk2"></a>
<a id="tocsobrisk2"></a>

```json
{
  "description": "The Risk section is sent by the initiating party to the ASPSP. It is used to specify additional details for risk scoring for Account Info.",
  "type": "object",
  "properties": {},
  "additionalProperties": false
}

```

The Risk section is sent by the initiating party to the ASPSP. It is used to specify additional details for risk scoring for Account Info.

### Properties

*None*

<h2 id="tocS_OBScheduledPayment3">OBScheduledPayment3</h2>
<!-- backwards compatibility -->
<a id="schemaobscheduledpayment3"></a>
<a id="schema_OBScheduledPayment3"></a>
<a id="tocSobscheduledpayment3"></a>
<a id="tocsobscheduledpayment3"></a>

```json
{
  "type": "object",
  "required": [
    "AccountId",
    "ScheduledPaymentDateTime",
    "ScheduledType",
    "InstructedAmount"
  ],
  "properties": {
    "AccountId": {
      "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
      "type": "string",
      "minLength": 1,
      "maxLength": 40
    },
    "ScheduledPaymentId": {
      "description": "A unique and immutable identifier used to identify the scheduled payment resource. This identifier has no meaning to the account owner.",
      "type": "string",
      "minLength": 1,
      "maxLength": 40
    },
    "ScheduledPaymentDateTime": {
      "description": "The date on which the scheduled payment will be made.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
      "type": "string",
      "format": "date-time"
    },
    "ScheduledType": {
      "description": "Specifies the scheduled payment date type requested",
      "type": "string",
      "enum": [
        "Arrival",
        "Execution"
      ]
    },
    "Reference": {
      "description": "Unique reference, as assigned by the creditor, to unambiguously refer to the payment transaction.\nUsage: If available, the initiating party should provide this reference in the structured remittance information, to enable reconciliation by the creditor upon receipt of the amount of money.\nIf the business context requires the use of a creditor reference or a payment remit identification, and only one identifier can be passed through the end-to-end chain, the creditor's reference or payment remittance identification should be quoted in the end-to-end transaction identification.",
      "type": "string",
      "minLength": 1,
      "maxLength": 35
    },
    "InstructedAmount": {
      "type": "object",
      "required": [
        "Amount",
        "Currency"
      ],
      "description": "Amount of money to be moved between the debtor and creditor, before deduction of charges, expressed in the currency as ordered by the initiating party.\nUsage: This amount has to be transported unchanged through the transaction chain.",
      "properties": {
        "Amount": {
          "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
          "type": "string",
          "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
        },
        "Currency": {
          "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
          "type": "string",
          "pattern": "^[A-Z]{3,3}$"
        }
      }
    },
    "CreditorAccount": {
      "type": "object",
      "required": [
        "SchemeName",
        "Identification"
      ],
      "description": "Provides the details to identify the beneficiary account.",
      "properties": {
        "SchemeName": {
          "description": "Name of the identification scheme, in a coded form as published in an external list.",
          "type": "string",
          "x-namespaced-enum": [
            "UK.OBIE.SortCodeAccountNumber"
          ]
        },
        "Identification": {
          "description": "Beneficiary account identification.",
          "type": "string",
          "minLength": 1,
          "maxLength": 256
        },
        "Name": {
          "description": "The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.\nNote, the account name is not the product name or the nickname of the account.",
          "type": "string",
          "minLength": 1,
          "maxLength": 350
        },
        "SecondaryIdentification": {
          "description": "This is secondary identification of the account, as assigned by the account servicing institution. \nThis can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).",
          "type": "string",
          "minLength": 1,
          "maxLength": 34
        }
      }
    }
  },
  "additionalProperties": false
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|AccountId|[AccountId](#schemaaccountid)|true|none|A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.|
|ScheduledPaymentId|[ScheduledPaymentId](#schemascheduledpaymentid)|false|none|A unique and immutable identifier used to identify the scheduled payment resource. This identifier has no meaning to the account owner.|
|ScheduledPaymentDateTime|[ScheduledPaymentDateTime](#schemascheduledpaymentdatetime)|true|none|The date on which the scheduled payment will be made.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|
|ScheduledType|[OBExternalScheduleType1Code](#schemaobexternalscheduletype1code)|true|none|Specifies the scheduled payment date type requested|
|Reference|[Reference](#schemareference)|false|none|Unique reference, as assigned by the creditor, to unambiguously refer to the payment transaction.<br>Usage: If available, the initiating party should provide this reference in the structured remittance information, to enable reconciliation by the creditor upon receipt of the amount of money.<br>If the business context requires the use of a creditor reference or a payment remit identification, and only one identifier can be passed through the end-to-end chain, the creditor's reference or payment remittance identification should be quoted in the end-to-end transaction identification.|
|InstructedAmount|[OBActiveOrHistoricCurrencyAndAmount_1](#schemaobactiveorhistoriccurrencyandamount_1)|true|none|Amount of money to be moved between the debtor and creditor, before deduction of charges, expressed in the currency as ordered by the initiating party.<br>Usage: This amount has to be transported unchanged through the transaction chain.|
|CreditorAccount|[OBCashAccount5_1](#schemaobcashaccount5_1)|false|none|Provides the details to identify the beneficiary account.|

<h2 id="tocS_OBScheduledPayment3Basic">OBScheduledPayment3Basic</h2>
<!-- backwards compatibility -->
<a id="schemaobscheduledpayment3basic"></a>
<a id="schema_OBScheduledPayment3Basic"></a>
<a id="tocSobscheduledpayment3basic"></a>
<a id="tocsobscheduledpayment3basic"></a>

```json
{
  "type": "object",
  "required": [
    "AccountId",
    "ScheduledPaymentDateTime",
    "ScheduledType",
    "InstructedAmount"
  ],
  "properties": {
    "AccountId": {
      "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
      "type": "string",
      "minLength": 1,
      "maxLength": 40
    },
    "ScheduledPaymentId": {
      "description": "A unique and immutable identifier used to identify the scheduled payment resource. This identifier has no meaning to the account owner.",
      "type": "string",
      "minLength": 1,
      "maxLength": 40
    },
    "ScheduledPaymentDateTime": {
      "description": "The date on which the scheduled payment will be made.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
      "type": "string",
      "format": "date-time"
    },
    "ScheduledType": {
      "description": "Specifies the scheduled payment date type requested",
      "type": "string",
      "enum": [
        "Arrival",
        "Execution"
      ]
    },
    "Reference": {
      "description": "Unique reference, as assigned by the creditor, to unambiguously refer to the payment transaction.\nUsage: If available, the initiating party should provide this reference in the structured remittance information, to enable reconciliation by the creditor upon receipt of the amount of money.\nIf the business context requires the use of a creditor reference or a payment remit identification, and only one identifier can be passed through the end-to-end chain, the creditor's reference or payment remittance identification should be quoted in the end-to-end transaction identification.",
      "type": "string",
      "minLength": 1,
      "maxLength": 35
    },
    "InstructedAmount": {
      "type": "object",
      "required": [
        "Amount",
        "Currency"
      ],
      "description": "Amount of money to be moved between the debtor and creditor, before deduction of charges, expressed in the currency as ordered by the initiating party.\nUsage: This amount has to be transported unchanged through the transaction chain.",
      "properties": {
        "Amount": {
          "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
          "type": "string",
          "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
        },
        "Currency": {
          "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
          "type": "string",
          "pattern": "^[A-Z]{3,3}$"
        }
      }
    }
  },
  "additionalProperties": false
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|AccountId|[AccountId](#schemaaccountid)|true|none|A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.|
|ScheduledPaymentId|[ScheduledPaymentId](#schemascheduledpaymentid)|false|none|A unique and immutable identifier used to identify the scheduled payment resource. This identifier has no meaning to the account owner.|
|ScheduledPaymentDateTime|[ScheduledPaymentDateTime](#schemascheduledpaymentdatetime)|true|none|The date on which the scheduled payment will be made.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|
|ScheduledType|[OBExternalScheduleType1Code](#schemaobexternalscheduletype1code)|true|none|Specifies the scheduled payment date type requested|
|Reference|[Reference](#schemareference)|false|none|Unique reference, as assigned by the creditor, to unambiguously refer to the payment transaction.<br>Usage: If available, the initiating party should provide this reference in the structured remittance information, to enable reconciliation by the creditor upon receipt of the amount of money.<br>If the business context requires the use of a creditor reference or a payment remit identification, and only one identifier can be passed through the end-to-end chain, the creditor's reference or payment remittance identification should be quoted in the end-to-end transaction identification.|
|InstructedAmount|[OBActiveOrHistoricCurrencyAndAmount_1](#schemaobactiveorhistoriccurrencyandamount_1)|true|none|Amount of money to be moved between the debtor and creditor, before deduction of charges, expressed in the currency as ordered by the initiating party.<br>Usage: This amount has to be transported unchanged through the transaction chain.|

<h2 id="tocS_OBScheduledPayment3Detail">OBScheduledPayment3Detail</h2>
<!-- backwards compatibility -->
<a id="schemaobscheduledpayment3detail"></a>
<a id="schema_OBScheduledPayment3Detail"></a>
<a id="tocSobscheduledpayment3detail"></a>
<a id="tocsobscheduledpayment3detail"></a>

```json
{
  "type": "object",
  "required": [
    "AccountId",
    "ScheduledPaymentDateTime",
    "ScheduledType",
    "InstructedAmount",
    "CreditorAccount"
  ],
  "properties": {
    "AccountId": {
      "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
      "type": "string",
      "minLength": 1,
      "maxLength": 40
    },
    "ScheduledPaymentId": {
      "description": "A unique and immutable identifier used to identify the scheduled payment resource. This identifier has no meaning to the account owner.",
      "type": "string",
      "minLength": 1,
      "maxLength": 40
    },
    "ScheduledPaymentDateTime": {
      "description": "The date on which the scheduled payment will be made.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
      "type": "string",
      "format": "date-time"
    },
    "ScheduledType": {
      "description": "Specifies the scheduled payment date type requested",
      "type": "string",
      "enum": [
        "Arrival",
        "Execution"
      ]
    },
    "Reference": {
      "description": "Unique reference, as assigned by the creditor, to unambiguously refer to the payment transaction.\nUsage: If available, the initiating party should provide this reference in the structured remittance information, to enable reconciliation by the creditor upon receipt of the amount of money.\nIf the business context requires the use of a creditor reference or a payment remit identification, and only one identifier can be passed through the end-to-end chain, the creditor's reference or payment remittance identification should be quoted in the end-to-end transaction identification.",
      "type": "string",
      "minLength": 1,
      "maxLength": 35
    },
    "InstructedAmount": {
      "type": "object",
      "required": [
        "Amount",
        "Currency"
      ],
      "description": "Amount of money to be moved between the debtor and creditor, before deduction of charges, expressed in the currency as ordered by the initiating party.\nUsage: This amount has to be transported unchanged through the transaction chain.",
      "properties": {
        "Amount": {
          "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
          "type": "string",
          "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
        },
        "Currency": {
          "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
          "type": "string",
          "pattern": "^[A-Z]{3,3}$"
        }
      }
    },
    "CreditorAccount": {
      "type": "object",
      "required": [
        "SchemeName",
        "Identification"
      ],
      "description": "Provides the details to identify the beneficiary account.",
      "properties": {
        "SchemeName": {
          "description": "Name of the identification scheme, in a coded form as published in an external list.",
          "type": "string",
          "x-namespaced-enum": [
            "UK.OBIE.SortCodeAccountNumber"
          ]
        },
        "Identification": {
          "description": "Beneficiary account identification.",
          "type": "string",
          "minLength": 1,
          "maxLength": 256
        },
        "Name": {
          "description": "The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.\nNote, the account name is not the product name or the nickname of the account.",
          "type": "string",
          "minLength": 1,
          "maxLength": 350
        },
        "SecondaryIdentification": {
          "description": "This is secondary identification of the account, as assigned by the account servicing institution. \nThis can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).",
          "type": "string",
          "minLength": 1,
          "maxLength": 34
        }
      }
    }
  },
  "additionalProperties": false
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|AccountId|[AccountId](#schemaaccountid)|true|none|A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.|
|ScheduledPaymentId|[ScheduledPaymentId](#schemascheduledpaymentid)|false|none|A unique and immutable identifier used to identify the scheduled payment resource. This identifier has no meaning to the account owner.|
|ScheduledPaymentDateTime|[ScheduledPaymentDateTime](#schemascheduledpaymentdatetime)|true|none|The date on which the scheduled payment will be made.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|
|ScheduledType|[OBExternalScheduleType1Code](#schemaobexternalscheduletype1code)|true|none|Specifies the scheduled payment date type requested|
|Reference|[Reference](#schemareference)|false|none|Unique reference, as assigned by the creditor, to unambiguously refer to the payment transaction.<br>Usage: If available, the initiating party should provide this reference in the structured remittance information, to enable reconciliation by the creditor upon receipt of the amount of money.<br>If the business context requires the use of a creditor reference or a payment remit identification, and only one identifier can be passed through the end-to-end chain, the creditor's reference or payment remittance identification should be quoted in the end-to-end transaction identification.|
|InstructedAmount|[OBActiveOrHistoricCurrencyAndAmount_1](#schemaobactiveorhistoriccurrencyandamount_1)|true|none|Amount of money to be moved between the debtor and creditor, before deduction of charges, expressed in the currency as ordered by the initiating party.<br>Usage: This amount has to be transported unchanged through the transaction chain.|
|CreditorAccount|[OBCashAccount5_1](#schemaobcashaccount5_1)|true|none|Provides the details to identify the beneficiary account.|

<h2 id="tocS_OBStandingOrder6">OBStandingOrder6</h2>
<!-- backwards compatibility -->
<a id="schemaobstandingorder6"></a>
<a id="schema_OBStandingOrder6"></a>
<a id="tocSobstandingorder6"></a>
<a id="tocsobstandingorder6"></a>

```json
{
  "type": "object",
  "required": [
    "AccountId",
    "Frequency"
  ],
  "properties": {
    "AccountId": {
      "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
      "type": "string",
      "minLength": 1,
      "maxLength": 40
    },
    "StandingOrderId": {
      "description": "A unique and immutable identifier used to identify the standing order resource. This identifier has no meaning to the account owner.",
      "type": "string",
      "minLength": 1,
      "maxLength": 40
    },
    "Frequency": {
      "description": "Individual Definitions:\nNotKnown - Not Known\nEvryDay - Every day\nEvryWorkgDay - Every working day\nIntrvlDay - An interval specified in number of calendar days (02 to 31)\nIntrvlWkDay - An interval specified in weeks (01 to 09), and the day within the week (01 to 07)\nWkInMnthDay - A monthly interval, specifying the week of the month (01 to 05) and day within the week (01 to 07)\nIntrvlMnthDay - An interval specified in months (between 01 to 06, 12, 24), specifying the day within the month (-05 to -01, 01 to 31)\nQtrDay - Quarterly (either ENGLISH, SCOTTISH, or RECEIVED)\nENGLISH = Paid on the 25th March, 24th June, 29th September and 25th December.\nSCOTTISH = Paid on the 2nd February, 15th May, 1st August and 11th November.\nRECEIVED = Paid on the 20th March, 19th June, 24th September and 20th December.\nIndividual Patterns:\nNotKnown (ScheduleCode)\nEvryDay (ScheduleCode)\nEvryWorkgDay (ScheduleCode)\nIntrvlDay:NoOfDay (ScheduleCode + NoOfDay)\nIntrvlWkDay:IntervalInWeeks:DayInWeek (ScheduleCode + IntervalInWeeks + DayInWeek)\nWkInMnthDay:WeekInMonth:DayInWeek (ScheduleCode + WeekInMonth + DayInWeek)\nIntrvlMnthDay:IntervalInMonths:DayInMonth (ScheduleCode + IntervalInMonths + DayInMonth)\nQtrDay: + either (ENGLISH, SCOTTISH or RECEIVED) ScheduleCode + QuarterDay\nThe regular expression for this element combines five smaller versions for each permitted pattern. To aid legibility - the components are presented individually here:\nNotKnown\nEvryDay\nEvryWorkgDay\nIntrvlDay:((0[2-9])|([1-2][0-9])|3[0-1])\nIntrvlWkDay:0[1-9]:0[1-7]\nWkInMnthDay:0[1-5]:0[1-7]\nIntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01])\nQtrDay:(ENGLISH|SCOTTISH|RECEIVED)\nFull Regular Expression:\n^(NotKnown)$|^(EvryDay)$|^(EvryWorkgDay)$|^(IntrvlDay:((0[2-9])|([1-2][0-9])|3[0-1]))$|^(IntrvlWkDay:0[1-9]:0[1-7])$|^(WkInMnthDay:0[1-5]:0[1-7])$|^(IntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01]))$|^(QtrDay:(ENGLISH|SCOTTISH|RECEIVED))$",
      "type": "string",
      "pattern": "^(NotKnown)$|^(EvryDay)$|^(EvryWorkgDay)$|^(IntrvlDay:((0[2-9])|([1-2][0-9])|3[0-1]))$|^(IntrvlWkDay:0[1-9]:0[1-7])$|^(WkInMnthDay:0[1-5]:0[1-7])$|^(IntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01]))$|^(QtrDay:(ENGLISH|SCOTTISH|RECEIVED))$"
    },
    "Reference": {
      "description": "Unique reference, as assigned by the creditor, to unambiguously refer to the payment transaction.\nUsage: If available, the initiating party should provide this reference in the structured remittance information, to enable reconciliation by the creditor upon receipt of the amount of money.\nIf the business context requires the use of a creditor reference or a payment remit identification, and only one identifier can be passed through the end-to-end chain, the creditor's reference or payment remittance identification should be quoted in the end-to-end transaction identification.",
      "type": "string",
      "minLength": 1,
      "maxLength": 35
    },
    "NextPaymentDateTime": {
      "description": "The date on which the next payment for a Standing Order schedule will be made.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
      "type": "string",
      "format": "date-time"
    },
    "StandingOrderStatusCode": {
      "description": "Specifies the status of the standing order in code form.",
      "type": "string",
      "enum": [
        "Active",
        "Inactive"
      ]
    },
    "NextPaymentAmount": {
      "type": "object",
      "required": [
        "Amount",
        "Currency"
      ],
      "description": "The amount of the next Standing Order.",
      "properties": {
        "Amount": {
          "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
          "type": "string",
          "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
        },
        "Currency": {
          "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
          "type": "string",
          "pattern": "^[A-Z]{3,3}$"
        }
      }
    },
    "CreditorAccount": {
      "type": "object",
      "required": [
        "SchemeName",
        "Identification"
      ],
      "description": "Provides the details to identify the beneficiary account.",
      "properties": {
        "SchemeName": {
          "description": "Name of the identification scheme, in a coded form as published in an external list.",
          "type": "string",
          "x-namespaced-enum": [
            "UK.OBIE.SortCodeAccountNumber"
          ]
        },
        "Identification": {
          "description": "Beneficiary account identification.",
          "type": "string",
          "minLength": 1,
          "maxLength": 256
        },
        "Name": {
          "description": "The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.\nNote, the account name is not the product name or the nickname of the account.",
          "type": "string",
          "minLength": 1,
          "maxLength": 350
        },
        "SecondaryIdentification": {
          "description": "This is secondary identification of the account, as assigned by the account servicing institution. \nThis can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).",
          "type": "string",
          "minLength": 1,
          "maxLength": 34
        }
      }
    }
  },
  "additionalProperties": false
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|AccountId|[AccountId](#schemaaccountid)|true|none|A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.|
|StandingOrderId|[StandingOrderId](#schemastandingorderid)|false|none|A unique and immutable identifier used to identify the standing order resource. This identifier has no meaning to the account owner.|
|Frequency|[Frequency_1](#schemafrequency_1)|true|none|Individual Definitions:<br>NotKnown - Not Known<br>EvryDay - Every day<br>EvryWorkgDay - Every working day<br>IntrvlDay - An interval specified in number of calendar days (02 to 31)<br>IntrvlWkDay - An interval specified in weeks (01 to 09), and the day within the week (01 to 07)<br>WkInMnthDay - A monthly interval, specifying the week of the month (01 to 05) and day within the week (01 to 07)<br>IntrvlMnthDay - An interval specified in months (between 01 to 06, 12, 24), specifying the day within the month (-05 to -01, 01 to 31)<br>QtrDay - Quarterly (either ENGLISH, SCOTTISH, or RECEIVED)<br>ENGLISH = Paid on the 25th March, 24th June, 29th September and 25th December.<br>SCOTTISH = Paid on the 2nd February, 15th May, 1st August and 11th November.<br>RECEIVED = Paid on the 20th March, 19th June, 24th September and 20th December.<br>Individual Patterns:<br>NotKnown (ScheduleCode)<br>EvryDay (ScheduleCode)<br>EvryWorkgDay (ScheduleCode)<br>IntrvlDay:NoOfDay (ScheduleCode + NoOfDay)<br>IntrvlWkDay:IntervalInWeeks:DayInWeek (ScheduleCode + IntervalInWeeks + DayInWeek)<br>WkInMnthDay:WeekInMonth:DayInWeek (ScheduleCode + WeekInMonth + DayInWeek)<br>IntrvlMnthDay:IntervalInMonths:DayInMonth (ScheduleCode + IntervalInMonths + DayInMonth)<br>QtrDay: + either (ENGLISH, SCOTTISH or RECEIVED) ScheduleCode + QuarterDay<br>The regular expression for this element combines five smaller versions for each permitted pattern. To aid legibility - the components are presented individually here:<br>NotKnown<br>EvryDay<br>EvryWorkgDay<br>IntrvlDay:((0[2-9])|([1-2][0-9])|3[0-1])<br>IntrvlWkDay:0[1-9]:0[1-7]<br>WkInMnthDay:0[1-5]:0[1-7]<br>IntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01])<br>QtrDay:(ENGLISH|SCOTTISH|RECEIVED)<br>Full Regular Expression:<br>^(NotKnown)$|^(EvryDay)$|^(EvryWorkgDay)$|^(IntrvlDay:((0[2-9])|([1-2][0-9])|3[0-1]))$|^(IntrvlWkDay:0[1-9]:0[1-7])$|^(WkInMnthDay:0[1-5]:0[1-7])$|^(IntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01]))$|^(QtrDay:(ENGLISH|SCOTTISH|RECEIVED))$|
|Reference|[Reference](#schemareference)|false|none|Unique reference, as assigned by the creditor, to unambiguously refer to the payment transaction.<br>Usage: If available, the initiating party should provide this reference in the structured remittance information, to enable reconciliation by the creditor upon receipt of the amount of money.<br>If the business context requires the use of a creditor reference or a payment remit identification, and only one identifier can be passed through the end-to-end chain, the creditor's reference or payment remittance identification should be quoted in the end-to-end transaction identification.|
|NextPaymentDateTime|[NextPaymentDateTime](#schemanextpaymentdatetime)|false|none|The date on which the next payment for a Standing Order schedule will be made.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|
|StandingOrderStatusCode|[OBExternalStandingOrderStatus1Code](#schemaobexternalstandingorderstatus1code)|false|none|Specifies the status of the standing order in code form.|
|NextPaymentAmount|[OBActiveOrHistoricCurrencyAndAmount_3](#schemaobactiveorhistoriccurrencyandamount_3)|false|none|The amount of the next Standing Order.|
|CreditorAccount|[OBCashAccount5_1](#schemaobcashaccount5_1)|false|none|Provides the details to identify the beneficiary account.|

<h2 id="tocS_OBStandingOrder6Basic">OBStandingOrder6Basic</h2>
<!-- backwards compatibility -->
<a id="schemaobstandingorder6basic"></a>
<a id="schema_OBStandingOrder6Basic"></a>
<a id="tocSobstandingorder6basic"></a>
<a id="tocsobstandingorder6basic"></a>

```json
{
  "type": "object",
  "required": [
    "AccountId",
    "Frequency"
  ],
  "properties": {
    "AccountId": {
      "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
      "type": "string",
      "minLength": 1,
      "maxLength": 40
    },
    "StandingOrderId": {
      "description": "A unique and immutable identifier used to identify the standing order resource. This identifier has no meaning to the account owner.",
      "type": "string",
      "minLength": 1,
      "maxLength": 40
    },
    "Frequency": {
      "description": "Individual Definitions:\nNotKnown - Not Known\nEvryDay - Every day\nEvryWorkgDay - Every working day\nIntrvlDay - An interval specified in number of calendar days (02 to 31)\nIntrvlWkDay - An interval specified in weeks (01 to 09), and the day within the week (01 to 07)\nWkInMnthDay - A monthly interval, specifying the week of the month (01 to 05) and day within the week (01 to 07)\nIntrvlMnthDay - An interval specified in months (between 01 to 06, 12, 24), specifying the day within the month (-05 to -01, 01 to 31)\nQtrDay - Quarterly (either ENGLISH, SCOTTISH, or RECEIVED)\nENGLISH = Paid on the 25th March, 24th June, 29th September and 25th December.\nSCOTTISH = Paid on the 2nd February, 15th May, 1st August and 11th November.\nRECEIVED = Paid on the 20th March, 19th June, 24th September and 20th December.\nIndividual Patterns:\nNotKnown (ScheduleCode)\nEvryDay (ScheduleCode)\nEvryWorkgDay (ScheduleCode)\nIntrvlDay:NoOfDay (ScheduleCode + NoOfDay)\nIntrvlWkDay:IntervalInWeeks:DayInWeek (ScheduleCode + IntervalInWeeks + DayInWeek)\nWkInMnthDay:WeekInMonth:DayInWeek (ScheduleCode + WeekInMonth + DayInWeek)\nIntrvlMnthDay:IntervalInMonths:DayInMonth (ScheduleCode + IntervalInMonths + DayInMonth)\nQtrDay: + either (ENGLISH, SCOTTISH or RECEIVED) ScheduleCode + QuarterDay\nThe regular expression for this element combines five smaller versions for each permitted pattern. To aid legibility - the components are presented individually here:\nNotKnown\nEvryDay\nEvryWorkgDay\nIntrvlDay:((0[2-9])|([1-2][0-9])|3[0-1])\nIntrvlWkDay:0[1-9]:0[1-7]\nWkInMnthDay:0[1-5]:0[1-7]\nIntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01])\nQtrDay:(ENGLISH|SCOTTISH|RECEIVED)\nFull Regular Expression:\n^(NotKnown)$|^(EvryDay)$|^(EvryWorkgDay)$|^(IntrvlDay:((0[2-9])|([1-2][0-9])|3[0-1]))$|^(IntrvlWkDay:0[1-9]:0[1-7])$|^(WkInMnthDay:0[1-5]:0[1-7])$|^(IntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01]))$|^(QtrDay:(ENGLISH|SCOTTISH|RECEIVED))$",
      "type": "string",
      "pattern": "^(NotKnown)$|^(EvryDay)$|^(EvryWorkgDay)$|^(IntrvlDay:((0[2-9])|([1-2][0-9])|3[0-1]))$|^(IntrvlWkDay:0[1-9]:0[1-7])$|^(WkInMnthDay:0[1-5]:0[1-7])$|^(IntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01]))$|^(QtrDay:(ENGLISH|SCOTTISH|RECEIVED))$"
    },
    "Reference": {
      "description": "Unique reference, as assigned by the creditor, to unambiguously refer to the payment transaction.\nUsage: If available, the initiating party should provide this reference in the structured remittance information, to enable reconciliation by the creditor upon receipt of the amount of money.\nIf the business context requires the use of a creditor reference or a payment remit identification, and only one identifier can be passed through the end-to-end chain, the creditor's reference or payment remittance identification should be quoted in the end-to-end transaction identification.",
      "type": "string",
      "minLength": 1,
      "maxLength": 35
    },
    "NextPaymentDateTime": {
      "description": "The date on which the next payment for a Standing Order schedule will be made.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
      "type": "string",
      "format": "date-time"
    },
    "StandingOrderStatusCode": {
      "description": "Specifies the status of the standing order in code form.",
      "type": "string",
      "enum": [
        "Active",
        "Inactive"
      ]
    },
    "NextPaymentAmount": {
      "type": "object",
      "required": [
        "Amount",
        "Currency"
      ],
      "description": "The amount of the next Standing Order.",
      "properties": {
        "Amount": {
          "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
          "type": "string",
          "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
        },
        "Currency": {
          "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
          "type": "string",
          "pattern": "^[A-Z]{3,3}$"
        }
      }
    }
  },
  "additionalProperties": false
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|AccountId|[AccountId](#schemaaccountid)|true|none|A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.|
|StandingOrderId|[StandingOrderId](#schemastandingorderid)|false|none|A unique and immutable identifier used to identify the standing order resource. This identifier has no meaning to the account owner.|
|Frequency|[Frequency_1](#schemafrequency_1)|true|none|Individual Definitions:<br>NotKnown - Not Known<br>EvryDay - Every day<br>EvryWorkgDay - Every working day<br>IntrvlDay - An interval specified in number of calendar days (02 to 31)<br>IntrvlWkDay - An interval specified in weeks (01 to 09), and the day within the week (01 to 07)<br>WkInMnthDay - A monthly interval, specifying the week of the month (01 to 05) and day within the week (01 to 07)<br>IntrvlMnthDay - An interval specified in months (between 01 to 06, 12, 24), specifying the day within the month (-05 to -01, 01 to 31)<br>QtrDay - Quarterly (either ENGLISH, SCOTTISH, or RECEIVED)<br>ENGLISH = Paid on the 25th March, 24th June, 29th September and 25th December.<br>SCOTTISH = Paid on the 2nd February, 15th May, 1st August and 11th November.<br>RECEIVED = Paid on the 20th March, 19th June, 24th September and 20th December.<br>Individual Patterns:<br>NotKnown (ScheduleCode)<br>EvryDay (ScheduleCode)<br>EvryWorkgDay (ScheduleCode)<br>IntrvlDay:NoOfDay (ScheduleCode + NoOfDay)<br>IntrvlWkDay:IntervalInWeeks:DayInWeek (ScheduleCode + IntervalInWeeks + DayInWeek)<br>WkInMnthDay:WeekInMonth:DayInWeek (ScheduleCode + WeekInMonth + DayInWeek)<br>IntrvlMnthDay:IntervalInMonths:DayInMonth (ScheduleCode + IntervalInMonths + DayInMonth)<br>QtrDay: + either (ENGLISH, SCOTTISH or RECEIVED) ScheduleCode + QuarterDay<br>The regular expression for this element combines five smaller versions for each permitted pattern. To aid legibility - the components are presented individually here:<br>NotKnown<br>EvryDay<br>EvryWorkgDay<br>IntrvlDay:((0[2-9])|([1-2][0-9])|3[0-1])<br>IntrvlWkDay:0[1-9]:0[1-7]<br>WkInMnthDay:0[1-5]:0[1-7]<br>IntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01])<br>QtrDay:(ENGLISH|SCOTTISH|RECEIVED)<br>Full Regular Expression:<br>^(NotKnown)$|^(EvryDay)$|^(EvryWorkgDay)$|^(IntrvlDay:((0[2-9])|([1-2][0-9])|3[0-1]))$|^(IntrvlWkDay:0[1-9]:0[1-7])$|^(WkInMnthDay:0[1-5]:0[1-7])$|^(IntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01]))$|^(QtrDay:(ENGLISH|SCOTTISH|RECEIVED))$|
|Reference|[Reference](#schemareference)|false|none|Unique reference, as assigned by the creditor, to unambiguously refer to the payment transaction.<br>Usage: If available, the initiating party should provide this reference in the structured remittance information, to enable reconciliation by the creditor upon receipt of the amount of money.<br>If the business context requires the use of a creditor reference or a payment remit identification, and only one identifier can be passed through the end-to-end chain, the creditor's reference or payment remittance identification should be quoted in the end-to-end transaction identification.|
|NextPaymentDateTime|[NextPaymentDateTime](#schemanextpaymentdatetime)|false|none|The date on which the next payment for a Standing Order schedule will be made.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|
|StandingOrderStatusCode|[OBExternalStandingOrderStatus1Code](#schemaobexternalstandingorderstatus1code)|false|none|Specifies the status of the standing order in code form.|
|NextPaymentAmount|[OBActiveOrHistoricCurrencyAndAmount_3](#schemaobactiveorhistoriccurrencyandamount_3)|false|none|The amount of the next Standing Order.|

<h2 id="tocS_OBStandingOrder6Detail">OBStandingOrder6Detail</h2>
<!-- backwards compatibility -->
<a id="schemaobstandingorder6detail"></a>
<a id="schema_OBStandingOrder6Detail"></a>
<a id="tocSobstandingorder6detail"></a>
<a id="tocsobstandingorder6detail"></a>

```json
{
  "type": "object",
  "required": [
    "AccountId",
    "Frequency"
  ],
  "properties": {
    "AccountId": {
      "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
      "type": "string",
      "minLength": 1,
      "maxLength": 40
    },
    "StandingOrderId": {
      "description": "A unique and immutable identifier used to identify the standing order resource. This identifier has no meaning to the account owner.",
      "type": "string",
      "minLength": 1,
      "maxLength": 40
    },
    "Frequency": {
      "description": "Individual Definitions:\nNotKnown - Not Known\nEvryDay - Every day\nEvryWorkgDay - Every working day\nIntrvlDay - An interval specified in number of calendar days (02 to 31)\nIntrvlWkDay - An interval specified in weeks (01 to 09), and the day within the week (01 to 07)\nWkInMnthDay - A monthly interval, specifying the week of the month (01 to 05) and day within the week (01 to 07)\nIntrvlMnthDay - An interval specified in months (between 01 to 06, 12, 24), specifying the day within the month (-05 to -01, 01 to 31)\nQtrDay - Quarterly (either ENGLISH, SCOTTISH, or RECEIVED)\nENGLISH = Paid on the 25th March, 24th June, 29th September and 25th December.\nSCOTTISH = Paid on the 2nd February, 15th May, 1st August and 11th November.\nRECEIVED = Paid on the 20th March, 19th June, 24th September and 20th December.\nIndividual Patterns:\nNotKnown (ScheduleCode)\nEvryDay (ScheduleCode)\nEvryWorkgDay (ScheduleCode)\nIntrvlDay:NoOfDay (ScheduleCode + NoOfDay)\nIntrvlWkDay:IntervalInWeeks:DayInWeek (ScheduleCode + IntervalInWeeks + DayInWeek)\nWkInMnthDay:WeekInMonth:DayInWeek (ScheduleCode + WeekInMonth + DayInWeek)\nIntrvlMnthDay:IntervalInMonths:DayInMonth (ScheduleCode + IntervalInMonths + DayInMonth)\nQtrDay: + either (ENGLISH, SCOTTISH or RECEIVED) ScheduleCode + QuarterDay\nThe regular expression for this element combines five smaller versions for each permitted pattern. To aid legibility - the components are presented individually here:\nNotKnown\nEvryDay\nEvryWorkgDay\nIntrvlDay:((0[2-9])|([1-2][0-9])|3[0-1])\nIntrvlWkDay:0[1-9]:0[1-7]\nWkInMnthDay:0[1-5]:0[1-7]\nIntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01])\nQtrDay:(ENGLISH|SCOTTISH|RECEIVED)\nFull Regular Expression:\n^(NotKnown)$|^(EvryDay)$|^(EvryWorkgDay)$|^(IntrvlDay:((0[2-9])|([1-2][0-9])|3[0-1]))$|^(IntrvlWkDay:0[1-9]:0[1-7])$|^(WkInMnthDay:0[1-5]:0[1-7])$|^(IntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01]))$|^(QtrDay:(ENGLISH|SCOTTISH|RECEIVED))$",
      "type": "string",
      "pattern": "^(NotKnown)$|^(EvryDay)$|^(EvryWorkgDay)$|^(IntrvlDay:((0[2-9])|([1-2][0-9])|3[0-1]))$|^(IntrvlWkDay:0[1-9]:0[1-7])$|^(WkInMnthDay:0[1-5]:0[1-7])$|^(IntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01]))$|^(QtrDay:(ENGLISH|SCOTTISH|RECEIVED))$"
    },
    "Reference": {
      "description": "Unique reference, as assigned by the creditor, to unambiguously refer to the payment transaction.\nUsage: If available, the initiating party should provide this reference in the structured remittance information, to enable reconciliation by the creditor upon receipt of the amount of money.\nIf the business context requires the use of a creditor reference or a payment remit identification, and only one identifier can be passed through the end-to-end chain, the creditor's reference or payment remittance identification should be quoted in the end-to-end transaction identification.",
      "type": "string",
      "minLength": 1,
      "maxLength": 35
    },
    "NextPaymentDateTime": {
      "description": "The date on which the next payment for a Standing Order schedule will be made.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
      "type": "string",
      "format": "date-time"
    },
    "StandingOrderStatusCode": {
      "description": "Specifies the status of the standing order in code form.",
      "type": "string",
      "enum": [
        "Active",
        "Inactive"
      ]
    },
    "NextPaymentAmount": {
      "type": "object",
      "required": [
        "Amount",
        "Currency"
      ],
      "description": "The amount of the next Standing Order.",
      "properties": {
        "Amount": {
          "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
          "type": "string",
          "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
        },
        "Currency": {
          "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
          "type": "string",
          "pattern": "^[A-Z]{3,3}$"
        }
      }
    },
    "CreditorAgent": {
      "type": "object",
      "required": [
        "SchemeName",
        "Identification"
      ],
      "description": "Party that manages the account on behalf of the account owner, that is manages the registration and booking of entries on the account, calculates balances on the account and provides information about the account.\nThis is the servicer of the beneficiary account.",
      "properties": {
        "SchemeName": {
          "description": "Name of the identification scheme, in a coded form as published in an external list.",
          "type": "string",
          "x-namespaced-enum": [
            "UK.OBIE.BICFI"
          ]
        },
        "Identification": {
          "description": "Unique and unambiguous identification of the servicing institution.",
          "type": "string",
          "minLength": 1,
          "maxLength": 35
        }
      }
    }
  },
  "additionalProperties": false
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|AccountId|[AccountId](#schemaaccountid)|true|none|A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.|
|StandingOrderId|[StandingOrderId](#schemastandingorderid)|false|none|A unique and immutable identifier used to identify the standing order resource. This identifier has no meaning to the account owner.|
|Frequency|[Frequency_1](#schemafrequency_1)|true|none|Individual Definitions:<br>NotKnown - Not Known<br>EvryDay - Every day<br>EvryWorkgDay - Every working day<br>IntrvlDay - An interval specified in number of calendar days (02 to 31)<br>IntrvlWkDay - An interval specified in weeks (01 to 09), and the day within the week (01 to 07)<br>WkInMnthDay - A monthly interval, specifying the week of the month (01 to 05) and day within the week (01 to 07)<br>IntrvlMnthDay - An interval specified in months (between 01 to 06, 12, 24), specifying the day within the month (-05 to -01, 01 to 31)<br>QtrDay - Quarterly (either ENGLISH, SCOTTISH, or RECEIVED)<br>ENGLISH = Paid on the 25th March, 24th June, 29th September and 25th December.<br>SCOTTISH = Paid on the 2nd February, 15th May, 1st August and 11th November.<br>RECEIVED = Paid on the 20th March, 19th June, 24th September and 20th December.<br>Individual Patterns:<br>NotKnown (ScheduleCode)<br>EvryDay (ScheduleCode)<br>EvryWorkgDay (ScheduleCode)<br>IntrvlDay:NoOfDay (ScheduleCode + NoOfDay)<br>IntrvlWkDay:IntervalInWeeks:DayInWeek (ScheduleCode + IntervalInWeeks + DayInWeek)<br>WkInMnthDay:WeekInMonth:DayInWeek (ScheduleCode + WeekInMonth + DayInWeek)<br>IntrvlMnthDay:IntervalInMonths:DayInMonth (ScheduleCode + IntervalInMonths + DayInMonth)<br>QtrDay: + either (ENGLISH, SCOTTISH or RECEIVED) ScheduleCode + QuarterDay<br>The regular expression for this element combines five smaller versions for each permitted pattern. To aid legibility - the components are presented individually here:<br>NotKnown<br>EvryDay<br>EvryWorkgDay<br>IntrvlDay:((0[2-9])|([1-2][0-9])|3[0-1])<br>IntrvlWkDay:0[1-9]:0[1-7]<br>WkInMnthDay:0[1-5]:0[1-7]<br>IntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01])<br>QtrDay:(ENGLISH|SCOTTISH|RECEIVED)<br>Full Regular Expression:<br>^(NotKnown)$|^(EvryDay)$|^(EvryWorkgDay)$|^(IntrvlDay:((0[2-9])|([1-2][0-9])|3[0-1]))$|^(IntrvlWkDay:0[1-9]:0[1-7])$|^(WkInMnthDay:0[1-5]:0[1-7])$|^(IntrvlMnthDay:(0[1-6]|12|24):(-0[1-5]|0[1-9]|[12][0-9]|3[01]))$|^(QtrDay:(ENGLISH|SCOTTISH|RECEIVED))$|
|Reference|[Reference](#schemareference)|false|none|Unique reference, as assigned by the creditor, to unambiguously refer to the payment transaction.<br>Usage: If available, the initiating party should provide this reference in the structured remittance information, to enable reconciliation by the creditor upon receipt of the amount of money.<br>If the business context requires the use of a creditor reference or a payment remit identification, and only one identifier can be passed through the end-to-end chain, the creditor's reference or payment remittance identification should be quoted in the end-to-end transaction identification.|
|NextPaymentDateTime|[NextPaymentDateTime](#schemanextpaymentdatetime)|false|none|The date on which the next payment for a Standing Order schedule will be made.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|
|StandingOrderStatusCode|[OBExternalStandingOrderStatus1Code](#schemaobexternalstandingorderstatus1code)|false|none|Specifies the status of the standing order in code form.|
|NextPaymentAmount|[OBActiveOrHistoricCurrencyAndAmount_3](#schemaobactiveorhistoriccurrencyandamount_3)|false|none|The amount of the next Standing Order.|
|CreditorAgent|[OBBranchAndFinancialInstitutionIdentification5_1](#schemaobbranchandfinancialinstitutionidentification5_1)|false|none|Party that manages the account on behalf of the account owner, that is manages the registration and booking of entries on the account, calculates balances on the account and provides information about the account.<br>This is the servicer of the beneficiary account.|

<h2 id="tocS_OBTransaction6">OBTransaction6</h2>
<!-- backwards compatibility -->
<a id="schemaobtransaction6"></a>
<a id="schema_OBTransaction6"></a>
<a id="tocSobtransaction6"></a>
<a id="tocsobtransaction6"></a>

```json
{
  "type": "object",
  "description": "Provides further details on an entry in the report.",
  "required": [
    "AccountId",
    "CreditDebitIndicator",
    "Status",
    "BookingDateTime",
    "Amount"
  ],
  "properties": {
    "AccountId": {
      "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
      "type": "string",
      "minLength": 1,
      "maxLength": 40
    },
    "TransactionId": {
      "description": "Unique identifier for the transaction within an servicing institution. This identifier is both unique and immutable.",
      "type": "string",
      "minLength": 1,
      "maxLength": 210
    },
    "TransactionReference": {
      "description": "Unique reference for the transaction. This reference is optionally populated, and may as an example be the FPID in the Faster Payments context.",
      "type": "string",
      "minLength": 1,
      "maxLength": 210
    },
    "CreditDebitIndicator": {
      "description": "Indicates whether the transaction is a credit or a debit entry.",
      "type": "string",
      "enum": [
        "Credit",
        "Debit"
      ]
    },
    "Status": {
      "description": "Status of a transaction entry on the books of the account servicer.",
      "type": "string",
      "enum": [
        "Booked",
        "Pending"
      ]
    },
    "BookingDateTime": {
      "description": "Date and time when a transaction entry is posted to an account on the account servicer's books.\nUsage: Booking date is the expected booking date, unless the status is booked, in which case it is the actual booking date.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
      "type": "string",
      "format": "date-time"
    },
    "ValueDateTime": {
      "description": "Date and time at which assets become available to the account owner in case of a credit entry, or cease to be available to the account owner in case of a debit transaction entry.\nUsage: If transaction entry status is pending and value date is present, then the value date refers to an expected/requested value date.\nFor transaction entries subject to availability/float and for which availability information is provided, the value date must not be used. In this case the availability component identifies the number of availability days.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
      "type": "string",
      "format": "date-time"
    },
    "TransactionInformation": {
      "description": "Further details of the transaction. \nThis is the transaction narrative, which is unstructured text.",
      "type": "string",
      "minLength": 1,
      "maxLength": 500
    },
    "AddressLine": {
      "description": "Information that locates and identifies a specific address for a transaction entry, that is presented in free format text.",
      "type": "string",
      "minLength": 1,
      "maxLength": 70
    },
    "Amount": {
      "type": "object",
      "required": [
        "Amount",
        "Currency"
      ],
      "description": "Amount of money in the cash transaction entry.",
      "properties": {
        "Amount": {
          "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
          "type": "string",
          "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
        },
        "Currency": {
          "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
          "type": "string",
          "pattern": "^[A-Z]{3,3}$"
        }
      }
    },
    "CurrencyExchange": {
      "type": "object",
      "required": [
        "SourceCurrency",
        "ExchangeRate"
      ],
      "description": "Set of elements used to provide details on the currency exchange.",
      "properties": {
        "SourceCurrency": {
          "description": "Currency from which an amount is to be converted in a currency conversion.",
          "type": "string",
          "pattern": "^[A-Z]{3,3}$"
        },
        "TargetCurrency": {
          "description": "Currency into which an amount is to be converted in a currency conversion.",
          "type": "string",
          "pattern": "^[A-Z]{3,3}$"
        },
        "UnitCurrency": {
          "description": "Currency in which the rate of exchange is expressed in a currency exchange. In the example 1GBP = xxxCUR, the unit currency is GBP.",
          "type": "string",
          "pattern": "^[A-Z]{3,3}$"
        },
        "ExchangeRate": {
          "description": "Factor used to convert an amount from one currency into another. This reflects the price at which one currency was bought with another currency.\nUsage: ExchangeRate expresses the ratio between UnitCurrency and QuotedCurrency (ExchangeRate = UnitCurrency/QuotedCurrency).",
          "type": "number"
        },
        "InstructedAmount": {
          "type": "object",
          "required": [
            "Amount",
            "Currency"
          ],
          "description": "Amount of money to be moved between the debtor and creditor, before deduction of charges, expressed in the currency as ordered by the initiating party.",
          "properties": {
            "Amount": {
              "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
              "type": "string",
              "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
            },
            "Currency": {
              "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
              "type": "string",
              "pattern": "^[A-Z]{3,3}$"
            }
          }
        }
      }
    },
    "ProprietaryBankTransactionCode": {
      "type": "object",
      "required": [
        "Code"
      ],
      "description": "Set of elements to fully identify a proprietary bank transaction code.",
      "properties": {
        "Code": {
          "description": "Proprietary bank transaction code to identify the underlying transaction.",
          "type": "string",
          "minLength": 1,
          "maxLength": 35
        }
      },
      "additionalProperties": false
    },
    "Balance": {
      "type": "object",
      "required": [
        "CreditDebitIndicator",
        "Type",
        "Amount"
      ],
      "description": "Set of elements used to define the balance as a numerical representation of the net increases and decreases in an account after a transaction entry is applied to the account.",
      "properties": {
        "CreditDebitIndicator": {
          "description": "Indicates whether the balance is a credit or a debit balance. \nUsage: A zero balance is considered to be a credit balance.",
          "type": "string",
          "enum": [
            "Credit",
            "Debit"
          ]
        },
        "Type": {
          "description": "Balance type, in a coded form.",
          "type": "string",
          "enum": [
            "Expected",
            "InterimBooked"
          ]
        },
        "Amount": {
          "type": "object",
          "required": [
            "Amount",
            "Currency"
          ],
          "description": "Amount of money of the cash balance after a transaction entry is applied to the account..",
          "properties": {
            "Amount": {
              "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
              "type": "string",
              "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
            },
            "Currency": {
              "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
              "type": "string",
              "pattern": "^[A-Z]{3,3}$"
            }
          }
        }
      },
      "additionalProperties": false
    },
    "MerchantDetails": {
      "type": "object",
      "description": "Details of the merchant involved in the transaction.",
      "properties": {
        "MerchantCategoryCode": {
          "description": "Category code conform to ISO 18245, related to the type of services or goods the merchant provides for the transaction.",
          "type": "string",
          "minLength": 3,
          "maxLength": 4
        }
      }
    },
    "CreditorAccount": {
      "type": "object",
      "description": "Unambiguous identification of the account of the creditor, in the case of a debit transaction.",
      "properties": {
        "SchemeName": {
          "description": "Name of the identification scheme, in a coded form as published in an external list.",
          "type": "string",
          "x-namespaced-enum": [
            "UK.OBIE.SortCodeAccountNumber"
          ]
        },
        "Identification": {
          "description": "Identification assigned by an institution to identify an account. This identification is known by the account owner.",
          "type": "string",
          "minLength": 1,
          "maxLength": 256
        },
        "Name": {
          "description": "The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.\nNote, the account name is not the product name or the nickname of the account.",
          "type": "string",
          "minLength": 1,
          "maxLength": 350
        },
        "SecondaryIdentification": {
          "description": "This is secondary identification of the account, as assigned by the account servicing institution. \nThis can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).",
          "type": "string",
          "minLength": 1,
          "maxLength": 34
        }
      }
    },
    "DebtorAccount": {
      "type": "object",
      "description": "Unambiguous identification of the account of the debtor, in the case of a crebit transaction.",
      "properties": {
        "SchemeName": {
          "description": "Name of the identification scheme, in a coded form as published in an external list.",
          "type": "string",
          "x-namespaced-enum": [
            "UK.OBIE.SortCodeAccountNumber"
          ]
        },
        "Identification": {
          "description": "Identification assigned by an institution to identify an account. This identification is known by the account owner.",
          "type": "string",
          "minLength": 1,
          "maxLength": 256
        },
        "Name": {
          "description": "The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.\nNote, the account name is not the product name or the nickname of the account.",
          "type": "string",
          "minLength": 1,
          "maxLength": 350
        },
        "SecondaryIdentification": {
          "description": "This is secondary identification of the account, as assigned by the account servicing institution. \nThis can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).",
          "type": "string",
          "minLength": 1,
          "maxLength": 34
        }
      }
    },
    "CardInstrument": {
      "type": "object",
      "required": [
        "CardSchemeName"
      ],
      "description": "Set of elements to describe the card instrument used in the transaction.",
      "properties": {
        "CardSchemeName": {
          "description": "Name of the card scheme.",
          "type": "string",
          "enum": [
            "AmericanExpress",
            "Diners",
            "Discover",
            "MasterCard",
            "VISA"
          ]
        },
        "AuthorisationType": {
          "description": "The card authorisation type.",
          "type": "string",
          "enum": [
            "ConsumerDevice",
            "Contactless",
            "None",
            "PIN"
          ]
        },
        "Name": {
          "description": "Name of the cardholder using the card instrument.",
          "type": "string",
          "minLength": 1,
          "maxLength": 70
        },
        "Identification": {
          "description": "Identification assigned by an institution to identify the card instrument used in the transaction. This identification is known by the account owner, and may be masked.",
          "type": "string",
          "minLength": 1,
          "maxLength": 34
        }
      },
      "additionalProperties": false
    }
  },
  "additionalProperties": false
}

```

Provides further details on an entry in the report.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|AccountId|[AccountId](#schemaaccountid)|true|none|A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.|
|TransactionId|[TransactionId](#schematransactionid)|false|none|Unique identifier for the transaction within an servicing institution. This identifier is both unique and immutable.|
|TransactionReference|[TransactionReference](#schematransactionreference)|false|none|Unique reference for the transaction. This reference is optionally populated, and may as an example be the FPID in the Faster Payments context.|
|CreditDebitIndicator|[OBCreditDebitCode_1](#schemaobcreditdebitcode_1)|true|none|Indicates whether the transaction is a credit or a debit entry.|
|Status|[OBEntryStatus1Code](#schemaobentrystatus1code)|true|none|Status of a transaction entry on the books of the account servicer.|
|BookingDateTime|[BookingDateTime](#schemabookingdatetime)|true|none|Date and time when a transaction entry is posted to an account on the account servicer's books.<br>Usage: Booking date is the expected booking date, unless the status is booked, in which case it is the actual booking date.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|
|ValueDateTime|[ValueDateTime](#schemavaluedatetime)|false|none|Date and time at which assets become available to the account owner in case of a credit entry, or cease to be available to the account owner in case of a debit transaction entry.<br>Usage: If transaction entry status is pending and value date is present, then the value date refers to an expected/requested value date.<br>For transaction entries subject to availability/float and for which availability information is provided, the value date must not be used. In this case the availability component identifies the number of availability days.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|
|TransactionInformation|[TransactionInformation](#schematransactioninformation)|false|none|Further details of the transaction. <br>This is the transaction narrative, which is unstructured text.|
|AddressLine|[AddressLine](#schemaaddressline)|false|none|Information that locates and identifies a specific address for a transaction entry, that is presented in free format text.|
|Amount|[OBActiveOrHistoricCurrencyAndAmount_9](#schemaobactiveorhistoriccurrencyandamount_9)|true|none|Amount of money in the cash transaction entry.|
|CurrencyExchange|[OBCurrencyExchange5](#schemaobcurrencyexchange5)|false|none|Set of elements used to provide details on the currency exchange.|
|ProprietaryBankTransactionCode|[ProprietaryBankTransactionCodeStructure1](#schemaproprietarybanktransactioncodestructure1)|false|none|Set of elements to fully identify a proprietary bank transaction code.|
|Balance|[OBTransactionCashBalance](#schemaobtransactioncashbalance)|false|none|Set of elements used to define the balance as a numerical representation of the net increases and decreases in an account after a transaction entry is applied to the account.|
|MerchantDetails|[OBMerchantDetails1](#schemaobmerchantdetails1)|false|none|Details of the merchant involved in the transaction.|
|CreditorAccount|[OBCashAccount6_0](#schemaobcashaccount6_0)|false|none|Unambiguous identification of the account of the creditor, in the case of a debit transaction.|
|DebtorAccount|[OBCashAccount6_1](#schemaobcashaccount6_1)|false|none|Unambiguous identification of the account of the debtor, in the case of a crebit transaction.|
|CardInstrument|[OBTransactionCardInstrument1](#schemaobtransactioncardinstrument1)|false|none|Set of elements to describe the card instrument used in the transaction.|

<h2 id="tocS_OBTransaction6Basic">OBTransaction6Basic</h2>
<!-- backwards compatibility -->
<a id="schemaobtransaction6basic"></a>
<a id="schema_OBTransaction6Basic"></a>
<a id="tocSobtransaction6basic"></a>
<a id="tocsobtransaction6basic"></a>

```json
{
  "type": "object",
  "description": "Provides further details on an entry in the report.",
  "required": [
    "AccountId",
    "CreditDebitIndicator",
    "Status",
    "BookingDateTime",
    "Amount"
  ],
  "properties": {
    "AccountId": {
      "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
      "type": "string",
      "minLength": 1,
      "maxLength": 40
    },
    "TransactionId": {
      "description": "Unique identifier for the transaction within an servicing institution. This identifier is both unique and immutable.",
      "type": "string",
      "minLength": 1,
      "maxLength": 210
    },
    "TransactionReference": {
      "description": "Unique reference for the transaction. This reference is optionally populated, and may as an example be the FPID in the Faster Payments context.",
      "type": "string",
      "minLength": 1,
      "maxLength": 210
    },
    "CreditDebitIndicator": {
      "description": "Indicates whether the transaction is a credit or a debit entry.",
      "type": "string",
      "enum": [
        "Credit",
        "Debit"
      ]
    },
    "Status": {
      "description": "Status of a transaction entry on the books of the account servicer.",
      "type": "string",
      "enum": [
        "Booked",
        "Pending"
      ]
    },
    "BookingDateTime": {
      "description": "Date and time when a transaction entry is posted to an account on the account servicer's books.\nUsage: Booking date is the expected booking date, unless the status is booked, in which case it is the actual booking date.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
      "type": "string",
      "format": "date-time"
    },
    "ValueDateTime": {
      "description": "Date and time at which assets become available to the account owner in case of a credit entry, or cease to be available to the account owner in case of a debit transaction entry.\nUsage: If transaction entry status is pending and value date is present, then the value date refers to an expected/requested value date.\nFor transaction entries subject to availability/float and for which availability information is provided, the value date must not be used. In this case the availability component identifies the number of availability days.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
      "type": "string",
      "format": "date-time"
    },
    "AddressLine": {
      "description": "Information that locates and identifies a specific address for a transaction entry, that is presented in free format text.",
      "type": "string",
      "minLength": 1,
      "maxLength": 70
    },
    "Amount": {
      "type": "object",
      "required": [
        "Amount",
        "Currency"
      ],
      "description": "Amount of money in the cash transaction entry.",
      "properties": {
        "Amount": {
          "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
          "type": "string",
          "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
        },
        "Currency": {
          "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
          "type": "string",
          "pattern": "^[A-Z]{3,3}$"
        }
      }
    },
    "CurrencyExchange": {
      "type": "object",
      "required": [
        "SourceCurrency",
        "ExchangeRate"
      ],
      "description": "Set of elements used to provide details on the currency exchange.",
      "properties": {
        "SourceCurrency": {
          "description": "Currency from which an amount is to be converted in a currency conversion.",
          "type": "string",
          "pattern": "^[A-Z]{3,3}$"
        },
        "TargetCurrency": {
          "description": "Currency into which an amount is to be converted in a currency conversion.",
          "type": "string",
          "pattern": "^[A-Z]{3,3}$"
        },
        "UnitCurrency": {
          "description": "Currency in which the rate of exchange is expressed in a currency exchange. In the example 1GBP = xxxCUR, the unit currency is GBP.",
          "type": "string",
          "pattern": "^[A-Z]{3,3}$"
        },
        "ExchangeRate": {
          "description": "Factor used to convert an amount from one currency into another. This reflects the price at which one currency was bought with another currency.\nUsage: ExchangeRate expresses the ratio between UnitCurrency and QuotedCurrency (ExchangeRate = UnitCurrency/QuotedCurrency).",
          "type": "number"
        },
        "InstructedAmount": {
          "type": "object",
          "required": [
            "Amount",
            "Currency"
          ],
          "description": "Amount of money to be moved between the debtor and creditor, before deduction of charges, expressed in the currency as ordered by the initiating party.",
          "properties": {
            "Amount": {
              "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
              "type": "string",
              "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
            },
            "Currency": {
              "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
              "type": "string",
              "pattern": "^[A-Z]{3,3}$"
            }
          }
        }
      }
    },
    "ProprietaryBankTransactionCode": {
      "type": "object",
      "required": [
        "Code"
      ],
      "description": "Set of elements to fully identify a proprietary bank transaction code.",
      "properties": {
        "Code": {
          "description": "Proprietary bank transaction code to identify the underlying transaction.",
          "type": "string",
          "minLength": 1,
          "maxLength": 35
        }
      },
      "additionalProperties": false
    },
    "CardInstrument": {
      "type": "object",
      "required": [
        "CardSchemeName"
      ],
      "description": "Set of elements to describe the card instrument used in the transaction.",
      "properties": {
        "CardSchemeName": {
          "description": "Name of the card scheme.",
          "type": "string",
          "enum": [
            "AmericanExpress",
            "Diners",
            "Discover",
            "MasterCard",
            "VISA"
          ]
        },
        "AuthorisationType": {
          "description": "The card authorisation type.",
          "type": "string",
          "enum": [
            "ConsumerDevice",
            "Contactless",
            "None",
            "PIN"
          ]
        },
        "Name": {
          "description": "Name of the cardholder using the card instrument.",
          "type": "string",
          "minLength": 1,
          "maxLength": 70
        },
        "Identification": {
          "description": "Identification assigned by an institution to identify the card instrument used in the transaction. This identification is known by the account owner, and may be masked.",
          "type": "string",
          "minLength": 1,
          "maxLength": 34
        }
      },
      "additionalProperties": false
    }
  },
  "additionalProperties": false
}

```

Provides further details on an entry in the report.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|AccountId|[AccountId](#schemaaccountid)|true|none|A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.|
|TransactionId|[TransactionId](#schematransactionid)|false|none|Unique identifier for the transaction within an servicing institution. This identifier is both unique and immutable.|
|TransactionReference|[TransactionReference](#schematransactionreference)|false|none|Unique reference for the transaction. This reference is optionally populated, and may as an example be the FPID in the Faster Payments context.|
|CreditDebitIndicator|[OBCreditDebitCode_1](#schemaobcreditdebitcode_1)|true|none|Indicates whether the transaction is a credit or a debit entry.|
|Status|[OBEntryStatus1Code](#schemaobentrystatus1code)|true|none|Status of a transaction entry on the books of the account servicer.|
|BookingDateTime|[BookingDateTime](#schemabookingdatetime)|true|none|Date and time when a transaction entry is posted to an account on the account servicer's books.<br>Usage: Booking date is the expected booking date, unless the status is booked, in which case it is the actual booking date.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|
|ValueDateTime|[ValueDateTime](#schemavaluedatetime)|false|none|Date and time at which assets become available to the account owner in case of a credit entry, or cease to be available to the account owner in case of a debit transaction entry.<br>Usage: If transaction entry status is pending and value date is present, then the value date refers to an expected/requested value date.<br>For transaction entries subject to availability/float and for which availability information is provided, the value date must not be used. In this case the availability component identifies the number of availability days.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|
|AddressLine|[AddressLine](#schemaaddressline)|false|none|Information that locates and identifies a specific address for a transaction entry, that is presented in free format text.|
|Amount|[OBActiveOrHistoricCurrencyAndAmount_9](#schemaobactiveorhistoriccurrencyandamount_9)|true|none|Amount of money in the cash transaction entry.|
|CurrencyExchange|[OBCurrencyExchange5](#schemaobcurrencyexchange5)|false|none|Set of elements used to provide details on the currency exchange.|
|ProprietaryBankTransactionCode|[ProprietaryBankTransactionCodeStructure1](#schemaproprietarybanktransactioncodestructure1)|false|none|Set of elements to fully identify a proprietary bank transaction code.|
|CardInstrument|[OBTransactionCardInstrument1](#schemaobtransactioncardinstrument1)|false|none|Set of elements to describe the card instrument used in the transaction.|

<h2 id="tocS_OBTransaction6Detail">OBTransaction6Detail</h2>
<!-- backwards compatibility -->
<a id="schemaobtransaction6detail"></a>
<a id="schema_OBTransaction6Detail"></a>
<a id="tocSobtransaction6detail"></a>
<a id="tocsobtransaction6detail"></a>

```json
{
  "type": "object",
  "description": "Provides further details on an entry in the report.",
  "required": [
    "AccountId",
    "CreditDebitIndicator",
    "Status",
    "BookingDateTime",
    "Amount"
  ],
  "properties": {
    "AccountId": {
      "description": "A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.",
      "type": "string",
      "minLength": 1,
      "maxLength": 40
    },
    "TransactionId": {
      "description": "Unique identifier for the transaction within an servicing institution. This identifier is both unique and immutable.",
      "type": "string",
      "minLength": 1,
      "maxLength": 210
    },
    "TransactionReference": {
      "description": "Unique reference for the transaction. This reference is optionally populated, and may as an example be the FPID in the Faster Payments context.",
      "type": "string",
      "minLength": 1,
      "maxLength": 210
    },
    "CreditDebitIndicator": {
      "description": "Indicates whether the transaction is a credit or a debit entry.",
      "type": "string",
      "enum": [
        "Credit",
        "Debit"
      ]
    },
    "Status": {
      "description": "Status of a transaction entry on the books of the account servicer.",
      "type": "string",
      "enum": [
        "Booked",
        "Pending"
      ]
    },
    "BookingDateTime": {
      "description": "Date and time when a transaction entry is posted to an account on the account servicer's books.\nUsage: Booking date is the expected booking date, unless the status is booked, in which case it is the actual booking date.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
      "type": "string",
      "format": "date-time"
    },
    "ValueDateTime": {
      "description": "Date and time at which assets become available to the account owner in case of a credit entry, or cease to be available to the account owner in case of a debit transaction entry.\nUsage: If transaction entry status is pending and value date is present, then the value date refers to an expected/requested value date.\nFor transaction entries subject to availability/float and for which availability information is provided, the value date must not be used. In this case the availability component identifies the number of availability days.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
      "type": "string",
      "format": "date-time"
    },
    "TransactionInformation": {
      "description": "Further details of the transaction. \nThis is the transaction narrative, which is unstructured text.",
      "type": "string",
      "minLength": 1,
      "maxLength": 500
    },
    "AddressLine": {
      "description": "Information that locates and identifies a specific address for a transaction entry, that is presented in free format text.",
      "type": "string",
      "minLength": 1,
      "maxLength": 70
    },
    "Amount": {
      "type": "object",
      "required": [
        "Amount",
        "Currency"
      ],
      "description": "Amount of money in the cash transaction entry.",
      "properties": {
        "Amount": {
          "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
          "type": "string",
          "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
        },
        "Currency": {
          "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
          "type": "string",
          "pattern": "^[A-Z]{3,3}$"
        }
      }
    },
    "CurrencyExchange": {
      "type": "object",
      "required": [
        "SourceCurrency",
        "ExchangeRate"
      ],
      "description": "Set of elements used to provide details on the currency exchange.",
      "properties": {
        "SourceCurrency": {
          "description": "Currency from which an amount is to be converted in a currency conversion.",
          "type": "string",
          "pattern": "^[A-Z]{3,3}$"
        },
        "TargetCurrency": {
          "description": "Currency into which an amount is to be converted in a currency conversion.",
          "type": "string",
          "pattern": "^[A-Z]{3,3}$"
        },
        "UnitCurrency": {
          "description": "Currency in which the rate of exchange is expressed in a currency exchange. In the example 1GBP = xxxCUR, the unit currency is GBP.",
          "type": "string",
          "pattern": "^[A-Z]{3,3}$"
        },
        "ExchangeRate": {
          "description": "Factor used to convert an amount from one currency into another. This reflects the price at which one currency was bought with another currency.\nUsage: ExchangeRate expresses the ratio between UnitCurrency and QuotedCurrency (ExchangeRate = UnitCurrency/QuotedCurrency).",
          "type": "number"
        },
        "InstructedAmount": {
          "type": "object",
          "required": [
            "Amount",
            "Currency"
          ],
          "description": "Amount of money to be moved between the debtor and creditor, before deduction of charges, expressed in the currency as ordered by the initiating party.",
          "properties": {
            "Amount": {
              "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
              "type": "string",
              "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
            },
            "Currency": {
              "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
              "type": "string",
              "pattern": "^[A-Z]{3,3}$"
            }
          }
        }
      }
    },
    "ProprietaryBankTransactionCode": {
      "type": "object",
      "required": [
        "Code"
      ],
      "description": "Set of elements to fully identify a proprietary bank transaction code.",
      "properties": {
        "Code": {
          "description": "Proprietary bank transaction code to identify the underlying transaction.",
          "type": "string",
          "minLength": 1,
          "maxLength": 35
        }
      },
      "additionalProperties": false
    },
    "Balance": {
      "type": "object",
      "required": [
        "CreditDebitIndicator",
        "Type",
        "Amount"
      ],
      "description": "Set of elements used to define the balance as a numerical representation of the net increases and decreases in an account after a transaction entry is applied to the account.",
      "properties": {
        "CreditDebitIndicator": {
          "description": "Indicates whether the balance is a credit or a debit balance. \nUsage: A zero balance is considered to be a credit balance.",
          "type": "string",
          "enum": [
            "Credit",
            "Debit"
          ]
        },
        "Type": {
          "description": "Balance type, in a coded form.",
          "type": "string",
          "enum": [
            "Expected",
            "InterimBooked"
          ]
        },
        "Amount": {
          "type": "object",
          "required": [
            "Amount",
            "Currency"
          ],
          "description": "Amount of money of the cash balance after a transaction entry is applied to the account..",
          "properties": {
            "Amount": {
              "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
              "type": "string",
              "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
            },
            "Currency": {
              "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
              "type": "string",
              "pattern": "^[A-Z]{3,3}$"
            }
          }
        }
      },
      "additionalProperties": false
    },
    "MerchantDetails": {
      "type": "object",
      "description": "Details of the merchant involved in the transaction.",
      "properties": {
        "MerchantCategoryCode": {
          "description": "Category code conform to ISO 18245, related to the type of services or goods the merchant provides for the transaction.",
          "type": "string",
          "minLength": 3,
          "maxLength": 4
        }
      }
    },
    "CreditorAccount": {
      "type": "object",
      "description": "Unambiguous identification of the account of the creditor, in the case of a debit transaction.",
      "properties": {
        "SchemeName": {
          "description": "Name of the identification scheme, in a coded form as published in an external list.",
          "type": "string",
          "x-namespaced-enum": [
            "UK.OBIE.SortCodeAccountNumber"
          ]
        },
        "Identification": {
          "description": "Identification assigned by an institution to identify an account. This identification is known by the account owner.",
          "type": "string",
          "minLength": 1,
          "maxLength": 256
        },
        "Name": {
          "description": "The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.\nNote, the account name is not the product name or the nickname of the account.",
          "type": "string",
          "minLength": 1,
          "maxLength": 350
        },
        "SecondaryIdentification": {
          "description": "This is secondary identification of the account, as assigned by the account servicing institution. \nThis can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).",
          "type": "string",
          "minLength": 1,
          "maxLength": 34
        }
      }
    },
    "DebtorAccount": {
      "type": "object",
      "description": "Unambiguous identification of the account of the debtor, in the case of a crebit transaction.",
      "properties": {
        "SchemeName": {
          "description": "Name of the identification scheme, in a coded form as published in an external list.",
          "type": "string",
          "x-namespaced-enum": [
            "UK.OBIE.SortCodeAccountNumber"
          ]
        },
        "Identification": {
          "description": "Identification assigned by an institution to identify an account. This identification is known by the account owner.",
          "type": "string",
          "minLength": 1,
          "maxLength": 256
        },
        "Name": {
          "description": "The account name is the name or names of the account owner(s) represented at an account level, as displayed by the ASPSP's online channels.\nNote, the account name is not the product name or the nickname of the account.",
          "type": "string",
          "minLength": 1,
          "maxLength": 350
        },
        "SecondaryIdentification": {
          "description": "This is secondary identification of the account, as assigned by the account servicing institution. \nThis can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).",
          "type": "string",
          "minLength": 1,
          "maxLength": 34
        }
      }
    },
    "CardInstrument": {
      "type": "object",
      "required": [
        "CardSchemeName"
      ],
      "description": "Set of elements to describe the card instrument used in the transaction.",
      "properties": {
        "CardSchemeName": {
          "description": "Name of the card scheme.",
          "type": "string",
          "enum": [
            "AmericanExpress",
            "Diners",
            "Discover",
            "MasterCard",
            "VISA"
          ]
        },
        "AuthorisationType": {
          "description": "The card authorisation type.",
          "type": "string",
          "enum": [
            "ConsumerDevice",
            "Contactless",
            "None",
            "PIN"
          ]
        },
        "Name": {
          "description": "Name of the cardholder using the card instrument.",
          "type": "string",
          "minLength": 1,
          "maxLength": 70
        },
        "Identification": {
          "description": "Identification assigned by an institution to identify the card instrument used in the transaction. This identification is known by the account owner, and may be masked.",
          "type": "string",
          "minLength": 1,
          "maxLength": 34
        }
      },
      "additionalProperties": false
    }
  },
  "additionalProperties": false
}

```

Provides further details on an entry in the report.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|AccountId|[AccountId](#schemaaccountid)|true|none|A unique and immutable identifier used to identify the account resource. This identifier has no meaning to the account owner.|
|TransactionId|[TransactionId](#schematransactionid)|false|none|Unique identifier for the transaction within an servicing institution. This identifier is both unique and immutable.|
|TransactionReference|[TransactionReference](#schematransactionreference)|false|none|Unique reference for the transaction. This reference is optionally populated, and may as an example be the FPID in the Faster Payments context.|
|CreditDebitIndicator|[OBCreditDebitCode_1](#schemaobcreditdebitcode_1)|true|none|Indicates whether the transaction is a credit or a debit entry.|
|Status|[OBEntryStatus1Code](#schemaobentrystatus1code)|true|none|Status of a transaction entry on the books of the account servicer.|
|BookingDateTime|[BookingDateTime](#schemabookingdatetime)|true|none|Date and time when a transaction entry is posted to an account on the account servicer's books.<br>Usage: Booking date is the expected booking date, unless the status is booked, in which case it is the actual booking date.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|
|ValueDateTime|[ValueDateTime](#schemavaluedatetime)|false|none|Date and time at which assets become available to the account owner in case of a credit entry, or cease to be available to the account owner in case of a debit transaction entry.<br>Usage: If transaction entry status is pending and value date is present, then the value date refers to an expected/requested value date.<br>For transaction entries subject to availability/float and for which availability information is provided, the value date must not be used. In this case the availability component identifies the number of availability days.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|
|TransactionInformation|[TransactionInformation](#schematransactioninformation)|false|none|Further details of the transaction. <br>This is the transaction narrative, which is unstructured text.|
|AddressLine|[AddressLine](#schemaaddressline)|false|none|Information that locates and identifies a specific address for a transaction entry, that is presented in free format text.|
|Amount|[OBActiveOrHistoricCurrencyAndAmount_9](#schemaobactiveorhistoriccurrencyandamount_9)|true|none|Amount of money in the cash transaction entry.|
|CurrencyExchange|[OBCurrencyExchange5](#schemaobcurrencyexchange5)|false|none|Set of elements used to provide details on the currency exchange.|
|ProprietaryBankTransactionCode|[ProprietaryBankTransactionCodeStructure1](#schemaproprietarybanktransactioncodestructure1)|false|none|Set of elements to fully identify a proprietary bank transaction code.|
|Balance|[OBTransactionCashBalance](#schemaobtransactioncashbalance)|false|none|Set of elements used to define the balance as a numerical representation of the net increases and decreases in an account after a transaction entry is applied to the account.|
|MerchantDetails|[OBMerchantDetails1](#schemaobmerchantdetails1)|false|none|Details of the merchant involved in the transaction.|
|CreditorAccount|[OBCashAccount6_0](#schemaobcashaccount6_0)|false|none|Unambiguous identification of the account of the creditor, in the case of a debit transaction.|
|DebtorAccount|[OBCashAccount6_1](#schemaobcashaccount6_1)|false|none|Unambiguous identification of the account of the debtor, in the case of a crebit transaction.|
|CardInstrument|[OBTransactionCardInstrument1](#schemaobtransactioncardinstrument1)|false|none|Set of elements to describe the card instrument used in the transaction.|

<h2 id="tocS_OBTransactionCardInstrument1">OBTransactionCardInstrument1</h2>
<!-- backwards compatibility -->
<a id="schemaobtransactioncardinstrument1"></a>
<a id="schema_OBTransactionCardInstrument1"></a>
<a id="tocSobtransactioncardinstrument1"></a>
<a id="tocsobtransactioncardinstrument1"></a>

```json
{
  "type": "object",
  "required": [
    "CardSchemeName"
  ],
  "description": "Set of elements to describe the card instrument used in the transaction.",
  "properties": {
    "CardSchemeName": {
      "description": "Name of the card scheme.",
      "type": "string",
      "enum": [
        "AmericanExpress",
        "Diners",
        "Discover",
        "MasterCard",
        "VISA"
      ]
    },
    "AuthorisationType": {
      "description": "The card authorisation type.",
      "type": "string",
      "enum": [
        "ConsumerDevice",
        "Contactless",
        "None",
        "PIN"
      ]
    },
    "Name": {
      "description": "Name of the cardholder using the card instrument.",
      "type": "string",
      "minLength": 1,
      "maxLength": 70
    },
    "Identification": {
      "description": "Identification assigned by an institution to identify the card instrument used in the transaction. This identification is known by the account owner, and may be masked.",
      "type": "string",
      "minLength": 1,
      "maxLength": 34
    }
  },
  "additionalProperties": false
}

```

Set of elements to describe the card instrument used in the transaction.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|CardSchemeName|string|true|none|Name of the card scheme.|
|AuthorisationType|string|false|none|The card authorisation type.|
|Name|string|false|none|Name of the cardholder using the card instrument.|
|Identification|string|false|none|Identification assigned by an institution to identify the card instrument used in the transaction. This identification is known by the account owner, and may be masked.|

#### Enumerated Values

|Property|Value|
|---|---|
|CardSchemeName|AmericanExpress|
|CardSchemeName|Diners|
|CardSchemeName|Discover|
|CardSchemeName|MasterCard|
|CardSchemeName|VISA|
|AuthorisationType|ConsumerDevice|
|AuthorisationType|Contactless|
|AuthorisationType|None|
|AuthorisationType|PIN|

<h2 id="tocS_OBTransactionCashBalance">OBTransactionCashBalance</h2>
<!-- backwards compatibility -->
<a id="schemaobtransactioncashbalance"></a>
<a id="schema_OBTransactionCashBalance"></a>
<a id="tocSobtransactioncashbalance"></a>
<a id="tocsobtransactioncashbalance"></a>

```json
{
  "type": "object",
  "required": [
    "CreditDebitIndicator",
    "Type",
    "Amount"
  ],
  "description": "Set of elements used to define the balance as a numerical representation of the net increases and decreases in an account after a transaction entry is applied to the account.",
  "properties": {
    "CreditDebitIndicator": {
      "description": "Indicates whether the balance is a credit or a debit balance. \nUsage: A zero balance is considered to be a credit balance.",
      "type": "string",
      "enum": [
        "Credit",
        "Debit"
      ]
    },
    "Type": {
      "description": "Balance type, in a coded form.",
      "type": "string",
      "enum": [
        "Expected",
        "InterimBooked"
      ]
    },
    "Amount": {
      "type": "object",
      "required": [
        "Amount",
        "Currency"
      ],
      "description": "Amount of money of the cash balance after a transaction entry is applied to the account..",
      "properties": {
        "Amount": {
          "description": "A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.",
          "type": "string",
          "pattern": "^\\d{1,13}$|^\\d{1,13}\\.\\d{1,5}$"
        },
        "Currency": {
          "description": "A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 \"Codes for the representation of currencies and funds\".",
          "type": "string",
          "pattern": "^[A-Z]{3,3}$"
        }
      }
    }
  },
  "additionalProperties": false
}

```

Set of elements used to define the balance as a numerical representation of the net increases and decreases in an account after a transaction entry is applied to the account.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|CreditDebitIndicator|[OBCreditDebitCode_2](#schemaobcreditdebitcode_2)|true|none|Indicates whether the balance is a credit or a debit balance. <br>Usage: A zero balance is considered to be a credit balance.|
|Type|[OBBalanceType1Code](#schemaobbalancetype1code)|true|none|Balance type, in a coded form.|
|Amount|object|true|none|Amount of money of the cash balance after a transaction entry is applied to the account..|
|» Amount|[OBActiveCurrencyAndAmount_SimpleType](#schemaobactivecurrencyandamount_simpletype)|true|none|A number of monetary units specified in an active currency where the unit of currency is explicit and compliant with ISO 4217.|
|» Currency|[ActiveOrHistoricCurrencyCode_1](#schemaactiveorhistoriccurrencycode_1)|true|none|A code allocated to a currency by a Maintenance Agency under an international identification scheme, as described in the latest edition of the international standard ISO 4217 "Codes for the representation of currencies and funds".|

<h2 id="tocS_OB_Amount1_0">OB_Amount1_0</h2>
<!-- backwards compatibility -->
<a id="schemaob_amount1_0"></a>
<a id="schema_OB_Amount1_0"></a>
<a id="tocSob_amount1_0"></a>
<a id="tocsob_amount1_0"></a>

```json
{
  "description": "Cap amount charged for a fee/charge",
  "type": "string",
  "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
}

```

Cap amount charged for a fee/charge

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Cap amount charged for a fee/charge|

<h2 id="tocS_OB_Amount1_1">OB_Amount1_1</h2>
<!-- backwards compatibility -->
<a id="schemaob_amount1_1"></a>
<a id="schema_OB_Amount1_1"></a>
<a id="tocSob_amount1_1"></a>
<a id="tocsob_amount1_1"></a>

```json
{
  "description": "Every additional tranche of an overdraft balance to which an overdraft fee is applied",
  "type": "string",
  "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
}

```

Every additional tranche of an overdraft balance to which an overdraft fee is applied

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Every additional tranche of an overdraft balance to which an overdraft fee is applied|

<h2 id="tocS_OB_Amount1_2">OB_Amount1_2</h2>
<!-- backwards compatibility -->
<a id="schemaob_amount1_2"></a>
<a id="schema_OB_Amount1_2"></a>
<a id="tocSob_amount1_2"></a>
<a id="tocsob_amount1_2"></a>

```json
{
  "description": "Amount charged for an overdraft fee/charge (where it is charged in terms of an amount rather than a rate)",
  "type": "string",
  "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
}

```

Amount charged for an overdraft fee/charge (where it is charged in terms of an amount rather than a rate)

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Amount charged for an overdraft fee/charge (where it is charged in terms of an amount rather than a rate)|

<h2 id="tocS_OB_Amount1_3">OB_Amount1_3</h2>
<!-- backwards compatibility -->
<a id="schemaob_amount1_3"></a>
<a id="schema_OB_Amount1_3"></a>
<a id="tocSob_amount1_3"></a>
<a id="tocsob_amount1_3"></a>

```json
{
  "description": "Fee Amount charged for a fee/charge (where it is charged in terms of an amount rather than a rate)",
  "type": "string",
  "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
}

```

Fee Amount charged for a fee/charge (where it is charged in terms of an amount rather than a rate)

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Fee Amount charged for a fee/charge (where it is charged in terms of an amount rather than a rate)|

<h2 id="tocS_OB_Amount1_4">OB_Amount1_4</h2>
<!-- backwards compatibility -->
<a id="schemaob_amount1_4"></a>
<a id="schema_OB_Amount1_4"></a>
<a id="tocSob_amount1_4"></a>
<a id="tocsob_amount1_4"></a>

```json
{
  "description": "Cap amount charged for a fee/charge (where it is charged in terms of an amount rather than a rate)",
  "type": "string",
  "pattern": "^(-?\\d{1,14}){1}(\\.\\d{1,4}){0,1}$"
}

```

Cap amount charged for a fee/charge (where it is charged in terms of an amount rather than a rate)

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Cap amount charged for a fee/charge (where it is charged in terms of an amount rather than a rate)|

<h2 id="tocS_OB_CodeMnemonic">OB_CodeMnemonic</h2>
<!-- backwards compatibility -->
<a id="schemaob_codemnemonic"></a>
<a id="schema_OB_CodeMnemonic"></a>
<a id="tocSob_codemnemonic"></a>
<a id="tocsob_codemnemonic"></a>

```json
{
  "description": "The four letter Mnemonic used within an XML file to identify a code",
  "type": "string",
  "pattern": "^\\\\w{0,4}$"
}

```

The four letter Mnemonic used within an XML file to identify a code

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|The four letter Mnemonic used within an XML file to identify a code|

<h2 id="tocS_OB_FeeCategory1Code">OB_FeeCategory1Code</h2>
<!-- backwards compatibility -->
<a id="schemaob_feecategory1code"></a>
<a id="schema_OB_FeeCategory1Code"></a>
<a id="tocSob_feecategory1code"></a>
<a id="tocsob_feecategory1code"></a>

```json
{
  "description": "Categorisation of fees and charges into standard categories.",
  "type": "string",
  "enum": [
    "FCOT",
    "FCRE",
    "FCSV"
  ]
}

```

Categorisation of fees and charges into standard categories.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Categorisation of fees and charges into standard categories.|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|FCOT|
|*anonymous*|FCRE|
|*anonymous*|FCSV|

<h2 id="tocS_OB_FeeFrequency1Code_0">OB_FeeFrequency1Code_0</h2>
<!-- backwards compatibility -->
<a id="schemaob_feefrequency1code_0"></a>
<a id="schema_OB_FeeFrequency1Code_0"></a>
<a id="tocSob_feefrequency1code_0"></a>
<a id="tocsob_feefrequency1code_0"></a>

```json
{
  "description": "Frequency at which the overdraft charge is applied to the account",
  "type": "string",
  "enum": [
    "FEAC",
    "FEAO",
    "FECP",
    "FEDA",
    "FEHO",
    "FEI",
    "FEMO",
    "FEOA",
    "FEOT",
    "FEPC",
    "FEPH",
    "FEPO",
    "FEPS",
    "FEPT",
    "FEPTA",
    "FEPTP",
    "FEQU",
    "FESM",
    "FEST",
    "FEWE",
    "FEYE"
  ]
}

```

Frequency at which the overdraft charge is applied to the account

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Frequency at which the overdraft charge is applied to the account|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|FEAC|
|*anonymous*|FEAO|
|*anonymous*|FECP|
|*anonymous*|FEDA|
|*anonymous*|FEHO|
|*anonymous*|FEI|
|*anonymous*|FEMO|
|*anonymous*|FEOA|
|*anonymous*|FEOT|
|*anonymous*|FEPC|
|*anonymous*|FEPH|
|*anonymous*|FEPO|
|*anonymous*|FEPS|
|*anonymous*|FEPT|
|*anonymous*|FEPTA|
|*anonymous*|FEPTP|
|*anonymous*|FEQU|
|*anonymous*|FESM|
|*anonymous*|FEST|
|*anonymous*|FEWE|
|*anonymous*|FEYE|

<h2 id="tocS_OB_FeeFrequency1Code_1">OB_FeeFrequency1Code_1</h2>
<!-- backwards compatibility -->
<a id="schemaob_feefrequency1code_1"></a>
<a id="schema_OB_FeeFrequency1Code_1"></a>
<a id="tocSob_feefrequency1code_1"></a>
<a id="tocsob_feefrequency1code_1"></a>

```json
{
  "description": "How often is the overdraft fee/charge calculated for the account.",
  "type": "string",
  "enum": [
    "FEAC",
    "FEAO",
    "FECP",
    "FEDA",
    "FEHO",
    "FEI",
    "FEMO",
    "FEOA",
    "FEOT",
    "FEPC",
    "FEPH",
    "FEPO",
    "FEPS",
    "FEPT",
    "FEPTA",
    "FEPTP",
    "FEQU",
    "FESM",
    "FEST",
    "FEWE",
    "FEYE"
  ]
}

```

How often is the overdraft fee/charge calculated for the account.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|How often is the overdraft fee/charge calculated for the account.|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|FEAC|
|*anonymous*|FEAO|
|*anonymous*|FECP|
|*anonymous*|FEDA|
|*anonymous*|FEHO|
|*anonymous*|FEI|
|*anonymous*|FEMO|
|*anonymous*|FEOA|
|*anonymous*|FEOT|
|*anonymous*|FEPC|
|*anonymous*|FEPH|
|*anonymous*|FEPO|
|*anonymous*|FEPS|
|*anonymous*|FEPT|
|*anonymous*|FEPTA|
|*anonymous*|FEPTP|
|*anonymous*|FEQU|
|*anonymous*|FESM|
|*anonymous*|FEST|
|*anonymous*|FEWE|
|*anonymous*|FEYE|

<h2 id="tocS_OB_FeeFrequency1Code_2">OB_FeeFrequency1Code_2</h2>
<!-- backwards compatibility -->
<a id="schemaob_feefrequency1code_2"></a>
<a id="schema_OB_FeeFrequency1Code_2"></a>
<a id="tocSob_feefrequency1code_2"></a>
<a id="tocsob_feefrequency1code_2"></a>

```json
{
  "description": "How frequently the fee/charge is applied to the account",
  "type": "string",
  "enum": [
    "FEAC",
    "FEAO",
    "FECP",
    "FEDA",
    "FEHO",
    "FEI",
    "FEMO",
    "FEOA",
    "FEOT",
    "FEPC",
    "FEPH",
    "FEPO",
    "FEPS",
    "FEPT",
    "FEPTA",
    "FEPTP",
    "FEQU",
    "FESM",
    "FEST",
    "FEWE",
    "FEYE"
  ]
}

```

How frequently the fee/charge is applied to the account

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|How frequently the fee/charge is applied to the account|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|FEAC|
|*anonymous*|FEAO|
|*anonymous*|FECP|
|*anonymous*|FEDA|
|*anonymous*|FEHO|
|*anonymous*|FEI|
|*anonymous*|FEMO|
|*anonymous*|FEOA|
|*anonymous*|FEOT|
|*anonymous*|FEPC|
|*anonymous*|FEPH|
|*anonymous*|FEPO|
|*anonymous*|FEPS|
|*anonymous*|FEPT|
|*anonymous*|FEPTA|
|*anonymous*|FEPTP|
|*anonymous*|FEQU|
|*anonymous*|FESM|
|*anonymous*|FEST|
|*anonymous*|FEWE|
|*anonymous*|FEYE|

<h2 id="tocS_OB_FeeFrequency1Code_3">OB_FeeFrequency1Code_3</h2>
<!-- backwards compatibility -->
<a id="schemaob_feefrequency1code_3"></a>
<a id="schema_OB_FeeFrequency1Code_3"></a>
<a id="tocSob_feefrequency1code_3"></a>
<a id="tocsob_feefrequency1code_3"></a>

```json
{
  "description": "How frequently the fee/charge is calculated",
  "type": "string",
  "enum": [
    "FEAC",
    "FEAO",
    "FECP",
    "FEDA",
    "FEHO",
    "FEI",
    "FEMO",
    "FEOA",
    "FEOT",
    "FEPC",
    "FEPH",
    "FEPO",
    "FEPS",
    "FEPT",
    "FEPTA",
    "FEPTP",
    "FEQU",
    "FESM",
    "FEST",
    "FEWE",
    "FEYE"
  ]
}

```

How frequently the fee/charge is calculated

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|How frequently the fee/charge is calculated|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|FEAC|
|*anonymous*|FEAO|
|*anonymous*|FECP|
|*anonymous*|FEDA|
|*anonymous*|FEHO|
|*anonymous*|FEI|
|*anonymous*|FEMO|
|*anonymous*|FEOA|
|*anonymous*|FEOT|
|*anonymous*|FEPC|
|*anonymous*|FEPH|
|*anonymous*|FEPO|
|*anonymous*|FEPS|
|*anonymous*|FEPT|
|*anonymous*|FEPTA|
|*anonymous*|FEPTP|
|*anonymous*|FEQU|
|*anonymous*|FESM|
|*anonymous*|FEST|
|*anonymous*|FEWE|
|*anonymous*|FEYE|

<h2 id="tocS_OB_FeeFrequency1Code_4">OB_FeeFrequency1Code_4</h2>
<!-- backwards compatibility -->
<a id="schemaob_feefrequency1code_4"></a>
<a id="schema_OB_FeeFrequency1Code_4"></a>
<a id="tocSob_feefrequency1code_4"></a>
<a id="tocsob_feefrequency1code_4"></a>

```json
{
  "description": "Period e.g. day, week, month etc. for which the fee/charge is capped",
  "type": "string",
  "enum": [
    "FEAC",
    "FEAO",
    "FECP",
    "FEDA",
    "FEHO",
    "FEI",
    "FEMO",
    "FEOA",
    "FEOT",
    "FEPC",
    "FEPH",
    "FEPO",
    "FEPS",
    "FEPT",
    "FEPTA",
    "FEPTP",
    "FEQU",
    "FESM",
    "FEST",
    "FEWE",
    "FEYE"
  ]
}

```

Period e.g. day, week, month etc. for which the fee/charge is capped

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Period e.g. day, week, month etc. for which the fee/charge is capped|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|FEAC|
|*anonymous*|FEAO|
|*anonymous*|FECP|
|*anonymous*|FEDA|
|*anonymous*|FEHO|
|*anonymous*|FEI|
|*anonymous*|FEMO|
|*anonymous*|FEOA|
|*anonymous*|FEOT|
|*anonymous*|FEPC|
|*anonymous*|FEPH|
|*anonymous*|FEPO|
|*anonymous*|FEPS|
|*anonymous*|FEPT|
|*anonymous*|FEPTA|
|*anonymous*|FEPTP|
|*anonymous*|FEQU|
|*anonymous*|FESM|
|*anonymous*|FEST|
|*anonymous*|FEWE|
|*anonymous*|FEYE|

<h2 id="tocS_OB_FeeType1Code">OB_FeeType1Code</h2>
<!-- backwards compatibility -->
<a id="schemaob_feetype1code"></a>
<a id="schema_OB_FeeType1Code"></a>
<a id="tocSob_feetype1code"></a>
<a id="tocsob_feetype1code"></a>

```json
{
  "description": "Fee/Charge Type",
  "type": "string",
  "enum": [
    "FEPF",
    "FTOT",
    "FYAF",
    "FYAM",
    "FYAQ",
    "FYCP",
    "FYDB",
    "FYMI",
    "FYXX"
  ]
}

```

Fee/Charge Type

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Fee/Charge Type|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|FEPF|
|*anonymous*|FTOT|
|*anonymous*|FYAF|
|*anonymous*|FYAM|
|*anonymous*|FYAQ|
|*anonymous*|FYCP|
|*anonymous*|FYDB|
|*anonymous*|FYMI|
|*anonymous*|FYXX|

<h2 id="tocS_OB_InterestCalculationMethod1Code">OB_InterestCalculationMethod1Code</h2>
<!-- backwards compatibility -->
<a id="schemaob_interestcalculationmethod1code"></a>
<a id="schema_OB_InterestCalculationMethod1Code"></a>
<a id="tocSob_interestcalculationmethod1code"></a>
<a id="tocsob_interestcalculationmethod1code"></a>

```json
{
  "description": "Methods of calculating interest",
  "type": "string",
  "enum": [
    "ITCO",
    "ITOT",
    "ITSI"
  ]
}

```

Methods of calculating interest

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Methods of calculating interest|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|ITCO|
|*anonymous*|ITOT|
|*anonymous*|ITSI|

<h2 id="tocS_OB_InterestFixedVariableType1Code">OB_InterestFixedVariableType1Code</h2>
<!-- backwards compatibility -->
<a id="schemaob_interestfixedvariabletype1code"></a>
<a id="schema_OB_InterestFixedVariableType1Code"></a>
<a id="tocSob_interestfixedvariabletype1code"></a>
<a id="tocsob_interestfixedvariabletype1code"></a>

```json
{
  "description": "Type of interest rate, Fixed or Variable",
  "type": "string",
  "enum": [
    "INVA"
  ]
}

```

Type of interest rate, Fixed or Variable

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Type of interest rate, Fixed or Variable|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|INVA|

<h2 id="tocS_OB_InterestRateType1Code_0">OB_InterestRateType1Code_0</h2>
<!-- backwards compatibility -->
<a id="schemaob_interestratetype1code_0"></a>
<a id="schema_OB_InterestRateType1Code_0"></a>
<a id="tocSob_interestratetype1code_0"></a>
<a id="tocsob_interestratetype1code_0"></a>

```json
{
  "description": "Rate type for overdraft fee/charge (where it is charged in terms of a rate rather than an amount)",
  "type": "string",
  "enum": [
    "INBB",
    "INFR",
    "INGR",
    "INLR",
    "INNE",
    "INOT"
  ]
}

```

Rate type for overdraft fee/charge (where it is charged in terms of a rate rather than an amount)

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Rate type for overdraft fee/charge (where it is charged in terms of a rate rather than an amount)|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|INBB|
|*anonymous*|INFR|
|*anonymous*|INGR|
|*anonymous*|INLR|
|*anonymous*|INNE|
|*anonymous*|INOT|

<h2 id="tocS_OB_InterestRateType1Code_1">OB_InterestRateType1Code_1</h2>
<!-- backwards compatibility -->
<a id="schemaob_interestratetype1code_1"></a>
<a id="schema_OB_InterestRateType1Code_1"></a>
<a id="tocSob_interestratetype1code_1"></a>
<a id="tocsob_interestratetype1code_1"></a>

```json
{
  "description": "Rate type for Fee/Charge (where it is charged in terms of a rate rather than an amount)",
  "type": "string",
  "enum": [
    "INBB",
    "INFR",
    "INGR",
    "INLR",
    "INNE",
    "INOT"
  ]
}

```

Rate type for Fee/Charge (where it is charged in terms of a rate rather than an amount)

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Rate type for Fee/Charge (where it is charged in terms of a rate rather than an amount)|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|INBB|
|*anonymous*|INFR|
|*anonymous*|INGR|
|*anonymous*|INLR|
|*anonymous*|INNE|
|*anonymous*|INOT|

<h2 id="tocS_OB_MinMaxType1Code">OB_MinMaxType1Code</h2>
<!-- backwards compatibility -->
<a id="schemaob_minmaxtype1code"></a>
<a id="schema_OB_MinMaxType1Code"></a>
<a id="tocSob_minmaxtype1code"></a>
<a id="tocsob_minmaxtype1code"></a>

```json
{
  "description": "Min Max type",
  "type": "string",
  "enum": [
    "FMMN",
    "FMMX"
  ]
}

```

Min Max type

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Min Max type|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|FMMN|
|*anonymous*|FMMX|

<h2 id="tocS_OB_OtherCodeType1_0">OB_OtherCodeType1_0</h2>
<!-- backwards compatibility -->
<a id="schemaob_othercodetype1_0"></a>
<a id="schema_OB_OtherCodeType1_0"></a>
<a id="tocSob_othercodetype1_0"></a>
<a id="tocsob_othercodetype1_0"></a>

```json
{
  "type": "object",
  "required": [
    "Name",
    "Description"
  ],
  "properties": {
    "Code": {
      "description": "The four letter Mnemonic used within an XML file to identify a code",
      "type": "string",
      "pattern": "^\\\\w{0,4}$"
    },
    "Name": {
      "description": "Long name associated with the code",
      "type": "string",
      "minLength": 1,
      "maxLength": 70
    },
    "Description": {
      "description": "Description to describe the purpose of the code",
      "type": "string",
      "minLength": 1,
      "maxLength": 350
    }
  },
  "additionalProperties": false
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Code|[OB_CodeMnemonic](#schemaob_codemnemonic)|false|none|The four letter Mnemonic used within an XML file to identify a code|
|Name|[Name_4](#schemaname_4)|true|none|Long name associated with the code|
|Description|[Description_3](#schemadescription_3)|true|none|Description to describe the purpose of the code|

<h2 id="tocS_OB_OtherCodeType1_1">OB_OtherCodeType1_1</h2>
<!-- backwards compatibility -->
<a id="schemaob_othercodetype1_1"></a>
<a id="schema_OB_OtherCodeType1_1"></a>
<a id="tocSob_othercodetype1_1"></a>
<a id="tocsob_othercodetype1_1"></a>

```json
{
  "type": "object",
  "required": [
    "Name",
    "Description"
  ],
  "description": "Other application frequencies that are not available in the standard code list",
  "properties": {
    "Code": {
      "description": "The four letter Mnemonic used within an XML file to identify a code",
      "type": "string",
      "pattern": "^\\\\w{0,4}$"
    },
    "Name": {
      "description": "Long name associated with the code",
      "type": "string",
      "minLength": 1,
      "maxLength": 70
    },
    "Description": {
      "description": "Description to describe the purpose of the code",
      "type": "string",
      "minLength": 1,
      "maxLength": 350
    }
  },
  "additionalProperties": false
}

```

Other application frequencies that are not available in the standard code list

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Code|[OB_CodeMnemonic](#schemaob_codemnemonic)|false|none|The four letter Mnemonic used within an XML file to identify a code|
|Name|[Name_4](#schemaname_4)|true|none|Long name associated with the code|
|Description|[Description_3](#schemadescription_3)|true|none|Description to describe the purpose of the code|

<h2 id="tocS_OB_OtherCodeType1_2">OB_OtherCodeType1_2</h2>
<!-- backwards compatibility -->
<a id="schemaob_othercodetype1_2"></a>
<a id="schema_OB_OtherCodeType1_2"></a>
<a id="tocSob_othercodetype1_2"></a>
<a id="tocsob_othercodetype1_2"></a>

```json
{
  "type": "object",
  "required": [
    "Name",
    "Description"
  ],
  "description": "Other calculation frequency which is not available in the standard code set.",
  "properties": {
    "Code": {
      "description": "The four letter Mnemonic used within an XML file to identify a code",
      "type": "string",
      "pattern": "^\\\\w{0,4}$"
    },
    "Name": {
      "description": "Long name associated with the code",
      "type": "string",
      "minLength": 1,
      "maxLength": 70
    },
    "Description": {
      "description": "Description to describe the purpose of the code",
      "type": "string",
      "minLength": 1,
      "maxLength": 350
    }
  },
  "additionalProperties": false
}

```

Other calculation frequency which is not available in the standard code set.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Code|[OB_CodeMnemonic](#schemaob_codemnemonic)|false|none|The four letter Mnemonic used within an XML file to identify a code|
|Name|[Name_4](#schemaname_4)|true|none|Long name associated with the code|
|Description|[Description_3](#schemadescription_3)|true|none|Description to describe the purpose of the code|

<h2 id="tocS_OB_OtherCodeType1_3">OB_OtherCodeType1_3</h2>
<!-- backwards compatibility -->
<a id="schemaob_othercodetype1_3"></a>
<a id="schema_OB_OtherCodeType1_3"></a>
<a id="tocSob_othercodetype1_3"></a>
<a id="tocsob_othercodetype1_3"></a>

```json
{
  "type": "object",
  "required": [
    "Name",
    "Description"
  ],
  "description": "Other Fee type which is not available in the standard code set",
  "properties": {
    "Code": {
      "description": "The four letter Mnemonic used within an XML file to identify a code",
      "type": "string",
      "pattern": "^\\\\w{0,4}$"
    },
    "Name": {
      "description": "Long name associated with the code",
      "type": "string",
      "minLength": 1,
      "maxLength": 70
    },
    "Description": {
      "description": "Description to describe the purpose of the code",
      "type": "string",
      "minLength": 1,
      "maxLength": 350
    }
  },
  "additionalProperties": false
}

```

Other Fee type which is not available in the standard code set

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Code|[OB_CodeMnemonic](#schemaob_codemnemonic)|false|none|The four letter Mnemonic used within an XML file to identify a code|
|Name|[Name_4](#schemaname_4)|true|none|Long name associated with the code|
|Description|[Description_3](#schemadescription_3)|true|none|Description to describe the purpose of the code|

<h2 id="tocS_OB_OtherCodeType1_4">OB_OtherCodeType1_4</h2>
<!-- backwards compatibility -->
<a id="schemaob_othercodetype1_4"></a>
<a id="schema_OB_OtherCodeType1_4"></a>
<a id="tocSob_othercodetype1_4"></a>
<a id="tocsob_othercodetype1_4"></a>

```json
{
  "type": "object",
  "required": [
    "Name",
    "Description"
  ],
  "description": "Other fee rate type code which is not available in the standard code set",
  "properties": {
    "Code": {
      "description": "The four letter Mnemonic used within an XML file to identify a code",
      "type": "string",
      "pattern": "^\\\\w{0,4}$"
    },
    "Name": {
      "description": "Long name associated with the code",
      "type": "string",
      "minLength": 1,
      "maxLength": 70
    },
    "Description": {
      "description": "Description to describe the purpose of the code",
      "type": "string",
      "minLength": 1,
      "maxLength": 350
    }
  },
  "additionalProperties": false
}

```

Other fee rate type code which is not available in the standard code set

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Code|[OB_CodeMnemonic](#schemaob_codemnemonic)|false|none|The four letter Mnemonic used within an XML file to identify a code|
|Name|[Name_4](#schemaname_4)|true|none|Long name associated with the code|
|Description|[Description_3](#schemadescription_3)|true|none|Description to describe the purpose of the code|

<h2 id="tocS_OB_OtherCodeType1_5">OB_OtherCodeType1_5</h2>
<!-- backwards compatibility -->
<a id="schemaob_othercodetype1_5"></a>
<a id="schema_OB_OtherCodeType1_5"></a>
<a id="tocSob_othercodetype1_5"></a>
<a id="tocsob_othercodetype1_5"></a>

```json
{
  "type": "object",
  "required": [
    "Name",
    "Description"
  ],
  "description": "Other fee rate type which is not in the standard rate type list",
  "properties": {
    "Code": {
      "description": "The four letter Mnemonic used within an XML file to identify a code",
      "type": "string",
      "pattern": "^\\\\w{0,4}$"
    },
    "Name": {
      "description": "Long name associated with the code",
      "type": "string",
      "minLength": 1,
      "maxLength": 70
    },
    "Description": {
      "description": "Description to describe the purpose of the code",
      "type": "string",
      "minLength": 1,
      "maxLength": 350
    }
  },
  "additionalProperties": false
}

```

Other fee rate type which is not in the standard rate type list

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Code|[OB_CodeMnemonic](#schemaob_codemnemonic)|false|none|The four letter Mnemonic used within an XML file to identify a code|
|Name|[Name_4](#schemaname_4)|true|none|Long name associated with the code|
|Description|[Description_3](#schemadescription_3)|true|none|Description to describe the purpose of the code|

<h2 id="tocS_OB_OtherCodeType1_6">OB_OtherCodeType1_6</h2>
<!-- backwards compatibility -->
<a id="schemaob_othercodetype1_6"></a>
<a id="schema_OB_OtherCodeType1_6"></a>
<a id="tocSob_othercodetype1_6"></a>
<a id="tocsob_othercodetype1_6"></a>

```json
{
  "type": "object",
  "required": [
    "Name",
    "Description"
  ],
  "description": "Other application frequencies not covered in the standard code list",
  "properties": {
    "Code": {
      "description": "The four letter Mnemonic used within an XML file to identify a code",
      "type": "string",
      "pattern": "^\\\\w{0,4}$"
    },
    "Name": {
      "description": "Long name associated with the code",
      "type": "string",
      "minLength": 1,
      "maxLength": 70
    },
    "Description": {
      "description": "Description to describe the purpose of the code",
      "type": "string",
      "minLength": 1,
      "maxLength": 350
    }
  },
  "additionalProperties": false
}

```

Other application frequencies not covered in the standard code list

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Code|[OB_CodeMnemonic](#schemaob_codemnemonic)|false|none|The four letter Mnemonic used within an XML file to identify a code|
|Name|[Name_4](#schemaname_4)|true|none|Long name associated with the code|
|Description|[Description_3](#schemadescription_3)|true|none|Description to describe the purpose of the code|

<h2 id="tocS_OB_OtherCodeType1_7">OB_OtherCodeType1_7</h2>
<!-- backwards compatibility -->
<a id="schemaob_othercodetype1_7"></a>
<a id="schema_OB_OtherCodeType1_7"></a>
<a id="tocSob_othercodetype1_7"></a>
<a id="tocsob_othercodetype1_7"></a>

```json
{
  "type": "object",
  "required": [
    "Name",
    "Description"
  ],
  "description": "Other calculation frequency which is not available in standard code set.",
  "properties": {
    "Code": {
      "description": "The four letter Mnemonic used within an XML file to identify a code",
      "type": "string",
      "pattern": "^\\\\w{0,4}$"
    },
    "Name": {
      "description": "Long name associated with the code",
      "type": "string",
      "minLength": 1,
      "maxLength": 70
    },
    "Description": {
      "description": "Description to describe the purpose of the code",
      "type": "string",
      "minLength": 1,
      "maxLength": 350
    }
  },
  "additionalProperties": false
}

```

Other calculation frequency which is not available in standard code set.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Code|[OB_CodeMnemonic](#schemaob_codemnemonic)|false|none|The four letter Mnemonic used within an XML file to identify a code|
|Name|[Name_4](#schemaname_4)|true|none|Long name associated with the code|
|Description|[Description_3](#schemadescription_3)|true|none|Description to describe the purpose of the code|

<h2 id="tocS_OB_OtherCodeType1_8">OB_OtherCodeType1_8</h2>
<!-- backwards compatibility -->
<a id="schemaob_othercodetype1_8"></a>
<a id="schema_OB_OtherCodeType1_8"></a>
<a id="tocSob_othercodetype1_8"></a>
<a id="tocsob_othercodetype1_8"></a>

```json
{
  "type": "object",
  "required": [
    "Name",
    "Description"
  ],
  "description": "Other fee rate type which is not available in the standard code set",
  "properties": {
    "Code": {
      "description": "The four letter Mnemonic used within an XML file to identify a code",
      "type": "string",
      "pattern": "^\\\\w{0,4}$"
    },
    "Name": {
      "description": "Long name associated with the code",
      "type": "string",
      "minLength": 1,
      "maxLength": 70
    },
    "Description": {
      "description": "Description to describe the purpose of the code",
      "type": "string",
      "minLength": 1,
      "maxLength": 350
    }
  },
  "additionalProperties": false
}

```

Other fee rate type which is not available in the standard code set

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Code|[OB_CodeMnemonic](#schemaob_codemnemonic)|false|none|The four letter Mnemonic used within an XML file to identify a code|
|Name|[Name_4](#schemaname_4)|true|none|Long name associated with the code|
|Description|[Description_3](#schemadescription_3)|true|none|Description to describe the purpose of the code|

<h2 id="tocS_OB_OtherFeeChargeDetailType">OB_OtherFeeChargeDetailType</h2>
<!-- backwards compatibility -->
<a id="schemaob_otherfeechargedetailtype"></a>
<a id="schema_OB_OtherFeeChargeDetailType"></a>
<a id="tocSob_otherfeechargedetailtype"></a>
<a id="tocsob_otherfeechargedetailtype"></a>

```json
{
  "type": "object",
  "required": [
    "FeeCategory",
    "Name",
    "Description"
  ],
  "description": "Other Fee/charge type which is not available in the standard code set",
  "properties": {
    "Code": {
      "description": "The four letter Mnemonic used within an XML file to identify a code",
      "type": "string",
      "pattern": "^\\\\w{0,4}$"
    },
    "FeeCategory": {
      "description": "Categorisation of fees and charges into standard categories.",
      "type": "string",
      "enum": [
        "FCOT",
        "FCRE",
        "FCSV"
      ]
    },
    "Name": {
      "description": "Long name associated with the code",
      "type": "string",
      "minLength": 1,
      "maxLength": 70
    },
    "Description": {
      "description": "Description to describe the purpose of the code",
      "type": "string",
      "minLength": 1,
      "maxLength": 350
    }
  },
  "additionalProperties": false
}

```

Other Fee/charge type which is not available in the standard code set

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Code|[OB_CodeMnemonic](#schemaob_codemnemonic)|false|none|The four letter Mnemonic used within an XML file to identify a code|
|FeeCategory|[OB_FeeCategory1Code](#schemaob_feecategory1code)|true|none|Categorisation of fees and charges into standard categories.|
|Name|[Name_4](#schemaname_4)|true|none|Long name associated with the code|
|Description|[Description_3](#schemadescription_3)|true|none|Description to describe the purpose of the code|

<h2 id="tocS_OB_OverdraftFeeType1Code">OB_OverdraftFeeType1Code</h2>
<!-- backwards compatibility -->
<a id="schemaob_overdraftfeetype1code"></a>
<a id="schema_OB_OverdraftFeeType1Code"></a>
<a id="tocSob_overdraftfeetype1code"></a>
<a id="tocsob_overdraftfeetype1code"></a>

```json
{
  "description": "Overdraft fee type",
  "type": "string",
  "enum": [
    "FBAO",
    "FBAR",
    "FBEB",
    "FBIT",
    "FBOR",
    "FBOS",
    "FBSC",
    "FBTO",
    "FBUB",
    "FBUT",
    "FTOT",
    "FTUT"
  ]
}

```

Overdraft fee type

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Overdraft fee type|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|FBAO|
|*anonymous*|FBAR|
|*anonymous*|FBEB|
|*anonymous*|FBIT|
|*anonymous*|FBOR|
|*anonymous*|FBOS|
|*anonymous*|FBSC|
|*anonymous*|FBTO|
|*anonymous*|FBUB|
|*anonymous*|FBUT|
|*anonymous*|FTOT|
|*anonymous*|FTUT|

<h2 id="tocS_OB_Period1Code">OB_Period1Code</h2>
<!-- backwards compatibility -->
<a id="schemaob_period1code"></a>
<a id="schema_OB_Period1Code"></a>
<a id="tocSob_period1code"></a>
<a id="tocsob_period1code"></a>

```json
{
  "description": "Period e.g. day, week, month etc. for which the fee/charge is capped",
  "type": "string",
  "enum": [
    "PACT",
    "PDAY",
    "PHYR",
    "PMTH",
    "PQTR",
    "PWEK",
    "PYER"
  ]
}

```

Period e.g. day, week, month etc. for which the fee/charge is capped

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Period e.g. day, week, month etc. for which the fee/charge is capped|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|PACT|
|*anonymous*|PDAY|
|*anonymous*|PHYR|
|*anonymous*|PMTH|
|*anonymous*|PQTR|
|*anonymous*|PWEK|
|*anonymous*|PYER|

<h2 id="tocS_OB_Rate1_0">OB_Rate1_0</h2>
<!-- backwards compatibility -->
<a id="schemaob_rate1_0"></a>
<a id="schema_OB_Rate1_0"></a>
<a id="tocSob_rate1_0"></a>
<a id="tocsob_rate1_0"></a>

```json
{
  "description": "Rate charged for overdraft fee/charge (where it is charged in terms of a rate rather than an amount)",
  "type": "string",
  "pattern": "^(-?\\d{1,3}){1}(\\.\\d{1,4}){0,1}$"
}

```

Rate charged for overdraft fee/charge (where it is charged in terms of a rate rather than an amount)

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Rate charged for overdraft fee/charge (where it is charged in terms of a rate rather than an amount)|

<h2 id="tocS_OB_Rate1_1">OB_Rate1_1</h2>
<!-- backwards compatibility -->
<a id="schemaob_rate1_1"></a>
<a id="schema_OB_Rate1_1"></a>
<a id="tocSob_rate1_1"></a>
<a id="tocsob_rate1_1"></a>

```json
{
  "description": "Rate charged for Fee/Charge (where it is charged in terms of a rate rather than an amount)",
  "type": "string",
  "pattern": "^(-?\\d{1,3}){1}(\\.\\d{1,4}){0,1}$"
}

```

Rate charged for Fee/Charge (where it is charged in terms of a rate rather than an amount)

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Rate charged for Fee/Charge (where it is charged in terms of a rate rather than an amount)|

<h2 id="tocS_OpeningDate">OpeningDate</h2>
<!-- backwards compatibility -->
<a id="schemaopeningdate"></a>
<a id="schema_OpeningDate"></a>
<a id="tocSopeningdate"></a>
<a id="tocsopeningdate"></a>

```json
{
  "description": "Date on which the account and related basic services are effectively operational for the account owner.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
  "type": "string",
  "format": "date-time"
}

```

Date on which the account and related basic services are effectively operational for the account owner.All dates in the JSON payloads are represented in ISO 8601 date-time format. 
All date-time fields in responses must include the timezone. An example is below:
2017-04-05T10:43:07+00:00

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string(date-time)|false|none|Date on which the account and related basic services are effectively operational for the account owner.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|

<h2 id="tocS_PartyId">PartyId</h2>
<!-- backwards compatibility -->
<a id="schemapartyid"></a>
<a id="schema_PartyId"></a>
<a id="tocSpartyid"></a>
<a id="tocspartyid"></a>

```json
{
  "description": "A unique and immutable identifier used to identify the customer resource. This identifier has no meaning to the account owner.",
  "type": "string",
  "minLength": 1,
  "maxLength": 40
}

```

A unique and immutable identifier used to identify the customer resource. This identifier has no meaning to the account owner.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|A unique and immutable identifier used to identify the customer resource. This identifier has no meaning to the account owner.|

<h2 id="tocS_PartyNumber">PartyNumber</h2>
<!-- backwards compatibility -->
<a id="schemapartynumber"></a>
<a id="schema_PartyNumber"></a>
<a id="tocSpartynumber"></a>
<a id="tocspartynumber"></a>

```json
{
  "description": "Number assigned by an agent to identify its customer.",
  "type": "string",
  "minLength": 1,
  "maxLength": 35
}

```

Number assigned by an agent to identify its customer.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Number assigned by an agent to identify its customer.|

<h2 id="tocS_PhoneNumber_0">PhoneNumber_0</h2>
<!-- backwards compatibility -->
<a id="schemaphonenumber_0"></a>
<a id="schema_PhoneNumber_0"></a>
<a id="tocSphonenumber_0"></a>
<a id="tocsphonenumber_0"></a>

```json
{
  "description": "Collection of information that identifies a phone number, as defined by telecom services.",
  "type": "string",
  "pattern": "\\+[0-9]{1,3}-[0-9()+\\-]{1,30}"
}

```

Collection of information that identifies a phone number, as defined by telecom services.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Collection of information that identifies a phone number, as defined by telecom services.|

<h2 id="tocS_PhoneNumber_1">PhoneNumber_1</h2>
<!-- backwards compatibility -->
<a id="schemaphonenumber_1"></a>
<a id="schema_PhoneNumber_1"></a>
<a id="tocSphonenumber_1"></a>
<a id="tocsphonenumber_1"></a>

```json
{
  "description": "Collection of information that identifies a mobile phone number, as defined by telecom services.",
  "type": "string",
  "pattern": "\\+[0-9]{1,3}-[0-9()+\\-]{1,30}"
}

```

Collection of information that identifies a mobile phone number, as defined by telecom services.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Collection of information that identifies a mobile phone number, as defined by telecom services.|

<h2 id="tocS_PostCode">PostCode</h2>
<!-- backwards compatibility -->
<a id="schemapostcode"></a>
<a id="schema_PostCode"></a>
<a id="tocSpostcode"></a>
<a id="tocspostcode"></a>

```json
{
  "description": "Identifier consisting of a group of letters and/or numbers that is added to a postal address to assist the sorting of mail.",
  "type": "string",
  "minLength": 1,
  "maxLength": 16
}

```

Identifier consisting of a group of letters and/or numbers that is added to a postal address to assist the sorting of mail.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Identifier consisting of a group of letters and/or numbers that is added to a postal address to assist the sorting of mail.|

<h2 id="tocS_PreviousPaymentDateTime">PreviousPaymentDateTime</h2>
<!-- backwards compatibility -->
<a id="schemapreviouspaymentdatetime"></a>
<a id="schema_PreviousPaymentDateTime"></a>
<a id="tocSpreviouspaymentdatetime"></a>
<a id="tocspreviouspaymentdatetime"></a>

```json
{
  "description": "Date of most recent direct debit collection.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
  "type": "string",
  "format": "date-time"
}

```

Date of most recent direct debit collection.All dates in the JSON payloads are represented in ISO 8601 date-time format. 
All date-time fields in responses must include the timezone. An example is below:
2017-04-05T10:43:07+00:00

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string(date-time)|false|none|Date of most recent direct debit collection.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|

<h2 id="tocS_ProprietaryBankTransactionCodeStructure1">ProprietaryBankTransactionCodeStructure1</h2>
<!-- backwards compatibility -->
<a id="schemaproprietarybanktransactioncodestructure1"></a>
<a id="schema_ProprietaryBankTransactionCodeStructure1"></a>
<a id="tocSproprietarybanktransactioncodestructure1"></a>
<a id="tocsproprietarybanktransactioncodestructure1"></a>

```json
{
  "type": "object",
  "required": [
    "Code"
  ],
  "description": "Set of elements to fully identify a proprietary bank transaction code.",
  "properties": {
    "Code": {
      "description": "Proprietary bank transaction code to identify the underlying transaction.",
      "type": "string",
      "minLength": 1,
      "maxLength": 35
    }
  },
  "additionalProperties": false
}

```

Set of elements to fully identify a proprietary bank transaction code.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|Code|string|true|none|Proprietary bank transaction code to identify the underlying transaction.|

<h2 id="tocS_Rate">Rate</h2>
<!-- backwards compatibility -->
<a id="schemarate"></a>
<a id="schema_Rate"></a>
<a id="tocSrate"></a>
<a id="tocsrate"></a>

```json
{
  "description": "Rate associated with the statement rate type.",
  "type": "string",
  "pattern": "^(-?\\d{1,3}){1}(\\.\\d{1,4}){0,1}$"
}

```

Rate associated with the statement rate type.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Rate associated with the statement rate type.|

<h2 id="tocS_Reference">Reference</h2>
<!-- backwards compatibility -->
<a id="schemareference"></a>
<a id="schema_Reference"></a>
<a id="tocSreference"></a>
<a id="tocsreference"></a>

```json
{
  "description": "Unique reference, as assigned by the creditor, to unambiguously refer to the payment transaction.\nUsage: If available, the initiating party should provide this reference in the structured remittance information, to enable reconciliation by the creditor upon receipt of the amount of money.\nIf the business context requires the use of a creditor reference or a payment remit identification, and only one identifier can be passed through the end-to-end chain, the creditor's reference or payment remittance identification should be quoted in the end-to-end transaction identification.",
  "type": "string",
  "minLength": 1,
  "maxLength": 35
}

```

Unique reference, as assigned by the creditor, to unambiguously refer to the payment transaction.
Usage: If available, the initiating party should provide this reference in the structured remittance information, to enable reconciliation by the creditor upon receipt of the amount of money.
If the business context requires the use of a creditor reference or a payment remit identification, and only one identifier can be passed through the end-to-end chain, the creditor's reference or payment remittance identification should be quoted in the end-to-end transaction identification.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Unique reference, as assigned by the creditor, to unambiguously refer to the payment transaction.<br>Usage: If available, the initiating party should provide this reference in the structured remittance information, to enable reconciliation by the creditor upon receipt of the amount of money.<br>If the business context requires the use of a creditor reference or a payment remit identification, and only one identifier can be passed through the end-to-end chain, the creditor's reference or payment remittance identification should be quoted in the end-to-end transaction identification.|

<h2 id="tocS_OBBeneficiaryType1Code">OBBeneficiaryType1Code</h2>
<!-- backwards compatibility -->
<a id="schemaobbeneficiarytype1code"></a>
<a id="schema_OBBeneficiaryType1Code"></a>
<a id="tocSobbeneficiarytype1code"></a>
<a id="tocsobbeneficiarytype1code"></a>

```json
{
  "description": "Specifies the Beneficiary Type.",
  "type": "string",
  "enum": [
    "Trusted",
    "Ordinary"
  ]
}

```

Specifies the Beneficiary Type.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Specifies the Beneficiary Type.|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|Trusted|
|*anonymous*|Ordinary|

<h2 id="tocS_ScheduledPaymentDateTime">ScheduledPaymentDateTime</h2>
<!-- backwards compatibility -->
<a id="schemascheduledpaymentdatetime"></a>
<a id="schema_ScheduledPaymentDateTime"></a>
<a id="tocSscheduledpaymentdatetime"></a>
<a id="tocsscheduledpaymentdatetime"></a>

```json
{
  "description": "The date on which the scheduled payment will be made.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
  "type": "string",
  "format": "date-time"
}

```

The date on which the scheduled payment will be made.All dates in the JSON payloads are represented in ISO 8601 date-time format. 
All date-time fields in responses must include the timezone. An example is below:
2017-04-05T10:43:07+00:00

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string(date-time)|false|none|The date on which the scheduled payment will be made.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|

<h2 id="tocS_ScheduledPaymentId">ScheduledPaymentId</h2>
<!-- backwards compatibility -->
<a id="schemascheduledpaymentid"></a>
<a id="schema_ScheduledPaymentId"></a>
<a id="tocSscheduledpaymentid"></a>
<a id="tocsscheduledpaymentid"></a>

```json
{
  "description": "A unique and immutable identifier used to identify the scheduled payment resource. This identifier has no meaning to the account owner.",
  "type": "string",
  "minLength": 1,
  "maxLength": 40
}

```

A unique and immutable identifier used to identify the scheduled payment resource. This identifier has no meaning to the account owner.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|A unique and immutable identifier used to identify the scheduled payment resource. This identifier has no meaning to the account owner.|

<h2 id="tocS_SecondaryIdentification">SecondaryIdentification</h2>
<!-- backwards compatibility -->
<a id="schemasecondaryidentification"></a>
<a id="schema_SecondaryIdentification"></a>
<a id="tocSsecondaryidentification"></a>
<a id="tocssecondaryidentification"></a>

```json
{
  "description": "This is secondary identification of the account, as assigned by the account servicing institution. \nThis can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).",
  "type": "string",
  "minLength": 1,
  "maxLength": 34
}

```

This is secondary identification of the account, as assigned by the account servicing institution. 
This can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|This is secondary identification of the account, as assigned by the account servicing institution. <br>This can be used by building societies to additionally identify accounts with a roll number (in addition to a sort code and account number combination).|

<h2 id="tocS_StandingOrderId">StandingOrderId</h2>
<!-- backwards compatibility -->
<a id="schemastandingorderid"></a>
<a id="schema_StandingOrderId"></a>
<a id="tocSstandingorderid"></a>
<a id="tocsstandingorderid"></a>

```json
{
  "description": "A unique and immutable identifier used to identify the standing order resource. This identifier has no meaning to the account owner.",
  "type": "string",
  "minLength": 1,
  "maxLength": 40
}

```

A unique and immutable identifier used to identify the standing order resource. This identifier has no meaning to the account owner.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|A unique and immutable identifier used to identify the standing order resource. This identifier has no meaning to the account owner.|

<h2 id="tocS_StartDateTime">StartDateTime</h2>
<!-- backwards compatibility -->
<a id="schemastartdatetime"></a>
<a id="schema_StartDateTime"></a>
<a id="tocSstartdatetime"></a>
<a id="tocsstartdatetime"></a>

```json
{
  "description": "Date and time at which the statement period starts.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
  "type": "string",
  "format": "date-time"
}

```

Date and time at which the statement period starts.All dates in the JSON payloads are represented in ISO 8601 date-time format. 
All date-time fields in responses must include the timezone. An example is below:
2017-04-05T10:43:07+00:00

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string(date-time)|false|none|Date and time at which the statement period starts.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|

<h2 id="tocS_StatusUpdateDateTime">StatusUpdateDateTime</h2>
<!-- backwards compatibility -->
<a id="schemastatusupdatedatetime"></a>
<a id="schema_StatusUpdateDateTime"></a>
<a id="tocSstatusupdatedatetime"></a>
<a id="tocsstatusupdatedatetime"></a>

```json
{
  "description": "Date and time at which the resource status was updated.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
  "type": "string",
  "format": "date-time"
}

```

Date and time at which the resource status was updated.All dates in the JSON payloads are represented in ISO 8601 date-time format. 
All date-time fields in responses must include the timezone. An example is below:
2017-04-05T10:43:07+00:00

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string(date-time)|false|none|Date and time at which the resource status was updated.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|

<h2 id="tocS_StreetName">StreetName</h2>
<!-- backwards compatibility -->
<a id="schemastreetname"></a>
<a id="schema_StreetName"></a>
<a id="tocSstreetname"></a>
<a id="tocsstreetname"></a>

```json
{
  "description": "Name of a street or thoroughfare.",
  "type": "string",
  "minLength": 1,
  "maxLength": 70
}

```

Name of a street or thoroughfare.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Name of a street or thoroughfare.|

<h2 id="tocS_TownName">TownName</h2>
<!-- backwards compatibility -->
<a id="schematownname"></a>
<a id="schema_TownName"></a>
<a id="tocStownname"></a>
<a id="tocstownname"></a>

```json
{
  "description": "Name of a built-up area, with defined boundaries, and a local government.",
  "type": "string",
  "minLength": 1,
  "maxLength": 35
}

```

Name of a built-up area, with defined boundaries, and a local government.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Name of a built-up area, with defined boundaries, and a local government.|

<h2 id="tocS_TransactionId">TransactionId</h2>
<!-- backwards compatibility -->
<a id="schematransactionid"></a>
<a id="schema_TransactionId"></a>
<a id="tocStransactionid"></a>
<a id="tocstransactionid"></a>

```json
{
  "description": "Unique identifier for the transaction within an servicing institution. This identifier is both unique and immutable.",
  "type": "string",
  "minLength": 1,
  "maxLength": 210
}

```

Unique identifier for the transaction within an servicing institution. This identifier is both unique and immutable.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Unique identifier for the transaction within an servicing institution. This identifier is both unique and immutable.|

<h2 id="tocS_TransactionInformation">TransactionInformation</h2>
<!-- backwards compatibility -->
<a id="schematransactioninformation"></a>
<a id="schema_TransactionInformation"></a>
<a id="tocStransactioninformation"></a>
<a id="tocstransactioninformation"></a>

```json
{
  "description": "Further details of the transaction. \nThis is the transaction narrative, which is unstructured text.",
  "type": "string",
  "minLength": 1,
  "maxLength": 500
}

```

Further details of the transaction. 
This is the transaction narrative, which is unstructured text.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Further details of the transaction. <br>This is the transaction narrative, which is unstructured text.|

<h2 id="tocS_TransactionReference">TransactionReference</h2>
<!-- backwards compatibility -->
<a id="schematransactionreference"></a>
<a id="schema_TransactionReference"></a>
<a id="tocStransactionreference"></a>
<a id="tocstransactionreference"></a>

```json
{
  "description": "Unique reference for the transaction. This reference is optionally populated, and may as an example be the FPID in the Faster Payments context.",
  "type": "string",
  "minLength": 1,
  "maxLength": 210
}

```

Unique reference for the transaction. This reference is optionally populated, and may as an example be the FPID in the Faster Payments context.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Unique reference for the transaction. This reference is optionally populated, and may as an example be the FPID in the Faster Payments context.|

<h2 id="tocS_Value">Value</h2>
<!-- backwards compatibility -->
<a id="schemavalue"></a>
<a id="schema_Value"></a>
<a id="tocSvalue"></a>
<a id="tocsvalue"></a>

```json
{
  "description": "Value associated with the statement value type.",
  "type": "string",
  "minLength": 1,
  "maxLength": 40
}

```

Value associated with the statement value type.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Value associated with the statement value type.|

<h2 id="tocS_ValueDateTime">ValueDateTime</h2>
<!-- backwards compatibility -->
<a id="schemavaluedatetime"></a>
<a id="schema_ValueDateTime"></a>
<a id="tocSvaluedatetime"></a>
<a id="tocsvaluedatetime"></a>

```json
{
  "description": "Date and time at which assets become available to the account owner in case of a credit entry, or cease to be available to the account owner in case of a debit transaction entry.\nUsage: If transaction entry status is pending and value date is present, then the value date refers to an expected/requested value date.\nFor transaction entries subject to availability/float and for which availability information is provided, the value date must not be used. In this case the availability component identifies the number of availability days.All dates in the JSON payloads are represented in ISO 8601 date-time format. \nAll date-time fields in responses must include the timezone. An example is below:\n2017-04-05T10:43:07+00:00",
  "type": "string",
  "format": "date-time"
}

```

Date and time at which assets become available to the account owner in case of a credit entry, or cease to be available to the account owner in case of a debit transaction entry.
Usage: If transaction entry status is pending and value date is present, then the value date refers to an expected/requested value date.
For transaction entries subject to availability/float and for which availability information is provided, the value date must not be used. In this case the availability component identifies the number of availability days.All dates in the JSON payloads are represented in ISO 8601 date-time format. 
All date-time fields in responses must include the timezone. An example is below:
2017-04-05T10:43:07+00:00

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string(date-time)|false|none|Date and time at which assets become available to the account owner in case of a credit entry, or cease to be available to the account owner in case of a debit transaction entry.<br>Usage: If transaction entry status is pending and value date is present, then the value date refers to an expected/requested value date.<br>For transaction entries subject to availability/float and for which availability information is provided, the value date must not be used. In this case the availability component identifies the number of availability days.All dates in the JSON payloads are represented in ISO 8601 date-time format. <br>All date-time fields in responses must include the timezone. An example is below:<br>2017-04-05T10:43:07+00:00|

<h2 id="tocS_Model">Model</h2>
<!-- backwards compatibility -->
<a id="schemamodel"></a>
<a id="schema_Model"></a>
<a id="tocSmodel"></a>
<a id="tocsmodel"></a>

```json
{
  "type": "object",
  "properties": {
    "id": {
      "type": "integer",
      "format": "int64"
    }
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|integer(int64)|false|none|none|

