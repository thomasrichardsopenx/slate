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
  - Authenticating users - Describes your options for authenticating a user to the Openx Platform API, with links to more information.
  - API client libraries - Describes the available OpenX client libaries that can help you integrate your application with the OpenX API.
  - Third-party libraries - Provides a list of links to recommended third-party libraries to consider for your OpenX API application integration.
  - Requests and responses - Explains the basic structure of an HTTP request and a JSON response.
  - Response codes and error handling - Describes the types of errors that are returned and what they mean.

# Authenticating users

If you are programmatically accessing the OpenX API, you must provide a method to authenticate a user. The OpenX API uses OAuth 1.0 to authenticate users. The following explains your options.

Your Authentication Options

You can programmatically authenticate an OpenX user in one of three ways:
  - (Recommended) Use the client libraries provided by OpenX.
  - (Recommended) Use a third-party library that already implements an OAuth 1.0 scheme. Note that OpenX does not support or manage the content of these libraries.
  - Write your own authentication code. OpenX provides a brief OAuth reference for those who want to take this approach.

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

# API Client Libraries

The OpenX Platform API provides a number of different API client libraries you can use to start integrating your application with the API. Currently, there are three available client libraries in the OpenX public GitHub repository you can use for application integration with the Platform API:
  - Java
  - PHP
  - Python

##Java Client Library

The Java client library enables you to implement OAuth authentication logic to work with the API so you will not need to implement the authentication logic yourself. This simplifies and speeds up the process of integrating your application with the OpenX Platform API. Within the client library, you will find a README.txt file that describes how you can install, configure and use the Java client library to work with your application, as well as demo authentication files (e.g. test.java/com/openx/oauthdemo) and a default.properties file you can use in your implementation.

To use the Java client library, navigate to the OpenX public GitHub location for the client libary and review the included README.txt file, which explains how to install and configure your OpenX instance, build the JAR file, and then use the Java package to integrate your application with the OpenX Platform API.

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
  - Objects. Support CRUD operations using the following HTTP verbs:
  - POST. Create the specified item
  - GET. Read a representation of the resource
  - PUT. Update the specified item
  - DELETE. Delete the specified item
  - Services. Support only read operations (i.e., GET)

For a list of resources, see the API reference.

To access a resource, construct a request according to the following URI format:

base_URIresource/identifier/parameterparametermethod&

Where:
  - method is an HTTP method, such as GET.
  - base-URI is your base URI provided by your account manager.
  - resource is the name of an API object or service.
  - identifier may provide a UID or a request for specific values if needed.
  - parameter indicates the first URI parameter string.
  - &parameter is an additional URI parameter string.

For example, in the call GET http://openx_server_name/ox/4.0/user/available_fieldsaction=create
  - GET is the method.
  - http://openx_server_name/ox/4.0/ is the base URI. The relative path /4.0/ indicates that your are using version 4 of the Platform API. If your base URI includes /3.0/, this API guide does not apply to your instance.
 - user is an API resource.
  - available_fields indicates a specific a request for information about the object's fields.
  - action=create is a URI parameter string indicating that the request is for fields available upon the object's creation.

## Requests and responses

Requests to the OpenX API require a Content-Type header set to application/json. The response format for all requests is a JSON object and an HTTP response code.
OpenX API v4 calls use the following general patterns:
  - GET /resource_type/. List all objects or services of the specified type.
  - GET /object_type/object_UID. Retrieve information about the object specified by its UID.
  - POST /object_type. Create a new object using the values encoded in a JSON object, which must include all fields that are required for the create operation. You can make batch create requests by including multiple JSON objects.
  - PUT /object_type. Batch update operation. Requests include a set of valid JSON objects with any fields that are being changed by the update. You can optionally include unchanged data.
  - PUT /object_type/object_UID. Update the specified object with changed values specified in a JSON object. You can optionally include unchanged data.
  - DELETE /object_type. Batch delete operation, where the request includes multiple UIDs.
  - DELETE /object_type/object_UID. Delete the specified object.

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
  - account_uid is the UID of the account that was created.
  - account_id is the ID of the account that was created.
  - For more details, see About IDs and UIDs.

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

  - acl. A field used for permissions that you can typically ignore.
  - url. A path that you can follow to the base URI to return a list of options. The corresponding field will only accept values that are found in the response to the specified options call.

Tip: You can also include action=create or action=update when calling the API for available and required fields of an object. For example, the set of required fields for creating a user is different than for updating a user, such as the user.email field, which is required when creating but not when updating.

#### Pagination

The size of the data displayed in a response is limited by the following request parameters:
  - limit. The maximum number of items to be returned on a single request. Its default value is 10.
  - offset. The number of the first item to display for the current request, where the offset for first item on the first page is 0. Its default value is 0.

If there is too much information to display in a single page, the response body will include "has_more": true. You can access the additional values by modifying the offset and limit paging values.

Sample list request including pagination parameters
For example, to list account records 50 through 75, specify an offset of 50 and a limit of 25:
curl -X GET http://openx_server_name/ox/4.0/accountoffset=50&limit=25 --cookie "openx3_access_token=token_string"

#### URI parameters

The URIs described in the API reference represent the supported request syntax, to which you can add certain supported request parameters. You can include request parameters in your API calls as query string arguments; you can append the first parameter after a question mark (?) and additional parameters separated by ampersands (&) in any order or combination.
The OpenX API supports the various parameters, such as the following:
  - action. When calling the API for available and required fields for an object, set this parameter to action=create or action=update to specify the action for which you want to retrieve values. For example, the set of required fields for creating a user (/user/available_fields?action=create) is different than the set of required fields for updating a user (/user/available_fields?action=update), such as the user.email field, which is required for creating a user but not for updating a user.
  - pretty. (For debugging purposes only) Display the JSON response into a more human readable format, encapsulated by HTML <pre> tags.

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

## Working with comments

If you do not wish to use the UI to add or edit comments, you may use the API to add (POST) and edit (PUT) comments for an object, as well as retrieve a list of all comments for a specific object (GET). The steps for adding and editing comments for objects are described below.
You can:
  - Add a new comment
  - Edit an existing comment
  - Get a list of comments

Add a new comment

To add a comment to an existing object, follow the steps described below.
Open a terminal window on your system.
Make the following POST API request to the OpenXserver to add a comment to the selected object (in this case, a line item). Note that there is a 1000 character limit for a comment, and a comment can be alphanumeric.
curl -s -X POST 'http://qa-v2-i16-lmi.api-v4-qa-ca.openx.net/ox/4.0/comment' --cookie $COOKIE --header "Content-Type:application/json" -d '{"obj_type": "lineitem","obj_uid": "600738b9-c002-fff1-8123-0c9a66","text": "A comment on a lineitem"}' | python -mjson.tool
The values listed in the table below make up the POST request to the OpenX server.
Add Comments Values
Value	Description	Example
resource URL	The URL used to make the API request to the OpenX server.	http://qa-v2-i16-lmi.api-v4-qa-ca.openx.net/ox/4.0/comment
cookie	The session cookie used in the header.	$COOKIE
header	The authentication header used to access the OpenX server.	Content-Type:application/json" -d
obj_type	The type of object that the comment is being added to.	lineitem
obj_uid	The unique identifier for the comment associated with the object.	600738b9-c002-fff1-8123-0c9a66
text	The actual text in the comment being added to the object.	A comment on a lineitem
The OpenX server processes this POST API request and returns a response output similar to the example response shown below.
[
    {
        "account_id": "1611253648",
        "account_uid": "6009c790-accf-fff1-8123-0c9a66",
        "created_date": "2015-10-15 20:58:39",
        "deleted": "0",
        "id": "1610638236",
        "instance_uid": "a505e730-0b7a-11e3-8ffd-0800200c9a66",
        "modified_date": "2015-10-15 20:58:39",
        "obj_id": "1611086009",
        "obj_type": "lineitem",
        "obj_uid": "600738b9-c002-fff1-8123-0c9a66",
        "revision": 1,
        "text": "A comment on a lineitem",
        "text_type": "text",
        "type": "comment",
        "uid": "6000639c-bbbb-fff1-8123-0c9a66",
        "user_id": "1610845423",
        "v": "3"
    }
]
The values listed in the table below are returned in the response output from the OpenX server.
Add Comment Values
Value	Description	Example
account_id	A unique ID of the account.	1611253648
account_uid	The unique identifier for the account, determined from account_id.	6009c790-accf-fff1-8123-0c9a66
created_date	The date/time when the comment was added to the object.	2015-10-15 20:58:39
deleted	A flag that specifies whether the comment has been deleted: 0 = FALSE 1 = TRUE.	0
id	The unique identifier for the comment.	1610638236
instance_uid	The platform_hash for the session.	a505e730-0b7a-11e3-8ffd-0800200c9a66
modified_date	The timestamp when the last change was made to the comment.	2015-10-15 20:58:39
obj_id	The ID determined from obj_uid.	1611086009
obj_type	The type of object referred to by comment.	lineitem
obj_uid	The unique identifier for the object.	600738b9-c002-fff1-8123-0c9a66
revision	The revision of the comment.	1
text	The text that makes up the comment being added to the object.	A comment on a lineitem
text_type	The type of text used in the comment (e.g. text, html).	text
type	The type of comment added to the object.	comment
uid	The unique identifier for the comment.	6000639c-bbbb-fff1-8123-0c9a66
user_id	The ID associated with the user who created the comment.	1610845423
user_uid	The UID associated with the user who created the comment.	200165d2-acc0-fff1-8123-0c9a66
v	The API version.	3

### Editing an existing comment

In addition to adding a comment to an object, you may also edit comments for an existing object. To edit a comment:
Open a terminal window on your system.
Make the following PUT API request to the OpenX server to edit/update a comment to the selected object (in this case, a line item).
$ curl -v -X PUT 'http://qa-v2-i16-lmi.api-v4-qa-ca.openx.net/ox/4.0/comment/1610638236'  -H "Content-Type:application/json" --cookie $COOKIE -d '{"text": "blah"}' | python -mjson.tool
The OpenX server processes the PUT request and updates the comment for the specified object. The response returned from the server is similar to the example below.
[
    {
        "account_id": "1611253648",
        "account_uid": "6009c790-accf-fff1-8123-0c9a66",
        "created_date": "2015-10-15 20:58:39",
        "deleted": "0",
        "id": "1610638236",
        "instance_uid": "a505e730-0b7a-11e3-8ffd-0800200c9a66",
        "modified_date": "2015-10-15 21:09:36",
        "obj_id": "1611086009",
        "obj_type": "lineitem",
        "obj_uid": "600738b9-c002-fff1-8123-0c9a66",
        "revision": 2,
        "text": "blah",
        "text_type": "text",
        "type": "comment",
        "uid": "6000639c-bbbb-fff1-8123-0c9a66",
        "user_id": "1610845423",
        "user_uid": "60038cef-acc0-fff1-8123-0c9a66",
        "v": "3"
    }
]
The values listed in the table below make up the POST request to the OpenX server.
Edit Comment Values
Value	Description	Example
resource URL	The URL used to make the API request to the OpenX server.	curl -s -X PUT 'http://127.0.0.1:8001/comment
header	The authentication header and cookie used to access the OpenX server.	Content-Type:application/json" -d
cookie	The session cookie used in the header.	openx3_access_token=09af
obj_type	The type of object.	lineitem
obj_uid	The unique identifier for the object being used.	60062525-c002-fff1-8123-0c9a66
text	The updated text for the comment.	The updated text for the comment
The values listed in the table below are returned in the response output from the OpenX server.
Value	Description	Example
account_id	The value determined from the account_uid	1611253648
account_uid	The account_uid of the object.	6009c790-accf-fff1-8123-0c9a66
created_date	The date/time when the comment was created.	2015-10-15 20:58:39
deleted	A flag that specifies whether the comment has been deleted.	0
id	The unique ID for the comment.	1610638236
instance_uid	The platform_hash of session.	a505e730-0b7a-11e3-8ffd-0800200c9a66
modified_date	The timestamp when the last change was made to the comment.	2015-10-15 21:09:36
obj_id	The ID associated with the object.	1611086009
obj_type	The type of object referred to by the comment.	lineitem
obj_uid	The unique ID of the object referred to in the comment.	600738b9-c002-fff1-8123-0c9a66
revision	The revision number for the comment.	2
text	The text of the comment.	blah
text_type	The type of text used in the comment (e.g. html).	text
type	The type of comment added to the object.	comment
uid	The unique ID associated with the comment.	6000639c-bbbb-fff1-8123-0c9a66
user_id	The ID associated with the user who created the comment.	1610845423
user_uid	The unique ID associated with the user who created the comment.	60038cef-acc0-fff1-8123-0c9a66
v	The version of the API.	3

### Get a list of comments

You can also get a list of comments associated with an object by making a GET request to the OpenX Server.
Open a terminal window on your system.
Make the following GET request to the OpenX server (in this case, a line item):
curl -v -X GET 'http://qa-v2-i16-lmi.api-v4-qa-ca.openx.net/ox/4.0/comment/1610638236'  -H "Content-Type:application/json"--cookie $COOKIE | python -mjson.tool
The values listed in the table below make up the GET request to the OpenX server.
GET Comments Values
Value	Description	Example
resource URL	The URL used to make the API request to the OpenX server.	http://qa-v2-i16-lmi.api-v4-qa-ca.openx.net/ox/4.0/comment
cookie	The session cookie used in the header.	$COOKIE
header	The authentication header and cookie used to access the OpenX server.	Content-Type:application/json" -d
The OpenX server processes the GET request and returns a list of all comments for that specified comment. An example of a comment is shown below.
[
    {
        "account_id":"1611253648",
        "account_uid":"6009c790-accf-fff1-8123-0c9a66",
        "created_date":"2015-10-15 20:58:39",
        "deleted": "0",
        "id":"1610638236",
        "instance_uid":"a505e730-0b7a-11e3-8ffd-0800200c9a66",
        "modified_date":"2015-10-15 20:58:39",
        "obj_id":"1611086009",
        "obj_type":"lineitem",
        "obj_uid":"600738b9-c002-fff1-8123-0c9a66",
        "revision":1,
        "text":"A comment on a lineitem",
        "text_type":"text",
        "type":"comment",
        "uid":"6000639c-bbbb-fff1-8123-0c9a66",
        "user_id":"1610845423",
        "user_uid":"60038cef-acc0-fff1-8123-0c9a66",
        "v": "3"
}
]
The values listed in the table below are returned in the response output from the OpenX server.
GET Comment Response Values
Values	Description	Example
account_id	The value determined from account_uid.	1611253648
account_uid	The account_uid for the associated object.	6009c790-accf-fff1-8123-0c9a66
created_date	The date/time when the comment was created.	2015-10-15 20:58:39
deleted	A flag that specifies whether the comment has been deleted.	0
id	The ID associated with the comment.	1610638236
instance_uid	Theplatform_hashof the session.	a505e730-0b7a-11e3-8ffd-0800200c9a66
modified_date	The timestamp when the last change was made to the comment.	2015-10-15 20:58:39
obj_id	The ID associated with the object.	1611086009
obj_type	The type of object referred to by the comment.	lineitem
obj_uid	The unique ID of the object.	600738b9-c002-fff1-8123-0c9a66
revision	The revision number for the comment.	1
text	The actual text in the comment.	a comment on a lineitem
text_type	The type of text used for the comment (e.g. html).	text
type	The type of comment added to the object.	comment
uid	The unique ID for the comment.	6000639c-bbbb-fff1-8123-0c9a66
user_id	The ID associated with the user who created the comment.	1610845423
user_uid	The unique ID associated with the user who created the comment.	60038cef-acc0-fff1-8123-0c9a66
v	The version of the API.

## Working with forecasts

The OpenX Platform API provides the ability to forecast the impact of creating a new line item on existing line items. Much like the OpenX UI, the API enables you to perform a number of different forecasting actions to create and manage forecasts. Before using the Forecasting API, you should already be familiar with how forecasting works using the OpenX user interface. If, however, you are unfamiliar with how forecasting works, refer to the Forecasting documentation for more information on forecasts and availability.
Forecasting enables you to:
Get a picture of how much inventory you will have available to sell.
Plan your inventory setup so you can get a better idea of potential revenue.
Calculate seasonality for your revenue, or the predicted number of impressions for the past year, based on historical trends.
Forecast performance.
Ensure you have enough inventory to fulfill any existing delivery goals for guaranteed line items.
When working with forecasting, you have the option to either use the OpenX user interface or API to run forecasts. This section describes how to run forecasts using the Forecasting API.

### API request structure

When you make an API request, you must follow the standard HTTP API request structure to ensure the request is properly formatted. You can find more information about how to construct an HTTP request in the Conventions section of this Developer's Guide.

### Running a forecast

There are different operations you can perform using the Forecasting API. Typically, when you run a forecast, you want to see what impact, if any, adding a new line item will have on existing line items and revenue. By making a single call with your desired arguments, you will see the impact of your line item on other line items and revenue.
The example steps below describe how to run a forecast with revenue information.
Note: : These steps assume you have already been authenticated. If you have not been authenticated, refer to the Authentication section for the steps on how to authenticate your application.
From your terminal window, make the following cURL request
curl '<your server instance>/ox/4.0/forecast_augur/my_availability_forecasts' -H 'Content-Type: application/json' -H 'Accept: application/json, text/javascript' -H 'Cache-Control: no-cache' -H 'Cookie: i=d7e17e57-71d3-4c33-22e3-d51789f40aba|1444755321; openx3_access_token=3fbacd1bad501a6f503b3300ace57a1e48cd4c9b0e541f0a9f39dd20668ba38750ee6ff445fcac2c20a429f11c8191825e828cc1ba845c674bc1a1c4eb9a12560561d38d1'--data-binary '{"start_date":"2015-10-13 00:00:00","end_date":"2015-10-31 00:00:00","buying_model":"exclusive","timezone":"America/Los_Angeles","pricing_rate":".50","full_oxtl_rule_id":"e02d9dc977cddb2860eafb70a7d920db","targeting_rule":"58a334fe-c92e-4e48-add8-cefe0c89b0fc","currency":"USD","make_good":true,"adunit_uid":"all"}'
Note that you are passing the following arguments in the call:
API Request Values
Argument	Value
Server address	Your server instance address
Client	ox
API Version	4.0
Forecast request	forecast_augur/my_availability_forecasts
Application headers	-H 'Content-Type: application/json' -H 'Accept: application/json, text/javascript' -H 'Cache-Control: no-cache' -H
Cookie	i=d7e17e57-71d3-4c33-22e3-d51789f40aba|1444755321;
Access Token	3fbacd1bad501a6f503b3300ace57a1e48cd4c9b0e541f0a9f39dd20668ba38750ee6ff445fcac2c20a429f11c8191825e828cc1ba845c674bc1a1c4eb9a12560561d38d1
Start Date	2015-10-13 00:00:00
End Date	2015-10-31 00:00:00
Buying Model	Exclusive
Timezone	America/Los_Angeles
Pricing Rate	50
OpenX Targeting Language RuleID	e02d9dc977cddb2860eafb70a7d920db
Targeting Rule	58a334fe-c92e-4e48-add8-cefe0c89b0fc
Currency	USD
Make Good	true
AdUnit UID	all
You can then perform one of the following API requests to return a list of available full_oxtl_rule_id and targeting_rule values. These values are defined as follows:
full_oxtl_rule_id - The ID of the targeting rule that you are creating, and contains all the information that you have entered - name, inventory, and the rest of the targeting critieria.
targeting_rule - The ID of the forecasting object that was created *from* the full oxtl rule. This is the object that is created that forecasting really cares about - which only contains a subset of the targeting critieria that you entered initially.
You will want to make these API calls so you can view the different targeting rules available before running a forecast.
curl '<your server instance>/ox/4.0/forecast_augur/forecastrules?avail2=true' -H 'Accept: application/json, text/javascript, /; q=0.01' -H 'X-Requested-With: XMLHttpRequest' -H 'Accept-Language: en-us' -H 'X-Openx-Client: 4.43.0.2335' -H 'User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/46.0.2490.86 Safari/537.36' -H 'Content-Type: application/json' --compressed
curl '<your server instance>/ox/4.0/forecast_augur/forecastrules?avail2=false' -H 'Accept: application/json, text/javascript, /; q=0.01' -H 'X-Requested-With: XMLHttpRequest' -H 'Accept-Language: en-us' -H 'X-Openx-Client: 4.43.0.2335' -H 'User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/46.0.2490.86 Safari/537.36' -H 'Content-Type: application/json' --compressed
In each of these API calls, there are a number of arguments that can be passed with the call, which are listed in the table below.
Forecast Request Values
Request Value	Value
Your server address.	your server instance
The OpenX client.	ox
The API version.	4.0
The forecasting service.	forecast_augur
The available targeting rules and rule IDs you can use in a forecast.	forecastrules
The application header used in the request.	-H 'Accept: application/json, text/JavaScript, /; q=0.01'
The HTTP request.	-H 'X-Requested-With: XMLHttpRequest'
The client header, including language (en-us).	-H 'Accept-Language: en-us' -H 'X-Openx-Client: 4.43.0.2335'
The user agent header used in the request, including browser and operating system.	-H 'User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/46.0.2490.86 Safari/537.36'
The application header returned to the client.	-H 'Content-Type: application/json' --compressed
When making this request, the server processes the request and runs a forecast, displaying the forecast. The table below describes each value returned in the response.
Forecast Return Values
Response Value	Description
status	The status response is used to perform data massaging for response messages since there are special cases for the API. The following response values can be returned: 0=successful, 1=unsuccessful
goal	The impression goal of the line item for the dates given.
impacted line items	Line items in the forecast that are impacted with your new line item. For each impacted line item, you will see the following values: loss, goal, previousAllocated, revenueLost, newAllocated, pricingRate, buyingModel, id, name.
pricingRate	The given pricing rate the user entered. 0.0 is returned if no pricing rate was given.
msg	Message that acknowledges whether the forecast was run successfully.
allocated	The total number of allocated impressions for this line item.
forecasted	The total number of forecasted impressions for this line item. This value is what the line item was forecasted to deliver on, but because of competing line items, did not meet the full forecasted impressions. You can get the unsold impressions by calculating forecasted - allocated = unsold impressions.
OpenSample
{   
   "status":0,
   "goal":4408897,
   "impactedLineItems":[   
      {   
         "loss":1700000,
         "goal":8400000,
         "previousAllocated":8197646,
         "revenueLost":2550000000,
         "newAllocated":6497646,
         "pricingRate":1500000,
         "buyingModel":"volume_goal",
         "id":537555677,
         "name":"engage: BDR_Res-CPM 10_09_15 - 12_31_15"
      },
      {   
         "loss":157389,
         "goal":42600000,
         "previousAllocated":1456361,
         "revenueLost":157389000,
         "newAllocated":1298972,
         "pricingRate":1000000,
         "buyingModel":"volume_goal",
         "id":537521128,
         "name":"3 Interactive - YB Video_08_12_15"
      },
      {   
         "loss":8199,
         "goal":1420000,
         "previousAllocated":50978,
         "revenueLost":8198000,
         "newAllocated":42779,
         "pricingRate":1000000,
         "buyingModel":"volume_goal",
         "id":537521123,
         "name":"3 Interactive - YB Video (Daily Me)_08_12_15"
      },
      {   
         "loss":2775,
         "goal":40000,
         "previousAllocated":14365,
         "revenueLost":4854500,
         "newAllocated":11590,
         "pricingRate":1750000,
         "buyingModel":"volume_goal",
         "id":537487409,
         "name":"3 Interactive - WSPS Desktop_10_01_15_12_31_15"
      },
      {   
         "loss":590,
         "goal":925000,
         "previousAllocated":43735,
         "revenueLost":767000,
         "newAllocated":43145,
         "pricingRate":1300000,
         "buyingModel":"volume_goal",
         "id":537545939,
         "name":"3 Interactive - AMEX CA_11_01_15_11_30_15"
      },
      {   
         "loss":366,
         "goal":925000,
         "previousAllocated":37013,
         "revenueLost":475800,
         "newAllocated":36647,
         "pricingRate":1300000,
         "buyingModel":"volume_goal",
         "id":537545940,
         "name":"3 Interactive - AMEX CA_12_01_15_12_31_15"
      }

   ],
   "pricingRate":0.5,
   "msg":"ok",
   "allocated":4408897,
   "forecasted":4408897
}

