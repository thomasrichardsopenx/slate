---
title: OpenX Platform API Reference

language_tabs:
  - shell
  - python
  - java
  - php

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by OpenX</a>

includes:
  - errors

search: true
---

# Introduction

OpenX provides a JSON-based API to communicate with the OpenX Platform and perform create, read, update, delete, and batch operations over HTTP. This allows you to develop and integrate applications. Any tasks that you can perform in the OpenX UI can be automated via calls to the OpenX Platform API.

This guide is intended for developers responsible for integrating their applications with OpenX. You should have a general understanding of OpenX ad serving objects and how they relate to one another, such as accounts, users, sites, ad units, orders, line items, and ads.

For detailed information about each Platform API object or service, see the Platform API reference. You can also query the API directly to get information about available fields for each object and available options. For details, see available fields.
To see what's new in the platform API, see the API Release Notes.

# Getting started

Use the links in this quick overview to get started using the OpenX API.
Authenticating users - Describes your options for authenticating a user to the Openx Platform API, with links to more information.
API client libraries - Describes the available OpenX client libaries that can help you integrate your application with the OpenX API.
Third-party libraries - Provides a list of links to recommended third-party libraries to consider for your OpenX API application integration.
Requests and responses - Explains the basic structure of an HTTP request and a JSON response.
Response codes and error handling - Describes the types of errors that are returned and what they mean.

# Authenticating users

If you are programmatically accessing the OpenX API, you must provide a method to authenticate a user. The OpenX API uses OAuth 1.0 to authenticate users. The following explains your options.
Your Authentication Options
You can programmatically authenticate an OpenX user in one of three ways:
(Recommended) Use the client libraries provided by OpenX.
(Recommended) Use a third-party library that already implements an OAuth 1.0 scheme. Note that OpenX does not support or manage the content of these libraries.
Write your own authentication code. OpenX provides a brief OAuth reference for those who want to take this approach.
Credentials Supplied by OpenX

When you become an API customer, OpenX provides you with the following credentials which you will use in initial authentication calls.
Credentials Supplied by OpenX

Parameters:

Parameter | Description |
--------- | ------- | 
username | Your account username provided by your account manager.
Password | Your account password provided by your account manager.
Consumer Key | The ID portion of your Consumer credentials, provided by your account manager.
Consumer Secret | The Consumer secret can be thought of as the password for the Consumer credentials. This is also provided by your account manager.
OAuth Realm | The realm value is a string, generally assigned by the origin server. In this case, the realm parameter is used for your OpenX server instance. For example, OAuth realm="http://server_name.com"

#API Client Libraries

The OpenX Platform API provides a number of different API client libraries you can use to start integrating your application with the API. Currently, there are three available client libraries in the OpenX public GitHub repository you can use for application integration with the Platform API:
Java
PHP
Python

##Java Client Library

The Java client library enables you to implement OAuth authentication logic to work with the API so you will not need to implement the authentication logic yourself. This simplifies and speeds up the process of integrating your application with the OpenX Platform API. Within the client library, you will find a README.txt file that describes how you can install, configure and use the Java client library to work with your application, as well as demo authentication files (e.g. test.java/com/openx/oauthdemo) and a default.properties file you can use in your implementation.

To use the Java client library, simply navigate to the OpenX public GitHub location for the client libary and review the included README.txt file, which explains how to install and configure your OpenX instance, build the JAR file, and then use the Java package to integrate your application with the OpenX Platform API.

In the client library, you will also see a number of example scripts and demo files in the src folder that you can use in your implementation of the OpenX Platform API.

## PHP Client Library

The PHP client library provides a client class with examples you can use to integrate your application with the OpenX Platform API. Included in this client library are a README text file, which describes how to install and configure the Zend Framework to use the API, some example scripts you can use to create new advertiser and publisher objects, and an example.php script that implements the OAuth authentication logic and the PHP client library.

If you would like to use the PHP client library, navigate to the OpenX public GitHub location for the client libary and review the included README.md file, which explains how to install the Zend Framework (versions 1 and 2), configure OAuth authentication, and then make different API calls (GET, POST, PUT and DELETE) using the client library.

## Python Client Library

The Python client library provides a client class you can use to integrate with the OpenX Platform API, including information on how to install the library and then authenticate your application. Using the client library simplifies the application integration process because the OAuth authentication logic is automatically implemented for you. The client library includes an init.py client library example, as well as a setup.py script.

When using the Python client library, make sure to review the included README.md, which describes basic usage, client library installation, and OAuth authentication. If you select the tests folder, you will find several sample python scripts that you can use in your implementation. If you select the ox3apiclient folder, you can use in the included init.py script as part of your implementation and integration with the OpenX Platform API.

Note: Platform API users should download the latest develop branch for each API client library and should put a watch on the master branch to know when updates are available. The master currently supports API v3.

Each API client library solves connection issues for developers because they all implement OAuth.

## API client library features

Feature | Java | Python | PHP
--------- | ------- | --------- | --------- |
OAuth | X | X | X |
HTTP Delete |  | X | X|
HTTP GET | X | X | X |
HTTP POST |  | X | X |
HTTP PUT |  | X | X |
Creative upload |  |  | X |
Updates |  | X | X |
Cookie Persistence |  | X |  |

Important: The OAuth realm parameter is in the OAuth specification, but it may be missing in some OAuth client libraries. You must use the realm parameter or your client cannot do more than retrieve a request token.

Important: The Platform API client libraries support single sign-on (SSO) when 4.0 is specified in the base URI.

#Conventions

The OpenX Platform API provides the following types of resources:
Objects. Support CRUD operations using the following HTTP verbs:
POST. Create the specified item
GET. Read a representation of the resource
PUT. Update the specified item
DELETE. Delete the specified item
Services. Support only read operations (i.e., GET)
For a list of resources, see the API reference.
To access a resource, construct a request according to the following URI format:
base_URIresource/identifier/parameterparametermethod&
Where:
method is an HTTP method, such as GET.
base-URI is your base URI provided by your account manager.
resource is the name of an API object or service.
identifier may provide a UID or a request for specific values if needed.
parameter indicates the first URI parameter string.
&parameter is an additional URI parameter string.
For example, in the call GET http://openx_server_name/ox/4.0/user/available_fieldsaction=create
GET is the method.
http://openx_server_name/ox/4.0/ is the base URI. The relative path /4.0/ indicates that your are using version 4 of the Platform API. If your base URI includes /3.0/, this API guide does not apply to your instance.
user is an API resource.
available_fields indicates a specific a request for information about the object's fields.
action=create is a URI parameter string indicating that the request is for fields available upon the object's creation.

## Requests and responses

Requests to the OpenX API require a Content-Type header set to application/json. The response format for all requests is a JSON object and an HTTP response code.
OpenX API v4 calls use the following general patterns:
GET /resource_type/. List all objects or services of the specified type.
GET /object_type/object_UID. Retrieve information about the object specified by its UID.
POST /object_type. Create a new object using the values encoded in a JSON object, which must include all fields that are required for the create operation. You can make batch create requests by including multiple JSON objects.
PUT /object_type. Batch update operation. Requests include a set of valid JSON objects with any fields that are being changed by the update. You can optionally include unchanged data.
PUT /object_type/object_UID. Update the specified object with changed values specified in a JSON object. You can optionally include unchanged data.
DELETE /object_type. Batch delete operation, where the request includes multiple UIDs.
DELETE /object_type/object_UID. Delete the specified object.
The request samples in this guide use cURL (client URL request library) to send HTTP requests to access, create, and manipulate OpenX API resources.

### Create

To create a new object, send a POST request including the JSON-encoded contents of the object:
Sample create request
curl -X POST --header "Content-Type: application/json" http://openx_server_name/ox/4.0/account \
	--cookie "openx3_access_token=token_string" \
	--data='{
	"account_uid": "account_uid",
	"account_id": "account_id",
	"name": "API+Demo+Advertiser",
	"country_of_business":"us",
	"currency":"USD",
	"experience":"advertiser",
	"notes":"",
	"single_ad_limitation":"0",
	"status":"Active",
	"timezone":"America/Los_Angeles",
	"type_full":"account.advertiser"
}'
Where: token_string is a string of characters returned by the GET session request at login.
When you create a single object, the response should contain a list with a single object.
Sample create response
When successful, 200 Created is returned along with the response body:
[
	{            
	"account_id": "account_id",
	"name": "API+Demo+Advertiser",
	"status":"Active",
	"account_type_id":"4",
	"currency":"USD",
	"timezone": "America/Los Angeles",
	"country_of_business_id":"us",
	"single_ad_limitation":"0",
	}
]
Where:
account_uid is the UID of the account that was created.
account_id is the ID of the account that was created.
For more details, see About IDs and UIDs.

### Update

To change the data on an object that already exists, send a PUT request to the object URI with the values you want to change:
Sample update request
curl -X PUT --header "Content-Type: application/json" http://openx_server_name/ox/4.0/account/account_uid \
--cookie "openx3_access_token=token_string" \
  --data='{
    "status":"Inactive",
  }'
