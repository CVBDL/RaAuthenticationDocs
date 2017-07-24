# RAAuthentication REST API

## Table of Contents

* [Overview](#overview)
  * [Current Version](#current-version)
  * [Schema](#schema)
  * [Parameters](#parameters)
  * [Root Endpoint](#root-endpoint)
  * [Client Errors](#client-errors)
  * [HTTP Verbs](#http-verbs)
  * [Cross Origin Resource Sharing](#cross-origin-resource-sharing)
* [Users](#users)
  * [Get token via username and password](#get-token-via-username-and-password)
  * [Get the authenticated user informatioin](#get-the-authenticated-user-informatioin)
* [Tokens](#tokens)
  * [Check a token](#check-a-token)

## Overview

### Current Version

The current version is `v1`.

### Schema

All API access is over HTTP. All data is sent and received as JSON.

Blank fields are included as `null` instead of being omitted.

All timestamps are returned in ISO 8601 format:

```text
YYYY-MM-DDTHH:MM:SSZ
```

### Parameters

Many API methods take optional parameters. For GET requests,any parameters not
specified as a segment in the path can be padded as an HTTP query string parameter:

```sh
curl -i http://hostname:port/api/tokens?q=something
```

### Root Endpoint

You can issue a `GET` request to the root endpoint to get all the endpoint
categories that the API supports:

```sh
curl http://hostname:port/api
```

### Client Errors

TBD

### HTTP Verbs

EagleEye Platform API will try to use appropriate HTTP verbs for each action.

Verb `PATCH` is an uncommon HTTP verb, so use `POST` instead.

| Verb       | Description                                                            |
| ---------- | ---------------------------------------------------------------------- |
| GET        | Used for retrieving resources.                                         |
| POST       | Used for creating resources or update a resource.                      |
| POST       | Used for updating resources with partial JSON data. Instead of `PATCH` |
| DELETE     | Used for deleting resources.                                           |

### Cross Origin Resource Sharing

The API supports Cross Origin Resource Sharing (CORS) for AJAX requests from any origin.
You can read the [CORS W3C Recommendation](http://www.w3.org/TR/cors/).

This is an example:

```sh
curl -i http://ra.com/api/user -H "Origin: http://example.com"
HTTP/1.1 200 OK
Access-Control-Allow-Origin: *
Access-Control-Allow-Headers: Origin, X-Requested-With, Content-Type, Accept
Access-Control-Allow-Methods: GET, PUT, POST, DELETE, OPTIONS
```

## Users

### Get token via username and password

User could exchange a token via windows domain account.

```text
POST /api/user
```

#### Input

| Name     | Type   | Description                               |
| -------- | ------ | ----------------------------------------- |
| UserName | string | Domain account username like: patrick.    |
| Password | string | The password.                             |

#### Example

```json
{
  "AccessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE0ODMyMDAwMDAwMDAsImVtYWlsIjoicGF0cmljay56aG9uZ0BleGFtcGxlLmNvbSJ9.jIBK2wO6qtoAdT4v5bGaPP_ytZfIMqW_4Ofh9UTLqj4"
}
```

### Get the authenticated user details

Get the full Active Directory information of the authenticated user.

```text
POST /api/user/details
```

#### Input

| Name     | Type   | Description                               |
| -------- | ------ | ----------------------------------------- |
| UserName | string | Domain account username like: patrick.    |
| Password | string | The password.                             |

#### Example

```json
{
  "DisplayName": "Patrick Zhong",
  "EmailAddress": "patrick.zhong@example.com",
  "EmployeeId": "A0123456789",
  "Name": "Patrick"
}
```

## Tokens

### Check a token

Validate the given JWT.

```
POST /api/tokens/validate
```

#### Input

| Name        | Type   | Description                               |
| ----------- | ------ | ----------------------------------------- |
| AccessToken | string | The JWT.                                  |

#### Example

```text
HTTP/1.1 204 No Content
```

Or

```text
HTTP/1.1 401 Unauthorized
```