## Working with history

You can get history information associated with an object by making a GET request to the OpenX Server. For more information about the history feature, see History.
Note: All examples assume that you have been authenticated.
From the terminal window on your system, make the following GET request to the OpenX server (in this case, an order):
curl -v -X GET 'http://server_name.openx.net/ox/4.0/audittrail?obj_type=order&obj_id=1611010849'  -H "Content-Type:application/json"--cookie $COOKIE | python -mjson.tool
The values listed in the table below are available for the GET request to the OpenX server.
History values
Value	Description
sort	Sorts the output by revision. Options:
sort=revision: Returns search results by revision in ascending order
sort=-revision: Returns search results by revision in descending order
obj_type	Specifies the object type. For example: obj_type='ad'.
offset=m	Specifies an offset value for a history record search. For example, to bypass the first ten records and start searching at the eleventh record, use "offset=10". To return the second ten records, use "offset=10 limit 10".
limit=n	Returns the most recent n history records.
revision	Specifies a revision number. For example, to return one history record, use the obj_id and revision.
timestamp	Use timestamp to return history information for an object based on a date or date range. For example, use timestamp > or timestamp < to specify history after or before a specific date. Operators include >, <, =, >=, <=, and !=. timestamp format is YYYY-MM-DD.
obj_id	Specifies an object's id. When just an object id is specified, returns all parsed history records for the specified object revision, displaying the most recent first.
user	Specifies the user, indicated by an email address, who made a modification to an object.
The OpenX server processes the GET request and returns a list of all history information for the specified order object. An example of history is shown below.
OpenExample
[{
	"uid": "60061321-c001-fff1-8123-b0769b",
	"primary_trafficker_uid": "6003bcb1-acc0-fff1-8123-b0769b",
	"id": "1611010849",
	"modified_date": "2016-01-21 23:08:39",
	"sales_lead_id": "1610857649",
	"primary_trafficker_id": "1610857649",
	"secondary_trafficker_id": "1610857649",
	"budget_pacing": "soft_budget",
	"primary_analyst_id": "1610857649",
	"type": "order",
	"start_date": "2016-01-21 08:00:00",
	"revision": 1,
	"status": "Pending",
	"account_id": "1611310336",
	"end_date": null,
	"deleted": "0",
	"created_date": "2016-01-21 23:08:39",
	"click_through_window": "86400",
	"sales_lead_uid": "6003bcb1-acc0-fff1-8123-b0769b",
	"primary_analyst_uid": "6003bcb1-acc0-fff1-8123-b0769b",
	"single_ad_limitation": "0",
	"instance_uid": "af2fd08d-7186-4d75-9a06-61d83cb0769b",
	"view_through_window": "86400",
	"name": "John Advertiser #1 Order #1",
	"notes": "",
	"budget": "1000.00",
	"account_uid": "600aa500-accf-fff1-8123-b0769b",
	"v": "3",
	"external_id": "",
	"secondary_trafficker_uid": "6003bcb1-acc0-fff1-8123-b0769b"
}]
The values listed in the table below are some of the values returned in the response output from the OpenX server. Some values depend on which object type was queried.
GET History Response Values
Value
Description
Example
has_more	Indicates whether more records exist: true or false.	false
obj_type	The type of object being quered for history.	order
obj_id	The ID associated of the object.	1611010849
ip	IP address.	10.0.50.56
reason	The reason or method of modification.	api
first_name	The user's first name.	John
last_name	The user's last name.	Advertiser #1
name	The user's first and last name.	John Advertiser #1
email	The user's email address. This is used to identify the person who modified an object.	john.smith+i59_adv1@openx.com
timestamp	The date and time that the object was modified.	2016-01-21 23:08:39
action	The action that was performed to the object.	create
real_user	The real user's email address.	john.smith+i59_adv1@openx.com
type	The type of object query.	audittrail
account_uid	The account_uid for the associated object.	600aa500-accf-fff1-8123-b0769b
instance_uid	The platform_hash of the session.	af2fd08d-7186-4d75-9a06-61d83cb0769b
revision	The revision number of the object.	1
limit	The maximum number of records to return.	30
offset	The distance (displacement) from the beginning of the object until a given element.	0
Samples
Note: The following samples show both ways to create a history call:
specify an object type and specific object (obj_id) and then append history (audittrail) parameters
query the history (audittrail) endpoint and pass in the object type, object id, and additional parameters
Note: Also, the following samples show the abbreviated form of the GET call as opposed to a full curl call.
Return object history with limit and sort
The following sample returns the history of changes for order number 1611010849. The default settings are: "limit=20 sort=-revision".
GET Request
Object id first; history parameters second
http://server_name.openx.net/ox/4.0/order/1611010849/audittrail
History (audittrail) object first; object id second
http://server_name.openx.net/ox/4.0/audittrail?obj_type=order&obj_id=1611010849
OpenSample response
{
	"has_more": false,
	"objects": [{
		"obj_type": "order",
		"changes": {
			"new_value": {
				"uid": "60061321-c001-fff1-8123-b0769b",
				"primary_trafficker_uid": "6003bcb1-acc0-fff1-8123-b0769b",
				"id": "1611010849",
				"modified_date": "2016-01-21 23:08:39",
				"sales_lead_id": "1610857649",
				"primary_trafficker_id": "1610857649",
				"secondary_trafficker_id": "1610857649",
				"budget_pacing": "soft_budget",
				"primary_analyst_id": "1610857649",
				"type": "order",
				"start_date": "2016-01-21 08:00:00",
				"revision": 1,
				"status": "Pending",
				"account_id": "1611310336",
				"end_date": null,
				"deleted": "0",
				"v": "3",
				"click_through_window": "86400",
				"sales_lead_uid": "6003bcb1-acc0-fff1-8123-b0769b",
				"primary_analyst_uid": "6003bcb1-acc0-fff1-8123-b0769b",
				"single_ad_limitation": "0",
				"instance_uid": "af2fd08d-7186-4d75-9a06-61d83cb0769b",
				"view_through_window": "86400",
				"name": "John Advertiser #1 Order #1",
				"notes": "",
				"budget": "1000.00",
				"secondary_trafficker_uid": "6003bcb1-acc0-fff1-8123-b0769b",
				"created_date": "2016-01-21 23:08:39",
				"external_id": "",
				"account_uid": "600aa500-accf-fff1-8123-b0769b"
			}
		},
		"obj_id": "1611010849",
		"ip": "10.0.50.56",
		"reason": "api",
		"program": [
			"uwsgi"
		],
		"user": {
			"first_name": "John",
			"last_name": "Advertiser #1",
			"name": "John Advertiser #1",
			"email": "john.smith+i59_adv1@openx.com"
		},
		"timestamp": "2016-01-21 23:08:39",
		"action": "create",
		"real_user": "john.smith+i59_adv1@openx.com",
		"type": "audittrail",
		"account_uid": "600aa500-accf-fff1-8123-b0769b",
		"instance_uid": "af2fd08d-7186-4d75-9a06-61d83cb0769b",
		"revision": 1
	}],
	"limit": 20,
	"offset": 0
}
Return history by revision
The following sample specifies a specific revision (1) in the history.
GET Request
Object id first; history parameters second
http://server_name.openx.net/ox/4.0/order/1611010849/audittrail?revision=1
History (audittrail) object first; object id second
http://server_name.openx.net/ox/4.0/audittrail?obj_type=order&obj_id=1611010849&revision=1
OpenSample response
{
	"has_more": false,
	"objects": [{
		"obj_type": "order",
		"changes": {
			"new_value": {
				"uid": "60061321-c001-fff1-8123-b0769b",
				"primary_trafficker_uid": "6003bcb1-acc0-fff1-8123-b0769b",
				"id": "1611010849",
				"modified_date": "2016-01-21 23:08:39",
				"sales_lead_id": "1610857649",
				"primary_trafficker_id": "1610857649",
				"secondary_trafficker_id": "1610857649",
				"budget_pacing": "soft_budget",
				"primary_analyst_id": "1610857649",
				"type": "order",
				"start_date": "2016-01-21 08:00:00",
				"revision": 1,
				"status": "Pending",
				"account_id": "1611310336",
				"end_date": null,
				"deleted": "0",
				"v": "3",
				"click_through_window": "86400",
				"sales_lead_uid": "6003bcb1-acc0-fff1-8123-b0769b",
				"primary_analyst_uid": "6003bcb1-acc0-fff1-8123-b0769b",
				"single_ad_limitation": "0",
				"instance_uid": "af2fd08d-7186-4d75-9a06-61d83cb0769b",
				"view_through_window": "86400",
				"name": "John Advertiser #1 Order #1",
				"notes": "",
				"budget": "1000.00",
				"secondary_trafficker_uid": "6003bcb1-acc0-fff1-8123-b0769b",
				"created_date": "2016-01-21 23:08:39",
				"external_id": "",
				"account_uid": "600aa500-accf-fff1-8123-b0769b"
			}
		},
		"obj_id": "1611010849",
		"ip": "10.0.50.56",
		"reason": "api",
		"program": [
			"uwsgi"
		],
		"user": {
			"first_name": "John",
			"last_name": "Advertiser #1",
			"name": "John Advertiser #1",
			"email": "john.smith+i59_adv1@openx.com"
		},
		"timestamp": "2016-01-21 23:08:39",
		"action": "create",
		"real_user": "john.smith+i59_adv1@openx.com",
		"type": "audittrail",
		"account_uid": "600aa500-accf-fff1-8123-b0769b",
		"instance_uid": "af2fd08d-7186-4d75-9a06-61d83cb0769b",
		"revision": 1
	}],
	"limit": 20,
	"offset": 0
}
Return history by user
The following sample specifies more filters including "user" which is an email address. Note that the email value "john.smith+i59_adv1@openx.com" must be url-encoded because the plus sign (+) has a special meaning (space) in URLs. Therefore, it must be encoded as %2b.
GET Request
Object id first; history parameters second
http://server_name.openx.net/ox/4.0/order/1611010849/audittrail?limit=30&timestamp>=2016-01-21&user=john.smith+2bi59_adv1@openx.com
History (audittrail) object first; object id second
http://server_name.openx.net/ox/4.0/audittrail?obj_type=order&obj_id=1611010849&limit=30&timestamp>=2016-01-21&user=john.smith+2bi59_adv1@openx.com
OpenSample response
{
	"has_more": false,
	"objects": [{
		"obj_type": "order",
		"changes": {
			"new_value": {
				"uid": "60061321-c001-fff1-8123-b0769b",
				"primary_trafficker_uid": "6003bcb1-acc0-fff1-8123-b0769b",
				"id": "1611010849",
				"modified_date": "2016-01-21 23:08:39",
				"sales_lead_id": "1610857649",
				"primary_trafficker_id": "1610857649",
				"secondary_trafficker_id": "1610857649",
				"budget_pacing": "soft_budget",
				"primary_analyst_id": "1610857649",
				"type": "order",
				"start_date": "2016-01-21 08:00:00",
				"revision": 1,
				"status": "Pending",
				"account_id": "1611310336",
				"end_date": null,
				"deleted": "0",
				"v": "3",
				"click_through_window": "86400",
				"sales_lead_uid": "6003bcb1-acc0-fff1-8123-b0769b",
				"primary_analyst_uid": "6003bcb1-acc0-fff1-8123-b0769b",
				"single_ad_limitation": "0",
				"instance_uid": "af2fd08d-7186-4d75-9a06-61d83cb0769b",
				"view_through_window": "86400",
				"name": "John Advertiser #1 Order #1",
				"notes": "",
				"budget": "1000.00",
				"secondary_trafficker_uid": "6003bcb1-acc0-fff1-8123-b0769b",
				"created_date": "2016-01-21 23:08:39",
				"external_id": "",
				"account_uid": "600aa500-accf-fff1-8123-b0769b"
			}
		},
		"obj_id": "1611010849",
		"ip": "10.0.50.56",
		"reason": "api",
		"program": [
			"uwsgi"
		],
		"user": {
			"first_name": "John",
			"last_name": "Advertiser #1",
			"name": "John Advertiser #1",
			"email": "john.smith+i59_adv1@openx.com"
		},
		"timestamp": "2016-01-21 23:08:39",
		"action": "create",
		"real_user": "john.smith+i59_adv1@openx.com",
		"type": "audittrail",
		"account_uid": "600aa500-accf-fff1-8123-b0769b",
		"instance_uid": "af2fd08d-7186-4d75-9a06-61d83cb0769b",
		"revision": 1
	}],
	"limit": 30,
	"offset": 0
}
Additional Samples
Note: The following samples show only the second way to create a history call (by querying the history (audittrail) endpoint and passing in the object type, object id, and additional parameters).
Note: The following samples show the full curl call.
Return all history for an object
The following sample returns all history records for the specified object id. If more than ten records exist in the database, it returns the most recent ten. To return more than ten records, use limit=int.
GET Request
curl -v -X GET 'http://server_name.openx.net/ox/4.0/audittrail?&obj_id=object_id'  -H "Content-Type:application/json"--cookie $COOKIE | python -mjson.tool
Return history for a specified revision
The following sample returns one history record for the specified object id and the specified revision.
GET Request
curl -v -X GET 'http://server_name.openx.net/ox/4.0/audittrail?&obj_id=object_id&revision=integer'  -H "Content-Type:application/json"--cookie $COOKIE | python -mjson.tool
Return history modified by a specified user
The following sample returns all history records for the specified object id that were modified by the specified user (indicated by the email address).
GET Request
curl -v -X GET 'http://server_name.openx.net/ox/4.0/audittrail?&obj_id=object_id&user=email_address'  -H "Content-Type:application/json"--cookie $COOKIE | python -mjson.tool
Return object history after a specified date
The following sample returns all history records for the specified object id, with a timestamp greater than (>) a specified date.
GET Request
curl -v -X GET 'http://server_name.openx.net/ox/4.0/audittrail?&obj_id=object_id&timestamp>2016-01-01'  -H "Content-Type:application/json"--cookie $COOKIE | python -mjson.tool
Return object history with offset and limit
The following sample returns all history records for the specified object id, with a limit of 30 and an offset of 10.
GET Request
curl -v -X GET 'http://server_name.openx.net/ox/4.0/audittrail?&obj_id=object_id&limit=30&offset=10'  -H "Content-Type:application/json"--cookie $COOKIE | python -mjson.tool
Return object history with offset, limit, and sort
The following sample returns all history records for the specified object id, with a limit of 30 and an offset of 10, sorted by timestamp (descending).
GET Request
curl -v -X GET 'http://server_name.openx.net/ox/4.0/audittrail?&obj_id=object_id&limit=30&offset=10&sort=-timestamp'  -H "Content-Type:application/json"--cookie $COOKIE | python -mjson.tool