Where: account_uid is the UID of the account to be updated. Alternatively, you can send the account_id.
Sample update response
The response body includes all of the object's fields:
[
	{
	"uid" : "account_uid",
	"name": "API+Demo+Advertiser",
	"status":"Inactive",
	"account_id":"account_id",
	"account_type_id":"4",
	"currency_id":"1",
	"timezone_id":"5",
	"country_of_business_id":"us",
	"single_ad_limitation":"0",
	"primary_contact_id":null,
	"billing_contact_id":null,
	"external_id":null
	"notes":null
	"uplift":null
	"requests":null
	"fill_rate":null
	"buyer_breakout":null
	"clicks":null
	"total_conversions":null
	}
]

###Delete

To delete an object, send a DELETE request to its URI:
Sample delete request
curl -X DELETE --header "Content-Type: application/json" http://openx_server_name/ox/4.0/account \
			--cookie "openx3_access_token=token_string" \
		--data='["account_uid_1", "account_uid_2"]'
Where:
account_uid_n is the UID of the account to be deleted. Alternatively, you can send account IDs.
Sample delete response
[
	{
	"account_uid_1": true,
	"account_uid_2": true
	}
]

### Read

To get the field values for a specific object:
Sample read request
curl -X GET http://openx_server_name/ox/4.0/account/account_uid--cookie "openx3_access_token=token_string"
Where: account_uid_n is the UID of the account to be read.
Sample read response
[
	{
	"uid" : "account_uid"
	"name": "API+Demo+Advertiser",
	"status":"Inactive",
	"account_id":"account_id",
	"account_type_id":"4",
	"currency_id":"1",
	"timezone_id":"5",
	"country_of_business_id":"us",
	"single_ad_limitation":"0",
	"primary_contact_id":null,
	"billing_contact_id":null,
	"external_id":null
	"notes":null
	"uplift":null
	"requests":null
	"fill_rate":null
	"buyer_breakout":null
	"clicks":null
	"total_conversions":null
	}
]
Sample list request
To list all the objects of the specified type:
curl -X GET http://openx_server_name/ox/4.0/account --cookie "openx3_access_token=token_string"

#### Batch Operations

Batch operations allow you to create, update, or delete multiple objects in one call.
Sample batch create
curl -X POST --header "Content-Type: application/json" openx_server_name/ox/4.0/account \
--cookie "openx3_access_token=token_string\
--data='[
               {    "account_uid": "[Code]parent_account_uid
                     parent_account_id",
                    "account_id": "parent_account_uid",
                    "name": "Batch Account 1",
                    "type_full": "account.publisher",
                    "currency": "USD",
                    "experience": "publisher"
                },
                {
                    "account_uid": "parent_account_idcurl -X POST --header "Content-Type: application/json" http://",
                    "account_id": "",
                    "name": "Batch Account 2",
                    "type_full": "account.advertiser",
                    "currency": "USD",
                    "experience": "advertiser"
                }
 ]'
Sample batch create response
When the creation is successful, the HTTP response includes 200 Created and a response body such as the following example:
[
{
    "account_uid": "
  parent_account_uid
  instance_uid",
     "acl_override": {},
     "compiled_acl": "__IGNORE__",
     "country_of_business": "us",
     "created_date": "2015-01-01 00:00:00",
     "currency": "USD",
     "currency_id": "1",
     "deleted": "0",
     "experience": "publisher",
     "external_id": null,
     "hidden": "0",
     "id": "__IGNORE__",
     "instance_uid": "parent_account_id",
     "market": null,
     "market_active": "0",
     "modified_date": "2015-01-01 00:00:00",
     "name": "Batch Account 1",
     "notes": "",
     "parent_name": "Parent Account Name",
     "payment_info": null,
     "status": "Active",
     "timezone": "UTC",
     "timezone_id": "1",
     "type": "account",
     "type_full": "account.publisher",
     "uid": "__IGNORE__",
     "v": "3"
 },
 {
     "account_id": "parent_account_uid",
     "account_uid": "instance_uid[
     "acl_override": {},
     "compiled_acl": "__IGNORE__",
     "country_of_business": "us",
     "created_date": "2015-01-01 00:00:00",
     "currency": "USD",
     "currency_id": "1",
     "deleted": "0",
     "exchange": null,
     "experience": "advertiser", 
     "external_id": null,
     "has_exchange": false,
     "has_ssrtb": false,
     "hidden": "0", 
     "id": "__IGNORE__",
     "instance_uid": "",
     "modified_date": "2015-01-01 00:00:00",
     "name": "Batch Account 2",
     "notes": "",
     "parent_name": "Parent Account Name",
     "single_ad_limitation": "0",
     "status": "Active",
     "timezone": "UTC",
     "timezone_id": "1",
     "type": "account",
     "type_full": "account.advertiser",
     "uid": "__IGNORE__",
     "v": "3"
 }
]
Sample batch delete
curl -X DELETE --header "Content-Type: application/json" http://openx_server_name/ox/4.0/account \
--cookie "openx3_access_token=token_string \
  --data='["account_uid_1", account_uid_2", "account_uid_3", "account_uid_n]'
  
#### Available Fields

