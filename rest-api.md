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
  * [Get the authenticated user details](#get-the-authenticated-user-details)
* [Token](#token)
  * [Check a token](#check-a-token)

## Overview

### Current Version

The current version is `v1`.

### Schema

All API access is over `HTTPS`, and accessed from the `https://hostname/raauthentication`.
All data is sent and received as JSON.

Blank fields are included as `null` instead of being omitted.

All timestamps are returned in ISO 8601 format:

```text
YYYY-MM-DDTHH:MM:SSZ
```

### Parameters

Many API methods take optional parameters. For GET requests,any parameters not
specified as a segment in the path can be padded as an HTTP query string parameter:

```sh
curl -i https://hostname/raauthentication/api/user?scope=none
```

### Root Endpoint

You can issue a `GET` request to the root endpoint to get all the endpoint
categories that the API supports:

```sh
curl https://hostname/raauthentication
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
curl -i https://hostname/raauthentication/api/user -H "Origin: http://example.com"
HTTP/1.1 200 OK
Access-Control-Allow-Origin: *
Access-Control-Allow-Headers: Origin, X-Requested-With, Content-Type, Accept
Access-Control-Allow-Methods: GET, PUT, POST, DELETE, OPTIONS
```

## Users

### Get token via username and password

User could obtain an ID token via windows domain account.
By default, the ID token's payload contains detail information of the user
by searching Active Directory, it may be slow.
But if you only want to authenticate the user without querying his(her) detailed
information, you could specify a `scope` query parameter on the URL.
When you specify `scope=none` on URL, the server will only authenticate the user,
and response an ID token with basic information. It is much faster.

Basic ID token payload sample:

```json
{
  "iss": "RAAuthentication",
  "iat": 1501054275,
  "exp": 1501057875,
  "aud": "patrick"
}
```

Detailed ID token payload sample:

```json
{
  "email": "patrick@example.com",
  "name": "Patrick Zhong",
  "iss": "RAAuthentication",
  "iat": 1501054225,
  "exp": 1501057825,
  "aud": "patrick"
}
```

```text
POST /api/user
```

#### Input

| Name     | Type   | Description                               |
| -------- | ------ | ----------------------------------------- |
| UserName | string | Domain account username like: patrick.    |
| Password | string | The password.                             |

#### Parameters

| Name   | Type   | Description                                                                         |
| ------ | ------ | ----------------------------------------------------------------------------------- |
| scope  | string | Optional. It determine what resources will be retrieved. Available value: `none`.   |

#### Example

```json
{
  "IdToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE0ODMyMDAwMDAsImVtYWlsIjoicGF0cmljay56aG9uZ0BleGFtcGxlLmNvbSJ9.E41uEnlFDhLk_05ftd95xNdbxSuVpO1X1TTJ5uJDStE"
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

## Token

### Check a token

Validate the given JWT.

```
POST /api/token/validate
```

#### Input

| Name     | Type   | Description                               |
| -------- | ------ | ----------------------------------------- |
| IdToken  | string | The ID token.                             |

#### Example

```text
HTTP/1.1 204 No Content
```

Or

```text
HTTP/1.1 401 Unauthorized
```