Accessing Reports via OpenX Reporting API
Even though you can access reports using the OpenX user interface, you may also run these same reports using the OpenX API. You can perform many of the same steps in accessing reports using the API as you would when using the OpenX user interface, with the main difference being that you will only need to make a single API request to configure the date range and attributes you want to see in the report.
If you decide to access reports via the OpenX API, it is assumed you are already intimately familiar with reporting and understand how these reports are configured and generated. If you are unfamiliar with OpenX reporting, please refer to the OpenX Reporting documentation for more detailed information about how reports are managed in the user interface.
To run simple reporting service (SRS) reports, make the following report object calls:
GET /report/get_reportlistList all reports that are available to you based on your account and user permissions.
The reports listed by this request typically correspond to the SRS reports available to you in the OpenX UI.
Sample request:
curl http://openx_server_name/ox/4.0/report/get_reportlist --cookie "openx3_access_token=token_string"
Where:token_string is a string of characters returned by the GET request at login. To obtain an openx3_access_token, see the authentication topic.
OpenSample response
{
  "content": [
    {
      "acl_resource_type": "inventory", 
      "code": "adunit_size_sum", 
      "context": "inventory", 
      "context_display_name": "Inventory", 
      "description": "The Ad Unit Size Summary report is designed to show delivery summary by Ad Unit Size for the specified date range.", 
      "title": "Ad Unit Size Summary"
    }, 
    {
      "acl_resource_type": "inventory", 
      "code": "adunit_sum", 
      "context": "inventory", 
      "context_display_name": "Inventory", 
      "description": "The Ad Unit Summary report is designed to show delivery summary by ad unit for the specified date range.", 
      "title": "Ad Unit Summary"
    }, 
    {
      "acl_resource_type": "audiencesegment", 
      "code": "aud_inv", 
      "context": "audience", 
      "context_display_name": "Audience", 
      "description": "The Audience Distribution By Site report provides a time-based view of audience distribution across sites.", 
      "title": "Audience Distribution By Site"
    }, 
    {
      "acl_resource_type": "audiencesegment", 
      "code": "aud_sum", 
      "context": "audience", 
      "context_display_name": "Audience", 
      "description": "The Audience Summary report provides day-by-day audience segment analysis, including segment size and daily reach.", 
      "title": "Audience Summary"
    }, 
    {
      "acl_resource_type": "inventory", 
      "code": "buyer_sum", 
      "context": "inventory", 
      "context_display_name": "Inventory", 
      "description": "The Buyer Summary report is designed to show delivery summary by buyer for the specified date range.", 
      "title": "Buyer Summary"
    }, 
    {
      "acl_resource_type": "inventory", 
      "code": "cust_keyval", 
      "context": "inventory", 
      "context_display_name": "Inventory", 
      "description": "The Custom Key Value report is designed to show delivery activity across the pre-defined custom key values for a specified date range. This report helps users understand volume, sales, and performance information broken up by individual custom key values.", 
      "title": "Custom Key Value"
    }, 
    {
      "acl_resource_type": "inventory", 
      "code": "inv_bill", 
      "context": "inventory", 
      "context_display_name": "Inventory", 
      "description": "The Platform Billing Report is designed to provide a summary of the total requests and impression data in UTC, broken down by Publishers, Sites and Ad Units.", 
      "title": "Platform Billing"
    }, 
    {
      "acl_resource_type": "inventory", 
      "code": "inv_daily_sum", 
      "context": "inventory", 
      "context_display_name": "Inventory", 
      "description": "The Daily Summary report is designed to show delivery summary by date for the specified date range.", 
      "title": "Daily Summary"
    }, 
    {
      "acl_resource_type": "inventory", 
      "code": "inv_del", 
      "context": "inventory", 
      "context_display_name": "Inventory", 
      "description": "The Inventory Delivery report is designed to show delivery activity across various inventory dimensions to specific orders, line-items and ads for activity that occurred during the specified date range.", 
      "title": "Inventory Delivery"
    }, 
    {
      "acl_resource_type": "inventory", 
      "code": "inv_perf_pub", 
      "context": "inventory", 
      "context_display_name": "Inventory", 
      "description": "The Inventory Detailed Performance report is designed to show delivery activity across various inventory dimensions, for activity that occurred during the specified date range. This report helps users understand volume, sales, and performance information by site.", 
      "title": "Inventory Detailed Performance"
    }, 
    {
      "acl_resource_type": "inventory", 
      "code": "inv_perf", 
      "context": "inventory", 
      "context_display_name": "Inventory", 
      "description": "The Inventory Performance report is designed to show delivery activity across various inventory dimensions, for activity that occurred during the specified date range. This report helps users understand volume, sales, and performance information by site and visitor geography.", 
      "title": "Inventory Performance"
    }, 
    {
      "acl_resource_type": "inventory", 
      "code": "inv_rev", 
      "context": "inventory", 
      "context_display_name": "Inventory", 
      "description": "The Inventory Revenue report is used to understand how revenue is spread across various dimensions of inventory, by sales channel, by day, for transactions that fit the specified date range.", 
      "title": "Inventory Revenue"
    }, 
    {
      "acl_resource_type": "inventory", 
      "code": "inv_sell", 
      "context": "inventory", 
      "context_display_name": "Inventory", 
      "description": "The Inventory Sell-Through report provides insight across various inventory dimensions as to how the inventory is being sold. Users can better understand which sales channels sell what volume of inventory, how they account for revenue, and how much goes unsold.", 
      "title": "Inventory Sell-Through"
    }, 
    {
      "acl_resource_type": "order", 
      "code": "mkt_del", 
      "context": "market", 
      "context_display_name": "Market", 
      "description": "The Market Delivery report shows how each market order is delivering across particular inventory segments, by time frame. Only orders with activity in the given date range are shown.", 
      "title": "Market Delivery"
    }, 
    {
      "acl_resource_type": "order", 
      "code": "mkt_perf", 
      "context": "market", 
      "context_display_name": "Market", 
      "description": "The Market Performance report shows detailed performance information, by time period, for market orders that won impressions during the specified date range. This report is useful to understand how your inventory is being sold to real-time buyers in the Market.", 
      "title": "Market Performance"
    }, 
    {
      "acl_resource_type": "order", 
      "code": "order_del", 
      "context": "order", 
      "context_display_name": "Order", 
      "description": "The Order Delivery report shows how each order (down to the line item and ad levels) is delivering across particular inventory segments, by day. Only orders (or line items or ads, depending on breakout) with activity in the given date range are shown.", 
      "title": "Order Delivery"
    }, 
    {
      "acl_resource_type": "order", 
      "code": "order_pace", 
      "context": "order", 
      "context_display_name": "Order", 
      "description": "The Order Pacing report shows the high-level delivery status of each line item the user has access to, provided that the line item has some delivery activity in the given date range. The intended uses of this report are for a user to quickly understand the state of any given order, identify line items/orders requiring intervention, and to understand how particular orders are delivering relative to one another.", 
      "title": "Order Pacing"
    }, 
    {
      "acl_resource_type": "order", 
      "code": "order_perf", 
      "context": "order", 
      "context_display_name": "Order", 
      "description": "The Order Performance report shows detailed performance information, by day, for orders (down to the ad level) with activity during the specified date range. This report is useful to understand day-by-day delivery of an order and to gauge changes in performance. Also common for sharing with 3rd parties, such as the advertiser or agency.", 
      "title": "Order Performance"
    }, 
    {
      "acl_resource_type": "order", 
      "code": "order_sum", 
      "context": "order", 
      "context_display_name": "Order", 
      "description": "The Order Summary report shows the high-level delivery status of each line item the user has access to, provided that the line item has some delivery activity in the given date range. The intended uses of this report are for a user to quickly understand the state of any given order, identify line items/orders requiring intervention, and to understand how particular orders are delivering relative to one another.", 
      "title": "Order Summary"
    }, 
    {
      "acl_resource_type": "inventory", 
      "code": "site_sum", 
      "context": "inventory", 
      "context_display_name": "Inventory", 
      "description": "The Site Summary report is designed to show delivery summary by site for the specified date range.", 
      "title": "Site Summary"
    }, 
    {
      "acl_resource_type": "order", 
      "code": "video_perf", 
      "context": "video", 
      "context_display_name": "Video", 
      "description": "The Video Events By Advertiser Report provides a time-based view of all VAST tracking events for video ads belonging to the advertiser.", 
      "title": "Video Events By Advertiser"
    }, 
    {
      "acl_resource_type": "order", 
      "code": "mkt_vol_by_cat", 
      "context": "market-wide", 
      "context_display_name": "Market-wide Volume", 
      "description": "The Market-wide Volume Category Breakdown report is designed to provide total requests in the Market broken down by category.", 
      "title": "Category Breakdown"
    }, 
    {
      "acl_resource_type": "order", 
      "code": "mkt_vol_by_geo", 
      "context": "market-wide", 
      "context_display_name": "Market-wide Volume", 
      "description": "The Market-wide Volume Geo Breakdown report is designed to provide total requests in the Market broken down by geography.", 
      "title": "Geo Breakdown"
    }, 
    {
      "acl_resource_type": "order", 
      "code": "mkt_vol_by_size", 
      "context": "market-wide", 
      "context_display_name": "Market-wide Volume", 
      "description": "The Market-wide Volume Ad Size Breakdown report is designed to provide total requests in the Market broken down by individual ad size.", 
      "title": "Ad Size Breakdown"
    }, 
    {
      "acl_resource_type": "order", 
      "code": "order_perf_by_geo", 
      "context": "order", 
      "context_display_name": "Performance", 
      "description": "The Geo Breakdown report is designed to show performance breakdown by geography.", 
      "title": "Geo Breakdown"
    }, 
    {
      "acl_resource_type": "order", 
      "code": "order_perf_by_lineitem", 
      "context": "order", 
      "context_display_name": "Performance", 
      "description": "The Line Item Breakdown report is designed to show performance breakdown by individual line item.", 
      "title": "Line Item Breakdown"
    }, 
    {
      "acl_resource_type": "order", 
      "code": "order_perf_by_order", 
      "context": "order", 
      "context_display_name": "Performance", 
      "description": "The Order Breakdown report is designed to show performance breakdown by individual orders.", 
      "title": "Order Breakdown"
    }, 
    {
      "acl_resource_type": "order", 
      "code": "order_perf_by_size", 
      "context": "order", 
      "context_display_name": "Performance", 
      "description": "The Ad Size Breakdown report is designed to show performance breakdown by individual ad size.", 
      "title": "Ad Size Breakdown"
    }, 
    {
      "acl_resource_type": "order", 
      "code": "order_perf_by_time", 
      "context": "order", 
      "context_display_name": "Performance", 
      "description": "The Time Period Breakdown report is designed to show performance breakdown by time period.", 
      "title": "Time Period Breakdown"
    }, 
    {
      "acl_resource_type": "order", 
      "code": "order_perf_detail", 
      "context": "order", 
      "context_display_name": "Performance", 
      "description": "The Detailed Performance report shows detailed performance information, by day and site, for orders (down to the ad level) with activity during the specified date range.", 
      "title": "Detailed Delivery"
    }, 
    {
      "acl_resource_type": "order", 
      "code": "order_techno_device", 
      "context": "device-demand", 
      "context_display_name": "Device", 
      "description": "The Device Breakdown report is designed to show performance of orders breakdown by Manufacture and Model.", 
      "title": "Device Breakdown"
    }, 
    {
      "acl_resource_type": "order", 
      "code": "order_techno_os", 
      "context": "device-demand", 
      "context_display_name": "Device", 
      "description": "The OS Breakdown report is designed to show performance of orders breakdown by operating system.", 
      "title": "OS Breakdown"
    }, 
    {
      "acl_resource_type": "inventory", 
      "code": "inv_rev_wari", 
      "context": "inventory", 
      "context_display_name": "Inventory", 
      "description": "The Inventory Revenue report is used to understand how revenue is spread across various dimensions of inventory, by sales channel, by day, for transactions that fit the specified date range.", 
      "title": "Inventory Revenue (with Auto Refreshed Impressions)"
    }, 
    {
      "acl_resource_type": "order", 
      "code": "order_perf_by_domain", 
      "context": "order", 
      "context_display_name": "Performance", 
      "description": "The Domain Breakdown report is designed to show performance breakdown by domain.", 
      "title": "Domain Breakdown"
    }
  ], 
  "lang": "en", 
  "status": "OK"
}
GET /report/get_report_inputs. Determine the needed parameters for the specified report.
Sample request for report inputs:
curl http://openx_server_name/ox/4.0/report/get_report_inputs?report=inv_perf_pub --cookie "openx3_access_token=token_string"
OpenSample response for the Inventory Detailed Performance report (inv_perf_pub) inputs
{
  "acl_resource_type": "inventory", 
  "content": [
    {
      "UI_SRS_defined_inputVals": null, 
      "UI_affect_inputVals_of": null, 
      "UI_custom_val_input": null, 
      "UI_display_name": "Start Date", 
      "UI_display_only_when": null, 
      "UI_inputVals_fetch_method": null, 
      "UI_input_type": "date", 
      "UI_selectAll_strings": null, 
      "multi": "0", 
      "name": "start_date", 
      "required": "1", 
      "type": "java.lang.Long"
    }, 
    {
      "UI_SRS_defined_inputVals": null, 
      "UI_affect_inputVals_of": null, 
      "UI_custom_val_input": null, 
      "UI_display_name": "End Date", 
      "UI_display_only_when": null, 
      "UI_inputVals_fetch_method": null, 
      "UI_input_type": "date", 
      "UI_selectAll_strings": null, 
      "multi": "0", 
      "name": "end_date", 
      "required": "1", 
      "type": "java.lang.Long"
    }, 
    {
      "UI_SRS_defined_inputVals": null, 
      "UI_affect_inputVals_of": [
        {
          "name": "ad_unit_id", 
          "predicate": " AND site_uid: ({0})", 
          "predicate_val_field": "uid"
        }
      ], 
      "UI_custom_val_input": null, 
      "UI_display_name": "Site", 
      "UI_display_only_when": null, 
      "UI_inputVals_fetch_method": {
        "include_parent_name": false, 
        "name_field": "name", 
        "remove_duplicates": false, 
        "uid_field": "id", 
        "url_path": "oxsearch?q=name:*{0}* AND type:site"
      }, 
      "UI_input_type": "selectAll", 
      "UI_selectAll_strings": {
        "all_label": "All Sites", 
        "select_hint": "Enter a Site", 
        "select_label": "Selected Sites"
      }, 
      "multi": "1", 
      "name": "site_id", 
      "required": "0", 
      "type": "java.util.Collection"
    }, 
    {
      "UI_SRS_defined_inputVals": null, 
      "UI_affect_inputVals_of": null, 
      "UI_custom_val_input": null, 
      "UI_display_name": "Ad Unit", 
      "UI_display_only_when": null, 
      "UI_inputVals_fetch_method": {
        "include_parent_name": false, 
        "name_field": "name", 
        "remove_duplicates": false, 
        "uid_field": "id", 
        "url_path": "oxsearch?q=name:*{0}* AND type:adunit"
      }, 
      "UI_input_type": "selectAll", 
      "UI_selectAll_strings": {
        "all_label": "All Ad Units", 
        "select_hint": "Enter an Ad Unit", 
        "select_label": "Selected Ad Units"
      }, 
      "multi": "1", 
      "name": "ad_unit_id", 
      "required": "0", 
      "type": "java.util.Collection"
    }, 
    {
      "UI_SRS_defined_inputVals": null, 
      "UI_affect_inputVals_of": [
        {
          "name": "ad_unit_size", 
          "predicate": "&delivery_medium_uid={0}", 
          "predicate_val_field": "id"
        }
      ], 
      "UI_custom_val_input": null, 
      "UI_display_name": "Delivery Mediums", 
      "UI_display_only_when": null, 
      "UI_inputVals_fetch_method": {
        "include_parent_name": false, 
        "name_field": "name", 
        "remove_duplicates": false, 
        "uid_field": "uid", 
        "url_path": "options/delivery_medium_options"
      }, 
      "UI_input_type": "selectAll", 
      "UI_selectAll_strings": {
        "all_label": "All Delivery Mediums", 
        "select_hint": "Enter a Delivery Medium", 
        "select_label": "Selected Delivery Mediums"
      }, 
      "multi": "1", 
      "name": "dmid", 
      "required": "0", 
      "type": "java.util.Collection"
    }, 
    {
      "UI_SRS_defined_inputVals": null, 
      "UI_affect_inputVals_of": null, 
      "UI_custom_val_input": {
        "inputs": [
          {
            "display_name": "Width", 
            "required": "1", 
            "uid_append_str": null, 
            "uid_prepend_str": null
          }, 
          {
            "display_name": "Length", 
            "required": "1", 
            "uid_append_str": null, 
            "uid_prepend_str": null
          }
        ], 
        "uid_format": "{$0} x {$1}"
      }, 
      "UI_display_name": "Ad Unit Sizes", 
      "UI_display_only_when": null, 
      "UI_inputVals_fetch_method": {
        "include_parent_name": false, 
        "name_field": "name", 
        "remove_duplicates": true, 
        "uid_field": "size_name", 
        "url_path": "options/adunit_size_options"
      }, 
      "UI_input_type": "selectAll", 
      "UI_selectAll_strings": {
        "all_label": "All Ad Unit Sizes", 
        "select_hint": "Enter an Ad Unit Size", 
        "select_label": "Selected Ad Unit Sizes"
      }, 
      "multi": "1", 
      "name": "ad_unit_size", 
      "required": "0", 
      "type": "java.util.Collection"
    }, 
    {
      "UI_SRS_defined_inputVals": [
        {
          "name": "Site Name", 
          "uid": "SiteName"
        }, 
        {
          "name": "Ad Unit", 
          "uid": "AdUnit"
        }, 
        {
          "name": "Delivery Medium", 
          "uid": "DeliveryMedium"
        }, 
        {
          "name": "Ad Unit Size", 
          "uid": "AdUnitSize"
        }
      ], 
      "UI_affect_inputVals_of": null, 
      "UI_custom_val_input": null, 
      "UI_display_name": "Breakout by", 
      "UI_display_only_when": null, 
      "UI_inputVals_fetch_method": null, 
      "UI_input_type": "select", 
      "UI_selectAll_strings": null, 
      "multi": "1", 
      "name": "do_break", 
      "required": "0", 
      "type": "java.util.Collection"
    }, 
    {
      "UI_SRS_defined_inputVals": [
        {
          "name": "Daily", 
          "uid": "daily"
        }, 
        {
          "name": "Hourly", 
          "uid": "hourly"
        }, 
        {
          "name": "Weekly", 
          "uid": "weekly"
        }, 
        {
          "name": "Monthly", 
          "uid": "monthly"
        }
      ], 
      "UI_affect_inputVals_of": null, 
      "UI_custom_val_input": null, 
      "UI_display_name": "Report Rollup", 
      "UI_display_only_when": null, 
      "UI_inputVals_fetch_method": null, 
      "UI_input_type": "select", 
      "UI_selectAll_strings": null, 
      "multi": "0", 
      "name": "rollup",
      "required": "0", 
      "type": "java.lang.String"
    }, 
    {
      "UI_SRS_defined_inputVals": [
        {
          "name": "xls", 
          "uid": "xls"
        }, 
        {
          "name": "pdf", 
          "uid": "pdf"
        }, 
        {
          "name": "csv", 
          "uid": "csv"
        }
      ], 
      "UI_affect_inputVals_of": null, 
      "UI_custom_val_input": null, 
      "UI_display_name": "Report Format", 
      "UI_display_only_when": null, 
      "UI_inputVals_fetch_method": null, 
      "UI_input_type": "select", 
      "UI_selectAll_strings": null, 
      "multi": "0", 
      "name": "report_format", 
      "required": "0", 
      "type": "java.lang.String"
    }
  ], 
  "context": "inventory", 
  "lang": "en", 
  "report": "inv_perf_pub", 
  "status": "OK"
}
Where: "required": "1" indicates a parameter that is required when running a report. You can include other values as needed.
GET /report/run. Run the specified report with required and desired URI parameters.
Available parameters for the report are listed in the response to GET /report/get_report_inputs.
Recommended run parameters:
report. The report code, such as report=adunit_size_sum. You can retrieve the available report codes using the GET /report/get_reportlist call.
start_date
A specific date in yyyy-mm-dd HH:MM:SS format
OR
An integer for the days backward from today. For example, 7 means "seven days ago" and 0 means "starting today" (inclusive).
end_date
A specific date in yyyy-mm-dd HH:MM:SS format
OR
A negative integer for the days from now. For example, -7 means "until seven days from now" and 0 means "before today" (exclusive).
report_format. The format of the report data to download, such as report_format=csv. Size limitations include:
csv. No limitations
json. Maximum of 1000 rows
pdf. No limitations
xls. Maximum of 65,000 rows
variable_parameter. (Optional) Depending on the report code, the response from GET /report/get_report_inputs provides additional parameters
Sample run request for the Inventory Detailed Performance report:
curl http://openx_server_name/ox/4.0/report/run?report=inv_perf_pub&start_date=0&end_date=0&do_break=SiteName,AdUnit,AdUnitSize&report_format=json --cookie "openx3_access_token=token_string"
Where:
report=inv_perf_pub indicates the Inventory Detailed Performance report
start_date=0 and end_date=0 indicates today
do_break=SiteName,AdUnit,AdUnitSize indicates you want to break the report out according to site, ad units, and ad unit sizes
report_format=json indicates you want to generate a JSON response
ClosedSample Inventory Detailed Performance report in JSON format
Parse the JSON response for use with your reporting integration.
See also
List of SRS reports in OpenX UI help
report object reference