You can make an available_fields request to determine an object's fields and values. Some available_fields requests require URI parameters, but if you do not include them the error response will indicate what is needed. For example, if you call GET /account/available_fields, you will receive the following error response:
Sample available_fields error response
{
  "attribute": "type_full", 
  "choices": [
    "account.agency", 
    "account.advertiser", 
    "account.network", 
    "account.publisher"
  ], 
  "field": {}, 
  "http_status": 400, 
  "message": "Field type_full value must be one of \"account.agency\", \"account.advertiser\", \"account.network\" or \"account.publisher\" (\"None\" not allowed)", 
  "type": "Value Error", 
  "value": null
}          
This response points out that you need to include a type_full request parameter in your call to the account object. For example, if you append ?type_full=account.publisher to your request, the Ad Server will provide information about all the account fields for an account of type account.publisher.
Sample available_fields request specifying a type_full value:
openx_server_name/ox/4.0/account/available_fields?type_full=account.publisher --cookie "openx3_access_token=curl -X GET http://token_string"
Sample available_fields response:
{
  "account_id": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "account_uid": {
    "has_dependencies": false, 
    "readonly": false, 
    "required": true, 
    "type": "account_uid", 
    "url": "/options/account_options"
  }, 
  "acl": {
    "acl": "acl", 
    "has_dependencies": false, 
    "readonly": false, 
    "required": false, 
    "type": "flags"
  }, 
  "acl_override": {
    "acl": "publisher.acl_override", 
    "available_fields": {
      "publisher.reporting.buyer_breakout.read": {
        "acl": "publisher.acl_override", 
        "default": false, 
        "has_dependencies": false, 
        "readonly": false, 
        "required": true, 
        "type": "bool"
      }, 
      "publisher.reporting.clicks.read": {
        "acl": "publisher.acl_override", 
        "default": false, 
        "has_dependencies": false, 
        "readonly": false, 
        "required": true, 
        "type": "bool"
      }, 
      "publisher.reporting.fill_rate.read": {
        "acl": "publisher.acl_override", 
        "default": true, 
        "has_dependencies": false, 
        "readonly": false, 
        "required": true, 
        "type": "bool"
      }, 
      "publisher.reporting.requests.read": {
        "acl": "publisher.acl_override", 
        "default": true, 
        "has_dependencies": false, 
        "readonly": false, 
        "required": true, 
        "type": "bool"
      }, 
      "publisher.reporting.total_conversions.read": {
        "acl": "publisher.acl_override", 
        "default": false, 
        "has_dependencies": false, 
        "readonly": false, 
        "required": true, 
        "type": "bool"
      }, 
      "publisher.reporting.total_impressions.read": {
        "acl": "publisher.acl_override", 
        "default": false, 
        "has_dependencies": false, 
        "readonly": false, 
        "required": true, 
        "type": "bool"
      }, 
      "publisher.reporting.uplift.read": {
        "acl": "publisher.acl_override", 
        "default": false, 
        "has_dependencies": false, 
        "readonly": false, 
        "required": true, 
        "type": "bool"
      }
    }, 
    "default": {
      "publisher.reporting.buyer_breakout.read": false, 
      "publisher.reporting.clicks.read": false, 
      "publisher.reporting.fill_rate.read": true, 
      "publisher.reporting.requests.read": true, 
      "publisher.reporting.total_conversions.read": false, 
      "publisher.reporting.total_impressions.read": false, 
      "publisher.reporting.uplift.read": false
    }, 
    "has_dependencies": false, 
    "readonly": false, 
    "required": false, 
    "type": "object"
  }, 
  "country_of_business": {
    "default": "us", 
    "has_dependencies": false, 
    "maxlen": 255, 
    "readonly": false, 
    "required": true, 
    "type": "varchar", 
    "url": "/options/country_options"
  }, 
  "created_date": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "datetime"
  }, 
  "currency": {
    "has_dependencies": false, 
    "maxlen": 255, 
    "readonly": false, 
    "required": true, 
    "type": "varchar", 
    "url": "/options/currency_options"
  }, 
  "currency_id": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "deleted": {
    "auto": true, 
    "default": "0", 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "experience": {
    "has_dependencies": false, 
    "maxlen": 64, 
    "readonly": false, 
    "required": true, 
    "type": "varchar", 
    "url": "/options/publisher_experience_options"
  }, 
  "external_id": {
    "has_dependencies": false, 
    "maxlen": 255, 
    "readonly": false, 
    "required": false, 
    "type": "varchar"
  }, 
  "hidden": {
    "acl": "hidden", 
    "default": "0", 
    "has_dependencies": false, 
    "readonly": false, 
    "required": false, 
    "type": "int"
  }, 
  "id": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "instance_uid": {
    "auto": true, 
    "has_dependencies": false, 
    "maxlen": 255, 
    "readonly": true, 
    "required": false, 
    "type": "varchar"
  }, 
  "market": {
    "acl": "instance.market_active", 
    "available_fields": {
      "allow_unbranded_buyers": {
        "default": "1", 
        "has_dependencies": false, 
        "readonly": false, 
        "required": false, 
        "type": "int"
      }, 
      "blocked_adcategories": {
        "has_dependencies": false, 
        "items": {
          "has_dependencies": false, 
          "maxlen": 255, 
          "readonly": false, 
          "required": true, 
          "type": "varchar", 
          "url": "/options/ad_category_options"
        }, 
        "readonly": false, 
        "required": false, 
        "type": "array"
      }, 
      "blocked_contentattributes": {
        "has_dependencies": false, 
        "items": {
          "has_dependencies": false, 
          "maxlen": 255, 
          "readonly": false, 
          "required": true, 
          "type": "varchar", 
          "url": "/options/content_attribute_options"
        }, 
        "readonly": false, 
        "required": false, 
        "type": "array"
      }, 
      "blocked_creativetypes": {
        "has_dependencies": false, 
        "items": {
          "has_dependencies": false, 
          "maxlen": 255, 
          "readonly": false, 
          "required": true, 
          "type": "varchar", 
          "url": "/options/creative_type_options"
        }, 
        "readonly": false, 
        "required": false, 
        "type": "array"
      }, 
      "blocked_languages": {
        "has_dependencies": false, 
        "items": {
          "has_dependencies": false, 
          "maxlen": 255, 
          "readonly": false, 
          "required": true, 
          "type": "varchar", 
          "url": "/options/language_options"
        }, 
        "readonly": false, 
        "required": false, 
        "type": "array"
      }, 
      "brand_labels": {
        "available_fields": {
          "label_ids": {
            "has_dependencies": false, 
            "items": {
              "has_dependencies": false, 
              "maxlen": 255, 
              "readonly": false, 
              "required": false, 
              "type": "varchar", 
              "url": "/options/market_brand_group_options"
            }, 
            "readonly": false, 
            "required": false, 
            "type": "array"
          }, 
          "op": {
            "default": "allow_all", 
            "has_dependencies": false, 
            "maxlen": 255, 
            "readonly": false, 
            "required": true, 
            "type": "varchar", 
            "url": "/options/market_filter_region_options"
          }
        }, 
        "has_dependencies": false, 
        "readonly": false, 
        "required": false, 
        "type": "object"
      }, 
      "brands": {
        "available_fields": {
          "ids": {
            "has_dependencies": false, 
            "items": {
              "has_dependencies": false, 
              "readonly": false, 
              "required": false, 
              "type": "int"
            }, 
            "readonly": false, 
            "required": false, 
            "type": "array"
          }, 
          "op": {
            "default": "allow_all", 
            "has_dependencies": false, 
            "maxlen": 255, 
            "readonly": false, 
            "required": true, 
            "type": "varchar", 
            "url": "/options/market_filter_region_options"
          }
        }, 
        "has_dependencies": false, 
        "readonly": false, 
        "required": false, 
        "type": "object"
      }, 
      "currency": {
        "default": "USD", 
        "has_dependencies": false, 
        "maxlen": 255, 
        "readonly": false, 
        "required": false, 
        "type": "varchar", 
        "url": "/options/currency_options"
      }, 
      "domains": {
        "has_dependencies": false, 
        "items": {
          "has_dependencies": false, 
          "maxlen": 255, 
          "readonly": false, 
          "required": true, 
          "type": "varchar"
        }, 
        "readonly": false, 
        "required": false, 
        "type": "array"
      }, 
      "filters": {
        "has_dependencies": false, 
        "items": {
          "available_fields": {
            "ids": {
              "has_dependencies": false, 
              "items": {
                "has_dependencies": false, 
                "readonly": false, 
                "required": true, 
                "type": "int"
              }, 
              "readonly": false, 
              "required": false, 
              "type": "array"
            }, 
            "op": {
              "default": "allow_all", 
              "has_dependencies": false, 
              "maxlen": 255, 
              "readonly": false, 
              "required": true, 
              "type": "varchar", 
              "url": "/options/market_filter_region_options"
            }, 
            "region": {
              "has_dependencies": false, 
              "maxlen": 255, 
              "readonly": false, 
              "required": true, 
              "type": "varchar", 
              "url": "/options/market_operators"
            }
          }, 
          "has_dependencies": false, 
          "readonly": false, 
          "required": false, 
          "type": "object"
        }, 
        "readonly": false, 
        "required": false, 
        "type": "array"
      }
    }, 
    "has_dependencies": false, 
    "readonly": false, 
    "required": false, 
    "type": "object"
  }, 
  "market_active": {
    "auto": true, 
    "default": "0", 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "market_currency_id": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "modified_date": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "datetime"
  }, 
  "name": {
    "has_dependencies": false, 
    "maxlen": 255, 
    "readonly": false, 
    "required": true, 
    "type": "varchar"
  }, 
  "notes": {
    "has_dependencies": false, 
    "maxlen": 25000, 
    "readonly": false, 
    "required": false, 
    "type": "varchar"
  }, 
  "payment_info": {
    "acl": "publisher.payment_info", 
    "available_fields": {
      "address_1": {
        "has_dependencies": false, 
        "maxlen": 255, 
        "readonly": false, 
        "required": true, 
        "type": "varchar"
      }, 
      "address_2": {
        "has_dependencies": false, 
        "maxlen": 255, 
        "readonly": false, 
        "required": false, 
        "type": "varchar"
      }, 
      "city": {
        "has_dependencies": false, 
        "maxlen": 255, 
        "readonly": false, 
        "required": true, 
        "type": "varchar"
      }, 
      "company_name": {
        "has_dependencies": false, 
        "maxlen": 255, 
        "readonly": false, 
        "required": false, 
        "type": "varchar"
      }, 
      "country": {
        "has_dependencies": false, 
        "maxlen": 255, 
        "readonly": false, 
        "required": true, 
        "type": "varchar", 
        "url": "/options/country_options"
      }, 
      "first_name": {
        "has_dependencies": false, 
        "maxlen": 255, 
        "readonly": false, 
        "required": true, 
        "type": "varchar"
      }, 
      "last_name": {
        "has_dependencies": false, 
        "maxlen": 255, 
        "readonly": false, 
        "required": true, 
        "type": "varchar"
      }, 
      "phone": {
        "has_dependencies": false, 
        "maxlen": 255, 
        "readonly": false, 
        "required": true, 
        "type": "varchar"
      }, 
      "state": {
        "has_dependencies": false, 
        "maxlen": 255, 
        "readonly": false, 
        "required": true, 
        "type": "varchar"
      }, 
      "zip": {
        "has_dependencies": false, 
        "maxlen": 255, 
        "readonly": false, 
        "required": true, 
        "type": "varchar"
      }
    }, 
    "has_dependencies": false, 
    "readonly": false, 
    "required": false, 
    "type": "object"
  }, 
  "revshare": {
    "acl": [
      "publisher.revshare", 
      "instance.rev_share"
    ], 
    "available_fields": {
      "cost_model": {
        "default": "variable", 
        "has_dependencies": false, 
        "maxlen": 255, 
        "options": [
          "variable", 
          "fixed"
        ], 
        "readonly": false, 
        "required": true, 
        "type": "varchar"
      }, 
      "deal_type": {
        "default": "FIXED_WITH_FILL", 
        "has_dependencies": false, 
        "maxlen": 255, 
        "readonly": false, 
        "required": true, 
        "type": "varchar", 
        "url": "/options/deal_type_options"
      }, 
      "deal_type_id": {
        "auto": true, 
        "default": "1", 
        "has_dependencies": false, 
        "readonly": true, 
        "required": false, 
        "type": "int"
      }, 
      "guaranteed_cpm": {
        "default": "0.0000", 
        "has_dependencies": false, 
        "maximum": "10000.0000", 
        "readonly": false, 
        "required": true, 
        "type": "decimal(9,4)"
      }, 
      "house_fallback": {
        "default": "0", 
        "has_dependencies": false, 
        "readonly": false, 
        "required": true, 
        "type": "int"
      }, 
      "house_payable": {
        "default": "0", 
        "has_dependencies": false, 
        "readonly": false, 
        "required": true, 
        "type": "int"
      }, 
      "rev_split": {
        "default": "100.00", 
        "has_dependencies": false, 
        "maximum": "100.00", 
        "readonly": false, 
        "required": true, 
        "type": "decimal(5,2)"
      }
    }, 
    "has_dependencies": false, 
    "readonly": false, 
    "required": false, 
    "type": "object"
  }, 
  "status": {
    "acl": "publisher.status", 
    "default": "Active", 
    "has_dependencies": false, 
    "maxlen": 12, 
    "readonly": false, 
    "required": true, 
    "type": "varchar", 
    "url": "/options/account_status_options"
  }, 
  "timezone": {
    "default": "UTC", 
    "has_dependencies": false, 
    "maxlen": 255, 
    "readonly": false, 
    "required": true, 
    "type": "varchar", 
    "url": "/options/timezone_options"
  }, 
  "timezone_id": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "type": {
    "auto": true, 
    "default": "account", 
    "has_dependencies": false, 
    "maxlen": 255, 
    "readonly": true, 
    "required": false, 
    "type": "varchar", 
    "value": "account"
  }, 
  "type_full": {
    "has_dependencies": false, 
    "maxlen": 64, 
    "readonly": false, 
    "required": true, 
    "type": "varchar", 
    "value": "account.publisher"
  }, 
  "uid": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "uid"
  }, 
  "v": {
    "auto": true, 
    "default": "3", 
    "has_dependencies": false, 
    "options": [], 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }
}
Where:
acl. A field used for permissions that you can typically ignore.
url. A path that you can follow to the base URI to return a list of options. The corresponding field will only accept values that are found in the response to the specified options call.
Tip: You can also include action=create or action=update when calling the API for available and required fields of an object. For example, the set of required fields for creating a user is different than for updating a user, such as the user.email field, which is required when creating but not when updating.

#### Pagination

The size of the data displayed in a response is limited by the following request parameters:
limit. The maximum number of items to be returned on a single request. Its default value is 10.
offset. The number of the first item to display for the current request, where the offset for first item on the first page is 0. Its default value is 0.
If there is too much information to display in a single page, the response body will include "has_more": true. You can access the additional values by modifying the offset and limit paging values.
Sample list request including pagination parameters
For example, to list account records 50 through 75, specify an offset of 50 and a limit of 25:
curl -X GET http://openx_server_name/ox/4.0/accountoffset=50&limit=25 --cookie "openx3_access_token=token_string"

#### URI parameters

The URIs described in the API reference represent the supported request syntax, to which you can add certain supported request parameters. You can include request parameters in your API calls as query string arguments; you can append the first parameter after a question mark (?) and additional parameters separated by ampersands (&) in any order or combination.
The OpenX API supports the various parameters, such as the following:
action. When calling the API for available and required fields for an object, set this parameter to action=create or action=update to specify the action for which you want to retrieve values.
For example, the set of required fields for creating a user (/user/available_fields?action=create) is different than the set of required fields for updating a user (/user/available_fields?action=update), such as the user.email field, which is required for creating a user but not for updating a user.
pretty. (For debugging purposes only) Display the JSON response into a more human readable format, encapsulated by HTML <pre> tags.
Important: Routine use of the pretty parameter negatively impacts performance. Do not use it in your production calls.
Pagination parameters. See pagination.
type_full. Some available_fields calls require this parameter. If so, the error response will list the choices for type_full if you do not specify an attribute. For example, when calling /ad/available_fields, you can specify the type_full=ad.image attribute. This parameter is also used for list operations.
Depending on the resource you are calling, many additional parameters may be available. Making calls without required parameters results in an error response, which typically indicates what was missing from the request. You can call available_fields for an object to see fields listed as required: true.
About IDs and UIDs
OpenX API objects have object_name_id ("ID") and object_name_uid ("UID") fields. For example, you can include account_id=string or account_uid=string values in some API calls.
IDs were supported in API v3 and OpenX continues to maintain IDs. UIDs were introduced with API v4 for internal reasons and they are typically interchangeable with IDs. You can usually specify both IDs and UIDs in API requests that include a full JSON object (for an example, see Create).
You do not have to specify UIDs unless they are listed as required by the available_fields create or update response for the object. For example, to create an order, the account_uid is required. If you specify the account_id instead, the call will not work. As a best practice, you should use IDs whenever possible; however, if IDs are not available or do not work, use UIDs in their place.

### Response codes and error handling

Most create and update operations require particular parameters. The API responds with an HTTP response code of 2xx when the operation is successful or an error code of 4xx, a text description of the error, and JSON content listing the incorrect or missing fields, along with the reason they were rejected. For example, if you POST an order without any parameters, the API responds with error code 400, and JSON in the response body indicating the required fields.
The following table describes the possible error types:
API error types
Error	Description
Dependency	Validation of the field failed due to restrictions based on the value of another field. The related field is specified in the field dictionary, and the field-specific error is detailed in the dependency dictionary.
Field Permission	The current user does not have permission to modify the given field.
Not Unique	
The field requires a unique value. For example, the value might need to be unique:
Across the entire instance
Within the object's parent account
Within the parent object
Within the object (in the case of an array field)
Object Not Found	The object matching the given UID either does not exist, or was deleted.
Object One To One	The object's parent must have exactly one child.
Object Type	The object matching the given UID is not of the expected type.
Object Validation	
Some of the input fields did not pass validation. Each field validation error includes:
Field definition in the field dictionary
Error type in type
A human-readable error in message
Each field validation error is detailed in the field_errors dictionary, where the keys are field names and the values are errors. When available, the object's UID is provided in uid.
Permission	The current user does not have permission to perform the specified action on the specified object.
Range	The input field value is either less than the minimum or greater than the maximum allowed value.
Read Only	The field value may not be modified.
Reference	Indicates that an object is referred to by another object and cannot be deleted. The referring objects are listed in referencing_objects.
Require	The field requires a non-null value.
Size	The input field value is too large or too small. For string fields, this is the allowed number of characters. For array fields, this is the allowed number of items in the array.
Type	The input field value is of the wrong type, and cannot be coerced into the correct type. For example, if a date/time field cannot parse a value into a native date, this error will be raised.
Unknown Field	The input field does not exist for the object or sub-object.
Validation	
One or more errors occurred within nested data structures. Sub-field validation errors are detailed in the error response under the following dictionaries:
field_errors dictionary. Error details for sub-object fields
item_errors dictionary. Error details for array fields
Value	The allowable values for the field are constrained to a given set or must match a regular expression pattern. If the value must be one of a given set, the allowed values are specified in a request to the url in the field definition.
For information about HTTP status codes, see RFC 1945.

## Use Cases

This section describes the following common tasks using the Platform API:
Working with accounts
Working with inventory
Working with event-level feeds
Working with demand objects
Working with comments
Working with forecasts
Working with reports

### Working with accounts

Depending on your use case, the account object is used in the following way:
Advertiser accounts are containers for orders and other demand objects.
Publisher accounts are containers for sites, ad units, and other inventory objects.
Ad networks are containers for other accounts.
The account object fields can change as a result of different parameters such as the following:
action
instance configuration
object type_full
To determine the available fields and appropriate values, you should make available_fields calls and options calls as needed. The Platform API provides informative responses even when the input has errors. Building a request may take a few attempts but the error messages will guide you to the correct request format.
To create an account:
Refer to the authentication topic to obtain an openx3_access_token.
Get the available fields for an account:
Example 1. Sample available_fields request
openx_server_name/ox/4.0/account/available_fields --cookie "openx3_access_token=curl http://token_string"
Where: token_string is a string of characters returned by the GET session request at login.
Example 2. Sample available_fields response
{
    "attribute": "type_full", 
    "choices": [      "account.agency",       "account.advertiser",       "account.network",       "account.publisher"    ], 
    "field": {}, 
    "http_status": 400, 
    "message": "Field type_full value must be one of \"account.agency\", \"account.advertiser\", \"account.network\" or \"account.publisher\" (\"None\" not allowed)", 
    "type": "Value Error", 
    "value": null
}'
Accounts fields change based the different account types. The response indicates that a type_full value is required.
Modify the request by adding a type_full URI parameter:
Example 3. Sample type_full account request
openx_server_name/ox/4.0/account/available_fieldstype_full=account.network --cookie "openx3_access_token=curl http://token_string"
OpenExample 4. Sample type_full account response
{
  "account_id": {
    "auto": true, 
    "has_dependencies": false,
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "account_uid": {
    "has_dependencies": false,
    "readonly": false, 
    "required": true, 
    "type": "account_uid", 
    "url": "/options/account_options"
  }, 
  "acl_override": {
    "acl": "network.acl_override", 
    "available_fields": {}, 
    "default": {}, 
    "has_dependencies": false, 
    "readonly": false, 
    "required": false, 
    "type": "object"
  }, 
  "country_of_business": {
    "acl": "network.region_settings", 
    "default": "us", 
    "has_dependencies": false, 
    "readonly": false, 
    "required": true, 
    "type": "varchar", 
    "url": "/options/country_options"
  }, 
  "created_date": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "datetime"
  }, 
  "currency": {
    "acl": "network.region_settings", 
    "has_dependencies": false, 
    "readonly": false, 
    "required": true, 
    "type": "varchar", 
    "url": "/options/currency_options"
  }, 
  "currency_id": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "deleted": {
    "auto": true, 
    "default": "0", 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "experience": {
    "has_dependencies": false, 
    "readonly": false, 
    "required": true, 
    "type": "varchar", 
    "url": "/options/network_experience_options"
  }, 
  "external_id": {
    "acl": "network.addl_info", 
    "has_dependencies": false, 
    "readonly": false, 
    "required": false, 
    "type": "varchar"
  }, 
  "hidden": {
    "acl": "network.hidden", 
    "default": "0", 
    "has_dependencies": false, 
    "readonly": false, 
    "required": false, 
    "type": "int"
  }, 
  "id": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "instance_uid": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "uid"
  }, 
  "modified_date": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "datetime"
  }, 
  "name": {
    "acl": "network.name", 
    "has_dependencies": false, 
    "readonly": false, 
    "required": true, 
    "type": "varchar"
  }, 
  "notes": {
    "acl": "network.addl_info", 
    "has_dependencies": false, 
    "readonly": false, 
    "required": false, 
    "type": "varchar"
  }, 
  "status": {
    "acl": "network.status", 
    "default": "Active", 
    "has_dependencies": false, 
    "readonly": false, 
    "required": true, 
    "type": "varchar", 
    "url": "/options/account_status_options"
  }, 
  "timezone": {
    "default": "UTC", 
    "has_dependencies": false, 
    "readonly": false, 
    "required": true, 
    "type": "varchar", 
    "url": "/options/timezone_options"
  }, 
  "timezone_id": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "type": {
    "auto": true, 
    "default": "account", 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "varchar", 
    "value": "account"
  }, 
  "type_full": {
    "has_dependencies": false, 
    "readonly": false, 
    "required": true, 
    "type": "varchar", 
    "value": "account.network"
  }, 
  "uid": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "uid"
  }, 
  "v": {
    "auto": true, 
    "default": "3", 
    "has_dependencies": false, 
    "options": [], 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }
}
Within the response, several fields are marked "required": true, such as account_uid. Of the required fields, most include a url field, such as account_uid's "url": "/options/account_options".
To see a list of available options, make the options calls indicated by the URLs. The following call shows an options call for only one of the required fields:
Example 5. Sample experience options request
openx_server_name/ox/4.0/options/network_experience_options --cookie "openx3_access_token=curl http://token_string"
Example 6. Sample experience options response
[  {    "id": "network.display",     "name": "Display Network"  },   {    "id": "network.mobile",     "name": "Mobile App Developer"  }]
By default, the acceptable values are the id values, such as network.display and network.mobile in the above sample. Some fields reference other API objects, such as the account_uid field. For such fields, their options call will return a list very similar to list calls (such as GET /account).
Build a request to create an account using the following five fields along with type_full:
Example 7. Sample create account request with experience and status typos
openx_server_name/ox/4.0/account --cookie "openx3_access_token=curl http://token_string" -X POST
      --header "Content-Type:application/json" --data '{"account_uid":
      "200571f0-accf-fff1-8123-0c9a66", "currency": "USD", "experience":
      "network.displayy", "name": "My Network", "status": "Activee", "type_full":
      "account.network", "timezone": "America/Los_Angeles"}'