Authentication Via Client Libraries
Before you can access reports via the OpenX API, you must first be authenticated. You can use an existing OpenX client library or your own library. If you use one of these client libraries, the back-end authentication logic is automatically implemented for you via the client library and you need not perform any OAuth implementation steps. If, however, you choose to write your own client library, you will then need to implement the OAuth authentication logic yourself.
For more detailed information on how to implement authentication logic, refer to the Authentication section.
Running a Report Using the OpenX API
When generating reports, you likely will want to use a program to make these calls on an predetermined automated basis (hourly, daily, weekly, etc.). If, however, you wish to make these API calls manually, or through a browser, then the examples below provide the necessary information for you to make these individual calls through a terminal window or browser.
When making a request using the API, there are a number of parameters you may pass in your request which will present specific information you want displayed in the report. Although there are only a few required input parameters you need to pass in the request, there are a number of other parameters you can include in your request.
Note: : There are two different report formats you can use in your request: CSV & JSON. you can specify the report format by entering format (csv or JSON) in the report_format parameter.
Bid Performance Report Example
One type of report you may wish to run is the Bid Performance Report, which provides insight into bid activity based on inventory, demand, and Private Marketplace deal information. This can help you evaluate both Ad Exchange and Private Marketplace performance.
When you make the Bid Performance Report request, the report is generated with different columns based on the input parameters and the breakout columns you have specified.
Report Columns
Column	Description
Date	The date the report was generated.
PublisherName	The name of the publisher.
PublisherID	The ID associated with the publisher.
SiteName	The name of the site.
AdUnit	The name of the ad unit.
AdUnitID	The ID associated with the ad unit.
AdUnitSize	The size of the ad unit.
DeliveryMedium	The attribute reports off the "Default Delivery Medium" set at the site level.
DemandPartner	The demand side platform the buyer is using for the deal.
DemandPartnerID	The ID associated with the demand side platform the buyer is using for the deal.
Buyer	The Buyer that is represented by the Exchange entity or DSP (Demand Partner).
AdvertiserName	The Advertiser represented by the Buyer and owner of the brand being advertised.
Domain	The domain where the inventory is running (e.g. CNN.com).
DealName	The name of the deal.
ExternalDealID	The ID associated for the external deal.
DealType	The type of deal. Options are: Preferred, Private Auction, WIthin Open Auction.
DealPriority	A slider that specifies the relative priority for the deal (1-10; 1=highest, 10=lowest)
DealFloor	The price set for the deal.
DealAccountExecutive	The sales person for the deal.
DealAccountManager	The individual responsible for managing the deal.
PackageName	The name of the package.
PackageID	The ID associated with the package.
Country	The country associated with the report.
PublisherCurrency	The type of currency for the publisher (e.g. USD, Euro)
Note: : It is assumed you have already been authenticated to use the OpenX Platform API. If you are not already authenticated, use your login credentials to log into your OpenX server instance. Also, because these are daily reports, data will be displayed for a day.
The table below lists the different metrics that can be returned in a report.
Report Metrics
Metric	Description
Impressions	The total number of impressions delivered, excluding PSA, house, and companion ads for the selected date range..
Revenue	The total revenue received for this date range.
CPM	Revenue per thousand impressions.
Bid Requests	The number of bid requests sent to the demand partner
Opportunities	The number of eligible bid opportunities sent to the demand partner. An opportunity is counted each time the buyer has a chance to win an impression. For example, if the bid was eligible for two deals of different priority, and had the opportunity to bid in both deals because the first deal did not fell the impression, these would be counted as two opportunities. On the other hand, if the buyer wins the impression from the first deal by meeting the deal criteria, then the second deal does not count as an opportunity
Eligibility Rate	= Opportunities/Bid Requests
Bids	The number of positive bids.
Bid Rate	= (Bids/Bid Requests)
Win Rate	= (Impressions/Bid Requests)
Bid Fill Rate	= (Impressions/Bid Requests)
Bid CPM	(Sum of bid amounts for all bids/bids) x 1000. Average bid for this date range. Includes both winning and losing bids.
Win CPM	= (Sum of bid amounts for all winning bids/impressions) x 1000. The average winning bid for this date range.
The examples below illustrate several different types of Bid Performance Reports, each request passing different parameters. This enables you to see how passing these parameters causes the report to display different sets of information.
Note: : Please remember these are daily reports so data will be displayed for a day.
If you would like to generate a Bid Performance Report with no additional optional (breakout) columns displayed, run the following command from your terminal window:
curl '<api_url>'/ox/4.0/report/run?start_date=2016-01-19 10:00:00&end_date=2016-01-19 15:00:00&report_format=csv&report=bid_perf
The table below lists the parameters being passed in this request.
Bid Performance Report w/o Optional Columns Request Values
Parameter	Description	
Example Value
api_url	The URL for the OpenX Reporting API.	
http://qa-v2-i24-ssp-only.api-v4-qa-ca.openx.net
api_client	The API client you are using	
ox
api_version	The version of the API.	
4.0
service	The type of service.	
report
start_date	The start date displayed in the report.	
2016-01-19 10:00:00
end_date	The end date displayed in the report.	
2016-01-19 15:00:00
report_format	The output format for the report. Options are: CSV, JSON	
csv
report	The type of report being generated. Options are: Exchange (exch_perf), SSP Revenue (ssp_rev), Bid Performance (bid_perf).	
The type of report being generated. Options are: Exchange (exch), SSP Revenue (ssp_rev), Bid Performance (bid_perf).
If, however, you want to generate a Bid Performance Report, but with all optional parameters, run the following command from your terminal window.
curl '<api_url>'/ox/4.0/report/run?start_date=2016-01-19 10:00:00&end_date=2016-01-19 15:00:00&do_break=SiteName,AdUnit,AdUnitSize,DeliveryMedium,DemandPartner,Buyer,AdvertiserName,domain,DealName,DealFloor,PackageName,Country&report_format=csv&report=bid_perf
In this example, note that you are passing the values listed in the table below.
Bid Performance Report w/ Optional Columns Request Values
Parameter	Description	
Example Value
api_url	The URL for the OpenX Reporting API.	
http://qa-v2-i24-ssp-only.api-v4-qa-ca.openx.net
api_client	The API client you are using	
ox
api_version	The version of the API.	
4.0
service	The type of service.	
report
start_date	The start date displayed in the report.	
2016-01-19 10:00:00
end_date	The end date displayed in the report.	
2016-01-19 15:00:00
do_break	The breakout parameters being used in the request.	
SiteName, AdUnit, AdUnitSize, DeliveryMedium, DemandPartner, Buyer, AdvertiserName, domain, DealName, DealFloor, PackageName, Country
report_format	The output format for the report. Options are CSV and JSON.	
csv
report
The type of report.
bid_perf
To generate a Bid Performance Report report, and filter on some publishers for which you have  access, run the following command from your terminal window:
curl '<api_url>'/ox/4.0/report/run?start_date=2016-01-19 10:00:00&end_date=2016-01-19 15:00:00&do_break=SiteName,AdUnit,AdUnitSize,DeliveryMedium,DemandPartner,Buyer,AdvertiserName,domain,DealName,DealFloor,PackageName,Country&report_format=csv&pub_id=1610635512,1610636382&report=bid_perf
Note that you are passing the values listed in the table below as part of the request.
Bid Performance Report Publisher Filters Request Values
Parameter	Description	
Example Value
api_url	The URL for the OpenX Reporting API.	
http://qa-v2-i24-ssp-only.api-v4-qa-ca.openx.net
api_client	The API client.	
ox
api_version	The API version.	
4.0
service	The service used in the request.	
report
start_date	The start date displayed in the report.	
2016-01-19 10:00:00
end_date	The end date displayed in the report.	
2016-01-19 15:00:00
do_break	The breakout parameters being used in the request.	
SiteName, AdUnit, AdUnitSize, DeliveryMedium, DemandPartner, Buyer, AdvertiserName, domain, DealName, DealFloor, PackageName, Country
report_format	The output format for the report. Options are CSV and JSON.	
csv
pub_id
ID of the publishers used in the report.
1610635512, 1610636382
report
The type of report.
bid_perf
Note: When entering dates in the request, the date format must be "YYYY-MM-DD  HH:MM:SS." Please note that the time displayed is the local time for the server instance.
Making a cURL Request
You may also generate a Bid Performance Report by making a single cURL call. Like the URL example above, this example assumes you are already logged in and authenticated; however, unlike the example above, make sure you take note of the cookie information, as you will need to include this information in your cURL call.
To generate a Bid Performance Report using a single cURL call, enter the following command:
curl '<api_url>'/ox/4.0/report/run?start_date=2016-01-19 10:00:00&end_date=2016-01-19 15:00:00&do_break=SiteName,AdUnit,AdUnitSize,DeliveryMedium,DemandPartner,Buyer,AdvertiserName,domain,DealName,DealFloor,PackageName,Country&report_format=csv&report=bid_perf' -H 'Cookie: openx3_access_token=cf708d438a38193a81c953fc5c7346ca438b056a5e2b9; reports=%7B%22name%22%3A%22600000e0-acc0-fff1-8123-9c5e2e%22%2C%22success%22%3Atrue%2C%22server%22%3A%22qa-mstr-er-ca-01.ca.dc.openx.org%22%2C%22project%22%3A%22New%20External%20Reporting%22%2C%22session_timeout%22%3A21600000%2C%22sessionState%22%3A%220.0000000111489889517bca0cd3b892ee89d916aa612a4c2256dd27f476f117c7a5f5526a22aa41b7.1033.0.1.UTC.wps*_1.0.1.1.New%20External%20Reporting.03CE130C11E4499E44870080EFB525B7.0-1033.1.1_-0.1.0_-1033.1.1_10.1.0.*0%22%2C%22last_reports_session_check%22%3A1453712065071%2C%22dates_selected%22%3A%7B%22prior_start%22%3A%2220160111%22%2C%22prior_end%22%3A%2220160117%22%2C%22start%22%3A%2220160118%22%2C%22end%22%3A%2220160124%22%2C%22start_offset%22%3A7%2C%22end_offset%22%3A1%2C%22prior_start_offset%22%3A14%2C%22prior_end_offset%22%3A8%2C%22id_selected%22%3A%22last_seven_days%22%7D%7D' --compressed
Note: : The following parameters are used in this request.
Bid Performance Report cURL Request Values
Parameter	Description	
Example Value
api_url	The URL for the OpenX Reporting API.	
http://qa-v2-i24-ssp-only.api-v4-qa-ca.openx.net
api_client	The API client.	
ox
api_version	The API version.	
4.0
service	The service used in the request.	
report
start_date	The start date displayed in the report.	
2016-01-19 10:00:00
end_date	The end date displayed in the report.	
2016-01-19 15:00:00
do_break	The breakout parameters being used in the request.	
SiteName, AdUnit, AdUnitSize, DeliveryMedium, DemandPartner, Buyer, AdvertiserName, domain, DealName, DealFloor, PackageName, Country
report_format	The output format for the report. Options are CSV and JSON.	
csv
report
The type of report.
bid_perf
cookie
The session access token and cookie.
openx3_access_token=cf708d438a38193a81c953fc5c7346ca438b056a5e2b9reports=%7B%22name%22%3A%22600000e0-acc0-fff1-8123 9c5e2e%22%2C%22success%22%3Atrue%2C%22server%22%3A%22
Working with a Completed Report
Once you have structured your request and selected your output format (CSV or JSON), the API will process the request and return raw data based on the selected input parameters. You can then use this data in your preferred visualization tool (e.g. Excel, Word, etc.) to sort and analyze the data.

## Platform API reference

Objects
The OpenX Platform API includes the following objects, which support CRUD operations:
account
ad
adunit
adunitgroup
audiencesegment
comment
competitiveexclusion
conversiontag
creative
deal
floorrule
lineitem
monetization
optimization
order
package
paymenthistory
report
site
sitesection
user
Tip: Objects are identified by both their object_name_id ("ID") and object_name_uid ("UID") fields. For more details, see About IDs and UIDs.
Services
The Platform API includes the following services, which support read (GET) operations:
adquality. (UI only) An ad quality service that handles the review and blocking of creatives
audit_trail
dashboard
eventfeed
forecast_augur. (UI only) A forecasting service that calculates impression estimates for inventory and predicts availability for line items
geo
i18n. (UI only) Language tools used for localization
market
options
oxsearch. (UI only) A service for searching OpenX data used in the UI
reports. (UI only) Supports OpenX reporting services via the UI
session
subtypes

### Account object

A business unit or business relationship as it is represented in OpenX, such as an ad network, advertiser, publisher, or agency
The account object has the following calls:
GET /account. List all accounts.
GET /account/account_UID. Read the specified account.
GET /account/account_UID/default_market. Retrieve market information from the parent account.
GET /account/account_UID/generate_cookie_mapping_url_template. Retrieve data for the cookie-mapping URL form (used by the OpenX UI).
GET /account/account_UID/generate_rtb_data_url_template. Retrieve data for the Real-Time Bid Settings form (used by the OpenX UI).
GET /account/account_UID/lifetime_budget_remaining. Retrieve payments-to-date minus lifetime-spending-to-date for the specified account.
GET /account/account_UID/list_accounts. List sub-accounts for the specified account.
GET /account/account_UID/list_audience_segments. List audience segments for the specified account.
GET /account/account_UID/list_competitive_exclusions. List competitive exclusions for the specified account.
GET /account/account_UID/list_conversion_tags. List conversion tags for the specified account.
GET /account/account_UID/list_creatives. List creatives for the specified account.
GET /account/account_UID/list_orders. List orders for the specified account.
GET /account/account_UID/list_packages. List packages for the specified account.
GET /account/account_UID/list_payment_history. List payment history for the specified account.
GET /account/account_UID/list_sites. List sites for the specified account.
GET /account/account_UID/list_users. List users for the specified account.
GET /account/account_UID/monthly_budget_remaining
For Ad Exchange accounts without payments, return the monthly budget minus monthly spending.
For Ad Exchange accounts with payments, return a number calculated based on lifetime payment, lifetime spending, monthly spending to date, and monthly budget.
Otherwise, return 0.
GET /account/account_UID/payments_to_date. Return the sum of all payments up to the current date for the specified account.
GET /account/all_unlinked_lift_accounts. List all accounts that are not data-linked advertisers.
GET /account/available_fieldstype_full=account.type. List the available fields to create or update an account of the specified type.
ClosedSample response for GET /account/available_fieldstype_full=account.network
GET /account/check_ssrtb_endpoint. Validate server-side real-time bidding (SSRTB) endpoints.
GET /account/performance/account_UID. Get the performance metrics for the specified account within the (optional) date range.
Parameters
start_date
A specific date in yyyy-mm-dd HH:MM:SS format
OR
An integer for the days backward from today. For example, 7 means "seven days ago" and 0 means "starting today" (inclusive).
end_date
A specific date in yyyy-mm-dd HH:MM:SS format
OR
A negative integer for the days from now. For example, -7 means "until seven days from now" and 0 means "before today" (exclusive).
GET /account/unlinked_lift_accounts. List advertiser accounts (without third-party network) and Ad Exchange advertiser accounts (with or without third-party network).
POST /account. Create one or more accounts.
Example
Sample batch create
 openx_server_name/ox/4.0/account \
--cookie "openx3_access_token=
                                    token_string
                                
                                " \
  --data='[                {                    "account_uid": "[Code]parent_account_uid
                                parent_account_id
                            ",
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
POST /account/account_UID/clone. Create a copy of the specified account.
PUT /account. Update the specified accounts.
PUT /account/account_UID. Update the specified account.

### Ad object