The API returns multiple errors and a field errors dictionary with experience and status keys indicating the values passed in are invalid.
OpenExample 8. Sample error response
[  {    "field_errors": {      "experience": {        "attribute": "experience",         "choices": [          "network.mobile",           "network.display"        ], 
        "field": {
          "has_dependencies": false, 
          "readonly": false, 
          "required": true, 
          "type": "varchar", 
          "url": "/options/network_experience_options"
        }, 
        "http_status": 400, 
        "message": "Field experience value must be one of \"network.mobile\" or \"network.display\" (\"network.displayy\" not allowed)", 
        "type": "Value Error", 
        "value": "network.displayy"
      }, 
      "status": {
        "attribute": "status", 
        "choices": [          "Active",           "Inactive"        ], 
        "field": {
          "acl": "network.status", 
          "default": "Active", 
          "has_dependencies": false, 
          "readonly": false, 
          "required": true, 
          "type": "varchar", 
          "url": "/options/account_status_options"
        }, 
        "http_status": 400, 
        "message": "Field status value must be one of \"Active\" or \"Inactive\" (\"Activee\" not allowed)", 
        "type": "Value Error", 
        "value": "Activee"
      }
    }, 
    "http_status": 400, 
    "message": "Validation of new networkaccount failed:\n  status: Field status value must be one of \"Active\" or \"Inactive\" (\"Activee\" not allowed)\n  experience: Field experience value must be one of \"network.mobile\" or \"network.display\" (\"network.displayy\" not allowed)", 
    "object": "networkaccount", 
    "type": "Object Validation Error", 
    "uid": null
  }
]
Fix the mistakes and try again:
Example 9. Sample create account request
openx_server_name/ox/4.0/account --cookie "openx3_access_token=curl http://token_string" -X POST --header "Content-Type:application/json" --data
      '{"account_uid": "200571f0-accf-fff1-8123-0c9a66", "currency": "USD", "experience":
      "network.display", "name": "My Network", "status": "Active", "type_full":
      "account.network", "timezone": "America/Los_Angeles"}'
The API returns the full object in the create response. Many fields that were not passed in are filled in with defaults, which depend on things such as instance settings, parent account, and other fields.
Example 10. Sample create response
[  {    "account_id": "537227760",     "account_uid": "200571f0-accf-fff1-8123-0c9a66",     "acl_override": {},     "country_of_business": "us",     "created_date": "2013-10-04 05:52:57",     "currency": "USD",     "currency_id": "1",     "deleted": "0",     "experience": "network.display",     "external_id": null,     "hidden": "0",     "id": "537241695",     "instance_uid": "[Code]instance_uid", 
    "modified_date": "2013-10-04 05:52:57", 
    "name": "My Network", 
    "notes": "", 
    "revision": 1, 
    "status": "Active", 
    "timezone": "America/Los_Angeles", 
    "timezone_id": "8", 
    "type": "account", 
    "type_full": "account.network", 
    "uid": "2005a85f-accf-fff1-8123-0c9a66", 
    "v": "3"
  }
]
Make a read call using the UID field of the newly created account:
Sample read account request
curl http://openx_server_name/ox/4.0/account/2005a85f-accf-fff1-8123-0c9a66 --cookie "openx3_access_token=token_string"
The account appears in the response and also for list calls:
Sample list accounts request
curl http://openx_server_name/ox/4.0/account --cookie "openx3_access_token=token_string"
See also
Accounts in the OpenX help.

### Working with inventory

This section describes how to use the Platform API to manage inventory objects.
To retrieve the lists of inventory objects to which the current user has access:
List all sites:
curl http://openx_server_name/ox/4.0/site --cookie "openx3_access_token=token_string"
List all site sections:
curl http://openx_server_name/ox/4.0/sitesection --cookie "openx3_access_token=token_string"
List all ad units:
curl http://openx_server_name/ox/4.0/adunit --cookie "openx3_access_token=token_string"
List all packages:
curl http://openx_server_name/ox/4.0/package --cookie "openx3_access_token=token_string"
List all audience segments:
curl http://openx_server_name/ox/4.0/audiencesegment --cookie "openx3_access_token=token_string"
To retrieve information about specific inventory objects, use the UIDs from the list responses for the following calls:
Read the site with the specified UID:
curl http://openx_server_name/ox/4.0/site/2000002b-e000-fff1-8123-0c9a66 --cookie "openx3_access_token=token_string"
Read the site section with the specified ID:
curl http://openx_server_name/ox/4.0/sitesection/536870925 --cookie "openx3_access_token=token_string"
Read the ad unit with the specified ID:
curl http://openx_server_name/ox/4.0/adunit/536871402 --cookie "openx3_access_token=token_string"
Read the package with the specified ID:
curl http://openx_server_name/ox/4.0/package/536870936 --cookie "openx3_access_token=token_string"
Read the audience segment with the specified ID:
curl http://openx_server_name/ox/4.0/audiencesegment/536871535 --cookie "openx3_access_token=token_string"
See also
Inventory in the OpenX help.

### Create a site

To create a site:
See working with accounts to determine the account for which you want to create a site.
Retrieve the list of available fields for creating a site.
curl http://openx_server_name/ox/4.0/site/available_fields?account_uid=publisher_account_UID
This returns the list of fields that you can set, and those that are required, for creating a site.
ClosedSample
Create the site object, passing in, at a minimum, the required parameters, which include:
The UID of the publisher account (account_uid)
The name for the new site (name)
The status for the site (status), which is described by /options/status_options_common
The URL for the site (url)
For example, create a site for the web delivery medium.
curl http://openx_server_name/ox/4.0/site --cookie "openx3_access_token=token_string" -X POST
      --header "Content-Type:application/json" --data '{"account_uid":
      "200571f0-accf-fff1-8123-0c9a66", "name": "API_demo", "status":
      "Active", "url": "http://www.example.com", "content_type_id": "99",
      "delivery_medium_id": "2"}'
The API creates the site and returns the ID for the new site object.
See also
Sites in the OpenX help.

### Creating an ad unit

To create an ad unit:
Retrieve the list of sites to determine the one to create the ad unit for.
Retrieve the list of available fields for creating an ad unit:
curl http://openx_server_name/ox/4.0/adunit/available_fields --cookie "openx3_access_token=token_string"
The following error response indicates that the type_full attribute is needed:
{
  "attribute": "type_full", 
  "choices": [
	"adunit.mobile",
	"adunit.videocompanion",
	"adunit.linearvideo",
	"adunit.nonlinearvideo",
	"adunit.web",
	"adunit.email"  ], 
	"field": {}, 
	"http_status": 400, 
	"message": "Field type_full value must be one of \"adunit.mobile\", \"adunit.videocompanion\", \"adunit.linearvideo\", \"adunit.nonlinearvideo\", \"adunit.web\" or \"adunit.email\" (\"None\" not allowed)", 
	"type": "Value Error", 
	"value": null
}
Specify the desired type_full value, such as adunit.web in the following sample:
curl http://openx_server_name/ox/4.0/adunit/available_fields?type_full=adunit.web --cookie "openx3_access_token=token_string"
Note: : When using adunit.mobile for the type_full value, you must also pass in the site_uid. For example:
curl http://openx_server_name/ox/4.0/adunit/available_fields?type_full=adunit.mobile&site_uid=site_uid --cookie "openx3_access_token=token_string"
This returns the list of fields that you can set, and those that are required, for creating a web ad unit:
{
  "account_id": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "account_uid": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "account_uid"
  }, 
  "alt_sizes": {
    "acl": "adunit.addl_sizes", 
    "has_dependencies": false, 
    "readonly": false, 
    "required": false, 
    "type": "varchar", 
    "url": "/options/web_size_options"
  },...
}
Create the ad unit object by passing in, at a minimum, the required parameters:
The ID for the site that the ad unit belongs to (site_uid)
The name for the ad unit (name)
The status for the ad unit (status), which is either Active or Inactive
The ID for the delivery medium that the new ad unit is intended to run on (delivery_medium_id)
The ID for the type of ad tag to use to request ads for this ad unit, such ad image or JavaScript (tag_type_id)
The type of ad unit (type_full) such as adunit.web
For example, create a web ad unit:
curl http://openx_server_name/ox/4.0/adunit --cookie "openx3_access_token=token_string" -X POST --header "Content-Type:application/json" --data '{"site_uid":
 "536870955", "name": "desired_adunit_name", "status": "Active", "delivery_medium_id": "2", "tag_type_id": "1", "content_type_id": "99", "type_full": "adunit.web", "size_id": "3"}'
The API creates the ad unit and returns the ID for the new ad unit object.

## Working with event-level feeds

This section describes how to use the eventfeed service to retrieve aggregated data. For more details, see event-level feeds.
Note: Accessing event-level feeds requires a specific OpenX configuration. Please contact your OpenX account manager for configuration and pricing details.
Event feed API topics include:
Listing the available files for an event feed
Sample response
Downloading the event feed file
Event feed file format
OpenResponse listing available files for an event feed
{
		  "dataset": [
		    {
		      "@dataSize": "352", 
		      "@id": "3169793", 
		      "@recordCount": "0", 
		      "@revision": "177", 
		      "@status": "READY", 
		      "dateCreated": "2015-10-26 08:08:07 UTC", 
		      "endTimestamp": "2015-10-22 21:01:00 UTC", 
		      "feed": {
		        "@id": "496", 
		        "@name": "d8568240-c334-11e2-8b8b-0800200c9a66_ox_click_log_minutely"
		      }, 
		      "parts": {
		        "part": {
		          "@compressionType": "GZIP", 
		          "@dataSize": "352", 
		          "@digest": "498b8331dcfa44c55d5980a6ead8a218", 
		          "@gzipped": "true", 
		          "@id": "3257451", 
		          "@index": "1", 
		          "@locator": "http://openx_server_name/ox/4.0/eventfeed/fetch?file=/d8568240-c334-11e2-8b8b-0800200c9a66/
		ox_click_log_minutely/2015-10/clicks_v4_2015-10-22_21-00_d8568240-c334-11e2-8b8b-0800200c9a66.txt.gz", 
		          "@recordCount": "0"
		        }
		      }, 
		      "readableInterval": "2015-10-22_21:00:00", 
		      "schema": {
		        "@locator": "http://openx_server_name/ox/4.0/eventfeed/schema?version=4&name=OX_EventLog", 
		        "@name": "OX_EventLog", 
		        "@version": "4"
		      }, 
		      "serial": "133612", 
		      "startTimestamp": "2015-10-22 21:00:00 UTC"
		    }, 
		    {
		      "@dataSize": "352", 
		      "@id": "3156608", 
		      "@recordCount": "0", 
		      "@revision": "1", 
		      "@status": "READY", 
		      "dateCreated": "2015-10-26 04:17:29 UTC", 
		      "endTimestamp": "2015-10-25 07:59:00 UTC", 
		      "feed": {
		        "@id": "496", 
		        "@name": "d8568240-c334-11e2-8b8b-0800200c9a66_ox_click_log_minutely"
		      }, 
		      "parts": {
		        "part": {
		          "@compressionType": "GZIP", 
		          "@dataSize": "352", 
		          "@digest": "498b8331dcfa44c55d5980a6ead8a218", 
		          "@gzipped": "true", 
		          "@id": "3244335", 
		          "@index": "1", 
		          "@locator": "http://openx_server_name/ox/4.0/eventfeed/fetch?file=/d8568240-c334-11e2-8b8b-0800200c9a66/
		ox_click_log_minutely/2015-10/clicks_v4_2015-10-25_07-58_d8568240-c334-11e2-8b8b-0800200c9a66.txt.gz", 
		          "@recordCount": "0"
		        }
		      }, 
		      "readableInterval": "2015-10-25_07:58:00", 
		      "schema": {
		        "@locator": "http://openx_server_name/ox/4.0/eventfeed/schema?version=4&name=OX_EventLog", 
		        "@name": "OX_EventLog", 
		        "@version": "4"
		      }, 
		      "serial": "133611", 
		      "startTimestamp": "2015-10-25 07:58:00 UTC"
		    }, 
		    {
		      "@dataSize": "352", 
		      "@id": "3156610", 
		      "@recordCount": "0", 
		      "@revision": "1", 
		      "@status": "READY", 
		      "dateCreated": "2015-10-26 04:17:29 UTC", 
		      "endTimestamp": "2015-10-25 08:00:00 UTC", 
		      "feed": {
		        "@id": "496", 
		        "@name": "d8568240-c334-11e2-8b8b-0800200c9a66_ox_click_log_minutely"
		      }, 
		      "parts": {
		        "part": {
		          "@compressionType": "GZIP", 
		          "@dataSize": "352", 
		          "@digest": "498b8331dcfa44c55d5980a6ead8a218", 
		          "@gzipped": "true", 
		          "@id": "3244340", 
		          "@index": "1", 
		          "@locator": "http://openx_server_name/ox/4.0/eventfeed/fetch?file=/d8568240-c334-11e2-8b8b-0800200c9a66/
		ox_click_log_minutely/2015-10/clicks_v4_2015-10-25_07-59_d8568240-c334-11e2-8b8b-0800200c9a66.txt.gz", 
		          "@recordCount": "0"
		        }
		      }, 
		      "readableInterval": "2015-10-25_07:59:00", 
		      "schema": {
		        "@locator": "http://openx_server_name/ox/4.0/eventfeed/schema?version=4&name=OX_EventLog", 
		        "@name": "OX_EventLog", 
		        "@version": "4"
		      }, 
		      "serial": "133611", 
		      "startTimestamp": "2015-10-25 07:59:00 UTC"
		    },
		    ...
		    
		 ]
		}
  
  ### Listing available fields for an event feed
  
  After you log in to the OpenX system that you want to access data for, you can retrieve the list of event feed files available to you per event type. For example, if you are interested in click events, retrieve the list of click event feed files. Then you can determine what files you want to download.
To retrieve the list of available data sets for a particular event type:
Build the URL and call the API, using the following format:
http://openx_server_name/ox/4.0/eventfeed?type=click&range=number&format=json&pretty
Where:
http is the protocol.
 openx_server_name is the hostname of the Platform API server.
/ox/4.0 is the base path for the API.
/eventfeed is the method, or action to perform, in this case, a request for the list of event feed files.
? indicates the start of URI parameters.
type=click represents the ad serving event feed that you want to track; in this case, click event feed. This can be either request, impression, click, or conversion, depending on your OpenX configuration.
format=json represents the format that you want to view the response in. This must be json.
pretty indicates that you want the response that OpenX returns to be formatted in an easily-readable form. This parameter is useful if you are performing a manual inspection of the event feed response (that is, if you paste the URL in your browser and click ENTER).
range=number indicates the serial number for the last feed that you looked at (located in the serial: key). The range parameter is like a bookmark that indicates where you want to continue viewing feeds. For example, use the value for the serial: key in the last feed that you viewed. Alternatively, specify the start and end parameters, but do not specify them in addition to range.
Tip: Do not use a range value of 0 because this can negatively impact the event feed.
Optional
breakdown=minutely is the default feed interval, which is not necessary to specify unless your account manager enabled a custom configuration for your account. The minutely value indicates that you want the response limited to minutely feeds only. The hourly value limits the response to hourly feeds, but it is disabled for most accounts. This field is required if you have both hourly and minutely feeds enabled. Please check with your OpenX account manager if you are unsure of what type of feed you have.
OpenX returns a JSON response similar to the one described in the sample response.
As necessary, look at event feed timestamps or serial numbers and rebuild the URL as necessary.
To avoid inconsistencies, especially when your implementation uses an automatic system to process event feeds, use the range parameter to retrieve serial numbers in the JSON response (serial: key) as your bookmarks for viewing feeds rather than the start and end parameters.
Parse the response for timestamps or serial numbers and file paths and download the event feed files that you want to access.
For more details, see eventfeed in the API reference.
 OpenResponse listing available files for an event feed