ad object
The functional unit that displays in the ad space and which represents the message that an advertiser wants an end-user to view
The ad object has the following calls:
DELETE /ad. Delete the specified ads.
DELETE /ad/ad_UID. Delete the specified ad.
GET /ad. List all ads.
GET /ad/ad_UID. Read the specified ad.
GET /ad/available_fieldstype_full=ad.type. List the available fields to create or update an ad object of the specified type.
OpenSample response for GET /ad/available_fieldstype_full=ad.type_full=ad.image
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
    "has_dependencies": false, 
    "maximum": "100.0", 
    "readonly": false, 
    "required": false, 
    "type": "decimal(4,1)"
  }, 
  "alt_text": {
    "has_dependencies": false, 
    "maxlen": 25000, 
    "readonly": false, 
    "required": false, 
    "type": "varchar"
  }, 
  "click_target_window": {
    "default": "_self", 
    "has_dependencies": false, 
    "maxlen": 255, 
    "readonly": false, 
    "required": true, 
    "type": "varchar", 
    "url": "/options/click_target_window_options"
  }, 
  "click_url": {
    "has_dependencies": false, 
    "maxlen": 2000, 
    "readonly": false, 
    "required": true, 
    "type": "varchar"
  }, 
  "created_date": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "datetime"
  }, 
  "days_of_week": {
    "default": {}, 
    "has_dependencies": false, 
    "items": {
      "has_dependencies": false, 
      "readonly": false, 
      "required": false, 
      "type": "int", 
      "url": "/options/days_of_week_options"
    }, 
    "readonly": false, 
    "required": false, 
    "type": "array"
  }, 
  "deleted": {
    "auto": true, 
    "default": "0", 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "deliver_by_default": {
    "has_dependencies": false, 
    "readonly": false, 
    "required": false, 
    "type": "int"
  }, 
  "end_date": {
    "has_dependencies": false, 
    "readonly": false, 
    "required": false, 
    "type": "datetime"
  }, 
  "external_id": {
    "has_dependencies": false, 
    "maxlen": 255, 
    "readonly": false, 
    "required": false, 
    "type": "varchar"
  }, 
  "flight_timezone": {
    "default": "Advertiser", 
    "has_dependencies": false, 
    "maxlen": 255, 
    "readonly": false, 
    "required": false, 
    "type": "varchar", 
    "url": "/options/flight_timezone_options"
  }, 
  "hours_of_day": {
    "default": {}, 
    "has_dependencies": false, 
    "items": {
      "has_dependencies": false, 
      "readonly": false, 
      "required": false, 
      "type": "int", 
      "url": "/options/hours_of_day_options"
    }, 
    "readonly": false, 
    "required": false, 
    "type": "array"
  }, 
  "id": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "individual_display_cap": {
    "has_dependencies": false, 
    "readonly": false, 
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
  "lineitem_id": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "lineitem_uid": {
    "has_dependencies": false, 
    "readonly": false, 
    "required": true, 
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
  "order_id": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "order_uid": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "uid"
  }, 
  "primary_creative_id": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "primary_creative_uid": {
    "has_dependencies": false, 
    "readonly": false, 
    "required": true, 
    "type": "uid"
  }, 
  "remote_media_file": {
    "has_dependencies": false, 
    "maxlen": 255, 
    "readonly": false, 
    "required": false, 
    "type": "varchar"
  }, 
  "session_display_cap": {
    "has_dependencies": false, 
    "readonly": false, 
    "required": false, 
    "type": "int"
  }, 
  "session_duration": {
    "has_dependencies": false, 
    "readonly": false, 
    "required": false, 
    "type": "int"
  }, 
  "size": {
    "has_dependencies": false, 
    "maxlen": 255, 
    "readonly": false, 
    "required": true, 
    "type": "varchar", 
    "url": "/options/web_size_options"
  }, 
  "start_date": {
    "has_dependencies": false, 
    "readonly": false, 
    "required": false, 
    "type": "datetime"
  }, 
  "status": {
    "has_dependencies": false, 
    "maxlen": 12, 
    "readonly": false, 
    "required": true, 
    "type": "varchar", 
    "url": "/options/ad_status_options"
  }, 
  "type": {
    "auto": true, 
    "has_dependencies": false, 
    "maxlen": 255, 
    "readonly": true, 
    "required": false, 
    "type": "varchar", 
    "url": "/options/model_types"
  }, 
  "type_full": {
    "has_dependencies": false, 
    "maxlen": 64, 
    "readonly": false, 
    "required": true, 
    "type": "varchar", 
    "value": "ad.image"
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
GET /ad/performance/ad_UID. Get performance metrics for the specified ad within the (optional) date range.
Parameters
start_date
A specific date in yyyy-mm-dd HH:MM:SS format
OR
An integer for the days backward from today. For example, 7 means "seven days ago" and 0 means "starting today" (inclusive).
end_date
A specific date in yyyy-mm-dd HH:MM:SS format
OR
A negative integer for the days from now. For example, -7 means "until seven days from now" and 0 means "before today" (exclusive).
POST /ad. Create one or more ads.
POST /ad/ad_UID/clone. Create a copy of the specified ad.
POST /ad/ad_UID/recycle. Restart the specified ad.
POST /ad/ad_UID/stop. Pause the specified ad.
PUT /ad. Update the specified ads.
PUT /ad/ad_UID. Update the specified ad.

### Ad unit object

Specific areas on your site where you want to display ads
The adunit object has the following calls:
DELETE /adunit. Delete the specified ad units.
DELETE /adunit/ad_unit_UID. Delete the specified ad unit.
GET /adunit. List all ad units.
GET /adunit/ad_unit_UID. Read the specified ad unit.
GET /adunit/ad_unit_UID/generate_tag. Generate the HTML tag for the specified ad unit.
GET /adunit/ad_unit_UID/list_adunitgroups. List ad unit groups for the specified ad unit UID.
GET /adunit/available_fieldstype_full=adunit.type. List the available fields to create or update an ad unit object of the specified type (such as adunit.web).
OpenSample response for GET /adunit/available_fieldstype_full=adunit.web
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
    "has_dependencies": false, 
    "maxlen": 255, 
    "readonly": false, 
    "required": false, 
    "type": "varchar", 
    "url": "/options/web_size_options"
  }, 
  "autorefresh_settings": {
    "acl": "adunit.auto_refresh", 
    "has_dependencies": false, 
    "items": {
      "available_fields": {
        "country": {
          "has_dependencies": false, 
          "maxlen": 255, 
          "readonly": false, 
          "required": false, 
          "type": "varchar", 
          "url": "/options/country_options"
        }, 
        "delay": {
          "has_dependencies": false, 
          "readonly": false, 
          "required": true, 
          "type": "int"
        }, 
        "max": {
          "has_dependencies": false, 
          "readonly": false, 
          "required": true, 
          "type": "int"
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
  }, 
  "content_topics": {
    "acl": "adunit.content_settings", 
    "has_dependencies": false, 
    "items": {
      "has_dependencies": false, 
      "readonly": false, 
      "required": false, 
      "type": "int", 
      "url": "/options/content_topic_options"
    }, 
    "readonly": false, 
    "required": false, 
    "type": "array"
  }, 
  "content_type": {
    "acl": "adunit.content_settings", 
    "has_dependencies": false, 
    "maxlen": 255, 
    "readonly": false, 
    "required": false, 
    "type": "varchar", 
    "url": "/options/content_type_options"
  }, 
  "content_type_id": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "created_date": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "datetime"
  }, 
  "deleted": {
    "auto": true, 
    "default": "0", 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "delivery_medium_id": {
    "auto": true, 
    "default": "2", 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int", 
    "value": "2"
  }, 
  "external_id": {
    "acl": "adunit.ext_id", 
    "has_dependencies": false, 
    "maxlen": 255, 
    "readonly": false, 
    "required": false, 
    "type": "varchar"
  }, 
  "fallback_ad_tag": {
    "acl": [      "adunit.ad_tag",       "instance.market_active"    ], 
    "has_dependencies": false, 
    "maxlen": 131072, 
    "readonly": false, 
    "required": false, 
    "type": "varchar"
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
  "location": {
    "acl": "adunit.screen_location", 
    "has_dependencies": false, 
    "maxlen": 255, 
    "readonly": false, 
    "required": false, 
    "type": "varchar", 
    "url": "/options/web_location_options"
  }, 
  "location_id": {
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
  "primary_size": {
    "has_dependencies": false, 
    "maxlen": 30, 
    "readonly": false, 
    "required": false, 
    "type": "varchar", 
    "url": "/options/web_size_options"
  }, 
  "real_time_bid_floor": {
    "acl": [      "instance.market_active",       "instance.features.selling_rules_ui"    ], 
    "has_dependencies": false, 
    "maximum": "1000000.00", 
    "readonly": false, 
    "required": false, 
    "type": "decimal(8,2)"
  }, 
  "revshare": {
    "acl": [      "instance.rev_share",       "adunit.revshare"    ], 
    "available_fields": {
      "cost_model": {
        "default": "variable", 
        "has_dependencies": false, 
        "maxlen": 255, 
        "options": [          "variable",           "fixed"        ], 
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
  "site_id": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "site_uid": {
    "has_dependencies": false, 
    "readonly": false, 
    "required": true, 
    "type": "uid"
  }, 
  "sitesection_id": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "sitesection_uid": {
    "acl": "adunit.section", 
    "has_dependencies": false, 
    "readonly": false, 
    "required": false, 
    "type": "uid"
  }, 
  "status": {
    "has_dependencies": false, 
    "maxlen": 12, 
    "readonly": false, 
    "required": true, 
    "type": "varchar", 
    "url": "/options/status_options_common"
  }, 
  "tag_type": {
    "default": "js.async", 
    "has_dependencies": false, 
    "maxlen": 255, 
    "readonly": false, 
    "required": true, 
    "type": "varchar", 
    "url": "/options/web_adunit_tag_type_options"
  }, 
  "tag_type_id": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "type": {
    "auto": true, 
    "has_dependencies": false, 
    "maxlen": 255, 
    "readonly": true, 
    "required": false, 
    "type": "varchar", 
    "url": "/options/model_types"
  }, 
  "type_full": {
    "has_dependencies": false, 
    "maxlen": 64, 
    "readonly": false, 
    "required": true, 
    "type": "varchar", 
    "value": "adunit.web"
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
  }, 
  "vast_tag": {
    "acl": "adunit.vast_tag", 
    "has_dependencies": false, 
    "maxlen": 2000, 
    "readonly": false, 
    "required": false, 
    "type": "varchar"
  }
}
GET /adunit/list_ad_tags. List ad tags and basic information for the specified adunit UIDs.
Parameters
adunit_uids (Required). A comma-separated string of ad unit UIDs, such as adunit_uids=278-e0ad-ff1-8123-0c9a,288-e0ad-ff1-8123-0c9a
format (Optional). Use format=txt to download ad tags in a text file.
GET /adunit/performance/ad_unit_UID. Get the performance metrics for the specified ad unit within the (optional) date range.
Parameters
start_date
A specific date in yyyy-mm-dd HH:MM:SS format
OR
An integer for the days backward from today. For example, 7 means "seven days ago" and 0 means "starting today" (inclusive).
end_date
A specific date in yyyy-mm-dd HH:MM:SS format
OR
A negative integer for the days from now. For example, -7 means "until seven days from now" and 0 means "before today" (exclusive).
POST /adunit. Create one or more ad units.
POST /adunit/ad_unit_UID/clone. Clone the specified ad unit.
POST /adunit/list_ad_tags. List ad tags and basic information for the specified adunit UIDs.
Parameters
adunit_uids (Required). A comma-separated string of ad unit UIDs, such as adunit_uids=278-e0ad-ff1-8123-0c9a,288-e0ad-ff1-8123-0c9a
format (Optional). Use format=txt to download ad tags in a text file.
PUT /adunit. Update the specified ad units.
PUT /adunit/ad_unit_UID. Update the specified ad unit.

### adunitgroup object

Collections of ad units used for inventory to be filled with companion ads or to map items in a CMS to inventory items
The adunitgroup object has the following calls:
DELETE /adunitgroup. Delete the specified ad unit groups.
DELETE /adunitgroup/ad_unit_group_UID. Delete the specified ad unit group.
GET /adunitgroup. List all ad unit groups.
GET /adunitgroup/ad_unit_group_UID. Read the specified ad unit group.
GET /adunitgroup/ad_unit_group_UID/generate_tag. Generate the HTML tag for the specified ad unit group.
GET /adunitgroup/ad_unit_group_UID/list_ad_units. List ad units for the specified account UID.
GET /adunitgroup/available_fields. List the available fields to create or update an ad unit group.
OpenSample available_fields response
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
  "adunit_ids": {
    "auto": true, 
    "has_dependencies": false, 
    "items": {
      "has_dependencies": false, 
      "readonly": false, 
      "required": false, 
      "type": "int"
    }, 
    "readonly": true, 
    "required": false, 
    "type": "array"
  }, 
  "adunit_uids": {
    "has_dependencies": false, 
    "items": {
      "has_dependencies": false, 
      "readonly": false, 
      "required": false, 
      "type": "uid", 
      "url": "/options/adunit_options"
    }, 
    "readonly": false, 
    "required": true, 
    "type": "array"
  }, 
  "created_date": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "datetime"
  }, 
  "deleted": {
    "auto": true, 
    "default": "0", 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "delivery_medium": {
    "has_dependencies": false, 
    "maxlen": 255, 
    "readonly": false, 
    "required": true, 
    "type": "varchar", 
    "url": "/options/delivery_medium_options"
  }, 
  "delivery_medium_id": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "description": {
    "has_dependencies": false, 
    "maxlen": 25000, 
    "readonly": false, 
    "required": false, 
    "type": "varchar"
  }, 
  "external_id": {
    "has_dependencies": false, 
    "maxlen": 255, 
    "readonly": false, 
    "required": false, 
    "type": "varchar"
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
  "masteradunit_id": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "masteradunit_uid": {
    "has_dependencies": false, 
    "readonly": false, 
    "required": true, 
    "type": "uid", 
    "url": "/options/adunit_options"
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
  "site_id": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "site_uid": {
    "has_dependencies": false, 
    "readonly": false, 
    "required": true, 
    "type": "uid"
  }, 
  "status": {
    "has_dependencies": false, 
    "maxlen": 12, 
    "readonly": false, 
    "required": true, 
    "type": "varchar", 
    "url": "/options/status_options_common"
  }, 
  "type": {
    "auto": true, 
    "has_dependencies": false, 
    "maxlen": 255, 
    "readonly": true, 
    "required": false, 
    "type": "varchar", 
    "url": "/options/model_types"
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
  }, 
  "vast_tag": {
    "auto": true, 
    "has_dependencies": false, 
    "maxlen": 2000, 
    "readonly": true, 
    "required": false, 
    "type": "varchar"
  }
}
GET /adunitgroup/performance/ad_unit_group_UID. Get the performance metrics for the specified ad unit group within the (optional) date range.
Parameters
start_date
A specific date in yyyy-mm-dd HH:MM:SS format
OR
An integer for the days backward from today. For example, 7 means "seven days ago" and 0 means "starting today" (inclusive).
end_date
A specific date in yyyy-mm-dd HH:MM:SS format
OR
A negative integer for the days from now. For example, -7 means "until seven days from now" and 0 means "before today" (exclusive).
POST /adunitgroup. Create one or more ad unit groups.
POST /adunitgroup/ad_unit_group_UID/clone. Clone the specified ad unit group.
PUT /adunitgroup. Update the specified ad unit groups.
PUT /adunitgroup/ad_unit_group_UID. Update the specified ad unit group.

### audiencesegment object

An inventory object that represents a group of users with similar traits or characteristics
The audiencesegment object has the following calls:
DELETE /audiencesegment. Delete the specified audience segments.
DELETE /audiencesegment/audience_segment_UID. Delete the specified audience segment.
GET /audiencesegment. List all audience segments.
GET /audiencesegment/audience_segment_UID. Read the specified audience segment.
GET /audiencesegment/audience_segment_UID/generate_tag. Generate the HTML tag for the audience segment.
GET /audiencesegment/available_fields. List the available fields to create or update an audience segment.
GET /audiencesegment/performance/audience_segment_UID. Get the performance metrics for the specified audience segment within the (optional) date range.
Parameters
start_date
A specific date in yyyy-mm-dd HH:MM:SS format
OR
An integer for the days backward from today. For example, 7 means "seven days ago" and 0 means "starting today" (inclusive).
end_date
A specific date in yyyy-mm-dd HH:MM:SS format
OR
A negative integer for the days from now. For example, -7 means "until seven days from now" and 0 means "before today" (exclusive).
GET /audiencesegment/reach_performance/audience_segment_UID. List the reach performance data for the specified audience segment.
GET /audiencesegment/revenue_performance/audience_segment_UID. List the revenue performance data for the specified audience segment.
POST /audiencesegment. Create one or more audience segments.
POST /audiencesegment/audience_segment_UID/clone. Clone the specified audience segment.
PUT /audiencesegment. Update the specified audience segments.
PUT /audiencesegment/audience_segment_UID. Update the specified audience segment.

### audit_trail service

A service that logs changes to data (creation, modification, or deletion) and allows a system administrator to review historical changes to the following objects:
account
ad
adunit
adunitgroup
creative
conversiontag
lineitem
order
site
The audit_trail service has the following call:
GET /audit_trail/object_UID. List the audit trail for the specified object (this call only returns the audit trail for a single UID).
The limit and offset URI parameters allow pagination of the results.

### comment object

The comment object supports the logging of comments for any API object, such as via its Description field in the OpenX UI. The comment object has the following calls:
GET /comment. List all comments.
GET /comment/available_fields. List the available fields used to create or update a comment.
OpenSample available_fields response for the comment object
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
  "created_date": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "datetime"
  }, 
  "deleted": {
    "auto": true, 
    "default": "0", 
    "has_dependencies": false, 
    "readonly": true, 
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
  "modified_date": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "datetime"
  }, 
  "obj_id": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "obj_type": {
    "has_dependencies": false, 
    "maxlen": 255, 
    "readonly": false, 
    "required": true, 
    "type": "varchar", 
    "url": "/options/comment_obj_type_options"
  }, 
  "obj_uid": {
    "has_dependencies": false, 
    "readonly": false, 
    "required": true, 
    "type": "uid"
  }, 
  "text": {
    "has_dependencies": false, 
    "maxlen": 1000, 
    "readonly": false, 
    "required": true, 
    "type": "varchar"
  }, 
  "text_type": {
    "default": "text", 
    "has_dependencies": false, 
    "maxlen": 255, 
    "readonly": false, 
    "required": true, 
    "type": "varchar", 
    "url": "/options/comment_text_type_options"
  }, 
  "type": {
    "auto": true, 
    "has_dependencies": false, 
    "maxlen": 255, 
    "readonly": true, 
    "required": false, 
    "type": "varchar", 
    "url": "/options/model_types"
  }, 
  "uid": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "uid"
  }, 
  "user_id": {
    "auto": true, 
    "has_dependencies": false, 
    "readonly": true, 
    "required": false, 
    "type": "int"
  }, 
  "user_uid": {
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
GET /comment?obj_type=object_name&obj_uid=object_UID. Read the comments for the specified object.
For example, GET /comment?obj_type=lineitem&obj_uid=60062525-c002-fff1-8123-0c9a66
POST /comment. Create one or more comments.
Sample POST to create a comment for a specified line item
Request
curl -X POST --header "Content-Type: application/json" http://openx_server_name/ox/4.0/comment \
	--cookie "openx3_access_token=token_string" \
	--data='{
	"obj_type": "lineitem",
	"obj_uid": "60062525-c002-fff1-8123-0c9a66",
	"text": "A comment on a lineitem"
}'
Response
[
	{
	"account_id": "537237799",
	"account_uid": "20059927-accf-fff1-8123-0c9a66",
	"created_date": "2015-03-26 00:06:47",
	"deleted": "0",
	"id": "536870938",
	"instance_uid": "110d4d80-0c23-11e3-8ffd-0800200c9a66",
	"modified_date": "2015-03-26 00:06:47",
	"obj_id": "1611015461",
	"obj_type": "lineitem",
	"obj_uid": "60062525-c002-fff1-8123-0c9a66",
	"revision": 1,
	"text": "A comment on a line item",
	"text_type": "text",
	"type": "comment",
	"uid": "2000001a-bbbb-fff1-8123-0c9a66",
	"user_id": "536962514",
	"user_uid": "200165d2-acc0-fff1-8123-0c9a66",
	"v": "3"
	}
]
PUT /comment/comment_UID. Update the comment specified by its ID or UID.

### competitiveexclusion object

Advertiser account rules used to block competing advertisers from displaying their ads on the same webpage
The competitiveexclusion object has the following calls:
DELETE /competitiveexclusion. Delete the specified competitive exclusions.
DELETE /competitiveexclusion/competitive_exclusion_UID. Delete the specified competitive exclusion.
GET /competitiveexclusion. List all competitive exclusions.
GET /competitiveexclusion/available_fields. List the available fields used to create or update a competitive exclusion.
GET /competitiveexclusion/competitive_exclusion_UID. Read the specified competitive exclusion.
POST /competitiveexclusion. Create one or more competitive exclusions.
POST /competitiveexclusion/competitive_exclusion_UID/clone. Clone the specified competitive exclusion.
PUT /competitiveexclusion. Update the specified competitive exclusion.
PUT /competitiveexclusion/competitive_exclusion_UID. Update the specified competitive exclusion.

### conversiontag object

A conversion tag is a piece of code that tracks how users respond to ads served for orders.
The conversiontag object has the following calls:
DELETE /conversiontag. Delete the specified conversion tags.
DELETE /conversiontag/conversion_tag_UID. Delete the specified conversion tag.
GET /conversiontag. List all conversion tags.
GET /conversiontag/available_fields. List the available fields to create or update a conversion tag.
GET /conversiontag/conversion_tag_UID. Read the specified conversion tag.
GET /conversiontag/conversion_tag_UID/generate_tag. Generate the HTML tag for the specified conversion tag.
GET /conversiontag/conversion_tag_UID/list_orders. List orders for the specified conversion tag UID.
POST /conversiontag. Create one or more conversion tags.
POST /conversiontag/conversion_tag_UID/clone. Clone the specified conversion tag.
PUT /conversiontag. Update the specified conversion tags.
PUT /conversiontag/conversion_tag_UID. Update the specified conversion tag.

### creative object

The media asset to use in your ad (such as an image or video file) that you can upload to the OpenX CDN. The creative references a local or remote file.
The creative object has the following calls:
DELETE /creative. Delete the specified creatives.
DELETE /creative/creative_UID. Delete the specified creative.
GET /creative. List all creatives.
GET /creative/available_fields. List the available fields to create or update a creative.
GET /creative/creative_UID. Read the specified creative.
POST /creative. Create one or more creatives.
POST /creative/creative_UID/clone. Clone the specified creative.
POST /creative/upload_creative. Upload a creative.
PUT /creative. Update the specified creatives.
PUT /creative/creative_UID. Update the specified creative.

### dashboard service

A set of high-level reports and links provided by the Simple Reporting Service (SRS)
The dashboard service has the following calls that return a JSON or XML representation of the corresponding report data:
GET /dashboard/totals_by_advt. Return Totals by Advertiser report data.
GET /dashboard/totals_by_day. Return Totals by Day report data.
GET /dashboard/totals_by_time. Return Totals by Time report data.

### deal object

A prearranged agreement to sell specific inventory, such as a direct deal between a publisher and a demand partner
The deal object has the following calls:
DELETE /deal. Delete the specified deals.
DELETE /deal/deal_UID. Delete the specified deal.
GET /deal. List all deals.
GET /deal/available_fields. List the available fields to create or update a deal.
GET /deal/deal_UID. Read the specified deal.
GET /deal/export. Export data in CSV format for all deals.
Tip: If a deal is associated with a custom package, its package_name and package_description values in the downloaded CSV file say "Private Selection." The corresponding Package Name entry in the UI says "Custom."
POST /deal. Create a deal.
POST /deal/deal_UID/clone. Clone the specified deal.
PUT /deal. Update the specified deals.
PUT /deal/deal_UID. Update the specified deal.

### eventfeed service

The eventfeed service retrieves transaction-level delivery data for various ad serving events. For details, see working with event-level feeds.
Note: Accessing event-level feeds requires a specific OpenX configuration. For details, contact your OpenX Account Manager.
The eventfeed service has the following calls:
GET /eventfeed/available_fields?type=event_feed_type&range=number&format=format_type. List the available data sets for the specified eventfeed calls, such as click or impression. For more details, see Listing available files for an event feed and available fields.
GET /eventfeed/fetch. Download files for the specified event feed. For details, see downloading the event feed file.
GET /eventfeed/schema. Retrieve a schema file for the specified event. For details, see event feed file format.

### floorrule object

The floorrule object allows you to manage selling rules regarding the minimum price you are willing to accept for ad space and related details. It has the following calls:
DELETE /floorrule. Delete the specified floor rules.
DELETE /floorrule/floor_rule_UID. Delete the specified floor rule.
GET /floorrule. List all floor rules.
GET /floorrule/available_fields. List the available fields to create or update a floor rule.
GET /floorrule/export. Export data in CSV format for all floor rules.
Tip: If a floor rule is associated with a custom targeting selection and not a saved package, its package_name and package_description values in the downloaded CSV file say "Private Selection." The corresponding Package Name entry in the UI says "Custom."
GET /floorrule/floor_rule_UID. Read the specified floor rule.
POST /floorrule. Create a floor rule.
POST /floorrule/floor_rule_UID/clone. Clone the specified floor rule.
PUT /floorrule. Update the specified floor rules.
PUT /floorrule/floor_rule_UID. Update the specified floor rule.

### geo service

The geo service provides geolocation values you can use for targeting. It has the following calls:
GET /geo/searchq=query_string&sort=field_name&order=sort_order&size=number. List geolocation search results for the specified query string.
Parameters
order (Optional). The sort order, such as order=asc for ascending and order=desc for descending
q (Required). The query string, such as q=us
size (Recommended). The maximum number of matching results to return, such as size=100.
sort (Optional). The field to sort by, such as sort=city
POST /geo/searchq=query_string&sort=field_name&order=sort_order&size=number. List geolocation search results for the specified query string.
Parameters
order (Optional). The field to sort by, such as sort=city
q (Required). The query string, such as q=us
size (Recommended). The maximum number of matching results to return. We recommend you limit the results of your queries, such as size=100.
sort (Optional). The field to sort by, such as sort=city

### lineitem object

The lineitem object provides line item details associated with orders, such as pricing, goals, and targeting. It has the following calls:
DELETE /lineitem. Delete the specified line items.
DELETE /lineitem/line_item_UID. Delete the specified line item.
GET /lineitem. List all line items.
GET /lineitem/available_fieldstype_full=lineitem.type. List the available fields to create or update a line item of the specified type (such as lineitem.exchange).
GET /lineitem/get_passback_tag. Return pass back tags for defaulting line items.
GET /lineitem/line_item_UID. Read the specified line item.
GET /lineitem/line_item_UID/list_ads. List ads for the specified line item.
GET /lineitem/lineitems_dashboard_widget. List data for the UI line item report on the dashboard.
GET /lineitem/performance/line_item_UID. Get the performance metrics for the specified line item within the (optional) date range.
Parameters
start_date
A specific date in yyyy-mm-dd HH:MM:SS format
OR
An integer for the days backward from today. For example, 7 means "seven days ago" and 0 means "starting today" (inclusive).
end_date
A specific date in yyyy-mm-dd HH:MM:SS format
OR
A negative integer for the days from now. For example, -7 means "until seven days from now" and 0 means "before today" (exclusive).
GET /lineitem/traffic_information. List traffic data for the specified line items. Use POST to avoid any URL length issues.
POST /lineitem. Create a line item.
POST /lineitem/line_item_UID/clone. Clone the specified line item.
POST /lineitem/line_item_UID/recycle. Restart the specified line item.
POST /lineitem/line_item_UID/stop. Stop the specified line item and its ads.
POST /lineitem/traffic_information. List traffic data for the specified line items.
PUT /lineitem. Update the specified line items.
PUT /lineitem/line_item_UID. Update the specified line item.

### market service

The market service retrieves information about advertisers, brands, and market operators. It has the following calls:
GET /market/advertisers. List market advertisers.
GET /market/brands. Query for brands.
GET /market/operators. List market operators.
POST /market/brands. Query for brands.

### monetization service

The monetization service retrieves information about demand sources from the market operator. It has the following calls:
GET /monetization/demand_sources. List all demand sources.
GET /monetization/demand_sources/demand_source_ID. List the specified demand source.

### optimization object

The optimization object represents real-time selling rules with fields for targeting criteria, minimum prices, advertiser and content filters, and so on. It has the following calls:
DELETE /optimization. Delete the specified optimizations.
DELETE /optimization/optimization_UID. Delete the specified optimization.
GET /optimization. List all optimizations.
GET /optimization/available_fields. List the available fields to create or update an optimization.
GET /optimization/optimization_UID. Read the specified optimization.
POST /optimization. Create an optimization.
POST /optimization/optimization_UID/clone. Clone the specified optimization.
PUT /optimization. Update the specified optimizations.
PUT /optimization/optimization_UID. Update the specified optimization.

### options service

The options service retrieves data for the specified options key. It has the following call:
GET /options/options_key
Option keys are sometimes included in responses from available_fields calls to indicate that more information is available. For example, the account_uid field in the response for a GET /account/available_fields request shows "url": "/options/account_options". When you make that call to the options service, the response includes all the account_uid values available for your account.
Sample GET /options/account_options request and response:
Request
curl -X GET http://openx_server_name/ox/4.0/options/account_options --cookie "openx3_access_token=token_string"
Response
[
{
	"id": "6003be14-accf-fff1-8123-0c9a66", 
	"name": "advertiser_account_1", 
	"object": "account"
	}, 
	{
	"id": "6002e785-accf-fff1-8123-0c9a66", 
	"name": "advertiser_account_2", 
	"object": "account"
	}, 
	...
	{
	"id": "60022e94-accf-fff1-8123-0c9a66", 
	"name": "regular_advertiser", 
	"object": "account"
}
]
Similarly, if you make a GET /options/country_options request, the response includes a list of all the available country_of_business values. In this way, you can query the Ad Server to reference any field values you may need.
OpenDepending on your account and permissions, available option keys may include:
account_options
account_status_options
account_type_options
acquisition_type_options
action_type_options
ad_category_options
ad_delivery_options
ad_status_options
ad_type_options
adquality_report_type_options
adunit_location_options
adunit_options
adunit_size_options
adunit_type_options
advertiser_experience_options
advertiser_user_role_options
agency_experience_options
agency_user_role_options
all_ad_category_options
all_audiencesegment_options
all_creative_type_options
all_location_options
attribution_tracker_mobile_platform_options
attribution_tracking_options
audience_segment_account_options
audiencesegment_options
available_deals
bid_type_options
browser_options
budget_pacing_options
budget_spend_rate_options
budget_type_options
buyer_options
buying_model_options
click_target_window_options
comment_text_type_options﻿﻿
companion_fill_method_options
connection_speed_options
connection_type_options
content_attribute_options
content_topic_options
content_type_options
continent_options
conversiontag_options
country_options
creative_options
creative_type_options
currency_options
datacenter_options
days_of_week_options
deal_source_options
deal_status_options
deal_type_options
delivery_medium_options
delivery_type_options
demand_channel_options
device_options
email_location_options
email_size_options
email_tag_type_options
exchange_audiencesegment_options
exchange_bid_type_options
exchange_size_options
exchange_third_party_server_options
exclusivity_options
flash_background_options
flight_timezone_options
floorrule_reason_options
hours_of_day_options
inventory_audience_segment_account_options
isp_carrier_options
language_options
lift_mapping_type_options
linear_video_allowable_options
linearvideo_duration_options
linearvideo_location_options
linearvideo_size_options
lineitem_ad_delivery_options
lineitem_delivery_medium_options
lineitem_status_options
list_publishers_with_segments
locale_languages
market_advertiser_options
market_brand_group_options
market_delivery_medium_options
market_filter_region_options
market_operators
master_content_topic_options
mediation_method_options
mediation_options
mime_type_options
mobile_adunit_tag_type_options
mobile_carrier_options
mobileapp_options
mobile_converstion_type_options (sic)
mobile_location_options
mobile_platform_options
mobile_size_options
mobileweb_adunit_tag_type_options
model_types
native_adunit_tag_type_options
native_sdk_mediation_network_options﻿ ﻿
native_size_options
network_experience_options
network_lineitem_all_delivery_medium_options
network_lineitem_deal_type_options
network_lineitem_delivery_medium_options
network_user_role_options
non_linear_ad_type_options
nonexchange_audiencesegment_options
nonlinearvideo_duration_options
nonlinearvideo_location_options
nonlinearvideo_size_options
open_auction_access_options
optimization_brand_labels_market_operator_options
optimization_brands_market_operator_options
optimization_filter_market_operator_options
order_options
order_status_options
os_options
pacing_model_options
package_status_options﻿
pmp_deal_type_options
pricing_method_options
pricing_model_options
pricing_type_options
publisher_experience_options
publisher_user_role_options
report_range_options
sales_channel_options
screen_resolution_options
server_mediation_network_options
site_delivery_medium_options
site_options
sitesection_options
size_options
source_type_options
ssrtb_protocol_options
status_options_common
subtype_names
subtypes
tag_type_options
targetable_deal_lineitem_options
targetable_demand_partner_options
targetable_package_options
targeting_language_options
third_party_network_options
third_party_platform_all_options
third_party_server_options
timezone_options
video_event_type_options
videocompanion_location_options
videocompanion_size_options
web_adunit_tag_type_options
web_location_options
web_size_options

### order object

The order object provides calls to manage direct demand for ad space. Depending on your OpenX configuration, use it to manage demand for third-party networks or exchanges. It has the following calls:
DELETE /order. Delete the specified order.
DELETE /order/order_UID. Delete the specified order.
GET /order. List all orders.
GET /order/available_fields. List the available fields to create or update an order.
GET /order/order_UID. Read the specified order.
GET /order/order_UID/list_conversion_tags. List conversion tags for the specified order.
GET /order/order_UID/list_line_items. List line items for the specified order.
GET /order/performance/order_UID. Get the performance metrics for the specified order within an (optional) date range.
Parameters
start_date
A specific date in yyyy-mm-dd HH:MM:SS format
OR
An integer for the days backward from today. For example, 7 means "seven days ago" and 0 means "starting today" (inclusive).
end_date
A specific date in yyyy-mm-dd HH:MM:SS format
OR
A negative integer for the days from now. For example, -7 means "until seven days from now" and 0 means "before today" (exclusive).
GET /order/order_UID/traffic_information. List traffic information for the specified order.
POST /order. Create one or more orders.
POST /order/order_UID/clone. Clone the specified order.
POST /order/order_UID/create_lineitem_from_package/package_UID. Create a line item from the specified package UID under the specified order UID.
POST /order/order_UID/recycle. Restart the specified order.
POST /order/order_UID/stop. Stop the specified order and its line items and ads.
PUT /order. Update the specified orders.
PUT /order/order_UID. Update the specified order.

### package object

The package object represents specific inventory associated with one or more deals, such as a site section, content categories, ad sizes, page positions, and so on. It has the following calls:
DELETE /package. Delete the specified packages.
DELETE /package/package_UID. Delete the specified package.
GET /package. List packages.
GET /package/available_fields. List the available fields to create or update a package.
GET /package/package_UID. Read the specified package.
GET /package/package_UID/list_deals. List deals for the specified package.
GET /package/package_UID/list_floorrules. List floor rules for the specified package.
POST /package. Create one or more packages.
POST /package/package_UID/clone. Clone the specified package.
PUT /package. Update the specified packages.
PUT /package/package_UID. Update the specified package.

### paymenthistory object

The paymenthistory object provides payment details for a specified account. It has the following calls:
DELETE /paymenthistory. Delete the specified payment histories.
DELETE /paymenthistory/payment_history_ID. Delete the specified payment history.
GET /paymenthistory. List all payment histories.
GET /paymenthistory/available_fields. List the available fields needed to create or update a payment history.
GET /paymenthistory/payment_history_ID. Read the specified payment history.
POST /paymenthistory. Create a payment history.
POST /paymenthistory/payment_history_ID/clone. Clone the specified payment history.
PUT /paymenthistory. Update the payment histories with the specified IDs.
PUT /paymenthistory/payment_history_ID. Update the specified payment history.

### report object

The report object provides the following calls to the simple reporting service (SRS):
DELETE /report. Delete the specified reports.
DELETE /report/report_ID. Delete the specified report.
GET /report/get_reportlist. List all reports available for your account.
GET /report/available_fields. List the available fields to create or update a report.
 ClosedSample available_fields response
GET /report/download/report_ID. Download a report from the SRS. You can get a report's ID from the response to the GET /report/run call.
Sample request:
curl http://openx_server_name/ox/4.0/report/download/3bcbd450-750a-45e6-b828-5cb448a2e374 --cookie  
"openx3_access_token=token_string"
GET /report/get_report_columns?report=report_code. Return the SRS columns for the specified report code.
ClosedSample response to GET /report/get_report_columns?report=adunit_size_sum
GET /report/get_report_inputs?report=report_code. Return the SRS inputs for the specified report code, such as report=adunit_size_sum.
You can retrieve the available report codes using the GET /report/get_reportlist call.
OpenSample response to GET /report/get_report_inputs?report=adunit_size_sum
{
"acl_resource_type": "inventory", 
"content": [
{
"UI_SRS_defined_inputVals": null, 
"UI_affect_inputVals_of": null, 
"UI_custom_val_input": null, 
"UI_display_name": "Start Date", 
"UI_display_only_when": null, 
"UI_inputVals_fetch_method": null, 
"UI_input_type": "date", 
"UI_selectAll_strings": null, 
"multi": "0", 
"name": "start_date", 
"required": "1", 
"type": "java.lang.Long"
}, 
{
"UI_SRS_defined_inputVals": null, 
"UI_affect_inputVals_of": null, 
"UI_custom_val_input": null, 
"UI_display_name": "End Date", 
"UI_display_only_when": null, 
"UI_inputVals_fetch_method": null, 
"UI_input_type": "date", 
"UI_selectAll_strings": null, 
"multi": "0", 
"name": "end_date", 
"required": "1", 
"type": "java.lang.Long"
}, 
{
"UI_SRS_defined_inputVals": null, 
"UI_affect_inputVals_of": [], 
"UI_custom_val_input": null, 
"UI_display_name": "Site", 
"UI_display_only_when": null, 
"UI_inputVals_fetch_method": {
"include_parent_name": false, 
"name_field": "name", 
"remove_duplicates": false, 
"uid_field": "id", 
"url_path": "oxsearch?q=name:*{0}* AND type:site"
}, 
"UI_input_type": "selectAll", 
"UI_selectAll_strings": {
"all_label": "All Sites", 
"select_hint": "Enter a Site", 
"select_label": "Selected Sites"
}, 
"multi": "1", 
"name": "site_id", 
"required": "0", 
"type": "java.util.Collection"
}, 
{
"UI_SRS_defined_inputVals": null, 
"UI_affect_inputVals_of": null, 
"UI_custom_val_input": {
"inputs": [
{
"display_name": "Width", 
"required": "1", 
"uid_append_str": null, 
"uid_prepend_str": null
}, 
{
"display_name": "Length", 
"required": "1", 
"uid_append_str": null, 
"uid_prepend_str": null
}
], 
"uid_format": "{$0} x {$1}"
}, 
"UI_display_name": "Ad Unit Sizes", 
"UI_display_only_when": null, 
"UI_inputVals_fetch_method": {
"include_parent_name": false, 
"name_field": "name", 
"remove_duplicates": true, 
"uid_field": "size_name", 
"url_path": "options/adunit_size_options"
}, 
"UI_input_type": "selectAll", 
"UI_selectAll_strings": {
"all_label": "All Ad Unit Sizes", 
"select_hint": "Enter an Ad Unit Size", 
"select_label": "Selected Ad Unit Sizes"
}, 
"multi": "1", 
"name": "ad_unit_size", 
"required": "0", 
"type": "java.util.Collection"
}, 
{
"UI_SRS_defined_inputVals": [
{
"name": "xls", 
"uid": "xls"
}, 
{
"name": "pdf", 
"uid": "pdf"
}, 
{
"name": "csv", 
"uid": "csv"
}
], 
"UI_affect_inputVals_of": null, 
"UI_custom_val_input": null, 
"UI_display_name": "Report Format", 
"UI_display_only_when": null, 
"UI_inputVals_fetch_method": null, 
"UI_input_type": "select", 
"UI_selectAll_strings": null, 
"multi": "0", 
"name": "report_format", 
"required": "0", 
"type": "java.lang.String"
}
], 
"context": "inventory", 
"lang": "en", 
"report": "adunit_size_sum", 
"status": "OK"
}
GET /report/get_reportlist. List reports and associated metadata for the UI.
OpenSample response
{
"content": [
{
"acl_resource_type": "inventory", 
"code": "adunit_size_sum", 
"context": "inventory", 
"context_display_name": "Inventory", 
"description": "The Ad Unit Size Summary report is designed to show delivery summary by Ad Unit Size for the specified date  
range.", 
"title": "Ad Unit Size Summary"
}, 
{
"acl_resource_type": "inventory", 
"code": "adunit_sum", 
"context": "inventory", 
"context_display_name": "Inventory", 
"description": "The Ad Unit Summary report is designed to show delivery summary by ad unit for the specified date range.", 
"title": "Ad Unit Summary"
}, 
{
"acl_resource_type": "audiencesegment", 
"code": "aud_inv", 
"context": "audience", 
"context_display_name": "Audience", 
"description": "The Audience Distribution By Site report provides a time-based view of audience distribution across sites.", 
"title": "Audience Distribution By Site"
}, 
{
"acl_resource_type": "audiencesegment", 
"code": "aud_sum", 
"context": "audience", 
"context_display_name": "Audience", 
"description": "The Audience Summary report provides day-by-day audience segment analysis, including segment size and daily  
reach.", 
"title": "Audience Summary"
}, 
{
"acl_resource_type": "inventory", 
"code": "buyer_sum", 
"context": "inventory", 
"context_display_name": "Inventory", 
"description": "The Buyer Summary report is designed to show delivery summary by buyer for the specified date range.", 
"title": "Buyer Summary"
}, 
{
"acl_resource_type": "inventory", 
"code": "cust_keyval", 
"context": "inventory", 
"context_display_name": "Inventory", 
"description": "The Custom Key Value report is designed to show delivery activity across the pre-defined custom key values for  
a specified date range. This report helps users 

understand volume, sales, and performance information broken up by individual custom key values.", 
"title": "Custom Key Value"
}, 
{
"acl_resource_type": "inventory", 
"code": "inv_bill", 
"context": "inventory", 
"context_display_name": "Inventory", 
"description": "The Platform Billing Report is designed to provide a summary of the total requests and impression data in UTC,  
broken down by Publishers, Sites and Ad Units.", 
"title": "Platform Billing"
}, 
{
"acl_resource_type": "inventory", 
"code": "inv_daily_sum", 
"context": "inventory", 
"context_display_name": "Inventory", 
"description": "The Daily Summary report is designed to show delivery summary by date for the specified date range.", 
"title": "Daily Summary"
}, 
{
"acl_resource_type": "inventory", 
"code": "inv_del", 
"context": "inventory", 
"context_display_name": "Inventory", 
"description": "The Inventory Delivery report is designed to show delivery activity across various inventory dimensions to  
specific orders, line-items and ads for activity that 
occurred during the specified date range.", 
"title": "Inventory Delivery"
}, 
{
"acl_resource_type": "inventory", 
"code": "inv_perf_pub", 
"context": "inventory", 
"context_display_name": "Inventory", 
"description": "The Inventory Detailed Performance report is designed to show delivery activity across various inventory  
dimensions, for activity that occurred during the 

specified date range. This report helps users understand volume, sales, and performance information by site.", 
"title": "Inventory Detailed Performance"
}, 
{
"acl_resource_type": "inventory", 
"code": "inv_perf", 
"context": "inventory", 
"context_display_name": "Inventory", 
"description": "The Inventory Performance report is designed to show delivery activity across various inventory dimensions, for  
activity that occurred during the specified date 

range. This report helps users understand volume, sales, and performance information by site and visitor geography.", 
"title": "Inventory Performance"
}, 
{
"acl_resource_type": "inventory", 
"code": "inv_rev", 
"context": "inventory", 
"context_display_name": "Inventory", 
"description": "The Inventory Revenue report is used to understand how revenue is spread across various dimensions of  
inventory, by sales channel, by day, for transactions 

that fit the specified date range.", 
"title": "Inventory Revenue"
}, 
{
"acl_resource_type": "inventory", 
"code": "inv_sell", 
"context": "inventory", 
"context_display_name": "Inventory", 
"description": "The Inventory Sell-Through report provides insight across various inventory dimensions as to how the inventory  
is being sold. Users can better understand 

which sales channels sell what volume of inventory, how they account for revenue, and how much goes unsold.", 
"title": "Inventory Sell-Through"
}, 
{
"acl_resource_type": "order", 
"code": "mkt_del", 
"context": "market", 
"context_display_name": "Market", 
"description": "The Market Delivery report shows how each market order is delivering across particular inventory segments, by  
time frame. Only orders with activity in the 

given date range are shown.", 
"title": "Market Delivery"
}, 
{
"acl_resource_type": "order", 
"code": "mkt_perf", 
"context": "market", 
"context_display_name": "Market", 
"description": "The Market Performance report shows detailed performance information, by time period, for market orders that  
won impressions during the specified date range. 

This report is useful to understand how your inventory is being sold to real-time buyers in the Market.", 
"title": "Market Performance"
}, 
{
"acl_resource_type": "order", 
"code": "order_del", 
"context": "order", 
"context_display_name": "Order", 
"description": "The Order Delivery report shows how each order (down to the line item and ad levels) is delivering across  
particular inventory segments, by day. Only orders (or 

line items or ads, depending on breakout) with activity in the given date range are shown.", 
"title": "Order Delivery"
}, 
{
"acl_resource_type": "order", 
"code": "order_pace", 
"context": "order", 
"context_display_name": "Order", 
"description": "The Order Pacing report shows the high-level delivery status of each line item the user has access to, provided  
that the line item has some delivery activity in 

the given date range. The intended uses of this report are for a user to quickly understand the state of any given order,  
identify line items/orders requiring intervention, and 

to understand how particular orders are delivering relative to one another.", 
"title": "Order Pacing"
}, 
{
"acl_resource_type": "order", 
"code": "order_perf", 
"context": "order", 
"context_display_name": "Order", 
"description": "The Order Performance report shows detailed performance information, by day, for orders (down to the ad level)  
with activity during the specified date range. 

This report is useful to understand day-by-day delivery of an order and to gauge changes in performance. Also common for  
sharing with 3rd parties, such as the advertiser or 

agency.", 
"title": "Order Performance"
}, 
{
"acl_resource_type": "order", 
"code": "order_sum", 
"context": "order", 
"context_display_name": "Order", 
"description": "The Order Summary report shows the high-level delivery status of each line item the user has access to,  
provided that the line item has some delivery activity 

in the given date range. The intended uses of this report are for a user to quickly understand the state of any given order,  
identify line items/orders requiring intervention, and 

to understand how particular orders are delivering relative to one another.", 
"title": "Order Summary"
}, 
{
"acl_resource_type": "inventory", 
"code": "site_sum", 
"context": "inventory", 
"context_display_name": "Inventory", 
"description": "The Site Summary report is designed to show delivery summary by site for the specified date range.", 
"title": "Site Summary"
}, 
{
"acl_resource_type": "order", 
"code": "video_perf", 
"context": "video", 
"context_display_name": "Video", 
"description": "The Video Events By Advertiser Report provides a time-based view of all VAST tracking events for video ads  
belonging to the advertiser.", 
"title": "Video Events By Advertiser"
}, 
{
"acl_resource_type": "order", 
"code": "mkt_vol_by_cat", 
"context": "market-wide", 
"context_display_name": "Market-wide Volume", 
"description": "The Market-wide Volume Category Breakdown report is designed to provide total requests in the Market broken  
down by category.", 
"title": "Category Breakdown"
}, 
{
"acl_resource_type": "order", 
"code": "mkt_vol_by_geo", 
"context": "market-wide", 
"context_display_name": "Market-wide Volume", 
"description": "The Market-wide Volume Geo Breakdown report is designed to provide total requests in the Market broken down  
by geography.", 
"title": "Geo Breakdown"
}, 
{
"acl_resource_type": "order", 
"code": "mkt_vol_by_size", 
"context": "market-wide", 
"context_display_name": "Market-wide Volume", 
"description": "The Market-wide Volume Ad Size Breakdown report is designed to provide total requests in the Market broken  
down by individual ad size.", 
"title": "Ad Size Breakdown"
}, 
{
"acl_resource_type": "order", 
"code": "order_perf_by_geo", 
"context": "order", 
"context_display_name": "Performance", 
"description": "The Geo Breakdown report is designed to show performance breakdown by geography.", 
"title": "Geo Breakdown"
}, 
{
"acl_resource_type": "order", 
"code": "order_perf_by_lineitem", 
"context": "order", 
"context_display_name": "Performance", 
"description": "The Line Item Breakdown report is designed to show performance breakdown by individual line item.", 
"title": "Line Item Breakdown"
}, 
{
"acl_resource_type": "order", 
"code": "order_perf_by_order", 
"context": "order", 
"context_display_name": "Performance", 
"description": "The Order Breakdown report is designed to show performance breakdown by individual orders.", 
"title": "Order Breakdown"
}, 
{
"acl_resource_type": "order", 
"code": "order_perf_by_size", 
"context": "order", 
"context_display_name": "Performance", 
"description": "The Ad Size Breakdown report is designed to show performance breakdown by individual ad size.", 
"title": "Ad Size Breakdown"
}, 
{
"acl_resource_type": "order", 
"code": "order_perf_by_time", 
"context": "order", 
"context_display_name": "Performance", 
"description": "The Time Period Breakdown report is designed to show performance breakdown by time period.", 
"title": "Time Period Breakdown"
}, 
{
"acl_resource_type": "order", 
"code": "order_perf_detail", 
"context": "order", 
"context_display_name": "Performance", 
"description": "The Detailed Performance report shows detailed performance information, by day and site, for orders (down to  
the ad level) with activity during the specified 

date range.", 
"title": "Detailed Delivery"
}, 
{
"acl_resource_type": "order", 
"code": "order_techno_device", 
"context": "device-demand", 
"context_display_name": "Device", 
"description": "The Device Breakdown report is designed to show performance of orders breakdown by Manufacture and  
Model.", 
"title": "Device Breakdown"
}, 
{
"acl_resource_type": "order", 
"code": "order_techno_os", 
"context": "device-demand", 
"context_display_name": "Device", 
"description": "The OS Breakdown report is designed to show performance of orders breakdown by operating system.", 
"title": "OS Breakdown"
}, 
{
"acl_resource_type": "inventory", 
"code": "inv_rev_wari", 
"context": "inventory", 
"context_display_name": "Inventory", 
"description": "The Inventory Revenue report is used to understand how revenue is spread across various dimensions of  
inventory, by sales channel, by day, for transactions 

that fit the specified date range.", 
"title": "Inventory Revenue (with Auto Refreshed Impressions)"
}, 
{
"acl_resource_type": "order", 
"code": "order_perf_by_domain", 
"context": "order", 
"context_display_name": "Performance", 
"description": "The Domain Breakdown report is designed to show performance breakdown by domain.", 
"title": "Domain Breakdown"
}
], 
"lang": "en", 
"status": "OK"
}
GET /report/list_accounts_with_segments. List accounts that have at least one audience segment.
GET /report/report_ID. Read the specified report.
GET /report/revenue_impact_by_floors. Return total revenue impact for the specified brand floors.
Parameters
>floor_uids. A comma-separated list of brand floor UIDs
start_date
A specific date in yyyy-mm-dd HH:MM:SS format
OR
An integer for the days backward from today. For example, 7 means "seven days ago" and 0 means "starting today" (inclusive).
end_date
A specific date in yyyy-mm-dd HH:MM:SS format
OR
A negative integer for the days from now. For example, -7 means "until seven days from now" and 0 means "before today" (exclusive).
GET /report/run. Run the report specified in the report parameter.
Parameters
report. The report code, such as report=adunit_size_sum. You can retrieve the available report codes using the GET /report/get_reportlist call.
start_date
A specific date in yyyy-mm-dd HH:MM:SS format
OR
An integer for the days backward from today. For example, 7 means "seven days ago" and 0 means "starting today" (inclusive).
end_date
A specific date in yyyy-mm-dd HH:MM:SS format
OR
A negative integer for the days from now. For example, -7 means "until seven days from now" and 0 means "before today" (exclusive).
report_format. The format of the report data to download, such as report_format=csv. Size limitations include:
csv. No limitations
json. Maximum of 1000 rows
pdf. No limitations
xls. Maximum of 65,000 rows
variable_parameter. (Optional) Depending on the report code, the response from GET /report/get_report_inputs provides additional parameters
Sample run request for the Inventory Revenue report
curl http://openx_server_name/ox/4.0/report/run?report=inv_rev&start_date=7&end_date=0&report_format=csv --cookie "openx3_access_token=token_string"
Where:
report_format=csv indicates you want to generate a comma-separated values (CSV) file for download.
OpenSample response
{
"ReportOutput": null, 
"content": [
{
"multi": "0", 
"name": "start_date", 
"required": "1", 
"status": "OK", 
"value": "1431414000"
}, 
{
"multi": "0", 
"name": "end_date", 
"required": "1", 
"status": "OK", 
"value": "1432105199"
}, 
{
"multi": "0", 
"name": "timezone", 
"required": "0", 
"status": "OK", 
"value": "America/Los_Angeles"
}, 
{
"multi": "1", 
"name": "saleschannel", 
"required": "0", 
"status": "OK", 
"value": ""
}, 
{
"multi": "1", 
"name": "pub_id", 
"required": "0", 
"status": "OK", 
"value": "ALL,1610777317,1610777318,1610777319,1610777327,1610777344,1610777346,1610778128, ...  
,1611145505,1611145648"
}, 
{
"multi": "1", 
"name": "site_id", 
"required": "0", 
"status": "OK", 
"value": ""
}, 
{
"multi": "1", 
"name": "sec_id", 
"required": "0", 
"status": "OK", 
"value": ""
}, 
{
"multi": "1", 
"name": "ad_unit_id", 
"required": "0", 
"status": "OK", 
"value": ""
}, 
{
"multi": "1", 
"name": "dmid", 
"required": "0", 
"status": "OK", 
"value": ""
}, 
{
"multi": "1", 
"name": "location", 
"required": "0", 
"status": "OK", 
"value": ""
}, 
{
"multi": "1", 
"name": "ad_unit_size", 
"required": "0", 
"status": "OK", 
"value": ""
}, 
{
"multi": "1", 
"name": "content_topic_id", 
"required": "0", 
"status": "OK", 
"value": ""
}, 
{
"multi": "1", 
"name": "geo", 
"required": "0", 
"status": "OK", 
"value": ""
}, 
{
"multi": "1", 
"name": "do_break", 
"required": "0", 
"status": "OK", 
"value": ""
}, 
{
"multi": "0", 
"name": "rollup", 
"required": "0", 
"status": "OK", 
"value": ""
}, 
{
"multi": "0", 
"name": "platform", 
"required": "1", 
"status": "OK", 
"value": "d9d38452-792f-4100-8191-5e5e3086957d"
}, 
{
"multi": "0", 
"name": "report_format", 
"required": "0", 
"status": "OK", 
"value": "csv"
}, 
{
"multi": "0", 
"name": "user_id", 
"required": "0", 
"status": "OK", 
"value": "1610745309"
}, 
{
"multi": "0", 
"name": "user_email", 
"required": "0", 
"status": "OK", 
"value": "ad_operations@example.com"
}, 
{
"multi": "0", 
"name": "email_notify", 
"required": "0", 
"status": "OK", 
"value": ""
}, 
{
"multi": "0", 
"name": "lang", 
"required": "1", 
"status": "OK", 
"value": "en_US"
}, 
{
"multi": "1", 
"name": "exclude_columns", 
"required": "0", 
"status": "OK", 
"value": ""
}, 
{
"multi": "1", 
"name": "roles", 
"required": "0", 
"status": "OK", 
"value": ""
}
], 
"lang": "en-US", 
"report": "inv_rev", 
"report_id": "3bcbd450-750a-45e6-b828-5cb448a2e374", 
"status": "OK",
"url": "/pickup/3bcbd450-750a-45e6-b828-5cb448a2e374/inv_rev.csv"	
}
Where:
"/pickup/3bcbd450-750a-45e6-b828-5cb448a2e374/inv_rev.csv" indicates the path where you can download the CSV file. For example:
http://openx_server_name/pickup/3bcbd450-750a-45e6-b828-5cb448a2e374/inv_rev.csv
Alternatively, you can download the specified report from the SRS using the GET /report/download/report_ID call. For example:
http://openx_server_name/ox/4.0/report/download/3bcbd450-750a-45e6-b828-5cb448a2e374
GET /report/traffic_by_account_time. Return traffic-by-account-time data.
Parameters
start_date
A specific date in yyyy-mm-dd HH:MM:SS format
OR
An integer for the days backward from today. For example, 7 means "seven days ago" and 0 means "starting today" (inclusive).
end_date
A specific date in yyyy-mm-dd HH:MM:SS format
OR
A negative integer for the days from now. For example, -7 means "until seven days from now" and 0 means "before today" (exclusive).
GET /report/traffic_by_order. Return traffic-by-order data.
Parameters:
start_date
A specific date in yyyy-mm-dd HH:MM:SS format
OR
An integer for the days backward from today. For example, 7 means "seven days ago" and 0 means "starting today" (inclusive).
end_date
A specific date in yyyy-mm-dd HH:MM:SS format
OR
A negative integer for the days from now. For example, -7 means "until seven days from now" and 0 means "before today" (exclusive).
GET /report/traffic_lineitem_alerts. Return traffic line item alerts.
Parameters
start_date
A specific date in yyyy-mm-dd HH:MM:SS format
OR
An integer for the days backward from today. For example, 7 means "seven days ago" and 0 means "starting today" (inclusive).
end_date
A specific date in yyyy-mm-dd HH:MM:SS format
OR
A negative integer for the days from now. For example, -7 means "until seven days from now" and 0 means "before today" (exclusive).
GET /report/video_ad_type_options. Return a list of video ad type IDs used to filter the Video Events By Advertiser report (video_perf).
Response
[
{
"id": "3rd Party Linear Video", 
"name": "3rd Party Linear Video"
}, 
{
"id": "3rd Party Non-Linear Video", 
"name": "3rd Party Non-Linear Video"
}, 
{
"id": "Linear Video", 
"name": "Linear Video"
}, 
{
"id": "Non-Linear Video", 
"name": "Non-Linear Video"
}
]
This response is only available for accounts that have access to the Video Events By Advertiser report.
POST /report. Create a report.
POST /report/report_ID/clone. Clone the specified report.
POST /report/revenue_impact_by_floors. Return total revenue impact for the specified brand floors.
Parameters
floor_uids. A comma-separated list of brand floor UIDs
start_date
A specific date in yyyy-mm-dd HH:MM:SS format
OR
An integer for the days backward from today. For example, 7 means "seven days ago" and 0 means "starting today" (inclusive).
end_date
A specific date in yyyy-mm-dd HH:MM:SS format
OR
A negative integer for the days from now. For example, -7 means "until seven days from now" and 0 means "before today" (exclusive).
PUT /report. Update the specified reports.
PUT /report/report_ID. Update the specified report.

### session service

The session service allows communication between OpenX and a user when they log in, interact with, and log out of the system. It has the following calls:
DELETE /session. Log out to terminate the API session.
GET /session. Log in to OpenX.

### site object

The site object represents a collection of ad unit inventory. It has the following calls:
DELETE /site. Delete the specified sites.
DELETE /site/site_UID. Delete the specified site.
GET /site. List all sites.
GET /site/available_fields?account_uid=publisher_account_UID. List the available fields to create or update a site for the specified publisher account.
GET /site/list_ad_tags. List ad tags and other information for the specified sites.
Parameters:
site_uids. (Required) A comma-separated string of site UIDs
format. (Optional) Set format=txt to export ad tags in a text file
GET /site/performance/site_UID. Get performance metrics for the specified site within the (optional) date range.
Parameters:
start_date
A specific date in yyyy-mm-dd HH:MM:SS format
OR
An integer for the days backward from today. For example, 7 means "seven days ago" and 0 means "starting today" (inclusive).
end_date
A specific date in yyyy-mm-dd HH:MM:SS format
OR
A negative integer for the days from now. For example, -7 means "until seven days from now" and 0 means "before today" (exclusive).
GET /site/site_UID. Read the specified site.
GET /site/site_UID/list_ad_unit_groups. List ad unit groups for the specified site.
GET /site/site_UID/list_ad_units. List ad units for the specified site.
GET /site/site_UID/list_site_sections. List sections of the specified site.
POST /site. Create a site.
POST /site/list_ad_tags. List ad tags and other information for the specified sites.
Parameters:
site_uids. (Required) A comma-separated string of site UIDs
format. (Optional) Set format=txt to export ad tags in a text file
POST /site/site_UID/clone. Clone the specified site.
POST /site/upload_icon. Upload an icon for the specified site.
PUT /site. Update the specified sites.
PUT /site/site_UID. Update the specified site.

### sitesection object

The sitesection object represents a grouping of ad space in a site. It has the following calls:
DELETE /sitesection. Delete the specified site sections.
DELETE /sitesection/site_section_UID. Delete the specified site section.
GET /sitesection. List all site sections.
GET /sitesection/available_fields. List the available fields to create or update a site section.
GET /sitesection/performance/site_section_UID. Get performance metrics for the specified site section within the (optional) specified date range.
Parameters:
start_date
A specific date in yyyy-mm-dd HH:MM:SS format
OR
An integer for the days backward from today. For example, 7 means "seven days ago" and 0 means "starting today" (inclusive).
end_date
A specific date in yyyy-mm-dd HH:MM:SS format
OR
A negative integer for the days from now. For example, -7 means "until seven days from now" and 0 means "before today" (exclusive).
GET /sitesection/site_section_UID. Read the specified site section.
POST /sitesection. Create a site section.
POST /sitesection/site_section_UID/clone. Clone the specified site section.
PUT /sitesection. Update the specified site sections.
PUT /sitesection/site_section_UID. Update a the specified site section.

### subtypes service

The subtypes service retrieves the subtypes that can be created for the various object, such as subtypes of the account, lineitem, order, and site objects. It has the following call:
GET /subtypes
OpenSample GET /subtypes response
{
  "account": [
    {
      "account": [
        "account.advertiser", 
        "account.publisher", 
        "account.network", 
        "account.agency"
      ], 
      "rule": {
        "type_full": "account.network"
      }
    }, 
    {
      "adproduct": true, 
      "audiencesegment": true, 
      "optimization": true, 
      "rule": {
        "type_full": "account.publisher"
      }
    }, 
    {
      "rule": {
        "type_full": "account.advertiser"
      }
    }
  ], 
  "lineitem": [
    {
      "rule": {
        "delivery_medium": "EMAIL"
      }
    }, 
    {
      "rule": {
        "delivery_medium": "WEB", 
        "type_full": [
          "lineitem.exclusive", 
          "lineitem.house", 
          "lineitem.non_guaranteed", 
          "lineitem.share_of_voice", 
          "lineitem.volume_goal"
        ]
      }
    }, 
    {
      "rule": {
        "delivery_medium": "LINEARVIDEO"
      }
    }, 
    {
      "rule": {
        "delivery_medium": "NONLINEARVIDEO"
      }
    }, 
    {
      "rule": {
        "delivery_medium": "MOBILE", 
        "type_full": [
          "lineitem.exclusive", 
          "lineitem.house", 
          "lineitem.non_guaranteed", 
          "lineitem.share_of_voice", 
          "lineitem.volume_goal"
        ]
      }
    }, 
    {
      "rule": {
        "ad_delivery": "companion", 
        "delivery_medium": "LINEARVIDEO"
      }
    }, 
    {
      "rule": {
        "ad_delivery": "companion", 
        "delivery_medium": "NONLINEARVIDEO"
      }
    }, 
    {
      "rule": {
        "ad_delivery": [
          "equal", 
          "manual"
        ], 
        "delivery_medium": "NONLINEARVIDEO"
      }
    }, 
    {
      "rule": {
        "ad_delivery": [
          "equal", 
          "manual"
        ], 
        "delivery_medium": "LINEARVIDEO"
      }
    }, 
    {
      "rule": {
        "bid_type": "PRICEDBID", 
        "type_full": "lineitem.exchange"
      }
    }, 
    {
      "rule": {
        "bid_type": "SSRTB", 
        "type_full": "lineitem.exchange"
      }
    }, 
    {
      "rule": {
        "bid_type": "SSRTBDEALS", 
        "type_full": "lineitem.exchange"
      }
    }
  ], 
  "order": [
    {
      "rule": {}
    }
  ], 
  "site": [
    {
      "adunitgroup": true, 
      "rule": true, 
      "sitesection": true
    }, 
    {
      "rule": {
        "delivery_medium": [
          "WEB", 
          "LINEARVIDEO", 
          "NONLINEARVIDEO", 
          "EMAIL", 
          "VIDEOCOMPANION", 
          null
        ]
      }
    }, 
    {
      "rule": {
        "delivery_medium": [
          "WEB", 
          "LINEARVIDEO", 
          "NONLINEARVIDEO", 
          "EMAIL", 
          "VIDEOCOMPANION", 
          null
        ]
      }
    }, 
    {
      "rule": {
        "delivery_medium": "MOBILE"
      }
    }
  ]
}

### user object

The user object represents an individual who will be logging in to OpenX and performing tasks on behalf of the accounts to which they have access. It has the following calls:
  - DELETE /user. Delete the specified users.
  - DELETE /user/user_ID. Delete the specified user.
  - GET /user. List all users.
  - GET /user/available_fields. List the available fields to create or update a user.
  - GET /user/user_ID. Read the specified user.
  - POST /user. Create a user.
  - POST /user/user_ID/clone. Clone the specified user.
  - POST /user/accept_terms. Special handling needed for accepting terms.
  - PUT /user. Update the specified user.
  - PUT /user/user_ID. Update the specified user.

## Authentication reference

What is authentication?
If you are developing your own client application to access OpenX, you will have to authenticate your users. OpenX authentication follows the OAuth 1.0 protocol. From the OAuth specification:
The OAuth protocol enables websites or applications (Consumers) to access Protected Resources from a web service (Service Provider) via an API, without requiring Users to disclose their Service Provider credentials to the Consumers. More generally, OAuth creates a freely-implementable and generic methodology for API authentication.
The following sections provide a brief overview that describes how to code your own OAuth scheme to access the OpenX API.

### Before you begin

What You Will Need
If you decide to implement your own authentication code, here is what you will need:
Basic knowledge of OAuth. For more information you can consult the following recommended sites: OAuth specification, The OAuth Bible, and Twitter Developers Documentation: OAuth.
Knowledge of the HTTP protocol
For programmatic authentication, you need to have knowledge of a client language such as PHP, Python, or Java. Note that OpenX already provides client libraries for each of these languages.
Credentials Supplied by OpenX
When you become an API customer, OpenX provides you with the following credentials which you will use in initial authentication calls.
Credentials Supplied by OpenX
Parameter	Description
Username	Your account username provided by your account manager
Password	Your account password provided by your account manager
Consumer Key	The ID portion of your Consumer credentials, provided by your account manager
Consumer Secret	The Consumer secret can be thought of as the password for the Consumer credentials. This is also provided by your account manager
OAuth Realm	The realm value is a string, generally assigned by the origin server. The realm parameter allows the protected resources on a server to be partitioned. For example, OAuth realm="http://server.example.com/"

### About OAuth

The following section briefly describes the concepts and workflow of the OAuth authentication process.
Tip: For more details about OAuth, refer to RFC 5849.
Roles
The OAuth process involves the following three entities:

The diagram above shows the following:
The double-headed arrow between the User and Consumer shows that a request and response is made between the two.
The double-headed arrow between the Consumer and the Service Provider shows that a request and response takes place between the two.
The arrow at the bottom shows that, after the OAuth process, the User is able to access resources from the Service Provider.
OAuth Roles
OAuth Role	Description	OpenX Implementation
Service Provider	A web application that allows access via OAuth.	OpenX
Consumer	A website or application that uses OAuth to access the Service Provider on behalf of the User.	Your client application
User	An individual who has an account with the Service Provider.	Users of your client application
URLs
OAuth defines three request URLs:
URLs
URL Type	Description	OpenX Implementation
Request Token URL	Used to obtain an unauthorized Request Token	https://sso.openx.com/api/index/initiate
User Authorization URL	Used to obtain User authorization for Consumer access	Browser-based: https://sso.openx.com/login/login Programmatic: https://sso.openx.com/login/process
Access Token URL	Used to exchange the User-authorized Request Token for an Access Token	https://sso.openx.com/api/index/token
Parameters
For a complete list of parameters, see the OAuth specification. There are a few parameters worth mentioning:
Parameters
Parameter	Description
Oauth_nonce	This parameter contains a nonce or "only once string"; it is a unique string that changes on each OAuth request. There is no specification on how the nonce should be constructed, but it is important to make sure it changes on each call; it is how the server knows there are no duplicate requests.
oauth_token	This value changes based on the stage of the OAuth handshake. Before authorization and when getting a request token, this parameter is excluded entirely. When retrieving an access token, this parameter is set to the request token. Once an access token has been obtained, this parameter, for all future requests, is set to the access token.
UNIX timestamp	The oauth_timestamp field requires a UNIX timestamp. Some programming libraries may provide a function for this.
Process Overview
The following graphic illustrates the flow of information in the OAuth process. It takes place is basically three steps:
The Consumer obtains an unauthorized Request Token (Steps 1.1 and 1.2)
The User authorized the Request Token (Steps 2.1, 2.2, and 2.3)
The Consumer exchanges the Request Token for an Access Token (Steps 3.1 and 3.2)
The numbered steps in the graphic correspond to the numbered steps in this section.

Solid arrow - Person using web browser/Manual entry
Dotted arrow - Consumer/Service Provider

### About signing requests

Signing Requests
OAuth defines a method for validating the authenticity of HTTP requests. This method is called Signing Requests. Instead of sending full credentials (specifically passwords), OAuth uses digital signatures with each request. Digital signatures allow the recipient to verify that the content of the request has not changed in transit. To do that, the sender uses a mathematical algorithm to calculate the signature of the request and includes it with the request.
All of your requests must be signed using the HMAC-SHA1 hashing algorithm. The server checks your hash to make sure it matches the hash it generates. If it does, the data you sent has not been compromised. The following explains how to sign requests.
Base String
To authenticate programmatically:
Create a "base string" using the "resource url" and the HTTP method (POST, GET) you will use before that:
POST http://sso_openx.com
Next, sort all of your parameters in alphabetical order. (Each request will be accompanied by a set of parameters. Each step in the process requires a different set of parameters, as described in the following sections. Alphabetically sorting them ensures that the hashing algorithm produces the same result on both Consumer and Provider sides).
Add a URL-encoded equal sign and value to each parameter. For every parameter except the last one, add a URL-encoded ampersand.
Add all of the parameters to the string you started in step 1.
The result is the final Base String. An example Python base string is shown below.
POST&http%3A%2F%2Fsso_openx.com%2Fapi%2Ftest.json&parama%3Dparamaval%26paramb%3Dparambval %26version%3D1.0
The parameter name and parameter value should be URL-encoded. You can build a function to handle URL encoding. Also make sure the URL itself is encoded.
Hashing
The Base String is hashed using token credentials. Token credentials are different depending on what part of the OAuth handshake you are currently performing. If you're asking for a request token, your token credentials are the Consumer secret followed by an ampersand (&) often called a "dangling ampersand". This is because you don't have a token yet, so there is no token secret.
If you have a request token and are requesting an access token, your token credentials are the Consumer secret, an ampersand (&), and the request token secret. If youÌve received authorization and can now make requests on the User's behalf, your token credentials are the Consumer secret, followed by an ampersand (&), followed by the access token secret. Whichever string is relevant to you, this string is what you apply as the key when applying the HMAC-SHA1 hashing algorithm. The Base String is the value to hash. You send your final signature to the server along with the rest of the request as the oauth_signature parameter.
URL Encoding
When you transmit special characters over the internet, they must be encoded so that they do not get misrepresented as meaningful characters. URL-encoding converts special-purpose characters such as the ampersand into hexadecimal notation based on the character's ASCII value. For instance, the ASCII value for a space ( ) character is 32. In binary, it looks like:
0010 0000
which is 0x20 in hexadecimal notation, so the URL-encoded space character is:
%20
Most language libraries, such as Python, provide a function to encode URLs.
Executing an HTTP Call
OAuth specifies three ways to send credentials:
On the query string (not recommended)
POST parameters (OK, but may have problems)
The authorization HTTP header (recommended)
A header is at the front of an HTTP request and gives the server information about the type of request, the length in bytes of the request, etc. For OAuth, one more header is added: "Authorization". The following shows the header. Line breaks have been inserted for readability, but when building your header, you would not include these line breaks.
Authorization: OAuth oauth_consumer_key="xxxx",
oauth_token="xxxx",
oauth_version="1.0",
oauth_signature="xxxx",
...

### Methods of authentication

You can authenticate in two ways:
Browser-Based - Use browser-based authentication if you are providing an interactive web-based login form for users other than yourself.
Programmatically - Use programmatic authentication if you are accessing OpenX protected resources with your own credentials only and don't plan on providing a web-based login form for other users.
Technical Differences
The following sections show the differences between browser-based and programmatic authentication.
URL Differences
Browser-based	Different?	Programmatic
https://sso.openx.com/api/index/initiate	No	https://sso.openx.com/api/index/initiate
https://sso.openx.com/login/login	Yes	https://sso.openx.com/login/process
https://sso.openx.com/api/index/token	No	https://sso.openx.com/api/index/token.
Callback URL Difference
Browser-based	Programmatic
Your applications' callback URL	oob (out of band)
Cookie Difference
Browser-based	Programmatic
Does not require that you persist the access token in a cookie. When the user has completed the session, the access token will be released.	Requires that you persist the access token in a cookie named openx3_access_token until the session is complete.

#### Browser-based authentication

Use Case: You would use browser-based authentication if you are providing an interactive web-based login form for users other than yourself.
Browser-based SSO logins redirect the User back to their application and pass oauth_token and oauth_verifier onto the end of the specified callbackUrl.
Authenticating a User using OAuth involves the following steps:
Step 1 - The Consumer (your application) requests and obtains an unauthorized request token
Step 2 - Authorize the User
Step 3 - Request an access token
Step 4 - Use the access token to access protected resources
These steps are described in detail below.
Step 1 - The Consumer (your application) requests and obtains an unauthorized request token
The client application requests and obtains a request token from the OAuth server (https://sso.openx.com/api/index/initiate). The request command also passes in the value of the callbackUrl parameter (the URL to which the OAuth server will redirect the User upon successful authentication).
Sub-step Overview
Sub-step 1.1: The client application sends a POST request to https://sso.openx.com/api/index/initiate including the parameters listed below.
Sub-step 1.2: When OpenX receives the request, it verifies the request using its copy of the Consumer secret and returns a response to the client application, including the request token
Each sub-step is explained below.
Sub-step 1.1 The client application sends a POST request for a request token
The first step requests a request token using the parameters listed below.
Request parameters
Parameter	Description
oauth_realm	The realm value is a string, generally assigned by the origin server. The realm parameter allows the protected resources on a server to be partitioned. For example, oauth realm="http://server.example.com/".
oauth_consumer_key	The Consumer Key, supplied to you by OpenX.
oauth_signature_method	The signature method the Consumer used to sign the request.
oauth_signature	The signature is created using the signature process which encodes the Consumer Secret and Token Secret into a verifiable value which is included with the request. For the first request (for the request token), no Token Secret yet exists. So, the signature creation process uses the Consumer Secret and an empty Token Secret.
oauth_timestamp	The timestamp is expressed in the number of seconds since January 1, 1970 00:00:00 GMT. The timestamp value MUST be a positive integer and MUST be equal or greater than the timestamp used in previous requests.
oauth_nonce	The Consumer must generate a Nonce value that is unique for all requests with the timestamp. A nonce is a random string, uniquely generated for each request. The nonce allows the Service Provider to verify that a request has never been made before and helps prevent replay attacks when requests are made over a non-secure channel (such as HTTP).
oauth_version	Optional. If present, value must be 1.0. Service Providers must assume the protocol version to be 1.0 if this parameter is not present. Service Providers' response to non-1.0 value is left undefined.
oauth_callback	For browser-based authentication, use this parameter to specify to which URL the User will be re-directed. For programmatic authentication, use oob (out of band).
Sub-step 1.2 Service Provider (OpenX) returns an unauthorized request token
The result of step one provides your client application (the Consumer) with an unauthorized request token and oauth_token_secret. The next step allows your User to authorize the request token. To do so, the client application redirects the User to the authorization server (OpenX) login page.
Step 2 - Authorize the User
The User can now authenticate to SSO using the Authorize URL (https://sso.openx.com/login/login), passing in the request token. If successful, the SSO server redirects the User to the callbackUrl (with the oauth_token and oauth_verifier query string parameters appended).
Sub-step Overview
Sub-step 2.1: For browser-based authentication, the client application (Consumer) should redirect the User to a login page using a POST request using the url: https://sso.openx.com/login/login. The POST request also passes in the unauthorized request token (oauth_token).
Sub-step 2.2: The User grants access to the client application by entering a valid username and password. The OpenX authorization server generates an authorized request token.
Sub-step 2.3: The Service Provider redirects the User to the URL specified by the callback URL (callbackUrl). The response includes both the oath_token and the oath_verifier.
Each sub-step is explained below.
Sub-step 2.1 Consumer directs the User to Service Provider (OpenX)
The Consumer (your app) redirects the User to the Service Provider by constructing an HTTP GET request to the Service Provider's User Authorization URL with the following parameters:
Parameters
Parameter	Description
oauth_token	The unauthorized request token obtained in the previous step.
oauth_callback	The Consumer should specify a URL the Service Provider will use to redirect the User back to the Consumer when Obtaining User Authorization is complete.
Sub-step 2.2 The Service Provider obtains the User's user name and password
The Service Provider verifies the User's identity by obtaining the User's username and password.
Sub-step 2.3 The Service Provider redirects the User back to the Consumer site
After the User authenticates with the Service Provider and grants permission for Consumer access, the Service Provider notifies the Consumer that the Request Token has been authorized by doing the following: The Service Provider constructs an HTTP GET request URL using the provided oauth_callback, and redirects the User's web browser back to that URL with the following parameters:
Parameters
Parameter	Description
oauth_token	The Request Token the User authorized or denied.
oauth_verifier	The verification code.
Step 3 - Request an access token
After the User has been authenticated in Step 2, in Step 3, the client application exchanges the authorized request token for an access token. The client application makes a POST request to https://sso.openx.com/api/index/token while passing the required parameters including oath_token and oath_verifier. The OpenX authorization server verifies the parameters and returns the access token in the response.
The Consumer exchanges the (temporary) Request Token for a (permanent) Access Token capable of accessing the Protected Resources. Obtaining an Access Token includes the following sub-steps:
Sub-step Overview
Sub-step 3.1: Consumer (your application) requests an access token
Sub-step 3.2: The Service Provider (OpenX) grants an access token
Each step is explained below.
3.1. The Consumer (Your application) requests an access token
The Request Token and Token Secret are exchanged for an Access Token and Token Secret. To request an Access Token, the Consumer makes an HTTP request to the Service Provider's Access Token URL. The Service Provider documentation specifies the HTTP method for this request, an HTTP POST is recommended. The request MUST be signed per Signing Requests, and contains the following parameters:
Request parameters
Parameter	Description
oauth_consumer_key	The Consumer Key, supplied to you by OpenX.
oauth_token	The Request Token obtained previously.
oauth_signature_method	The signature method the Consumer used to sign the request.
oauth_signature	The signature is created using the signature process which encodes the Consumer Secret and Token Secret into a verifiable value which is included with the request.
oauth_timestamp	The timestamp. See previous description.
oauth_nonce	The Consumer must generate a Nonce value that is unique for all requests with the timestamp. See previous description.
oauth_version	Optional. If present, value must be 1.0. Service Providers must assume the protocol version to be 1.0 if this parameter is not present. Service Providers' response to non-1.0 value is left undefined.
oauth_callback	For browser-based authentication, use this parameter to specify to which URL the User will be re-directed. For programmatic authentication, use oob (out of band).
No additional Service Provider specific parameters are allowed when requesting an Access Token to ensure all Token related information is present prior to seeking User approval.
3.2. Service Provider Grants an Access Token (Response)
The Service Provider generates an Access Token and Token Secret and returns them in the HTTP response body. Your application must store the Access Token and Token Secret and use them when signing Protected Resources requests. The response contains the following parameters:
Parameters
Parameter	Description
oauth_token	The Access Token.
oauth_token_secret	The Token Secret.
Step 4 - Use the access token to access protected resources
The client application uses the access token to perform OpenX API operations.

#### Programmatic authentication

Use Case: You would use programmatic authentication if you are accessing OpenX protected resources with your own credentials only and don't plan on providing a web-based login form for other Users.
To run automated processes, include a valid username and password in your code or make them available to the code. If successful, programmatic logins return oauth_token and oauth_verifier in the body of the response.
Important: Your client application must be able to persist cookies across an HTTP 302 redirect in a cookie named openx3_access_token, which must be present in all API requests.
Authenticating a User using OAuth involves the following steps:
  - Step 1 - Request an unauthorized request token
  - Step 2 - Authorize the User
  - Step 3 - Request an access token
  - Step 4 - Use the access token to access protected resources
These steps are the same as Browser-Based Authentication except for a few details, which will be explained in each step. Only the differences will be explained in this procedure.

Step 1 - Request an unauthorized request token
Difference between programmatic and browser-based authentication:
Because this is programmatic authentication instead of browser-based, you must set the callbackUrl to oob (out-of-band), which tells the OAuth server that you are not redirecting a User to a URL. The OAuth Server returns the request token.

Step 2 - Authorize the User
Difference between programmatic and browser-based authentication:
Authorize the request token by sending an HTTP POST request to https://sso.openx.com/login/process with the following parameters:
Request parameters
Parameter	Description
Email	The User's email address
Password	The User's password
oauth_token	The OAuth request token
Sub-steps
For programmatic authentication, the client application must pass in the request token (oauth_token) and the User's email and password to https://sso.openx.com/login/process (NOT https://sso.openx.com/login/login).

Step 3 - Request an access token
Difference between programmatic and browser-based authentication:
This process is the same as browser-based authentication with the addition of a third sub-step:
Sub-step 3.3: Consumer (your application) persists the access token in a cookie
The Consumer (your application) should now persist the access token in a cookie so that it can be used to access the protected resources. The cookie should be named openx3_access_token, which must be present in all API requests.

Step 4 - Use the access token to access protected resources
Difference between programmatic and browser-based authentication:
None
The client application uses the access token to perform OpenX API operations.

Important: You must refresh the OpenX session at least once every two hours or your session will expire.

Important: Passwords expire after six months. Ten days before your password expires, OpenX will send you an email reminder to change your password.

Logging out
When finished with your API session, you should terminate it explicitly by sending DELETE /session to log out.

#### Programmatic authentication sample

All calls to the Platform API must be authenticated with a security token, which you can retrieve through the OpenX OAuth Server located at https://sso.openx.com. You can then include the token in subsequent API calls.
The following sample OAuth session log shows successfully signed OAuth requests using the following calls:

Step 1 - POST /api/index/initiate
Step 2 - POST /login/process
Step 3 - POST /api/index/token

In the final call to sso.openx.com/api/index/token, the oauth_token value in the response is the value used for the openx3_access_token cookie for API requests.

Step 1 - Request an unauthorized request token
The following sample shows the values for the header fields. To send these values on a command line, you could use curl but your client application will most likely transfer these values using your preferred language (PHP, Python, Ruby on Rails, etc.)
Request: POST /api/index/initiate
> POST /api/index/initiate HTTP/1.1
> Accept-Encoding: identity
> Content-Length: 0
> Host: sso.openx.com
> User-Agent: Python-urllib/2.7
> Connection: close
> Content-Type: application/x-www-form-urlencoded
> Authorization: OAuth realm="", oauth_nonce="5318660", oauth_timestamp="1387318515", 
  oauth_consumer_key="3bb1...ae5", oauth_signature_method="HMAC-SHA1", oauth_version="1.0", 
  oauth_signature="O33SvRJBlBsrVglailYYPutCmGI%3D", oauth_callback="oob"
Response to POST /api/index/initiate
> HTTP/1.1 200 OK
< Date: Tue, 17 Dec 2015 22:15:15 GMT
< Server: Apache/2.2.3 (CentOS)
< X-Powered-By: PHP/5.3.24
< Set-Cookie: PHPSESSID=e735c37c5cl9778j1on41v6rj5; path=/; secure; HttpOnly
< Expires: Fri, 17 Jan 2016 22:15:15 GMT
< Cache-Control: private; must-revalidate
< Pragma: no-cache
< Content-Length: 327
< Connection: close
< Content-Type: application/x-www-form-urlencoded
<
< oauth_token=944b...ccf3&oauth_token_secret=8111...03f&oauth_callback_confirmed=true
Step 2 - Authorize User
Request: POST /login/process
> POST /login/process HTTP/1.1
> Accept-Encoding: identity
> Content-Length: 211
> Connection: close
> User-Agent: Python-urllib/2.7
> Host: sso.openx.com
> Cookie: PHPSESSID=e735c37c5cl9778j1on41v6rj5
> Content-Type: application/x-www-form-urlencoded
> Authorization: OAuth realm="", oauth_nonce="62082529", oauth_timestamp="1387318515", 
  oauth_consumer_key="3bb1Öae5", oauth_signature_method="HMAC-SHA1", oauth_version="1.0", 
  oauth_token="944b...ccf3", oauth_signature="QTp3PJmWeXVzWQCf%2FmDZJcRxX1Y%3D", oauth_callback="oob"
>
> password=Testing123&oauth_token=944b...ccf3&email=test_account_google@openx.com
Response to POST /login/process
< HTTP/1.1 200 OK
< Date: Tue, 17 Dec 2015 22:15:15 GMT
< Server: Apache/2.2.3 (CentOS)
< X-Powered-By: PHP/5.3.24
< Expires: Fri, 17 Jan 2016 22:15:15 GMT
< Cache-Control: private; must-revalidate
< Pragma: no-cache
< Content-Length: 179
< Connection: close
< Content-Type: text/html; charset=UTF-8
<
< oob?oauth_token=944b...ccf3&oauth_verifier=fb6f21ce8e
Step 3 - Request an access token
Request: POST /api/index/token
> POST /api/index/token HTTP/1.1
> Accept-Encoding: identity
> Content-Length: 0
> Connection: close
> User-Agent: Python-urllib/2.7
> Host: sso.openx.com
> Cookie: PHPSESSID=e735c37c5cl9778j1on41v6rj5
> Content-Type: application/x-www-form-urlencoded
> Authorization: OAuth realm="", oauth_nonce="93393548", oauth_timestamp="1387318516", 
  oauth_signature_method="HMAC-SHA1", oauth_consumer_key="3bb1Öae5", oauth_verifier="fb6f21ce8e", 
  oauth_version="1.0", oauth_token="944b...ccf3", oauth_signature="QjBqYFGhCtp6vmtqDsxXElB8Mh8%3D", oauth_callback="oob"
Response to POST /api/index/token
< HTTP/1.1 200 OK
< Date: Tue, 17 Dec 2015 22:15:16 GMT
< Server: Apache/2.2.3 (CentOS)
< X-Powered-By: PHP/5.3.24
< Expires: Fri, 17 Jan 2016 22:15:16 GMT
< Cache-Control: private; must-revalidate
< Pragma: no-cache
< Content-Length: 334
< Connection: close
< Content-Type: application/x-www-form-urlencoded
< oauth_token=7e1a...ccf4&oauth_token_secret=dc5d...43ad&email=test_account_google@openx.com
Access is granted and the final oauth_token above (7e1a...ccf4) becomes the openx3_access_token cookie in your API requests and must be sent every time.
Step 4 - Use the access token to access protected resources
Syntax
curl -X GET http://<openx_server_name>/ox/4.0/account/<account_uid> --cookie
        "openx3_access_token=token_string"
Example
curl -X GET http://openx_myserver.com/ox/4.0/account/879546 --cookie
        "openx3_access_token=e735c37c5cl...9778j1on41v6rj5"

### Release notes

See also:
Release Notes for Publishers
Android SDK Release Notes
iOS SDK Release Notes
May 4, 2016
TFCD Query Argument Added
The tfcd query argument has been added to the Ad Request API request parameters to ensure COPPA compliance. Possible values are 0 and 1.
0 = Not COPPA compliant
1 = COPPA compliant
For more information about the tfcd query argument, refer to the Ad Tag Parameters page.
February 11, 2016
'Adproduct' call removed
For this release, all adproduct calls have been removed and are no longer accessible to users via the OpenX UI and Platform API.
Updated ad tags
OpenX ad tags are now automatically updated to detect HTTP/HTTPS. Ad tags are now protocol-less and will work automatically for both secure and non-secure sites without requiring users to manually change the tags from HTTP to HTTPS.
January 28, 2016
API Functionality and Technical Documentation
The OpenX Platform API now supports enhanced API functionality for forecasting and legacy SRS reporting. UI functionality remains unchanged for both of these features, however, users will be able to access forecasting and reporting features via the OpenX Platform API, as well as detailed technical documentation describing how to use these features in the API Developer's Guide.
Forecasting
Users may now run forecasts using the OpenX Platform API. Users wil still be able to use this feature via the user interface; however, if you choose to use the OpenX Platform API, you only need to make a single POST call to run a forecast.
For more detailed information about forecasting via the OpenX Platform API, please see the Forecasting documentation.
Reporting
With this release, you may generate three different reports using the Platform API. You will still be able to access and generate these reports via the user interface, but you will now also be able to generate these reports via the Platform API. The three type of reports you can generate using the Platform API are:
Bid Performance Report
Exchange Report
SSP Revenue Report
Please see the Reporting documentation for more detailed information on how to use the OpenX Platform API for reporting.
November 9, 2015
Comments
The OpenX API now supports adding, editing, and retrieving comments using the OpenX API. With the OpenX API, you can:
Add a new comment to an object
Edit your own comments
Retrieve a list of comments for an object
Comments can be used to add notes to objects (e.g. line items, creatives, etc.) for other users to see; however, users will not be notified about new comments. Furthermore, one can only view comments when in the edit mode of an object.
If you are using the API to add/edit a comment, the following objects can have comments applied: account, ad, adunit, adunitgroup, audiencesegment, creative, creativetemplate, lineitem, optimization, order, site, deal, floorrule, and package
Refer to the Working With Comments section for more detailed information on using this feature.