"@id": "496", 
	"@name": "d8568240-c334-11e2-8b8b-0800200c9a66_ox_click_log_minutely"
	}, 
	"parts": {
	"part": {
	"@compressionType": "GZIP", 
	"@dataSize": "352", 
	"@digest": "498b8331dcfa44c55d5980a6ead8a218", 
	"@gzipped": "true", 
	"@id": "3257451", 
	"@index": "1", 
	"@locator": "http://openx_server_name/ox/4.0/eventfeed/fetch?file=/d8568240-c334-11e2-8b8b-0800200c9a66/
	ox_click_log_minutely/2015-10/clicks_v4_2015-10-22_21-00_d8568240-c334-11e2-8b8b-0800200c9a66.txt.gz", 
	"@recordCount": "0"
	}
	}, 
	"readableInterval": "2015-10-22_21:00:00", 
	"schema": {
	"@locator": "http://openx_server_name/ox/4.0/eventfeed/schema?version=4&name=OX_EventLog", 
	"@name": "OX_EventLog", 
	"@version": "4"
	}, 
	"serial": "133612", 
	"startTimestamp": "2015-10-22 21:00:00 UTC"
	}, 
	{
	"@dataSize": "352", 
	"@id": "3156608", 
	"@recordCount": "0", 
	"@revision": "1", 
	"@status": "READY", 
	"dateCreated": "2015-10-26 04:17:29 UTC", 
	"endTimestamp": "2015-10-25 07:59:00 UTC", 
	"feed": {
	"@id": "496", 
	"@name": "d8568240-c334-11e2-8b8b-0800200c9a66_ox_click_log_minutely"
	}, 
	"parts": {
	"part": {
	"@compressionType": "GZIP", 
	"@dataSize": "352", 
	"@digest": "498b8331dcfa44c55d5980a6ead8a218", 
	"@gzipped": "true", 
	"@id": "3244335", 
	"@index": "1", 
	"@locator": "http://openx_server_name/ox/4.0/eventfeed/fetch?file=/d8568240-c334-11e2-8b8b-0800200c9a66/
	ox_click_log_minutely/2015-10/clicks_v4_2015-10-25_07-58_d8568240-c334-11e2-8b8b-0800200c9a66.txt.gz", 
	"@recordCount": "0"
	}
	}, 
	"readableInterval": "2015-10-25_07:58:00", 
	"schema": {
	"@locator": "http://openx_server_name/ox/4.0/eventfeed/schema?version=4&name=OX_EventLog", 
	"@name": "OX_EventLog", 
	"@version": "4"
	}, 
	"serial": "133611", 
	"startTimestamp": "2015-10-25 07:58:00 UTC"
	}, 
	{
	"@dataSize": "352", 
	"@id": "3156610", 
	"@recordCount": "0", 
	"@revision": "1", 
	"@status": "READY", 
	"dateCreated": "2015-10-26 04:17:29 UTC", 
	"endTimestamp": "2015-10-25 08:00:00 UTC", 
	"feed": {
	"@id": "496", 
	"@name": "d8568240-c334-11e2-8b8b-0800200c9a66_ox_click_log_minutely"
	}, 
	"parts": {
	"part": {
	"@compressionType": "GZIP", 
	"@dataSize": "352", 
	"@digest": "498b8331dcfa44c55d5980a6ead8a218", 
	"@gzipped": "true", 
	"@id": "3244340", 
	"@index": "1", 
	"@locator": "http://openx_server_name/ox/4.0/eventfeed/fetch?file=/d8568240-c334-11e2-8b8b-0800200c9a66/
	ox_click_log_minutely/2015-10/clicks_v4_2015-10-25_07-59_d8568240-c334-11e2-8b8b-0800200c9a66.txt.gz", 
	"@recordCount": "0"
	}
	}, 
	"readableInterval": "2015-10-25_07:59:00", 
	"schema": {
	"@locator": "http://openx_server_name/ox/4.0/eventfeed/schema?version=4&name=OX_EventLog", 
	"@name": "OX_EventLog", 
	"@version": "4"
	}, 
	"serial": "133611", 
	"startTimestamp": "2015-10-25 07:59:00 UTC"
	},
...
	]
}

### Downloading the event feed file

After you parse the response for the serial numbers and feed files, you can download the files with the data you want to access. Use the URL located in the @locator: key in the JSON response (see bold text in the sample response). For example, in the sample response described in the previous section, if you wanted to download the last event feed file in the response, you would use the following URL:
http://openx_server_name/ox/4.0/eventfeed/fetch?file=/d8568240-c334-11e2-8b8b-0800200c9a66/ox_click_log_minutely/2015-10/clicks_v4_2015-10-22_21-00_d8568240-c334-11e2-8b8b-0800200c9a66.txt.gz
This returns the event feed file with the data fields for the event. OpenX recommends that you validate each file by comparing the value in the @digest key with a locally executed MD5 checksum. This ensures that you have correctly downloaded a complete and valid file before moving on with further processing.

### Event feed file format

After you uncompress a GZIP feed file, you can see that the contents are UTF-8 encoded and that each file contains a header row. The feed schema indicates the field delimiter, which is a tab.
If any of the following characters occur in the source data, they are escaped with a backslash:
tab (U+0009). Becomes a literal string \t
newline (U+000A). Becomes a literal string \n
backslash (U+005C). Becomes a literal string \\

## Working with demand objects

This section describes how to use the API to list or create orders, line items, ads, and creatives.
You can:
Retrieve lists of orders, line items, and ads
Retrieve lists of deals
Create an order
Create a line item
Create an ad
Upload a creative

### Retrieving the list of demand objects

To retrieve the list of demand objects that the current user has access to, issue one of the following HTTP GET requests:
GET /order
GET /lineitem
GET /ad
For example:
To retrieve the list of orders:
curl -X GET http://openx_server_name/ox/4.0/order --cookie "openx3_access_token=token_string"
To retrieve the list of line items:
curl -X GET http://openx_server_name/ox/4.0/lineitem --cookie "openx3_access_token=token_string"
To retrieve the list of ads in alphabetical order:
curl -X GET http://openx_server_name/ox/4.0/ad --cookie "openx3_access_token=token_string

### Retrieve information about available deals

To retrieve a list of private marketplace deals that you are eligible to bid on, make the following options call:
curl -X GET http://openx_server_name/ox/4.0/options/available_deals?account_uid=account_uid --cookie "openx3_access_token=token_string"
The API returns details about your eligible deals, such as shown in the following sample:
OpenSample
[
{
	buyer: [ ],
    code: "OX-Mas-ik6ReO",
    currency: {
      currency: "USD"
    },
    end_date: null,
    floor_price: "2.00",
    id: "1610624813",
    name: "special_pmp_deal",
    seller: {
      id: 357,
      name: "seller1"
    },
    start_date: "2014-10-06 07:00:00",
    type: {
      id: "2",
      name: "Private Auction"
    }
  },
  {
    buyer: [ ],
    code: "OX-Mas-acfQvL",
    currency: {
      currency: "USD"
    },
    end_date: "2014-10-31 07:00:00",
    floor_price: "4.00",
    id: "1610625435",
    name: "Oc Deal",
    seller: {
      id: 357,
      name: "seller2"
    },
    start_date: "2014-06-11 07:00:00",
    type: {
      id: "1",
      name: "Preferred Deal"
    }
  },
  {
    buyer: [ ],
    code: "OX-GI_-4oEDz9",
    currency: {
      currency: "USD"
    },
    end_date: null,
    floor_price: "11.00",
    id: "1610633711",
    name: "GI_Deal",
    seller: {
      id: 357,
      name: "seller3"
    },
    start_date: "2014-09-23 07:00:00",
    type: {
      id: "3",
      name: "Within Open Auction"
    }
  },
...
  }
]
If you are not eligible for any deals, the API returns an empty array:
[ ]

### Retrieve information about a demand object

Query the API for details about a specific demand object:
GET /order/<order_uid>
GET /lineitem/<lineitem_uid>
GET /ad/<ad_uid>
For example, query the API for details about order UID 20003303-c001-fff1-8123-0c9a66:
curl -X GET http://openx_server_name/ox/4.0/order/20003303-c001-fff1-8123-0c9a66 --cookie "openx3_access_token=token_string"
The API returns the attributes and value details for the specific order:
[
	{
	"account_id": "537242118", 
	"account_uid": "2005aa06-accf-fff1-8123-0c9a66", 
	"budget": null, 
	"click_through_window": "86400", 
	"created_date": "2013-10-10 20:38:49", 
	"deleted": "0", 
	"end_date": null, 
	"external_id": "", 
	"id": "536883971", 
	"instance_uid": "f2d88c60-cd72-11e2-8b8b-0800200c9a66", 
	"modified_date": "2013-10-10 20:38:49", 
	"name": " Kcarey_Revshare_SubNetwork1_ExchAdv1_Order1", 
	"notes": "", 
	"primary_analyst_id": null, 
	"primary_analyst_uid": null, 
	"primary_trafficker_id": null, 
	"primary_trafficker_uid": null, 
	"revision": 1, 
	"sales_lead_id": null, 
	"sales_lead_uid": null, 
	"secondary_trafficker_id": null, 
	"secondary_trafficker_uid": null, 
	"single_ad_limitation": null, 
	"start_date": "2013-10-10 00:00:00", 
	"status": "Pending", 
	"type": "order", 
	"uid": "20003303-c001-fff1-8123-0c9a66", 
	"v": "3", 
	"view_through_window": "86400"
	}
]

### Creating an order

To create an order:
Retrieve the list of accounts to determine the account UID for which you want to create the order.
Retrieve the list of available fields for creating an order.
curl http://openx_server_name/ox/4.0/order/available_fields --cookie "openx3_access_token=token_string"
This returns the list of fields that you can set, and those that are required, for creating an order.
{
  "account_id": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "account_uid": {
    "has_dependencies": false, 
    "readonly": false, 
    "required": true, 
    "type": "account_uid"
  }, 
  "budget": {
    "acl": "order.budget", 
    "has_dependencies": false, 
    "readonly": false, 
    "required": false, 
    "type": "decimal(21,2)"
  }, 
  "click_through_window": {
    "acl": "order.conversion_window", 
    "default": "86400", 
    "has_dependencies": false, 
    "readonly": false, 
    "required": false, 
    "type": "int"
  }, 
  "created_date": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "datetime"
  },...
}
Create the order object, passing in, at a minimum, the required parameters, which include:
The name of the new order (name)
The status for the new order (status)
The beginning date for the new order (start_date)
The ID for the advertiser account that the new order belongs to (account_uid)
For example, create an order for a specified account:
curl http://openx_server_name/ox/4.0/order --cookie "openx3_access_token=token_string" -X POST
      --header "Content-Type:application/json" --data '{"account_uid":
      "20058a53-accf-fff1-8123-0c9a66", "name": "demo_order", "status": 
      "Pending", "start_date": "2014-01-21", "view_through_window": "86400",
      "click_through_window": "86400"}'
The API creates the order and returns the ID for the new order object.

### Creating a line item

To create a line item:
Create the order that you want to create the line item for, or retrieve the list of orders to determine the target order.
Get the list of available fields for creating a line item:
curl -X GET http://openx_server_name/ox/4.0/lineitem/available_fields --cookie "openx3_access_token=token_string"
The following error response indicates that a type_full value is required:
OpenError response
{
	"attribute": "type_full", 
	"choices": [
	"lineitem.exchange", 
	"lineitem.house", 
	"lineitem.preferred", 
	"lineitem.exclusive", 
	"lineitem.share_of_voice", 
	"lineitem.volume_goal", 
	"lineitem.non_guaranteed"
	], 
	"field": {}, 
	"http_status": 400, 
	"message": "Field type_full value must be one of \"lineitem.exchange\", \"lineitem.house\", \"lineitem.preferred\", \"lineitem.exclusive\", \"lineitem.share_of_voice\", \"lineitem.volume_goal\" or \"lineitem.non_guaranteed\" (\"None\" not allowed)", 
	"type": "Value Error", 
	"value": null
}
Modify the request by adding a type_full URI parameter:
curl -X GET http://openx_server_name/ox/4.0/lineitem/available_fieldstype_full=lineitem.house --cookie
      "openx3_access_token=token_string"
The response lists the fields that you can set and those that are required for creating a line item:
OpenResponse
{
  "account_id": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "account_uid": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "account_uid"
  }, 
  "ad_delivery": {
    "acl": "lineitem.ad_delivery_mode", 
    "default": "equal", 
    "has_dependencies": false, 
    "readonly": false, 
    "required": true, 
    "type": "varchar", 
    "url": "/options/ad_delivery_options"
  }, 
  "ad_delivery_id": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "buyer_id": {
    "acl": "instance.json_flags.programmatic_premium", 
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "buyer_uid": {
    "acl": "instance.json_flags.programmatic_premium", 
    "has_dependencies": false, 
    "readonly": false, 
    "required": false, 
    "type": "uid", 
    "url": "/options/market_advertiser_options"
  },...
}
You can retrieve details about fields that include a url value. For example, ad_delivery includes the following value: "url": "/options/ad_delivery_options"
curl -X GET http://openx_server_name/ox/4.0/options/ad_delivery_options --cookie
      "openx3_access_token=token_string"
Create the line item object, passing in, at a minimum, the required parameters, which include:
The ad delivery method (ad_delivery)
The name of the line item (name)
The status of the line item (status)
The ID for the order that the line item belongs to (order_uid)
The beginning date for the line item (start_date)
The delivery medium (delivery_medium)
Targeting rules (targeting)
The type of line item (type_full)
For example, create a house line item:
curl -X POST --header "Content-Type: application/json" http://openx_server_name/ox/4.0/lineitem \
--cookie "openx3_access_token=token_string" \
  --data='{
    "ad_delivery":"manual",
    "delivery_medium":"WEB",
    "name":"Demo line item",
    "status":"Pending",
    "order_uid":"20004055-c001-fff1-8123-0c9a66",
    "start_date":"now",
    "targeting":{"geographic":{"value_count":0},"content":{"value_count":0},"inter_dimension_operator":"OR"}
    "type_full":"lineitem.house",
    "account_uid":"2005aa92-accf-fff1-8123-0c9a66"
    }'
The API creates the line item and returns the UID for the new line item object.

### Creating an ad

To create an ad:
Create a line item and retrieve its UID, or retrieve the list of line items to determine the one to create the ad for.
Upload a creative.
Create a creative.
Get the list of available fields for creating an ad:
curl -X GET http://openx_server_name/ox/4.0/ad/available_fields --cookie "openx3_access_token=token_string"
The following error response indicates that a type_full value is required:
OpenError response
{
	"attribute": "type_full", 
	"choices": [
	"ad.nonlinearvast", 
	"ad.nonlinearvideo.html", 
	"ad.linearvast", 
	"ad.mobile", 
	"ad.image", 
	"ad.linearvideo", 
	"ad.thirdparty", 
	"ad.nonlinearvideo.flash", 
	"ad.exchange.ssrtb", 
	"ad.nonlinearvideo.image", 
	"ad.flash", 
	"ad.exchange.image", 
	"ad.exchange.html", 
	"ad.programmatic", 
	"ad.html", 
	"ad.exchange.thirdparty", 
	"ad.mobilehtml"
	], 
	"field": {}, 
	"http_status": 400, 
	"message": "Field type_full value must be one of \"ad.nonlinearvast\", \"ad.nonlinearvideo.html\", \"ad.linearvast\", \"ad.mobile\", \"ad.image\", \"ad.linearvideo\", \"ad.thirdparty\", \"ad.nonlinearvideo.flash\", \"ad.exchange.ssrtb\", \"ad.nonlinearvideo.image\", \"ad.flash\", \"ad.exchange.image\", \"ad.exchange.html\", \"ad.programmatic\", \"ad.html\", \"ad.exchange.thirdparty\" or \"ad.mobilehtml\" (\"None\" not allowed)", 
					"type": "Value Error", 
					"value": null
					}
Modify the request by adding a type_full URI parameter. For example, retrieve the list of available fields for an image ad:
curl -X GET http://openx_server_name/ox/4.0/ad/available_fields?type_full=ad.image --cookie
					"openx3_access_token=token_string"
This returns the list of fields that you can set, and those that are required, for creating an image ad.
OpenSample
{
  "account_id": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "account_uid": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "account_uid"
  }, 
  "ad_type_id": {
    "auto": true, 
    "default": "1", 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int", 
    "value": "1"
  }, 
  "ad_weight": {
    "acl": "ad.weight", 
    "has_dependencies": false, 
    "readonly": false, 
    "required": false, 
    "type": "decimal(4,1)"
  }, 
  "alt_text": {
    "acl": "ad.alt_text", 
    "has_dependencies": false, 
    "readonly": false, 
    "required": false, 
    "type": "varchar"
  }, 
  "click_target_window": {
    "acl": "ad.click_info", 
    "default": "_self", 
    "has_dependencies": false, 
    "readonly": false, 
    "required": true, 
    "type": "varchar", 
    "url": "/options/click_target_window_options"
  }, ...
 }
Create the image ad object, passing in, at a minimum, the required parameters, which include:
The name for the new image ad (name)
The status of the new ad (status)
The ID for the type of ad you are creating, such as image (ad_type_id)
The UID for the line item that the new ad belongs to (line_item_uid)
The size of the ad (size)
The UID of the creative (primary_creative_uid)
The click URL for the image ad (click_url)
The click target window for the image ad (click_target_window)
The type of ad (type_full)
For example, create an image ad:
curl -X POST --header "Content-Type: application/json" http://openx_server_name/ox/4.0/ad \
		--cookie "openx3_access_token=token_string" \
		--data='{
		"name":"Demo image ad",
		"status":"Active",
		"start_date":"now",
		"click_target_window":"_blank",
		"click_url":"http://www.example.com",
		"line_item_uid":"20004055-c001-fff1-8123-0c9a66",
		"primary_creative_uid":"null",
		"size":"300x600",
		"type_full":"ad.image"
		}'
The API creates the image ad and returns the ID for the new image ad object.

### Uploading a creative

Upload a creative file to the OpenX Ad Server to reference in creatives and use in ads.
curl -X POST --header "Content-Type: application/json" http://openx_server_name/ox/4.0/creative/upload_creative \
--cookie "openx3_access_token=token_string"
The API uploads the creative and returns the details for the creative.

### Creating a creative

Create a creative object which references an uploaded creative.
For example, create an image creative for account ID 22770.
curl -X POST --header "Content-Type: application/json" http://openx_server_name/ox/4.0/creative/upload_creative \ --cookie "openx3_access_token=token_string" \
  --data='{
    "account_id": "537242118", 
    "account_uid": "2005aa06-accf-fff1-8123-0c9a66", 
    "ad_type_full":"ad.image"
    "name":"Demo creative",
    "uri":"f45%2Ff45cf7e896a8a7d2508a796dcd03349d9a98798b%2F8a1%2F8a1a22f85a5da31ae06693fcf9ec8354.jpg",
    "width":"728",
    "height":"90",
    "bitrate":"null",
    "orig_name":"8a1a22f85a5da31ae06693fcf9ec8354.jpg",
    }'
The API creates the creative and returns the ID for the new creative object.

