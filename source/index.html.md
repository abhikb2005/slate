---
title: SpringVerify US API Documentation

language_tabs: # must be one of https://git.io/vQNgJ
  - shell: cURL
  - javascript
  - php
  - python
  - ruby

toc_footers:
  - 
  - 

search: true

code_clipboard: true
---

# Welcome!

**Hello and welcome to SpringVerify.**

This is the API documentation section of SpringVerify for customers and users located in the USA. It is divided into three sections for ease of navigation:

1. Introductions - _where entities, environments, and authentication protocols are detailed._
2. Company API flow - _where functions available to users logged in as company admins are detailed._
3. User API flow - _where functions available to users logged in as employees are detailed._

---

## Introduction

SpringVerify currently has three environments -- stage, acceptance and production. All three environments have independent databases. While in the stage phase, it is mandatory to use the base URL of the stage environment.

The language bindings are in cURL, node.js, and PHP. The API documentation is set up with a display area on the right to show examples.

## Internal objects

| User Roles | Response |
| ---------- | -------- |
| Company | An entity that has users. These users can be employees who are **performing the verification** or are **being verified**. |
| Admin | A user who is performing the verification. |
| Employee | A user whose details are being verified on the platform. |

## Environment URLs

**Stage**: https://api-stage.us.springverify.com  
**Acceptance**: https://api-acceptance.us.springverify.com  
**Production**: https://api.us.springverify.com  

>Currently we have two types of coupons -- "open" and "closed". Please contact sales@springverify.com regarding coupon codes for your company.

## Authentication

Authenticating in SpringVerify is done on the basis of JSON Web Tokens. Once registered, a user will receive a token to be used for subsequent API requests. If misplaced, the token can be retrieved by logging in again.

>The user must replace `JWT_TOKEN` in the examples below with their personal token.

---

# Company

This section covers the API details available for users logged in as _admins_, i.e., users who wish to conduct background verification checks on employees (or prospective employees).

## Signup

The signup API is used to register an admin - a user that performs verifications - in a company. Once a user is registered, a verification email is sent to the registered address.

<aside class="notice">
The <code>`Password`</code> and <code>`Confirm Password`</code> fields should be hashed using SHA256 beforehand.
</aside>

```shell
curl --location --request POST 'https://api.us.springverify.com/auth/signup' \
--header 'Content-Type: application/json' \
--data-raw '{
    "first_name": "John",
    "last_name": "Wick",
    "phone": "999999999",
    "email": "john@wick.com",
    "password": "daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5",
    "confirm_password":"daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5",
    "domain": "wick.com"
}'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/auth/signup', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json'
    },
    body: JSON.stringify({ "first_name": "John", "last_name": "Wick", "phone": "999999999", "email": "john@wick.com", "password": "daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5", "confirm_password":"daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5", "domain": "wick.com" })
});

// REQUEST

var request = require('request');

var headers = {
    'Content-Type': 'application/json'
};

var dataString = '{ "first_name": "John", "last_name": "Wick", "phone": "999999999", "email": "john@wick.com", "password": "daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5", "confirm_password":"daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5", "domain": "wick.com" }';

var options = {
    url: 'https://api.us.springverify.com/auth/signup',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Content-Type' => 'application/json'
);
$data = '{ "first_name": "John", "last_name": "Wick", "phone": "999999999", "email": "john@wick.com", "password": "daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5", "confirm_password":"daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5", "domain": "wick.com" }';
$response = Requests::post('https://api.us.springverify.com/auth/signup', $headers, $data);
```

```python
import requests

headers = {
    'Content-Type': 'application/json',
}

data = '{ "first_name": "John", "last_name": "Wick", "phone": "999999999", "email": "john@wick.com", "password": "daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5", "confirm_password":"daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5", "domain": "wick.com" }'

response = requests.post('https://api.us.springverify.com/auth/signup', headers=headers, data=data)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/auth/signup")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "success": true,
    "data": {
        "message": "Account created successfully."
    }
}
```

> Error Response

```json
{
    "success": true,
    "data": {
        "message": "Account with this email address already exists."
    }
}
```

**Query Parameters**

| Parameter | Type | Description | Mandatory |
| --- | --- | --- | --- |
| email | `string` | The email address used to register with SpringVerify | Yes |
| password | `string` | A strong, hashed password of minimum 8 characters | Yes |
| confirm_password | `string` | Repeat the password to confirm | Yes |
| first_name | `string` | The user's first name | Yes |
| last_name | `string` | The user's last name | No |
| phone | `string` | The user's preferred phone number | No |
| domain | `string` | The company's domain. Must be the same as the email domain | Yes |

With this, the user is successfully registered into a company's domain as an admin.

## Verify an Admin's Email address

```shell
curl --location --request GET 'https://api.us.springverify.com/auth/verify?token=TOKEN_SENT_IN_EMAIL'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/auth/verify?token=TOKEN_SENT_IN_EMAIL');

// REQUEST

var request = require('request');

var options = {
    url: 'https://api.us.springverify.com/auth/verify?token=TOKEN_SENT_IN_EMAIL'
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array();
$response = Requests::get('https://api.us.springverify.com/auth/verify?token=TOKEN_SENT_IN_EMAIL', $headers);
```

```python
import requests

params = (
    ('token', 'TOKEN_SENT_IN_EMAIL'),
)

response = requests.get('https://api.us.springverify.com/auth/verify', params=params)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/auth/verify?token=TOKEN_SENT_IN_EMAIL")
response = Net::HTTP.get_response(uri)

# response.code
# response.body
```

> Success Response

```json
{
    "success": true,
    "data": {
        "message": "Activated."
    }
}
```

> Error Response

```json
{
    "success": true,
    "data": {
        "message": "Account not found."
    }
}
```

This API is used to verify a company admin's email address. If this user is the first user for a company, then the company will be created as an entity on the admin being successfully verified.

## Resend the verification email to a company admin

```shell
curl --location --request POST 'https://api.us.springverify.com/auth/resend-email' \
--header 'Content-Type: application/json' \
--data-raw '{
    "email":"john@wick.com"
}'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/auth/resend-email', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json'
    },
    body: JSON.stringify({ "email":"john@wick.com" })
});

// REQUEST

var request = require('request');

var headers = {
    'Content-Type': 'application/json'
};

var dataString = '{ "email":"john@wick.com" }';

var options = {
    url: 'https://api.us.springverify.com/auth/resend-email',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Content-Type' => 'application/json'
);
$data = '{ "email":"john@wick.com" }';
$response = Requests::post('https://api.us.springverify.com/auth/resend-email', $headers, $data);
```

```python
import requests

headers = {
    'Content-Type': 'application/json',
}

data = '{ "email":"john@wick.com" }'

response = requests.post('https://api.us.springverify.com/auth/resend-email', headers=headers, data=data)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/auth/resend-email")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "success": true,
    "data": {
        "message": "Email Sent."
    }
}
```

> Error Response

```json
{
    "message": "user already verified"
}
```

If an admin requires the verification email to be sent again, this API should be used.

## Logging in

```shell
curl --location --request POST 'https://api.us.springverify.com/auth/login' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--data-raw '{
    "email": "john@wick.com",
    "password": "daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5",
    "role":"admin"
}'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/auth/login', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
        'Authorization': 'Bearer JWT_TOKEN'
    },
    body: JSON.stringify({ "email": "john@wick.com", "password": "daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5", "role":"admin" })
});

// REQUEST

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/auth/login', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
        'Authorization': 'Bearer JWT_TOKEN'
    },
    body: JSON.stringify({ "email": "john@wick.com", "password": "daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5", "role":"admin" })
});
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Content-Type' => 'application/json',
    'Authorization' => 'Bearer JWT_TOKEN'
);
$data = '{ "email": "john@wick.com", "password": "daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5", "role":"admin" }';
$response = Requests::post('https://api.us.springverify.com/auth/login', $headers, $data);
```

```python
import requests

headers = {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer JWT_TOKEN',
}

data = '{ "email": "john@wick.com", "password": "daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5", "role":"admin" }'

response = requests.post('https://api.us.springverify.com/auth/login', headers=headers, data=data)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/auth/login")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "token": "JWT_TOKEN",
    "profile": {
        "first_name": "John",
        "last_name": "Wick",
        "email": "john@wick.com",
        "domain": "wick.com",
        "company": {
            "id": 44,
            "name": "SmartBrains",
            "address": "901, Downtown Manhattan",
            "city": "NY",
            "state": "NY",
            "zipcode": "91319",
            "tax_id_number": "1234567890",
            "s3_logo": "link"
        }
    }
}
```

> Error Response:

```json
{
    "errors": [
        {
            "value": "",
            "msg": "Not a valid role",
            "param": "role",
            "location": "body"
        }
    ]
}
```

For logging in, the aim is to generate a JSON web token that is to be used in all subsequent API calls. The JWT generated will be valid for one hour.

To call the subsequent APIs, the user will need to send the JWT successfully in the header of those APIs.

<aside class="notice">
The password should be hashed using SHA256 beforehand.
</aside>

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| email | `string` | The email address used to register with SpringVerify |
| password | `string` | A strong, hashed password of minimum 8 characters |
| role | `string` | The role of the user being logged in - in this case, `admin` |

## Forgot password

```shell
curl --location --request POST 'https://api.us.springverify.com/auth/forgot-password' \
--header 'Content-Type: application/json' \
--data-raw '{
    "email":"johndoe@gmail.com"
}'
```

```javascript
// FETCH
var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/auth/forgot-password', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json'
    },
    body: JSON.stringify({ "email":"johndoe@gmail.com" })
});

// REQUEST
var request = require('request');

var headers = {
    'Content-Type': 'application/json'
};

var dataString = '{ "email":"johndoe@gmail.com" }';

var options = {
    url: 'https://api.us.springverify.com/auth/forgot-password',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Content-Type' => 'application/json'
);
$data = '{ "email":"johndoe@gmail.com" }';
$response = Requests::post('https://api.us.springverify.com/auth/forgot-password', $headers, $data);
```

```python
import requests

headers = {
    'Content-Type': 'application/json',
}

data = '{ "email":"johndoe@gmail.com" }'

response = requests.post('https://api.us.springverify.com/auth/forgot-password', headers=headers, data=data)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/auth/forgot-password")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "success": true,
    "data": "success"
}
```

In case of a misplaced password, this API should be used to send an email to the user to reset their password. The email will contain a token-embedded URL which will be used to call the [Reset Password](https://docs.us.springverify.com/#reset-password) API.

## Reset Password

```shell
curl --location --request POST 'https://api.us.springverify.com/profile/reset-password' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "password":"17f80754644d33ac685b0842a402229adbb43fc9312f7bdf36ba24237a1f1ffb"
}'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/profile/reset-password', {
    method: 'POST',
    headers: {
        'Authorization': 'Bearer JWT_TOKEN',
        'Content-Type': 'application/json'
    },
    body: JSON.stringify({ "password":"17f80754644d33ac685b0842a402229adbb43fc9312f7bdf36ba24237a1f1ffb" })
});

// REQUEST

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN',
    'Content-Type': 'application/json'
};

var dataString = '{ "password":"17f80754644d33ac685b0842a402229adbb43fc9312f7bdf36ba24237a1f1ffb" }';

var options = {
    url: 'https://api.us.springverify.com/profile/reset-password',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Authorization' => 'Bearer JWT_TOKEN',
    'Content-Type' => 'application/json'
);
$data = '{ "password":"17f80754644d33ac685b0842a402229adbb43fc9312f7bdf36ba24237a1f1ffb" }';
$response = Requests::post('https://api.us.springverify.com/profile/reset-password', $headers, $data);
```

```python
import requests

headers = {
    'Authorization': 'Bearer JWT_TOKEN',
    'Content-Type': 'application/json',
}

data = '{ "password":"17f80754644d33ac685b0842a402229adbb43fc9312f7bdf36ba24237a1f1ffb" }'

response = requests.post('https://api.us.springverify.com/profile/reset-password', headers=headers, data=data)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/profile/reset-password")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "success": true,
    "data": "success"
}
```

When a user requests declares a forgotten password, an email will be sent using the [Forgot Password](https://docs.us.springverify.com/#forgot-password) API. This email will consist of a token-embedded URL that is to be passed in this API along with the new password.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| password | `string` | A strong, hashed password of minimum 8 characters |

## Register your company

```shell
curl --location --request POST 'https://api.us.springverify.com/company' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "SmartBrains",
    "address": "901, Downtown Manhattan",
    "city": "NY",
    "state": "NY",
    "zipcode": "91319",
    "tax_id_number":"1234567890"
}'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/company', {
    method: 'POST',
    headers: {
        'Authorization': 'Bearer JWT_TOKEN',
        'Content-Type': 'application/json'
    },
    body: JSON.stringify({ "name": "SmartBrains", "address": "901, Downtown Manhattan", "city": "NY", "state": "NY", "zipcode": "91319", "tax_id_number":"1234567890" })
});

// REQUEST

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN',
    'Content-Type': 'application/json'
};

var dataString = '{ "name": "SmartBrains", "address": "901, Downtown Manhattan", "city": "NY", "state": "NY", "zipcode": "91319", "tax_id_number":"1234567890" }';

var options = {
    url: 'https://api.us.springverify.com/company',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Authorization' => 'Bearer JWT_TOKEN',
    'Content-Type' => 'application/json'
);
$data = '{ "name": "SmartBrains", "address": "901, Downtown Manhattan", "city": "NY", "state": "NY", "zipcode": "91319", "tax_id_number":"1234567890" }';
$response = Requests::post('https://api.us.springverify.com/company', $headers, $data);
```

```python
import requests

headers = {
    'Authorization': 'Bearer JWT_TOKEN',
    'Content-Type': 'application/json',
}

data = '{ "name": "SmartBrains", "address": "901, Downtown Manhattan", "city": "NY", "state": "NY", "zipcode": "91319", "tax_id_number":"1234567890" }'

response = requests.post('https://api.us.springverify.com/company', headers=headers, data=data)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/company")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response:

```json
{
    "success": true,
    "data": {
        "message": "Company updated successfully"
    }
}
```

A user with the role of an admin can create a company once their profile is [verified](https://docs.us.springverify.com/#verify-company-admin-email). A valid JWT will be needed to authenicate the user in this API.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| name | `string` | Name of the company being registered |
| address | `string` | Address of the company being registered |
| city | `string` | The city where the company being registered is located |
| state | `string` | The state where the company being registered is located |
| zipcode | `string` | The ZIP code of the postal district in which the company is located |
| tax_id_number | `string` | Tax number of the company that is being registered |

## Upload a logo

```shell
curl --location --request POST 'https://api.us.springverify.com/company/upload-logo' \
--header 'Authorization: Bearer JWT_TOKEN' \
--form 'logo=@/path/to/file'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/company/upload-logo', {
    method: 'POST',
    headers: {
        'Authorization': 'Bearer JWT_TOKEN'
    }
});

// REQUEST

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN'
};

var options = {
    url: 'https://api.us.springverify.com/company/upload-logo',
    method: 'POST',
    headers: headers
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Authorization' => 'Bearer JWT_TOKEN'
);
$response = Requests::post('https://api.us.springverify.com/company/upload-logo', $headers);
```

```python
import requests

headers = {
    'Authorization': 'Bearer JWT_TOKEN',
}

response = requests.post('https://api.us.springverify.com/company/upload-logo', headers=headers)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/company/upload-logo")
request = Net::HTTP::Post.new(uri)
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "success": true,
    "data": "https://spring-verify-us.s3.amazonaws.com/company/wick.com/logo"
}
```

Upload the company's logo using this API.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| logo | `file` | Raw file of the logo |

## Get company details

```shell
curl --location --request GET 'https://api.us.springverify.com/company' \
--header 'Authorization: Bearer JWT_TOKEN'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/company', {
    headers: {
        'Authorization': 'Bearer JWT_TOKEN'
    }
});

// REQUEST

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN'
};

var options = {
    url: 'https://api.us.springverify.com/company',
    headers: headers
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Authorization' => 'Bearer JWT_TOKEN'
);
$response = Requests::get('https://api.us.springverify.com/company', $headers);
```

```python
import requests

headers = {
    'Authorization': 'Bearer JWT_TOKEN',
}

response = requests.get('https://api.us.springverify.com/company', headers=headers)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/company")
request = Net::HTTP::Get.new(uri)
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "success": true,
    "data": {
        "id": 44,
        "created_by": "john@wick.com",
        "name": "SmartBrains",
        "address": "901, Downtown Manhattan",
        "city": "NY",
        "state": "NY",
        "zipcode": "91319",
        "tax_id_number": "1234567890",
        "credits": 0,
        "domain": "wick.com",
        "employment_limit": null,
        "education_limit": null,
        "license_limit": null,
        "civilcourt_limit": null,
        "dl_limit": null,
        "s3_logo": "https://spring-verify-us.s3.amazonaws.com/company/wick.com/logo",
        "created_at": "2020-08-24T10:20:20.000Z",
        "updated_at": "2020-08-24T10:31:49.000Z",
        "employee_invite_groups": []
    }
}
```

For a registered admin with a valid JWT, this API can be used to get the details of the company. This is different from the company profile. A company's profile contains details of the company given during registration as well as the count of the employees whose profiles were verified, failed or is pending.

## Get company profile

```shell
curl --location --request GET 'https://api.us.springverify.com/profile' \
--header 'Authorization: Bearer JWT_TOKEN'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/profile', {
    headers: {
        'Authorization': 'Bearer JWT_TOKEN'
    }
});

// REQUEST

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN'
};

var options = {
    url: 'https://api.us.springverify.com/profile',
    headers: headers
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Authorization' => 'Bearer JWT_TOKEN'
);
$response = Requests::get('https://api.us.springverify.com/profile', $headers);
```

```python
import requests

headers = {
    'Authorization': 'Bearer JWT_TOKEN',
}

response = requests.get('https://api.us.springverify.com/profile', headers=headers)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/profile")
request = Net::HTTP::Get.new(uri)
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "success": true,
    "data": {
        "first_name": "John",
        "last_name": "Wick",
        "phone": "999999999",
        "email": "john@wick.com",
        "domain": "wick.com",
        "stripe_id": null,
        "company": {
            "name": "SmartBrains",
            "address": "901, Downtown Manhattan",
            "city": "NY",
            "state": "NY",
            "zipcode": "91319",
            "tax_id_number": "1234567890",
            "s3_logo": "https://spring-verify-us.s3.amazonaws.com/company/wick.com/logo"
        },
        "count": [
            {
                "count": 0,
                "type": null
            },
            {
                "count": 0,
                "type": "FAILED"
            },
            {
                "count": 0,
                "type": "PENDING"
            },
            {
                "count": 0,
                "type": "VERIFIED"
            }
        ]
    }
}
```

For a registered admin with a valid JWT, this API can be used to get the company's profile. This is different from the company details. A company's profile contains details of the company given during registration as well as the count of the employees whose profiles were verified, failed, or is pending.

## Get available packages

```shell
curl --location --request GET 'http://api-stage.us.springverify.com/company/package' \
        --header 'Authorization: Bearer JWT_TOKEN'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('http://api-stage.us.springverify.com/company/package', {
    headers: {
        'Authorization': 'Bearer JWT_TOKEN'
    }
});

// REQUEST

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN'
};

var options = {
    url: 'http://api-stage.us.springverify.com/company/package',
    headers: headers
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Authorization' => 'Bearer JWT_TOKEN'
);
$response = Requests::get('http://api-stage.us.springverify.com/company/package', $headers);
```

```python
import requests

headers = {
    'Authorization': 'Bearer JWT_TOKEN',
}

response = requests.get('http://api-stage.us.springverify.com/company/package', headers=headers)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("http://api-stage.us.springverify.com/company/package")
request = Net::HTTP::Get.new(uri)
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response:

```json
{
    "success": true,
    "data": {
        "package": [
            {
                "id": "1",
                "name": "bronze",
                "employment": null,
                "education": "0",
                "professional_license": "0",
                "civil_court": "0",
                "driving_license": "0",
                "county_criminal_search": "0",
                "all_county_criminal_search": false,
                "national_criminal_search": "1",
                "sex_offender_search": "1",
                "ssn_trace": "1",
                "global_watchlist": "1",
                "price": "750",
                "created_at": "2019-07-03T11:07:27.000Z",
                "updated_at": "2019-11-11T11:53:38.000Z"
            },
            {
                "id": "2",
                "name": "silver",
                "employment": null,
                "education": "0",
                "professional_license": "0",
                "civil_court": "0",
                "driving_license": "0",
                "county_criminal_search": "0",
                "all_county_criminal_search": false,
                "national_criminal_search": "1",
                "sex_offender_search": "1",
                "ssn_trace": "1",
                "global_watchlist": "1",
                "price": "1500",
                "created_at": "2019-07-03T11:07:27.000Z",
                "updated_at": "2019-11-11T11:53:38.000Z"
            },
            {
                "id": "3",
                "name": "gold",
                "employment": null,
                "education": "0",
                "professional_license": "0",
                "civil_court": "0",
                "driving_license": "0",
                "county_criminal_search": "1",
                "all_county_criminal_search": false,
                "national_criminal_search": "1",
                "sex_offender_search": "1",
                "ssn_trace": "1",
                "global_watchlist": "1",
                "price": "2000",
                "created_at": "2019-07-03T11:07:27.000Z",
                "updated_at": "2019-11-11T11:53:39.000Z"
            },
            {
                "id": "4",
                "name": "platinum",
                "employment": null,
                "education": "1",
                "professional_license": "0",
                "civil_court": "0",
                "driving_license": "0",
                "county_criminal_search": "0",
                "all_county_criminal_search": true,
                "national_criminal_search": "1",
                "sex_offender_search": "1",
                "ssn_trace": "1",
                "global_watchlist": "1",
                "price": "3000",
                "created_at": "2019-07-03T11:07:27.000Z",
                "updated_at": "2019-11-11T11:53:39.000Z"
            },
            {
                "id": "5",
                "name": "diamond",
                "employment": null,
                "education": "1",
                "professional_license": "0",
                "civil_court": "1",
                "driving_license": "0",
                "county_criminal_search": "0",
                "all_county_criminal_search": true,
                "national_criminal_search": "1",
                "sex_offender_search": "1",
                "ssn_trace": "1",
                "global_watchlist": "1",
                "price": "4000",
                "created_at": "2019-07-03T11:07:27.000Z",
                "updated_at": "2019-11-11T11:53:39.000Z"
            }
        ],
        "prices": {
            "id": "1",
            "env": "development",
            "passport": 200,
            "driving_license": 200,
            "enhanced_driving_license": 200,
            "kba": 400,
            "employment": 750,
            "education": 750,
            "professional_license": 500,
            "civil_court": 1000,
            "one_county_criminal_search": 1000,
            "all_county_criminal_search": 1000,
            "county_criminal_search": 1000,
            "package": "",
            "created_at": "2019-07-03T11:07:27.000Z",
            "updated_at": "2019-09-18T13:27:52.000Z"
        }
    }
}
```

This API is used to get the available pricing plans and packages.

## Invite candidates

```shell
curl --location --request POST 'localhost:3080/employee/invite' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--data-raw '{
    "email_list": ["johndoe@gmail.com"],
    "package": "diamond",
    "add_ons": {
        "employment": 2,
        "education": 1,
        "license": 0,
        "driving_license": 0,
        "civil_court": 1,
        "all_county_criminal_search": true,
        "county_criminal_search": 0
    },
    coupon_code:""
}'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('localhost:3080/employee/invite', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
        'Authorization': 'Bearer JWT_TOKEN'
    },
    body: '{ "email_list": ["johndoe@gmail.com"], "package": "diamond", "add_ons": { "employment": 2, "education": 1, "license": 0, "driving_license": 0, "civil_court": 1, "all_county_criminal_search": true, "county_criminal_search": 0 }, coupon_code:"" }'
});

// REQUEST

var request = require('request');

var headers = {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer JWT_TOKEN'
};

var dataString = '{ "email_list": ["johndoe@gmail.com"], "package": "diamond", "add_ons": { "employment": 2, "education": 1, "license": 0, "driving_license": 0, "civil_court": 1, "all_county_criminal_search": true, "county_criminal_search": 0 }, coupon_code:"" }';

var options = {
    url: 'localhost:3080/employee/invite',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Content-Type' => 'application/json',
    'Authorization' => 'Bearer JWT_TOKEN'
);
$data = '{ "email_list": ["johndoe@gmail.com"], "package": "diamond", "add_ons": { "employment": 2, "education": 1, "license": 0, "driving_license": 0, "civil_court": 1, "all_county_criminal_search": true, "county_criminal_search": 0 }, coupon_code:"" }';
$response = Requests::post('localhost:3080/employee/invite', $headers, $data);
```

```python
import requests

headers = {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer JWT_TOKEN',
}

data = '{ "email_list": ["johndoe@gmail.com"], "package": "diamond", "add_ons": { "employment": 2, "education": 1, "license": 0, "driving_license": 0, "civil_court": 1, "all_county_criminal_search": true, "county_criminal_search": 0 }, coupon_code:"" }'

response = requests.post('http://localhost:3080/employee/invite', headers=headers, data=data)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("http://localhost:3080/employee/invite")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

>Success Response:

```json
{
    "success": true,
    "data": {
        "price": 4000,
        "id": "2612d112-5827-4b1c-a177-06a85eabd03d",
        "count": 1
    }
}
```

This API is used to invite existing and/or prospective employees to get their profiles verified. It can be used to invite employees in bulk. However, emails will be sent to the employees only once the payment is successfully completed.

>This API is to be used only after using the [Get available packages](https://docs.us.springverify.com/#get-available-packages) API.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| email_list | `array` | Contains a list of the email addresses of the employees to be contacted |
| package | `string` | Name of the package as retrieved from the [Get available packages](https://docs.us.springverify.com/#get-available-packages) API |
| add_ons | `object` | To add additional services into the retrieved package |
| coupon_code | `string` | Any coupon code that the admin has for a discounted pricing |

**Response Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| price | `number` | The price (in cents) that is to be paid |
| id | `string` | The reference ID against which the payment is being made |
| count | `number` | The total number of candidates that are being verified |

## Save Credit Card in Stripe for Payments

```shell
curl --location --request POST 'https://api.stripe.com/v1/tokens' \
--header 'Authorization: Bearer STRIPE_TOKEN' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'card[number]=4242424242424242' \
--data-urlencode 'card[exp_month]=12' \
--data-urlencode 'card[exp_year]=2020' \
--data-urlencode 'card[cvc]=123'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.stripe.com/v1/tokens', {
    headers: {
        'Authorization': 'Bearer STRIPE_TOKEN',
        'Content-Type': 'application/x-www-form-urlencoded'
    }
});

// REQUEST

var request = require('request');

var headers = {
    'Authorization': 'Bearer STRIPE_TOKEN',
    'Content-Type': 'application/x-www-form-urlencoded'
};

var options = {
    url: 'https://api.stripe.com/v1/tokens',
    headers: headers
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Authorization' => 'Bearer STRIPE_TOKEN',
    'Content-Type' => 'application/x-www-form-urlencoded'
);
$response = Requests::get('https://api.stripe.com/v1/tokens', $headers);
```

```python
import requests

headers = {
    'Authorization': 'Bearer STRIPE_TOKEN',
    'Content-Type': 'application/x-www-form-urlencoded',
}

response = requests.get('https://api.stripe.com/v1/tokens', headers=headers)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.stripe.com/v1/tokens")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/x-www-form-urlencoded"
request["Authorization"] = "Bearer STRIPE_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
  "id": "tok_1GidnJ4wweuFtc0n81Lzq51P",
  "object": "token",
  "card": {
    "id": "card_1GidnJ4wweuFtc0nu2WUlvc5",
    "object": "card",
    "address_city": null,
    "address_country": null,
    "address_line1": null,
    "address_line1_check": null,
    "address_line2": null,
    "address_state": null,
    "address_zip": null,
    "address_zip_check": null,
    "brand": "Visa",
    "country": "US",
    "customer": null,
    "cvc_check": null,
    "dynamic_last4": null,
    "exp_month": 12,
    "exp_year": 2020,
    "fingerprint": "hV2YANZB970jzVzt",
    "funding": "credit",
    "last4": "4242",
    "metadata": {},
    "name": null,
    "tokenization_method": null
  },
  "client_ip": "106.51.30.155",
  "created": 1589450161,
  "livemode": false,
  "type": "card",
  "used": false
}
```

This API is used to register the credit card with Stripe for payments. If this API call is successful, the response generated will include an `id` parameter that is used in the [Payment for Invites](https://docs.us.springverify.com/#payment-for-invites) API.

| Parameter | Type | Environment | Value |
| --- | --- | --- | --- |
| STRIPE_TOKEN | `string` | Development | pk_test_51H1niLFq1aDrrzKzoQEG7espm3z3HirSoy5IJl8tWbxJk18pZDx67Y70mCynyLKOrEJFWarCiVSigW9RuW1bqdeU00lRNETXxe |
| STRIPE_TOKEN | `string` | Production | pk_live_51H1niLFq1aDrrzKztsRsWduNwtnBIIuRWSdeAJtIsgFefyWukEuqx8J6T8djCLiHAMDNNvdKpYkdiqq7iP9hwtCK00VdWazMPg |

## Save payment info

```shell
curl --location --request POST 'https://api.us.springverify.com/payment/save-card' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--data-raw '{
	"source":"tok_1EkXOa4wweuFtc0nQd0eOOhs"
}'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/payment/save-card', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
        'Authorization': 'Bearer JWT_TOKEN'
    },
    body: JSON.stringify({ "source":"tok_1EkXOa4wweuFtc0nQd0eOOhs" })
});

// REQUEST

var request = require('request');

var headers = {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer JWT_TOKEN'
};

var dataString = '{ "source":"tok_1EkXOa4wweuFtc0nQd0eOOhs" }';

var options = {
    url: 'https://api.us.springverify.com/payment/save-card',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Content-Type' => 'application/json',
    'Authorization' => 'Bearer JWT_TOKEN'
);
$data = '{ "source":"tok_1EkXOa4wweuFtc0nQd0eOOhs" }';
$response = Requests::post('https://api.us.springverify.com/payment/save-card', $headers, $data);
```

```python
import requests

headers = {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer JWT_TOKEN',
}

data = '{ "source":"tok_1EkXOa4wweuFtc0nQd0eOOhs" }'

response = requests.post('https://api.us.springverify.com/payment/save-card', headers=headers, data=data)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/payment/save-card")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "success": true,
    "data": "card saved for the specified user"
}
```

To save the token received from the [Save Credit Card in Stripe for Payments](https://docs.us.springverify.com/#save-credit-card-in-stripe-for-payments), use this API.

## Payment For Invites

```shell
curl --location --request POST 'https://api.us.springverify.com/payment/charge-user' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--data-raw '{
    "id": "2612d112-5827-4b1c-a177-03a85eabd03d",
    "send_email": true
}'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/payment/charge-user', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
        'Authorization': 'Bearer JWT_TOKEN'
    },
    body: JSON.stringify({ "id": "2612d112-5827-4b1c-a177-03a85eabd03d", "send_email": true })
});

// REQUEST

var request = require('request');

var headers = {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer JWT_TOKEN'
};

var dataString = '{ "id": "2612d112-5827-4b1c-a177-03a85eabd03d", "send_email": true }';

var options = {
    url: 'https://api.us.springverify.com/payment/charge-user',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Content-Type' => 'application/json',
    'Authorization' => 'Bearer JWT_TOKEN'
);
$data = '{ "id": "2612d112-5827-4b1c-a177-03a85eabd03d", "send_email": true }';
$response = Requests::post('https://api.us.springverify.com/payment/charge-user', $headers, $data);
```

```python
import requests

headers = {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer JWT_TOKEN',
}

data = '{ "id": "2612d112-5827-4b1c-a177-03a85eabd03d", "send_email": true }'

response = requests.post('https://api.us.springverify.com/payment/charge-user', headers=headers, data=data)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/payment/charge-user")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

>Success Response

```json
{
    "success": true,
    "data": {
        "id": "ch_1HJdvnFq1aDrrzKzZE6729eZ",
        "object": "charge",
        "amount": 9999,
        "amount_refunded": 0,
        "application": null,
        "application_fee": null,
        "application_fee_amount": null,
        "balance_transaction": "txn_1HJdvnFq1aDrrzKzbZXCgvAO",
        "billing_details": {
            "address": {
                "city": null,
                "country": null,
                "line1": null,
                "line2": null,
                "postal_code": "42424",
                "state": null
            },
            "email": null,
            "name": "johndoe@gmail.com",
            "phone": null
        },
        "calculated_statement_descriptor": "SPRINGVERIFY.COM",
        "captured": true,
        "created": 1598268823,
        "currency": "usd",
        "customer": "cus_HjgWvwfPN7bKob",
        "description": null,
        "destination": null,
        "dispute": null,
        "disputed": false,
        "failure_code": null,
        "failure_message": null,
        "fraud_details": {},
        "invoice": null,
        "livemode": false,
        "metadata": {},
        "on_behalf_of": null,
        "order": null,
        "outcome": {
            "network_status": "approved_by_network",
            "reason": null,
            "risk_level": "normal",
            "risk_score": 28,
            "seller_message": "Payment complete.",
            "type": "authorized"
        },
        "paid": true,
        "payment_intent": null,
        "payment_method": "card_1HAD9gFq1aDrrzKzPZUtu418",
        "payment_method_details": {
            "card": {
                "brand": "visa",
                "checks": {
                    "address_line1_check": null,
                    "address_postal_code_check": "pass",
                    "cvc_check": null
                },
                "country": "US",
                "exp_month": 4,
                "exp_year": 2024,
                "fingerprint": "IpgpMeLB1qGPPEnT",
                "funding": "credit",
                "installments": null,
                "last4": "4242",
                "network": "visa",
                "three_d_secure": null,
                "wallet": null
            },
            "type": "card"
        },
        "receipt_email": "johndoe@gmail.com",
        "receipt_number": null,
        "receipt_url": "receipt_url_link",
        "refunded": false,
        "refunds": {
            "object": "list",
            "data": [],
            "has_more": false,
            "total_count": 0,
            "url": "/v1/charges/ch_1HJdvnFq1aDrrzKzZE6729eZ/refunds"
        },
        "review": null,
        "shipping": null,
        "source": {
            "id": "card_1HAD9gFq1aDrrzKzPZUtu418",
            "object": "card",
            "address_city": null,
            "address_country": null,
            "address_line1": null,
            "address_line1_check": null,
            "address_line2": null,
            "address_state": null,
            "address_zip": "42424",
            "address_zip_check": "pass",
            "brand": "Visa",
            "country": "US",
            "customer": "cus_HjgWvwfPN7bKob",
            "cvc_check": null,
            "dynamic_last4": null,
            "exp_month": 4,
            "exp_year": 2024,
            "fingerprint": "IygpMeLB1qGPPEnT",
            "funding": "credit",
            "last4": "4242",
            "metadata": {},
            "name": "johndoe@gmail.com",
            "tokenization_method": null
        },
        "source_transfer": null,
        "statement_descriptor": null,
        "statement_descriptor_suffix": null,
        "status": "succeeded",
        "transfer_data": null,
        "transfer_group": null
    }
}
```

Once a user has been invited to get themselves verified, the verification must be paid for. This API is used for the payment purpose and must be called immediately after the [Invite Candidates](https://docs.us.springverify.com/#invite-candidates) API. The reference ID retrieved from the "Invite Candidates" API is used here. Emails to employees (or prospective employees) requesting verification will be sent only after the payment is successful. If the `send_email` field is set to `false`, the API will return the verification links of the employees in the response.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| id | `string` | The reference ID retrieved from the [Invite Candidates](https://docs.us.springverify.com/#invite-candidates) API. |
| send_email | `boolean` | `true` -- sends emails to the candidates. |
| | | `false` -- returns the verifications links of the candidates. |

## Get Single Candidate (or employee)

```shell
curl --location --request GET 'https://api.us.springverify.com/company/employee?id=EMPLOYEE_ID' \
--header 'Authorization: Bearer JWT_TOKEN' \
--data-raw ''
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/company/employee?id=EMPLOYEE_ID', {
    method: 'POST',
    headers: {
        'Authorization': 'Bearer JWT_TOKEN'
    }
});

// REQUEST

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN'
};

var options = {
    url: 'https://api.us.springverify.com/company/employee?id=EMPLOYEE_ID',
    method: 'POST',
    headers: headers
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Authorization' => 'Bearer JWT_TOKEN'
);
$data = array(

);
$response = Requests::post('https://api.us.springverify.com/company/employee?id=EMPLOYEE_ID', $headers, $data);
```

```python
import requests

headers = {
    'Authorization': 'Bearer JWT_TOKEN',
}

params = (
    ('id', 'EMPLOYEE_ID'),
)

response = requests.post('https://api.us.springverify.com/company/employee', headers=headers, params=params)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/company/employee?id=EMPLOYEE_ID")
request = Net::HTTP::Get.new(uri)
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "success": true,
    "data": {
        "employee": {
            "id": "6d6d81f5-6f00-4a38-a7d7-249cc2127ab6",
            "access_id": null,
            "email": "johndoe@gmail.com",
            "password_hash": null,
            "first_name": null,
            "middle_name": null,
            "last_name": null,
            "name_verified": null,
            "created_at": "2020-08-24T10:57:01.000Z",
            "updated_at": "2020-08-24T10:57:01.000Z",
            "employer_id": "fd725660-58f7-4a97-b626-5a4588e4ca31",
            "payment_id": "0e9a7695-91bd-4e60-aefc-ba47f80ab0f0",
            "email_sent": true,
            "payment": false,
            "status": null,
            "flow_completed": null,
            "company_created_by": "john@wick.com",
            "employee_limit_id": "bd95351f-fc2a-4ff9-8652-88abbfe67bdd",
            "kbaqna": null,
            "employments": [],
            "education": [],
            "professional_licenses": [],
            "employee_limit": {
                "id": "bd95351f-fc2a-4ff9-8652-88abbfe67bdd",
                "employment": 2,
                "education": 1,
                "professional_license": 0,
                "all_county_criminal_search": true,
                "county_criminal_search": 0,
                "civil_court": 1,
                "driving_license": 0,
                "package_id": null,
                "created_at": "2020-08-24T10:57:01.000Z",
                "updated_at": "2020-08-24T10:57:01.000Z",
                "employee_invite_group_id": "4153a150-4157-48ab-b258-9a9b47a05fc2",
                "employee_invite_group": {
                    "id": "4153a150-4157-48ab-b258-9a9b47a05fc2",
                    "package": "diamond",
                    "active": true,
                    "created_at": "2020-08-24T10:57:00.000Z",
                    "updated_at": "2020-08-24T10:57:00.000Z",
                    "company_created_by": "john@wick.com",
                    "package_id": "5"
                }
            },
            "employee_detail": null,
            "employee_verification": null,
            "s3_files": [],
            "criminal_statuses": [],
            "cic_criminal_records": [],
            "sjv_criminal_reports": []
        },
        "review": [],
        "criminal_status": {
            "national_criminal": "PENDING",
            "sex_offender": "PENDING",
            "global_watchlist": "PENDING",
            "county_criminal_search": "PENDING",
            "civil_court": "PENDING",
            "overall_criminal_status": "PENDING"
        }
    }
}
```

When the details of a specific user that is already verified -- a candidate or an employee -- is to be retrieved, use this API. This API retrivies the details of the user that have been verified, as well as the pricing package in which the user's profile was verified, along with the date of the most recent verification.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
|id | `uuid` | Contains the unique ID of the user. |

## Search Employee

```shell
curl --location --request POST 'https://api.us.springverify.com/company/employee/search' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "search":"johndoe@gmail.com"
}'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/company/employee/search', {
    method: 'POST',
    headers: {
        'Authorization': 'Bearer JWT_TOKEN',
        'Content-Type': 'application/json'
    },
    body: JSON.stringify({ "search":"johndoe@gmail.com" })
});

// REQUEST

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN',
    'Content-Type': 'application/json'
};

var dataString = '{ "search":"johndoe@gmail.com" }';

var options = {
    url: 'https://api.us.springverify.com/company/employee/search',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Authorization' => 'Bearer JWT_TOKEN',
    'Content-Type' => 'application/json'
);
$data = '{ "search":"johndoe@gmail.com" }';
$response = Requests::post('https://api.us.springverify.com/company/employee/search', $headers, $data);
```

```python
import requests

headers = {
    'Authorization': 'Bearer JWT_TOKEN',
    'Content-Type': 'application/json',
}

data = '{ "search":"johndoe@gmail.com" }'

response = requests.post('https://api.us.springverify.com/company/employee/search', headers=headers, data=data)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/company/employee/search")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "success": true,
    "data": [
        {
            "id": "6d6d81f5-6f00-4a38-a7d7-249cc2127ab6",
            "email": "johndoe@gmail.com",
            "first_name": null,
            "middle_name": null,
            "last_name": null,
            "status": null,
            "employee_limit": {
                "id": "bd95351f-fc2a-4ff9-8652-88abbfe67bdd",
                "employee_invite_group": {
                    "id": "4153a150-4157-48ab-b258-9a9b47a05fc2",
                    "package": "diamond"
                }
            }
        }
    ]
}
```

This API searches and retrieves the profile of a specific employee that is already verified.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| search | `string`| Contains the search term |

## Get company action

```shell
curl --location --request POST 'https://api.us.springverify.com/company/actions' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "limit":10,
    "offset":0
}'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/company/actions', {
    method: 'POST',
    headers: {
        'Authorization': 'Bearer JWT_TOKEN',
        'Content-Type': 'application/json'
    },
    body: JSON.stringify({ "limit":10, "offset":0 })
});

// REQUEST

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN',
    'Content-Type': 'application/json'
};

var dataString = '{ "limit":10, "offset":0 }';

var options = {
    url: 'https://api.us.springverify.com/company/actions',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Authorization' => 'Bearer JWT_TOKEN',
    'Content-Type' => 'application/json'
);
$data = '{ "limit":10, "offset":0 }';
$response = Requests::post('https://api.us.springverify.com/company/actions', $headers, $data);
```

```python
import requests

headers = {
    'Authorization': 'Bearer JWT_TOKEN',
    'Content-Type': 'application/json',
}

data = '{ "limit":10, "offset":0 }'

response = requests.post('https://api.us.springverify.com/company/actions', headers=headers, data=data)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/company/actions")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
  "success": true,
  "data": [
    {
      "string": "identity verification failed. (using their driving license)",
      "time": "2020-08-27T05:59:02.000Z",
      "id": "9282698a-72ac-49df-a750-85c169bfe08e",
      "first_name": "John",
      "middle_name": "Mark",
      "last_name": "Smith",
      "email": "johndoe@gmail.com"
    },
    {
      "string": "identity verification failed. (using their driving license)",
      "time": "2020-08-27T05:59:02.000Z",
      "id": "9282698a-72ac-49df-a750-85c169bfe08e",
      "first_name": "John",
      "middle_name": "Mark",
      "last_name": "Smith",
      "email": "johndoe@gmail.com"
    },
    {
      "string": "identity verification failed. (using their driving license)",
      "time": "2020-08-27T05:59:02.000Z",
      "id": "9282698a-72ac-49df-a750-85c169bfe08e",
      "first_name": "John",
      "middle_name": "Mark",
      "last_name": "Smith",
      "email": "johndoe@gmail.com"
    },
    {
      "string": "has successfully verified their identity. (using their driving license)",
      "time": "2020-08-27T05:57:03.000Z",
      "id": "9282698a-72ac-49df-a750-85c169bfe08e",
      "first_name": "John",
      "middle_name": "Mark",
      "last_name": "Smith",
      "email": "johndoe@gmail.com"
    },
    {
      "string": "has uploaded their driving license.",
      "time": "2020-08-27T05:56:44.000Z",
      "id": "9282698a-72ac-49df-a750-85c169bfe08e",
      "first_name": "John",
      "middle_name": "Mark",
      "last_name": "Smith",
      "email": "johndoe@gmail.com"
    },
    {
      "string": "has entered personal details.",
      "time": "2020-08-27T05:55:40.000Z",
      "id": "9282698a-72ac-49df-a750-85c169bfe08e",
      "first_name": "John",
      "middle_name": "Mark",
      "last_name": "Smith",
      "email": "johndoe@gmail.com"
    },
    {
      "string": "has been added as a candidate and invited to enter his information.",
      "time": "2020-08-27T05:54:24.000Z",
      "id": "9282698a-72ac-49df-a750-85c169bfe08e",
      "first_name": "John",
      "middle_name": "Mark",
      "last_name": "Smith",
      "email": "johndoe@gmail.com"
    },
    {
      "string": "identity verification failed. (using their driving license)",
      "time": "2020-08-27T04:29:02.000Z",
      "id": "8bf63c9a-b89c-4dff-a03c-0d571dc4c47e",
      "first_name": "John",
      "middle_name": "Mark",
      "last_name": "Smith",
      "email": "johndoe@gmail.com"
    },
    {
      "string": "identity verification failed. (using their driving license)",
      "time": "2020-08-27T04:29:02.000Z",
      "id": "8bf63c9a-b89c-4dff-a03c-0d571dc4c47e",
      "first_name": "John",
      "middle_name": "Mark",
      "last_name": "Smith",
      "email": "johndoe@gmail.com"
    },
    {
      "string": "identity verification failed. (using their driving license)",
      "time": "2020-08-27T04:29:02.000Z",
      "id": "8bf63c9a-b89c-4dff-a03c-0d571dc4c47e",
      "first_name": "John",
      "middle_name": "Mark",
      "last_name": "Smith",
      "email": "johndoe@gmail.com"
    }
  ]
}
```

This API informs an admin/s of the activities of a specific employee or a candidate in the following scenarios:

* The candidate has been invited to provide their information to initiate the verification.
* The candidate has entereed their personal information.
* The candidate has successfully uploaded their driver's license.
* The candidate has successfully verified their identity using their driver's license.
* The candidate has failed to verify their identity using their driver's license.

## Get Company Actions Pertaining to an employee

```shell
curl --location --request POST 'https://api.us.springverify.com/company/actions' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "limit":10,
    "offset":0,
    "email":"employee@email.com"
}'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/company/actions', {
    method: 'POST',
    headers: {
        'Authorization': 'Bearer JWT_TOKEN',
        'Content-Type': 'application/json'
    },
    body: JSON.stringify({ "limit":10, "offset":0, "email":"employee@email.com" })
});

// REQUEST

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN',
    'Content-Type': 'application/json'
};

var dataString = '{ "limit":10, "offset":0, "email":"employee@email.com" }';

var options = {
    url: 'https://api.us.springverify.com/company/actions',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Authorization' => 'Bearer JWT_TOKEN',
    'Content-Type' => 'application/json'
);
$data = '{ "limit":10, "offset":0, "email":"employee@email.com" }';
$response = Requests::post('https://api.us.springverify.com/company/actions', $headers, $data);
```

```python
import requests

headers = {
    'Authorization': 'Bearer JWT_TOKEN',
    'Content-Type': 'application/json',
}

data = '{ "limit":10, "offset":0, "email":"employee@email.com" }'

response = requests.post('https://api.us.springverify.com/company/actions', headers=headers, data=data)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/company/actions")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
  "success": true,
  "data": [
    {
      "string": "employment check is in progress.",
      "time": "2020-08-13T14:32:50.000Z",
      "id": "298af70a-b3a3-4605-8789-37387bb276a1",
      "first_name": "Mark",
      "middle_name": "James",
      "last_name": "Smith",
      "email": "johhndoe@gmail.com"
    },
    {
      "string": "education check is in progress.",
      "time": "2020-08-13T14:32:08.000Z",
      "id": "298af70a-b3a3-4605-8789-37387bb276a1",
      "first_name": "Mark",
      "middle_name": "James",
      "last_name": "Smith",
      "email": "johhndoe@gmail.com"
    },
    {
      "string": "has entered their education.",
      "time": "2020-08-13T14:31:34.000Z",
      "id": "298af70a-b3a3-4605-8789-37387bb276a1",
      "first_name": "Mark",
      "middle_name": "James",
      "last_name": "Smith",
      "email": "johhndoe@gmail.com"
    },
    {
      "string": "has entered their employment.",
      "time": "2020-08-13T14:26:25.000Z",
      "id": "298af70a-b3a3-4605-8789-37387bb276a1",
      "first_name": "Mark",
      "middle_name": "James",
      "last_name": "Smith",
      "email": "johhndoe@gmail.com"
    },
    {
      "string": "has successfully verified their identity. (using their driving license)",
      "time": "2020-08-13T14:24:44.000Z",
      "id": "298af70a-b3a3-4605-8789-37387bb276a1",
      "first_name": "Mark",
      "middle_name": "James",
      "last_name": "Smith",
      "email": "johhndoe@gmail.com"
    },
    {
      "string": "has uploaded their driving license.",
      "time": "2020-08-13T14:23:15.000Z",
      "id": "298af70a-b3a3-4605-8789-37387bb276a1",
      "first_name": "Mark",
      "middle_name": "James",
      "last_name": "Smith",
      "email": "johhndoe@gmail.com"
    },
    {
      "string": "has entered personal details.",
      "time": "2020-08-13T14:21:13.000Z",
      "id": "298af70a-b3a3-4605-8789-37387bb276a1",
      "first_name": "Mark",
      "middle_name": "James",
      "last_name": "Smith",
      "email": "johhndoe@gmail.com"
    },
    {
      "string": "has been added as a candidate and invited to enter his information.",
      "time": "2020-08-13T14:19:32.000Z",
      "id": "298af70a-b3a3-4605-8789-37387bb276a1",
      "first_name": "Mark",
      "middle_name": "James",
      "last_name": "Smith",
      "email": "johhndoe@gmail.com"
    }
  ]
}
```

This API informs an admin/s of the activities of a specific employee or a candidate in the following scenarios:

* The candidate has been invited to provide their information to initiate the verification.
* The candidate has entereed their personal information.
* The candidate has successfully uploaded their driver's license.
* The candidate has successfully verified their identity using their driver's license.
* The candidate has entered their education information.
* The candidate has entered their employment information.
* Verification for the education information has been initiated.
* Verification for the employment information has been initiated.

## Get Company Employees by Filter

```shell
curl --location --request POST 'https://api.us.springverify.com/company/employees' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "limit":1,
    "offset":0,
    "filter":"NULL"
}'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/company/employees', {
    method: 'POST',
    headers: {
        'Authorization': 'Bearer JWT_TOKEN',
        'Content-Type': 'application/json'
    },
    body: JSON.stringify({ "limit":1, "offset":0, "filter":"NULL" })
});

// REQUEST

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN',
    'Content-Type': 'application/json'
};

var dataString = '{ "limit":1, "offset":0, "filter":"NULL" }';

var options = {
    url: 'https://api.us.springverify.com/company/employees',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Authorization' => 'Bearer JWT_TOKEN',
    'Content-Type' => 'application/json'
);
$data = '{ "limit":1, "offset":0, "filter":"NULL" }';
$response = Requests::post('https://api.us.springverify.com/company/employees', $headers, $data);
```

```python
import requests

headers = {
    'Authorization': 'Bearer JWT_TOKEN',
    'Content-Type': 'application/json',
}

data = '{ "limit":1, "offset":0, "filter":"NULL" }'

response = requests.post('https://api.us.springverify.com/company/employees', headers=headers, data=data)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/company/employees")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "success": true,
    "data": {
        "employees": [{
            "id": "5d7bea97-fdca-4f2b-be32-72637a3491f4",
            "email": "johbobby@Johns.com",
            "first_name": "John",
            "middle_name": "Bobby",
            "last_name": "Johns",
            "status": "PENDING",
            "created_at": "2020-08-19T16:32:11.000Z",
            "kbaqna": null,
            "employments": [{
                "status": 0,
                "super_admin_status": null,
                "status_new": null,
                "super_admin_status_new": null,
                "adverse_action": null
            }],
            "education": [{
                "status": 3,
                "super_admin_status": null,
                "status_new": null,
                "super_admin_status_new": null,
                "adverse_action": null
            }],
            "professional_licenses": [],
            "employee_limit": {
                "employment": 2,
                "education": 1,
                "professional_license": 0,
                "civil_court": 0,
                "county_criminal_search": 0,
                "all_county_criminal_search": true,
                "employee_invite_group": {
                    "package": "platinum"
                }
            },
            "employee_verification": {
                "s3_passport_verified": null,
                "s3_dl_verified": 1,
                "verification_type": "id",
                "super_admin_status": null,
                "passport_status": null,
                "dl_status": null,
                "super_admin_status_new": null
            },
            "criminal_statuses": {
                "national_criminal": "PENDING",
                "sex_offender": "PENDING",
                "global_watchlist": "PENDING",
                "county_criminal_search": "PENDING"
            },
            "cic_criminal_records": [{
                    "id": "8b454419-8691-4688-9719-72571012978c",
                    "record_type": "SEX_OFFENDER",
                    "reviewed": false,
                    "adverse_action": {
                        "status": "CLOSED",
                        "cleared": false,
                        "id": "af1cef6e-e38f-4545-8fc4-6a31c0886b37"
                    }
                },
                {
                    "id": "ade7b7cd-c80c-4302-a2f0-6066f81322b3",
                    "record_type": "SEX_OFFENDER",
                    "reviewed": true,
                    "adverse_action": {
                        "status": "CLOSED",
                        "cleared": false,
                        "id": "af1cef6e-e38f-4545-8fc4-6a31c0886b37"
                    }
                }
            ],
            "sjv_criminal_reports": [{
                    "id": "428fc0d7-9122-4eed-8a76-a382be5e847f",
                    "sjv_search_type": "COUNTY_CRIMINAL",
                    "county_name": {
                        "county_name": "Ocean County"
                    },
                    "adverse_action": {
                        "status": "PENDING",
                        "cleared": null,
                        "id": "350849d9-24c2-40ea-ad20-d5654854fac9"
                    }
                },
                {
                    "id": "281edcbe-cdb4-411e-8d8e-87f7aea33b2a",
                    "sjv_search_type": "COUNTY_CRIMINAL_NOTIFICATION",
                    "county_name": null,
                    "adverse_action": null
                },
                {
                    "id": "96b6aef0-1114-4e5b-99a4-f9ed917504a2",
                    "sjv_search_type": "NATIONAL_CRIMINAL",
                    "county_name": {
                        "county_name": "Palm Beach County"
                    },
                    "adverse_action": null
                }
            ]
        }],
        "count": 238
    }
}
```

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| limit | `integer` | Page limit |
| offset | `integer` | Page offset |
| filter | `string` | ALL/COMPLETED/VERIFIED/PENDING/NULL(AWAITING EMPLOYEE INPUT)/FAILED.

### Get Company Adverse Action Counts

```shell
curl --location --request GET 'https://api.us.springverify.com/company/action/adverse/counts' \
--header 'Authorization: Bearer JWT_TOKEN'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/company/action/adverse/counts', {
    headers: {
        'Authorization': 'Bearer JWT_TOKEN'
    }
});

// REQUEST

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN'
};

var options = {
    url: 'https://api.us.springverify.com/company/action/adverse/counts',
    headers: headers
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Authorization' => 'Bearer JWT_TOKEN'
);
$response = Requests::get('https://api.us.springverify.com/company/action/adverse/counts', $headers);
```

```python
import requests

headers = {
    'Authorization': 'Bearer JWT_TOKEN',
}

response = requests.get('https://api.us.springverify.com/company/action/adverse/counts', headers=headers)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/company/action/adverse/counts")
request = Net::HTTP::Get.new(uri)
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "message": "successfully retrieved count",
    "data": {
        "PENDING": {
            "pendingCount": 4,
            "totalCount": 8
        },
        "NOTICE_SENT": {
            "pendingCount": 2,
            "totalCount": 19
        },
        "IN_REVIEW": {
            "pendingCount": 2,
            "totalCount": 3
        },
        "FINAL_CALL": {
            "pendingCount": 0,
            "totalCount": 24
        },
        "CLOSED": {
            "pendingCount": 5,
            "totalCount": 30
        }
    }
}
```

This API gives the count of the total adverse actions that are/have been:

* closed -- the employee's response has been accepted or rejected and the verification is completed.
* pending -- the employee is to be informed of the issue; or the employee's response is to be received.
* being reviewed -- the employee's response is being reviewed.
* sent a notice -- the employee has been informed of an issue with their verification.
* taken a final call on -- a decision has been taken by the company on the adverse action and is to be informed to the employee.

## Get Company Adverse Actions

```shell
curl --location --request POST 'https://api.us.springverify.com/company/action/adverse/fetch' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "limit":10,
    "offset":0,
    "status":"PENDING"
}'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/company/action/adverse/fetch', {
    method: 'POST',
    headers: {
        'Authorization': 'Bearer JWT_TOKEN',
        'Content-Type': 'application/json'
    },
    body: JSON.stringify({ "limit":10, "offset":0, "status":"PENDING" })
});

// REQUEST

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN',
    'Content-Type': 'application/json'
};

var dataString = '{ "limit":10, "offset":0, "status":"PENDING" }';

var options = {
    url: 'https://api.us.springverify.com/company/action/adverse/fetch',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Authorization' => 'Bearer JWT_TOKEN',
    'Content-Type' => 'application/json'
);
$data = '{ "limit":10, "offset":0, "status":"PENDING" }';
$response = Requests::post('https://api.us.springverify.com/company/action/adverse/fetch', $headers, $data);
```

```python
import requests

headers = {
    'Authorization': 'Bearer JWT_TOKEN',
    'Content-Type': 'application/json',
}

data = '{ "limit":10, "offset":0, "status":"PENDING" }'

response = requests.post('https://api.us.springverify.com/company/action/adverse/fetch', headers=headers, data=data)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/company/action/adverse/fetch")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "success": true,
    "data": {
        "adverse_actions": [
            {
                "id": "2504d7c7-3f68-47a7-b9dc-be2adb99936e",
                "check_id": null,
                "status": "PENDING",
                "type": "SEX_OFFENDER",
                "cleared": null,
                "comments": null,
                "created_at": "2020-08-25T08:17:24.000Z",
                "updated_at": "2020-08-25T08:17:24.000Z",
                "deleted_at": null,
                "education": null,
                "employment": null,
                "read_statuses": [],
                "cic_criminal_records": [
                    {
                        "id": "0548ea1c-b3de-4008-8147-1376c5870b8f",
                        "employee_email_fk": "johndoe@gmail.com",
                        "cic_criminal_report_id_fk": "62b71576-8456-42db-8ce9-4d12b2d864cb",
                        "created_at": "2020-08-13T14:24:42.000Z",
                        "updated_at": "2020-08-25T08:17:24.000Z",
                        "employee": {
                            "id": "298af70a-b3a3-4605-8789-37387bb276a1",
                            "access_id": "05065b6d-8272-4ba5-b31e-b9f576c0a44a",
                            "email": "johndoe@gmail.com",
                            "password_hash": "qwerty123",
                            "first_name": "John",
                            "middle_name": "Mark",
                            "last_name": "Smith",
                            "name_verified": null,
                            "created_at": "2020-08-13T13:59:22.000Z",
                            "updated_at": "2020-08-25T08:17:24.000Z",
                            "employer_id": "1d4fb8ba-09ac-412c-aa62-58970b4d7472",
                            "payment_id": "3aa91fd2-aca5-456b-a375-cf4df6ededed",
                            "email_sent": true,
                            "payment": false,
                            "status": "VERIFIED",
                            "flow_completed": true,
                            "company_created_by": "john@wick.com",
                            "employee_limit_id": "0e3ee39a-32c2-4eb6-a7f2-61a3507a26ba"
                        }
                    }
                ],
                "sjv_criminal_reports": []
            },
            {
                "id": "df359f7d-fff3-4de6-bbfa-f3719b1f4b9b",
                "check_id": null,
                "status": "PENDING",
                "type": "SEX_OFFENDER",
                "cleared": null,
                "comments": null,
                "created_at": "2020-08-20T12:45:29.000Z",
                "updated_at": "2020-08-20T12:45:29.000Z",
                "deleted_at": null,
                "education": null,
                "employment": null,
                "read_statuses": [],
                "cic_criminal_records": [
                    {
                        "id": "271e6b31-cef1-4b67-a8b5-28a1781081c5",
                        "employee_email_fk": "johndoe@gmail.com",
                        "cic_criminal_report_id_fk": "45127f48-b878-4aa5-8bab-881b79883d4a",
                        "created_at": "2020-08-20T12:07:49.000Z",
                        "updated_at": "2020-08-20T12:45:29.000Z",
                        "employee": {
                            "id": "37d40f7f-90a9-415d-8679-75500654aa5e",
                            "access_id": "3d5ed4fd-f2c4-4d86-94c0-c8836fe12132",
                            "email": "johndoe@gmail.com",
                            "password_hash": "100d5fcb45dc552d1a9011e2707b937904f79df410199fcb7a1e2b3c022d9911",
                            "first_name": "John",
                            "middle_name": "Mark",
                            "last_name": "Smith",
                            "name_verified": null,
                            "created_at": "2020-08-20T11:27:01.000Z",
                            "updated_at": "2020-08-20T12:45:30.000Z",
                            "employer_id": "1d4fb8ba-09ac-412c-aa62-58970b4d7472",
                            "payment_id": "881e74c4-0c8c-4c7c-9238-be5053474ee2",
                            "email_sent": true,
                            "payment": false,
                            "status": "FAILED",
                            "flow_completed": true,
                            "company_created_by": "john@wick.com",
                            "employee_limit_id": "8a51971e-bf6f-4b03-9db9-9062546e0533"
                        }
                    }
                ],
                "sjv_criminal_reports": []
            },
            {
                "id": "350849d9-24c2-40ea-ad20-d5654854fac9",
                "check_id": null,
                "status": "PENDING",
                "type": "COUNTY_CRIMINAL",
                "cleared": null,
                "comments": null,
                "created_at": "2020-08-19T18:11:53.000Z",
                "updated_at": "2020-08-19T18:11:53.000Z",
                "deleted_at": null,
                "education": null,
                "employment": null,
                "read_statuses": [],
                "cic_criminal_records": [],
                "sjv_criminal_reports": [
                    {
                        "id": "428fc0d7-9122-4eed-8a76-a382be5e847f",
                        "employee_email_fk": "johndoe@gmail.com",
                        "status": "RECEIVED",
                        "sjv_search_type": "COUNTY_CRIMINAL",
                        "reference_id": null,
                        "cic_criminal_record_fk": null,
                        "report_link": "report_link",
                        "marked_done": 0,
                        "marked_reviewed": 0,
                        "county_id": 1789,
                        "adverse_action_fk": "350849d9-24c2-40ea-ad20-d5654854fac9",
                        "authenticating_unique_identifier": "08bdee45-0cf6-4630-9f34-a9b0e50d2fcf",
                        "created_at": "2020-08-19T18:11:51.000Z",
                        "updated_at": "2020-08-19T18:11:53.000Z",
                        "county_name": {
                            "id": "1789",
                            "county_name": "Ocean County",
                            "state_name": "New Jersey",
                            "created_at": null,
                            "updated_at": null
                        },
                        "employee": {
                            "id": "5d7bea97-fdca-4f2b-be32-72637a3491f4",
                            "access_id": "67a61c82-8bf6-4717-8df1-a0ece4ffd2c8",
                            "email": "johndoe@gmail.com",
                            "password_hash": "100d5fcb45dc552d1a9011e2707b937904f79df410199fcb7a1e2b3c022d9911",
                            "first_name": "Jane",
                            "middle_name": "Elizabeth",
                            "last_name": "Smith",
                            "name_verified": null,
                            "created_at": "2020-08-19T16:32:11.000Z",
                            "updated_at": "2020-08-19T17:45:05.000Z",
                            "employer_id": "1d4fb8ba-09ac-412c-aa62-58970b4d7472",
                            "payment_id": "c26bc8a4-c136-4af6-bf2e-497745597085",
                            "email_sent": true,
                            "payment": false,
                            "status": "PENDING",
                            "flow_completed": true,
                            "company_created_by": "john@wick.com",
                            "employee_limit_id": "372b4cc6-63ce-4811-9d80-eb0527bce1ea"
                        }
                    }
                ]
            },
            {
                "id": "5c0a073a-ebd8-4417-989f-7ac5f3b11f98",
                "check_id": "b33aaaa7-5253-4542-a18f-ab4ff91ae8d3",
                "status": "PENDING",
                "type": "EMPLOYMENT",
                "cleared": null,
                "comments": null,
                "created_at": "2020-08-12T13:59:03.000Z",
                "updated_at": "2020-08-12T13:59:03.000Z",
                "deleted_at": null,
                "education": null,
                "employment": {
                    "id": "b33aaaa7-5253-4542-a18f-ab4ff91ae8d3",
                    "email": "johndoe@gmail.com",
                    "access_id": null,
                    "employer_name": "Nags",
                    "employer_name_verified": null,
                    "employer_phone": null,
                    "employer_phone_verified": null,
                    "employer_address": "Philadelphia International Airport (PHL), Essington Ave, Philadelphia, PA, USA",
                    "employer_address_verified": null,
                    "employer_town": "Philadelphia",
                    "employer_town_verified": null,
                    "state": "PA",
                    "state_verified": null,
                    "zipcode": "19153",
                    "zipcode_verified": null,
                    "employer_country": "United States",
                    "employer_country_verified": null,
                    "job_title": "sefdwe",
                    "job_title_verified": null,
                    "start_date": "01-01-2010",
                    "start_date_verified": null,
                    "end_date": "01-01-2018",
                    "end_date_verified": null,
                    "supervisor_name": "ber",
                    "supervisor_contact": null,
                    "current_employment": null,
                    "current_employment_verified": null,
                    "contract_type": null,
                    "contract_type_verified": null,
                    "source": null,
                    "created_at": "2020-08-12T13:58:20.000Z",
                    "updated_at": "2020-08-19T14:14:15.000Z",
                    "job_type": "full time employee",
                    "reason_for_leaving": null,
                    "status": 2,
                    "status_new": "FAILED",
                    "super_admin_status": null,
                    "super_admin_status_new": null,
                    "consent": false,
                    "employee": {
                        "id": "ee561c16-db51-4da0-a2b5-57780703ed3c",
                        "access_id": "36761e70-abcc-4d25-8b88-cf9f5ecd0ae5",
                        "email": "johndoe@gmail.com",
                        "password_hash": null,
                        "first_name": "Jane",
                        "middle_name": "Elizabeth",
                        "last_name": "Smith",
                        "name_verified": null,
                        "created_at": "2020-08-12T13:53:46.000Z",
                        "updated_at": "2020-08-12T13:55:43.000Z",
                        "employer_id": "1d4fb8ba-09ac-412c-aa62-58970b4d7472",
                        "payment_id": "b76847d7-fd23-4a64-8fb9-5d57f3c2693a",
                        "email_sent": true,
                        "payment": false,
                        "status": null,
                        "flow_completed": null,
                        "company_created_by": "john@wick.com",
                        "employee_limit_id": "2621a7d1-b00c-408d-99bc-c1f072e4c8ac"
                    }
                },
                "read_statuses": [],
                "cic_criminal_records": [],
                "sjv_criminal_reports": []
            },
            {
                "id": "b376ef1b-65d7-4f5a-ab0c-ccb81a05f55b",
                "check_id": null,
                "status": "PENDING",
                "type": "SEX_OFFENDER",
                "cleared": null,
                "comments": null,
                "created_at": "2020-07-17T15:11:45.000Z",
                "updated_at": "2020-07-17T15:11:45.000Z",
                "deleted_at": null,
                "education": null,
                "employment": null,
                "read_statuses": [
                    {
                        "id": "772ae675-f222-4baa-b31b-75007cba318a",
                        "reference_id": "b376ef1b-65d7-4f5a-ab0c-ccb81a05f55b",
                        "user_email": "john@wick.com",
                        "read": true,
                        "table": null,
                        "user_role": null,
                        "created_at": "2020-07-17T15:38:28.000Z",
                        "updated_at": "2020-07-17T15:38:28.000Z"
                    }
                ],
                "cic_criminal_records": [
                    {
                        "id": "bdb73700-4445-4954-8b36-9c144c46ab29",
                        "employee_email_fk": "johndoe@gmail.com",
                        "cic_criminal_report_id_fk": "234220d6-1a3b-49c5-9ea1-a1daa1cb710f",
                        "created_at": "2020-07-17T15:09:02.000Z",
                        "updated_at": "2020-07-20T13:32:48.000Z",
                        "employee": {
                            "id": "7cdbce62-b788-461a-8560-3264d10eeaac",
                            "access_id": "ce907ccd-ccd6-4aab-bc81-e2b7053fd915",
                            "email": "johndoe@gmail.com",
                            "password_hash": "100d5fcb45dc552d1a9011e2707b937904f79df410199fcb7a1e2b3c022d9911",
                            "first_name": "Jane",
                            "middle_name": "Elizabeth",
                            "last_name": "Smith",
                            "name_verified": null,
                            "created_at": "2020-07-17T15:06:30.000Z",
                            "updated_at": "2020-07-17T15:09:29.000Z",
                            "employer_id": "1d4fb8ba-09ac-412c-aa62-58970b4d7472",
                            "payment_id": "8779db6f-99a8-4d2d-b05a-6481112f1c5c",
                            "email_sent": true,
                            "payment": false,
                            "status": "PENDING",
                            "flow_completed": true,
                            "company_created_by": "john@wick.com",
                            "employee_limit_id": "797dbc70-04df-4d26-8140-dfd11bdb37aa"
                        }
                    }
                ],
                "sjv_criminal_reports": []
            },
            {
                "id": "0c495ae6-756b-4681-b295-3b0f8ea96eac",
                "check_id": null,
                "status": "PENDING",
                "type": "SEX_OFFENDER",
                "cleared": null,
                "comments": null,
                "created_at": "2020-07-09T14:57:35.000Z",
                "updated_at": "2020-07-09T14:57:35.000Z",
                "deleted_at": null,
                "education": null,
                "employment": null,
                "read_statuses": [
                    {
                        "id": "7347e6c6-d863-4de4-b945-049dc00583c6",
                        "reference_id": "0c495ae6-756b-4681-b295-3b0f8ea96eac",
                        "user_email": "john@wick.com",
                        "read": true,
                        "table": null,
                        "user_role": null,
                        "created_at": "2020-07-24T10:49:38.000Z",
                        "updated_at": "2020-07-24T10:49:38.000Z"
                    }
                ],
                "cic_criminal_records": [
                    {
                        "id": "e8e33362-8fb0-4fed-8e5c-b2f6575f33bf",
                        "employee_email_fk": "johndoe@gmail.com",
                        "cic_criminal_report_id_fk": "3dca9124-83e9-4a2c-adac-a16b80e40fa5",
                        "created_at": "2020-07-09T10:52:32.000Z",
                        "updated_at": "2020-07-09T14:57:35.000Z",
                        "employee": {
                            "id": "00890df3-2595-4cb9-9a76-6d743277ef21",
                            "access_id": "573ab2de-9aea-4355-8c73-34329f815ac4",
                            "email": "johndoe@gmail.com",
                            "password_hash": null,
                            "first_name": "John",
                            "middle_name": "Mark",
                            "last_name": "Smith",
                            "name_verified": null,
                            "created_at": "2020-07-09T10:38:53.000Z",
                            "updated_at": "2020-07-09T10:43:23.000Z",
                            "employer_id": "1d4fb8ba-09ac-412c-aa62-58970b4d7472",
                            "payment_id": "acf07517-7b5f-4145-8b19-311e3b42ca9e",
                            "email_sent": true,
                            "payment": false,
                            "status": null,
                            "flow_completed": false,
                            "company_created_by": "john@wick.com",
                            "employee_limit_id": "9bd3df0b-eabe-4e2e-bf29-0d8f9448b977"
                        }
                    }
                ],
                "sjv_criminal_reports": []
            },
            {
                "id": "9f27106e-3536-429e-b33c-a6c6949552cd",
                "check_id": "734bbf7d-fada-490d-a448-68601f29b235",
                "status": "PENDING",
                "type": "EMPLOYMENT",
                "cleared": null,
                "comments": null,
                "created_at": "2020-06-26T13:44:02.000Z",
                "updated_at": "2020-06-26T13:44:02.000Z",
                "deleted_at": null,
                "education": null,
                "employment": {
                    "id": "734bbf7d-fada-490d-a448-68601f29b235",
                    "email": "kavya@yopmail.com",
                    "access_id": null,
                    "employer_name": "Reliance",
                    "employer_name_verified": null,
                    "employer_phone": "1-9089877877",
                    "employer_phone_verified": null,
                    "employer_address": "3045 West 26th Street, Chicago, IL, USA",
                    "employer_address_verified": null,
                    "employer_town": "Chicago",
                    "employer_town_verified": null,
                    "state": "IL",
                    "state_verified": null,
                    "zipcode": "60623",
                    "zipcode_verified": null,
                    "employer_country": "United States",
                    "employer_country_verified": null,
                    "job_title": "SDET",
                    "job_title_verified": null,
                    "start_date": "01-01-2010",
                    "start_date_verified": null,
                    "end_date": "01-01-2017",
                    "end_date_verified": null,
                    "supervisor_name": "Kiya",
                    "supervisor_contact": null,
                    "current_employment": null,
                    "current_employment_verified": null,
                    "contract_type": null,
                    "contract_type_verified": null,
                    "source": null,
                    "created_at": "2020-06-26T13:39:30.000Z",
                    "updated_at": "2020-08-19T14:14:15.000Z",
                    "job_type": "full time employee",
                    "reason_for_leaving": null,
                    "status": 2,
                    "status_new": "FAILED",
                    "super_admin_status": null,
                    "super_admin_status_new": null,
                    "consent": false,
                    "employee": {
                        "id": "e4782824-57ac-49da-9a7c-f5d2633d73c1",
                        "access_id": "a7f3d1c0-2b52-48a5-b49d-d1bbe1f5b25c",
                        "email": "kavya@yopmail.com",
                        "password_hash": "319f97ff7cbeee9de027f4b4ae09b3e50b05a747d5b1ebd6bed4f8014fdd1ca2",
                        "first_name": "John",
                        "middle_name": "Mark",
                        "last_name": "Smith",
                        "name_verified": null,
                        "created_at": "2019-12-26T09:03:33.000Z",
                        "updated_at": "2020-06-26T13:39:51.000Z",
                        "employer_id": "1d4fb8ba-09ac-412c-aa62-58970b4d7472",
                        "payment_id": "1cfefa51-6698-42f7-a864-eb377509b4a6",
                        "email_sent": true,
                        "payment": false,
                        "status": "PENDING",
                        "flow_completed": true,
                        "company_created_by": "john@wick.com",
                        "employee_limit_id": "7dae42ca-5718-4fb3-ab21-74099c737c9c"
                    }
                },
                "read_statuses": [
                    {
                        "id": "dc1955a0-4903-4621-80e5-4dc2224e4edc",
                        "reference_id": "9f27106e-3536-429e-b33c-a6c6949552cd",
                        "user_email": "john@wick.com",
                        "read": true,
                        "table": "AdverseAction",
                        "user_role": null,
                        "created_at": "2020-06-26T13:44:02.000Z",
                        "updated_at": "2020-06-29T13:29:59.000Z"
                    }
                ],
                "cic_criminal_records": [],
                "sjv_criminal_reports": []
            },
            {
                "id": "99c97685-1653-4245-96a7-1b20d18e62d2",
                "check_id": "5844aec7-534b-4249-8a09-789cd76efeb2",
                "status": "PENDING",
                "type": "EMPLOYMENT",
                "cleared": null,
                "comments": null,
                "created_at": "2020-04-29T11:29:02.000Z",
                "updated_at": "2020-04-29T11:29:02.000Z",
                "deleted_at": null,
                "education": null,
                "employment": {
                    "id": "5844aec7-534b-4249-8a09-789cd76efeb2",
                    "email": "johndoe@gmail.com",
                    "access_id": null,
                    "employer_name": "HAweifg",
                    "employer_name_verified": null,
                    "employer_phone": null,
                    "employer_phone_verified": null,
                    "employer_address": "",
                    "employer_address_verified": null,
                    "employer_town": "Bengaluru",
                    "employer_town_verified": null,
                    "state": "KA",
                    "state_verified": null,
                    "zipcode": "560103",
                    "zipcode_verified": null,
                    "employer_country": "India",
                    "employer_country_verified": null,
                    "job_title": "Erew",
                    "job_title_verified": null,
                    "start_date": "01-01-2000",
                    "start_date_verified": null,
                    "end_date": "01-01-2010",
                    "end_date_verified": null,
                    "supervisor_name": "berge",
                    "supervisor_contact": null,
                    "current_employment": null,
                    "current_employment_verified": null,
                    "contract_type": null,
                    "contract_type_verified": null,
                    "source": null,
                    "created_at": "2020-04-29T10:34:19.000Z",
                    "updated_at": "2020-08-19T14:14:15.000Z",
                    "job_type": "full time employee",
                    "reason_for_leaving": null,
                    "status": 2,
                    "status_new": "FAILED",
                    "super_admin_status": 1,
                    "super_admin_status_new": "VERIFIED",
                    "consent": false,
                    "employee": {
                        "id": "dd924f25-ee91-4b94-af86-71451cec6695",
                        "access_id": "c6fcd2ff-d56f-4afd-b0bc-64039a00fc6b",
                        "email": "johndoe@gmail.com",
                        "password_hash": "100d5fcb45dc552d1a9011e2707b937904f79df410199fcb7a1e2b3c022d9911",
                        "first_name": "John",
                        "middle_name": "Mark",
                        "last_name": "Smith",
                        "name_verified": null,
                        "created_at": "2020-04-29T09:04:01.000Z",
                        "updated_at": "2020-05-14T14:23:08.000Z",
                        "employer_id": "1d4fb8ba-09ac-412c-aa62-58970b4d7472",
                        "payment_id": "5e758c74-4f5c-4846-9297-93ac85fab3d5",
                        "email_sent": true,
                        "payment": false,
                        "status": "VERIFIED",
                        "flow_completed": true,
                        "company_created_by": "john@wick.com",
                        "employee_limit_id": "8e4d574c-8ff9-472d-b432-9d9e1383abf8"
                    }
                },
                "read_statuses": [
                    {
                        "id": "adde9c54-3b73-4dd7-860c-823fc598c8a8",
                        "reference_id": "99c97685-1653-4245-96a7-1b20d18e62d2",
                        "user_email": "john@wick.com",
                        "read": true,
                        "table": "AdverseAction",
                        "user_role": null,
                        "created_at": "2020-04-29T11:29:02.000Z",
                        "updated_at": "2020-05-04T12:04:57.000Z"
                    }
                ],
                "cic_criminal_records": [],
                "sjv_criminal_reports": []
            }
        ],
        "count": 8
    }
}
```

This API can be used to get a list of the adversities found across all the profiles that have been processed.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| limit | `integer` | _Raw file of the logo._ |
| offset | `integer` | _Raw file of the logo._ |
| status | `string` | Current status of the action: (PENDING/NOTICE_SENT/IN_REVIEW/FINAL_CALL/CLOSED)

### Adverse Action of Single Employee

```shell
curl --location --request GET 'https://api.us.springverify.com/company/action/adverse?adverse_action_id=2504d7c7-3f68-47a7-b9dc-be2adb99936e' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/company/action/adverse?adverse_action_id=2504d7c7-3f68-47a7-b9dc-be2adb99936e', {
    headers: {
        'Authorization': 'Bearer JWT_TOKEN',
        'Content-Type': 'application/json'
    }
});

// REQUEST

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN',
    'Content-Type': 'application/json'
};

var options = {
    url: 'https://api.us.springverify.com/company/action/adverse?adverse_action_id=2504d7c7-3f68-47a7-b9dc-be2adb99936e',
    headers: headers
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Authorization' => 'Bearer JWT_TOKEN',
    'Content-Type' => 'application/json'
);
$response = Requests::get('https://api.us.springverify.com/company/action/adverse?adverse_action_id=2504d7c7-3f68-47a7-b9dc-be2adb99936e', $headers);
```

```python
import requests

headers = {
    'Authorization': 'Bearer JWT_TOKEN',
    'Content-Type': 'application/json',
}

params = (
    ('adverse_action_id', '2504d7c7-3f68-47a7-b9dc-be2adb99936e'),
)

response = requests.get('https://api.us.springverify.com/company/action/adverse', headers=headers, params=params)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/company/action/adverse?adverse_action_id=2504d7c7-3f68-47a7-b9dc-be2adb99936e")
request = Net::HTTP::Get.new(uri)
request.content_type = "application/json"
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response:

```json
{
    "success": true,
    "data": {
        "id": "2504d7c7-3f68-47a7-b9dc-be2adb99936e",
        "check_id": null,
        "status": "CLOSED",
        "type": "SEX_OFFENDER",
        "cleared": true,
        "comments": null,
        "created_at": "2020-08-25T08:17:24.000Z",
        "updated_at": "2020-08-26T12:00:11.000Z",
        "deleted_at": null,
        "education": null,
        "employment": null,
        "adverse_action_ledgers": [
            {
                "id": "21484067-1194-4495-a2f2-e80d54aff4be",
                "reference_id": "2504d7c7-3f68-47a7-b9dc-be2adb99936e",
                "status": "NOTICE_SENT",
                "comments": null,
                "cleared": null,
                "deleted_at": null,
                "created_at": "2020-08-26T11:58:27.000Z",
                "updated_at": "2020-08-26T11:58:27.000Z"
            },
            {
                "id": "622008d7-2553-4c32-bca0-3f5c833d2b66",
                "reference_id": "2504d7c7-3f68-47a7-b9dc-be2adb99936e",
                "status": "PENDING",
                "comments": null,
                "cleared": null,
                "deleted_at": null,
                "created_at": "2020-08-25T08:17:24.000Z",
                "updated_at": "2020-08-25T08:17:24.000Z"
            },
            {
                "id": "6d0a88fb-a297-47ca-a892-f06257fe2e48",
                "reference_id": "2504d7c7-3f68-47a7-b9dc-be2adb99936e",
                "status": "CLOSED",
                "comments": null,
                "cleared": true,
                "deleted_at": null,
                "created_at": "2020-08-26T12:00:11.000Z",
                "updated_at": "2020-08-26T12:00:11.000Z"
            }
        ],
        "adverse_action_comments": [],
        "cic_criminal_records": [
            {
                "id": "0548ea1c-b3de-4008-8147-1376c5870b8f",
                "employee_email_fk": "johndoe@gmail.com",
                "cic_criminal_report_id_fk": "62b71576-8456-42db-8ce9-4d12b2d864cb",
                "adverse_action_id_fk": "2504d7c7-3f68-47a7-b9dc-be2adb99936e",
                "record_type": "SEX_OFFENDER",
                "reviewed": false,
                "deleted_at": null,
                "created_at": "2020-08-13T14:24:42.000Z",
                "updated_at": "2020-08-25T08:17:24.000Z",
                "employee": {
                    "id": "298af70a-b3a3-4605-8789-37387bb276a1",
                    "access_id": "05065b6d-8272-4ba5-b31e-b9f576c0a44a",
                    "email": "johndoe@gmail.com",
                    "password_hash": "qwerty123",
                    "first_name": "John",
                    "middle_name": "Mark",
                    "last_name": "Smith",
                    "name_verified": null,
                    "created_at": "2020-08-13T13:59:22.000Z",
                    "updated_at": "2020-08-25T08:17:24.000Z",
                    "employer_id": "1d4fb8ba-09ac-412c-aa62-58970b4d7472",
                    "payment_id": "3aa91fd2-aca5-456b-a375-cf4df6ededed",
                    "email_sent": true,
                    "payment": false,
                    "status": "VERIFIED",
                    "flow_completed": true,
                    "company_created_by": "ajitesh.tiwari@springrole.com",
                    "employee_limit_id": "0e3ee39a-32c2-4eb6-a7f2-61a3507a26ba"
                },
                "record_data": {
                    "subject": {
                        "full_name": "JONATHAN DOE",
                        "dob": 19510000,
                        "image": "https://www.securecontinfo.com/user/images/sampleoffender.jpg",
                        "category": "SEX OFFENDER",
                        "source": "WI SEX OFFENDER REGISTRY",
                        "sex": "MALE",
                        "race": "BROWN",
                        "hair_color": "BROWN",
                        "eye_color": "BROWN",
                        "weight": "199 LBS",
                        "height": "5 FEET 10 INCHES",
                        "age": 64,
                        "case_number": 123456789,
                        "state": "CA",
                        "address": "123 CONSUMER RD, ca 95555",
                        "comments": "PHOTO DATE:  12/20/2010; CUSTODY/SUPERVISION:  TERMINATED; REGISTRATION END DATE:  LIFE REGISTRATION; COMPLIANCE STATUS:  COMPLIANT; PROGRAM INFORMATION:  DOC SORP ADMINISTRATION 3099 E WASHINGTON AVENUE Jane WI 53704 (608)240-5830;ID NUMBERS: DEPARTMENT OF CORRECTIONS NUMBER:00223018;  OFFENDER REGISTER DATE: 19901127",
                        "alias": "GREGORY CONSUMER"
                    },
                    "offenses": {
                        "offense": {
                            "description": "SEXUAL ASSAULT OF A CHILD",
                            "statute": "948.02(1)",
                            "conviction_date": "11/27/1990",
                            "conviction_location": "ROCK, WI"
                        }
                    }
                }
            },
            {
                "id": "63216b70-1c5e-4c46-bd5e-9d72ce7d78f5",
                "employee_email_fk": "johndoe@gmail.com",
                "cic_criminal_report_id_fk": "62b71576-8456-42db-8ce9-4d12b2d864cb",
                "adverse_action_id_fk": "2504d7c7-3f68-47a7-b9dc-be2adb99936e",
                "record_type": "SEX_OFFENDER",
                "reviewed": false,
                "deleted_at": null,
                "created_at": "2020-08-13T14:24:42.000Z",
                "updated_at": "2020-08-25T08:17:24.000Z",
                "employee": {
                    "id": "298af70a-b3a3-4605-8789-37387bb276a1",
                    "access_id": "05065b6d-8272-4ba5-b31e-b9f576c0a44a",
                    "email": "johndoe@gmail.com",
                    "password_hash": "qwerty123",
                    "first_name": "John",
                    "middle_name": "Mark",
                    "last_name": "Smith",
                    "name_verified": null,
                    "created_at": "2020-08-13T13:59:22.000Z",
                    "updated_at": "2020-08-25T08:17:24.000Z",
                    "employer_id": "1d4fb8ba-09ac-412c-aa62-58970b4d7472",
                    "payment_id": "3aa91fd2-aca5-456b-a375-cf4df6ededed",
                    "email_sent": true,
                    "payment": false,
                    "status": "VERIFIED",
                    "flow_completed": true,
                    "company_created_by": "ajitesh.tiwari@springrole.com",
                    "employee_limit_id": "0e3ee39a-32c2-4eb6-a7f2-61a3507a26ba"
                },
                "record_data": {
                    "subject": {
                        "full_name": "JONATHAN DOE",
                        "dob": "12/04/1951",
                        "category": "CRIMINAL",
                        "source": "FL DEPT OF CORRECTIONS- INMATE",
                        "sex": "MALE",
                        "race": "BLACK",
                        "hair_color": "BLACK",
                        "eye_color": "BROWN",
                        "weight": "240 LBS",
                        "height": "5 FEET 10 INCHES",
                        "age": 64,
                        "scars_marks": "GLASSES",
                        "case_number": 1550682,
                        "state": "FL",
                        "comments": "SENTENCE LENGTH: 13Y 0M 0D",
                        "alias": "JONATHAN CONSUMER,JONATHAN QUINCY CONSUMER,JOSEPH QUINCY CONSUMER"
                    },
                    "offenses": {
                        "offense": [
                            {
                                "description": "ROBB. GUN/DEADLY WPN(CONSPIRACY TO COMMIT)",
                                "disposition": "NOT PROVIDED BY SOURCE",
                                "disposition_date": "02-17-2012",
                                "offense_date": "07-26-2010",
                                "commitment_date": "03-12-2012",
                                "release_date": "07-26-2023"
                            },
                            {
                                "description": "2ND DEG.MURD,DANGEROUS ACT",
                                "disposition": "NOT PROVIDED BY SOURCE",
                                "disposition_date": "02-17-2012",
                                "offense_date": "07-26-2010",
                                "commitment_date": "03-12-2012",
                                "release_date": "07-26-2023"
                            },
                            {
                                "description": "ROBB. GUN/DEADLY WPN",
                                "disposition": "NOT PROVIDED BY SOURCE",
                                "disposition_date": "02-17-2012",
                                "offense_date": "07-26-2010",
                                "commitment_date": "03-12-2012",
                                "release_date": "07-26-2023"
                            }
                        ]
                    }
                }
            }
        ],
        "sjv_criminal_reports": []
    }
}
```

For a specific employee whose verification has resulted in an adverse action, this API can be used to get a list of the adversities found in the profile.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| adverse_action_id | `uuid` | The ID of the adverse action. |

## Mark Adverse Action as Read

```shell
curl --location --request PUT 'http://localhost:3080/company/action/adverse/mark-read' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "IDs":["2504d7c7-3f68-47a7-b9dc-be2adb99936e"]
}'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('http://localhost:3080/company/action/adverse/mark-read', {
    method: 'POST',
    headers: {
        'Authorization': 'Bearer JWT_TOKEN',
        'Content-Type': 'application/json'
    },
    body: JSON.stringify({ "IDs":["2504d7c7-3f68-47a7-b9dc-be2adb99936e"] })
});

// REQUEST

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN',
    'Content-Type': 'application/json'
};

var dataString = '{ "IDs":["2504d7c7-3f68-47a7-b9dc-be2adb99936e"] }';

var options = {
    url: 'http://localhost:3080/company/action/adverse/mark-read',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Authorization' => 'Bearer JWT_TOKEN',
    'Content-Type' => 'application/json'
);
$data = '{ "IDs":["2504d7c7-3f68-47a7-b9dc-be2adb99936e"] }';
$response = Requests::post('http://localhost:3080/company/action/adverse/mark-read', $headers, $data);
```

```python
import requests

headers = {
    'Authorization': 'Bearer JWT_TOKEN',
    'Content-Type': 'application/json',
}

data = '{ "IDs":["2504d7c7-3f68-47a7-b9dc-be2adb99936e"] }'

response = requests.post('http://localhost:3080/company/action/adverse/mark-read', headers=headers, data=data)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("http://localhost:3080/company/action/adverse/mark-read")
request = Net::HTTP::Put.new(uri)
request.content_type = "application/json"
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response 

```json
{
    "success": true,
    "data": true
}
```

Once an admin has been notified of a list of adversity (of a single employee's profile or of multiple profiles), this API is used to let the admin mark the notification as "read".

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| IDs | `array` | An array of the IDs that are to be marked as read. |

## Clear or Send Adverse Action Notice

```shell
curl --location --request PUT 'http://localhost:3080/company/action/adverse/pending' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "adverse_action_id":"2504d7c7-3f68-47a7-b9dc-be2adb99936e",
    "status":"NOTICE_SENT"
}'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('http://localhost:3080/company/action/adverse/pending', {
    method: 'POST',
    headers: {
        'Authorization': 'Bearer JWT_TOKEN',
        'Content-Type': 'application/json'
    },
    body: JSON.stringify({ "adverse_action_id":"2504d7c7-3f68-47a7-b9dc-be2adb99936e", "status":"NOTICE_SENT" })
});

// REQUEST

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN',
    'Content-Type': 'application/json'
};

var dataString = '{ "adverse_action_id":"2504d7c7-3f68-47a7-b9dc-be2adb99936e", "status":"NOTICE_SENT" }';

var options = {
    url: 'http://localhost:3080/company/action/adverse/pending',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Authorization' => 'Bearer JWT_TOKEN',
    'Content-Type' => 'application/json'
);
$data = '{ "adverse_action_id":"2504d7c7-3f68-47a7-b9dc-be2adb99936e", "status":"NOTICE_SENT" }';
$response = Requests::post('http://localhost:3080/company/action/adverse/pending', $headers, $data);
```

```python
import requests

headers = {
    'Authorization': 'Bearer JWT_TOKEN',
    'Content-Type': 'application/json',
}

data = '{ "adverse_action_id":"2504d7c7-3f68-47a7-b9dc-be2adb99936e", "status":"NOTICE_SENT" }'

response = requests.post('http://localhost:3080/company/action/adverse/pending', headers=headers, data=data)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("http://localhost:3080/company/action/adverse/pending")
request = Net::HTTP::Put.new(uri)
request.content_type = "application/json"
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```


> Success Response

```json
{
    "success": true,
    "data": true
}
```

Once notified, an admin can decide to take an action on the adversity notified or ignore it. This is the initial action particular adverse action lifecycle for the Admin.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| status | `string` | Current status of the action: NOTICE_SENT/IGNORED. |
| adverse_action_id | `string` | The ID of the adverse action whose status is being updated. |

## Set Final Adverse Action Status

```shell
curl --location --request PUT 'http://localhost:3080/company/action/adverse/final' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "adverse_action_id":"2504d7c7-3f68-47a7-b9dc-be2adb99936e",
    "status":"CLEAR"
}'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('http://localhost:3080/company/action/adverse/final', {
    method: 'POST',
    headers: {
        'Authorization': 'Bearer JWT_TOKEN',
        'Content-Type': 'application/json'
    },
    body: JSON.stringify({ "adverse_action_id":"2504d7c7-3f68-47a7-b9dc-be2adb99936e", "status":"CLEAR" })
});

// REQUEST

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN',
    'Content-Type': 'application/json'
};

var dataString = '{ "adverse_action_id":"2504d7c7-3f68-47a7-b9dc-be2adb99936e", "status":"CLEAR" }';

var options = {
    url: 'http://localhost:3080/company/action/adverse/final',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Authorization' => 'Bearer JWT_TOKEN',
    'Content-Type' => 'application/json'
);
$data = '{ "adverse_action_id":"2504d7c7-3f68-47a7-b9dc-be2adb99936e", "status":"CLEAR" }';
$response = Requests::post('http://localhost:3080/company/action/adverse/final', $headers, $data);
```

```python
import requests

headers = {
    'Authorization': 'Bearer JWT_TOKEN',
    'Content-Type': 'application/json',
}

data = '{ "adverse_action_id":"2504d7c7-3f68-47a7-b9dc-be2adb99936e", "status":"CLEAR" }'

response = requests.post('http://localhost:3080/company/action/adverse/final', headers=headers, data=data)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("http://localhost:3080/company/action/adverse/final")
request = Net::HTTP::Put.new(uri)
request.content_type = "application/json"
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "success": true,
    "data": true
}
```

On an employee profile that is being processed for adversities, an admin can set a final adverse action using this API. This is the final action in a particular adverse action lifecycle for the Admin.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| status | `string` | Current status of the action: CLEAR/REJECT. |
| adverse_action_id | `string` | The ID of the adverse action whose status is being updated. |

## Get Criminal Report

```shell
curl --location --request GET 'http://localhost:3080/company/criminal/sjv/report?sjv_id=0db2b92a-ec0d-4506-a356-fcb97f31e0c3' \
--header 'Authorization: Bearer JWT_TOKEN'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('http://localhost:3080/company/criminal/sjv/report?sjv_id=0db2b92a-ec0d-4506-a356-fcb97f31e0c3', {
    headers: {
        'Authorization': 'Bearer JWT_TOKEN'
    }
});

// REQUEST

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN'
};

var options = {
    url: 'http://localhost:3080/company/criminal/sjv/report?sjv_id=0db2b92a-ec0d-4506-a356-fcb97f31e0c3',
    headers: headers
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Authorization' => 'Bearer JWT_TOKEN'
);
$response = Requests::get('http://localhost:3080/company/criminal/sjv/report?sjv_id=0db2b92a-ec0d-4506-a356-fcb97f31e0c3', $headers);
```

```python
import requests

headers = {
    'Authorization': 'Bearer JWT_TOKEN',
}

params = (
    ('sjv_id', '0db2b92a-ec0d-4506-a356-fcb97f31e0c3'),
)

response = requests.get('http://localhost:3080/company/criminal/sjv/report', headers=headers, params=params)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("http://localhost:3080/company/criminal/sjv/report?sjv_id=0db2b92a-ec0d-4506-a356-fcb97f31e0c3")
request = Net::HTTP::Get.new(uri)
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "message": "successfully retrieved sjv report",
    "data": {
        "glossary.title": "example glossary",
        "glossary.GlossDiv.title": "S",
        "glossary.GlossDiv.GlossList.GlossEntry.ID": "SGML",
        "glossary.GlossDiv.GlossList.GlossEntry.SortAs": "SGML",
        "glossary.GlossDiv.GlossList.GlossEntry.GlossTerm": "Standard Generalized Markup Language",
        "glossary.GlossDiv.GlossList.GlossEntry.Acronym": "SGML",
        "glossary.GlossDiv.GlossList.GlossEntry.Abbrev": "ISO 8879:1986",
        "glossary.GlossDiv.GlossList.GlossEntry.GlossDef.para": "A meta-markup language, used to create markup languages such as DocBook.",
        "glossary.GlossDiv.GlossList.GlossEntry.GlossDef.GlossSeeAlso.0": "GML",
        "glossary.GlossDiv.GlossList.GlossEntry.GlossDef.GlossSeeAlso.1": "XML",
        "glossary.GlossDiv.GlossList.GlossEntry.GlossSee": "markup"
    }
}
```

On an employee profile that is being processed for adversities, an admin can get the criminal report using the SJV ID in the adverse action object of this API. This is a sample JSON response.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| sjv_id | `string` | The SJV ID of the criminal report. |

---

# User

This section covers the API details available for users logged in as _employees_ (or candidates), i.e., users who wish to get background verification checks performed on their profiles.

## Submit Personal Details

```shell
curl --location --request POST 'https://api.us.springverify.com/employee/personal-details' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: text/plain' \
--data-raw '{
	"first_name":"John",
	"middle_name":"David",
	"last_name":"Doe",
	"dob":"12-11-1980",
	"ssn":"123456789",
	"email":"johndoe@gmail.com",
    "house_number": "239",
    "street_name": "Avea street"
	"address":"236 Avea street",
	"city":"Gotham",
	"state":"CA",
	"zip_code":"33433",
	"phone":"56-999222992"
}'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/employee/personal-details', {
    method: 'POST',
    headers: {
        'Content-Type': 'text/plain',
        'Authorization': 'Bearer JWT_TOKEN'
    },
    body: '{ "first_name":"John", "middle_name":"David", "last_name":"Doe", "dob":"12-11-1980", "ssn":"123456789", "email":"johndoe@gmail.com", "house_number": "239", "street_name": "Avea street" "address":"236 Avea street", "city":"Gotham", "state":"CA", "zip_code":"33433", "phone":"56-999222992" }'
});

// REQUEST

var request = require('request');

var headers = {
    'Content-Type': 'text/plain',
    'Authorization': 'Bearer JWT_TOKEN'
};

var dataString = '{ "first_name":"John", "middle_name":"David", "last_name":"Doe", "dob":"12-11-1980", "ssn":"123456789", "email":"johndoe@gmail.com", "house_number": "239", "street_name": "Avea street" "address":"236 Avea street", "city":"Gotham", "state":"CA", "zip_code":"33433", "phone":"56-999222992" }';

var options = {
    url: 'https://api.us.springverify.com/employee/personal-details',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Content-Type' => 'text/plain',
    'Authorization' => 'Bearer JWT_TOKEN'
);
$data = '{ "first_name":"John", "middle_name":"David", "last_name":"Doe", "dob":"12-11-1980", "ssn":"123456789", "email":"johndoe@gmail.com", "house_number": "239", "street_name": "Avea street" "address":"236 Avea street", "city":"Gotham", "state":"CA", "zip_code":"33433", "phone":"56-999222992" }';
$response = Requests::post('https://api.us.springverify.com/employee/personal-details', $headers, $data);
```

```python
import requests

headers = {
    'Content-Type': 'text/plain',
    'Authorization': 'Bearer JWT_TOKEN',
}

data = '{ "first_name":"John", "middle_name":"David", "last_name":"Doe", "dob":"12-11-1980", "ssn":"123456789", "email":"johndoe@gmail.com", "house_number": "239", "street_name": "Avea street" "address":"236 Avea street", "city":"Gotham", "state":"CA", "zip_code":"33433", "phone":"56-999222992" }'

response = requests.post('https://api.us.springverify.com/employee/personal-details', headers=headers, data=data)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/employee/personal-details")
request = Net::HTTP::Post.new(uri)
request.content_type = "text/plain"
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "success": true,
    "data": {
        "id": "9ca37df2-1d81-40df-90aa-b88e081b8103",
        "access_id": "6e7d5f15-9456-430d-b9da-9df67a4d9996",
        "email": "johndoe@gmail.com",
        "password_hash": null,
        "first_name": "John",
        "middle_name": "David",
        "last_name": "Doe",
        "name_verified": null,
        "created_at": "2020-08-25T14:28:47.000Z",
        "updated_at": "2020-08-25T14:32:46.785Z",
        "employer_id": "1d4fb8ba-09ac-412c-aa62-58970b4d7472",
        "payment_id": "6c75e302-ac2a-4efa-9ba9-c5dfc97b941e",
        "email_sent": true,
        "payment": false,
        "status": null,
        "flow_completed": null,
        "company_created_by": "zed@max.com",
        "employee_limit_id": "e1e2fef4-a1dd-4be2-8dd1-b0ab8ec29ff8",
        "employments": [],
        "education": [],
        "cic_criminal_records": [],
        "professional_licenses": [],
        "employee_detail": {
            "id": "efa4191a-331d-48c3-8e52-8915ed8167be",
            "access_id": null,
            "address": "236 Avea street",
            "address_verified": null,
            "driving_number": null,
            "driving_number_verified": null,
            "city": "Gotham",
            "city_verified": null,
            "state": "CA",
            "state_verified": null,
            "zipcode": "33433",
            "zipcode_verified": null,
            "country": null,
            "country_verified": null,
            "birthdate": "12-11-1980",
            "birthdate_verified": null,
            "phone": "56-999222992",
            "phone_verified": null,
            "ssn": "6789",
            "ssn_verified": null,
            "created_at": "2020-08-25T14:32:46.000Z",
            "updated_at": "2020-08-25T14:32:46.000Z",
            "employee_email": "johndoe@gmail.com"
        },
        "employee_verification": null,
        "employee_limit": {
            "id": "e1e2fef4-a1dd-4be2-8dd1-b0ab8ec29ff8",
            "employment": 2,
            "education": 1,
            "professional_license": 0,
            "all_county_criminal_search": true,
            "county_criminal_search": 0,
            "civil_court": 1,
            "driving_license": 0,
            "package_id": null,
            "created_at": "2020-08-25T14:28:47.000Z",
            "updated_at": "2020-08-25T14:28:47.000Z",
            "employee_invite_group_id": "b630d0ec-e8d6-49a0-bbf2-8fd26a791a85",
            "employee_invite_group": {
                "id": "b630d0ec-e8d6-49a0-bbf2-8fd26a791a85",
                "package": "diamond",
                "active": true,
                "created_at": "2020-08-25T14:28:47.000Z",
                "updated_at": "2020-08-25T14:28:47.000Z",
                "company_created_by": "zed@max.com",
                "package_id": "5"
            }
        },
        "kbaqna": null,
        "employee_flow": {
            "id": 289,
            "employment_flow": null,
            "education_flow": null,
            "professional_license_flow": null,
            "created_at": "2020-08-25T14:32:40.000Z",
            "updated_at": "2020-08-25T14:32:40.000Z",
            "employee_email": "johndoe@gmail.com"
        },
        "s3_files": [],
        "criminal_statuses": [],
        "sjv_criminal_reports": []
    }
}
```

An employee can submit their personal details using this API. It is of the utmost importance that the details entered here are absolutely accurate. For creating the profile, the same email ID must be used to which the verification request was sent.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| first_name | `string` | The employee's first name. |
| middle_name | `string` | The employee's middle name. |
| last_name | `string` | The employee's last name. |
| dob | `string` | The employee's date of birth in the format DD-MM-YYYY. |
| ssn | `string` | The employee's SSN. |
| email | `string` | The employee's email address - must be the same address to which the verification request was sent. |
| house_number | `integer` | The employee's house number. |
| street_name | `string` | The name of the street where the employee's house is located. |
| address | `string` | The employee's residential address. |
| city | `string` | The city where the employee's house is located. |
| state | `string` | The state where the employee's house is located. |
| zip_code | `string` | The ZIP code of the postal district where the employee's house is located. |
| phone | `string` | The employee's phone number. |

## Update Personal Details

```shell
curl --location --request PUT 'https://api.us.springverify.com/employee/personal-details' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: text/plain' \
--data-raw '{
	"first_name":"John",
	"middle_name":"David",
	"last_name":"Doe",
	"dob":"12-11-1980",
	"ssn":"123456789",
	"email":"johndoe@gmail.com",
    "house_number": "239",
    "street_name": "A clear street"
	"address":"236 A clear street",
	"city":"Gotham",
	"state":"CA",
	"zip_code":"33433",
	"phone":"56-999222992"
}'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/employee/personal-details', {
    method: 'POST',
    headers: {
        'Content-Type': 'text/plain',
        'Authorization': 'Bearer JWT_TOKEN'
    },
    body: '{ "first_name":"John", "middle_name":"David", "last_name":"Doe", "dob":"12-11-1980", "ssn":"123456789", "email":"johndoe@gmail.com", "house_number": "239", "street_name": "A clear street" "address":"236 A clear street", "city":"Gotham", "state":"CA", "zip_code":"33433", "phone":"56-999222992" }'
});

// REQUEST

var request = require('request');

var headers = {
    'Content-Type': 'text/plain',
    'Authorization': 'Bearer JWT_TOKEN'
};

var dataString = '{ "first_name":"John", "middle_name":"David", "last_name":"Doe", "dob":"12-11-1980", "ssn":"123456789", "email":"johndoe@gmail.com", "house_number": "239", "street_name": "Avea street" "address":"236 Avea street", "city":"Gotham", "state":"CA", "zip_code":"33433", "phone":"56-999222992" }';

var options = {
    url: 'https://api.us.springverify.com/employee/personal-details',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Content-Type' => 'text/plain',
    'Authorization' => 'Bearer JWT_TOKEN'
);
$data = '{ "first_name":"John", "middle_name":"David", "last_name":"Doe", "dob":"12-11-1980", "ssn":"123456789", "email":"johndoe@gmail.com", "house_number": "239", "street_name": "Avea street" "address":"236 Avea street", "city":"Gotham", "state":"CA", "zip_code":"33433", "phone":"56-999222992" }';
$response = Requests::post('https://api.us.springverify.com/employee/personal-details', $headers, $data);
```

```python
import requests

headers = {
    'Content-Type': 'text/plain',
    'Authorization': 'Bearer JWT_TOKEN',
}

data = '{ "first_name":"John", "middle_name":"David", "last_name":"Doe", "dob":"12-11-1980", "ssn":"123456789", "email":"johndoe@gmail.com", "house_number": "239", "street_name": "Avea street" "address":"236 Avea street", "city":"Gotham", "state":"CA", "zip_code":"33433", "phone":"56-999222992" }'

response = requests.post('https://api.us.springverify.com/employee/personal-details', headers=headers, data=data)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/employee/personal-details")
request = Net::HTTP::Put.new(uri)
request.content_type = "text/plain"
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "success": true,
    "data": {
        "id": "cd0cbf74-a3a3-4270-98be-75b236dad50b",
        "access_id": "2148fefc-d0db-466a-9f15-8234ce30c101",
        "email": "johndoe@gmail.com",
        "password_hash": null,
        "first_name": "John",
        "middle_name": "David",
        "last_name": "Doe",
        "name_verified": null,
        "created_at": "2020-08-26T08:34:31.000Z",
        "updated_at": "2020-08-26T12:58:23.488Z",
        "employer_id": "1d4fb8ba-09ac-412c-aa62-58970b4d7472",
        "payment_id": "02247ac0-b3f0-402c-a413-df463dae52bb",
        "email_sent": true,
        "payment": false,
        "status": null,
        "flow_completed": null,
        "company_created_by": "zed@max.com",
        "employee_limit_id": "e4bdd91d-8509-4989-bafb-8b997cbb6d47",
        "employments": [],
        "education": [],
        "cic_criminal_records": [],
        "professional_licenses": [],
        "employee_detail": {
            "id": "41098fa9-cfd9-4ac7-a5e1-a8191c46edff",
            "access_id": null,
            "address": "236 A clear street",
            "address_verified": null,
            "driving_number": null,
            "driving_number_verified": null,
            "city": "Gotham",
            "city_verified": null,
            "state": "CA",
            "state_verified": null,
            "zipcode": "33433",
            "zipcode_verified": null,
            "country": null,
            "country_verified": null,
            "birthdate": "12-11-1980",
            "birthdate_verified": null,
            "phone": "56-999222992",
            "phone_verified": null,
            "ssn": "6789",
            "ssn_verified": null,
            "created_at": "2020-08-26T10:19:02.000Z",
            "updated_at": "2020-08-26T12:58:23.000Z",
            "employee_email": "johndoe@gmail.com"
        },
        "employee_verification": {
            "id": "40ac9a1a-5f1d-48c1-92e7-53dea339e5cf",
            "s3_gov_id": "link",
            "s3_gov_id_back": null,
            "s3_gov_id_match": false,
            "s3_web_img": null,
            "s3_passport_verified": 2,
            "passport_status": "FAILED",
            "s3_dl_verified": null,
            "dl_status": null,
            "verification_type": "id",
            "address": null,
            "address_verified": null,
            "city": null,
            "city_verified": null,
            "state": null,
            "state_verified": null,
            "zipcode": null,
            "zipcode_verified": null,
            "country": null,
            "country_verified": null,
            "birthdate": null,
            "birthdate_verified": null,
            "criminal_verified": null,
            "global_watchlist_verified": null,
            "created_at": "2020-08-26T10:19:46.000Z",
            "updated_at": "2020-08-26T10:23:31.000Z",
            "is_report_checked": false,
            "summary_of_rights_accepted": true,
            "background_check_disclosure_accepted": true,
            "id_manual_review": null,
            "super_admin_status": null,
            "super_admin_status_new": null,
            "consent_link": "link",
            "spring_sign_ref_id": "5f46374017710d0014423b76",
            "employee_email": "johndoe@gmail.com"
        },
        "employee_limit": {
            "id": "e4bdd91d-8509-4989-bafb-8b997cbb6d47",
            "employment": 2,
            "education": 1,
            "professional_license": 0,
            "all_county_criminal_search": true,
            "county_criminal_search": 0,
            "civil_court": 1,
            "driving_license": 0,
            "package_id": null,
            "created_at": "2020-08-26T08:34:31.000Z",
            "updated_at": "2020-08-26T08:34:31.000Z",
            "employee_invite_group_id": "bee68098-30b6-43a0-96d0-a45d8a18fc64",
            "employee_invite_group": {
                "id": "bee68098-30b6-43a0-96d0-a45d8a18fc64",
                "package": "diamond",
                "active": false,
                "created_at": "2020-08-26T08:34:31.000Z",
                "updated_at": "2020-08-26T12:13:14.000Z",
                "company_created_by": "zed@max.com",
                "package_id": "5"
            }
        },
        "kbaqna": {
            "id": "ac407a95-ca32-4fb7-832b-f8655ed1a5ef",
            "access_id": "a8f67651-2206-46ab-be18-1d956ee662de",
            "email": "johndoe@gmail.com",
            "questions_flag": 1,
            "kba_enabled": 1,
            "question_count": "5",
            "correct_answers": "4",
            "verified": false,
            "created_at": "2020-08-26T10:20:08.000Z",
            "updated_at": "2020-08-26T10:21:08.000Z",
            "idm_session_id": "8871924c3b585878",
            "employee_email": "johndoe@gmail.com"
        },
        "employee_flow": {
            "id": 290,
            "employment_flow": null,
            "education_flow": null,
            "professional_license_flow": null,
            "created_at": "2020-08-26T10:19:00.000Z",
            "updated_at": "2020-08-26T10:19:00.000Z",
            "employee_email": "johndoe@gmail.com"
        },
        "s3_files": [
            {
                "id": "70617497-2e00-422f-a1d8-0d652f87193b",
                "type": "ID",
                "sub_type": "FRONT",
                "relation_id": "d0e2aefd-4266-4db0-90fa-9ec4e1a54291",
                "link": "link",
                "reference_id": "cd0cbf74-a3a3-4270-98be-75b236dad50b",
                "status": null,
                "comments": null,
                "deleted_at": null,
                "created_at": "2020-08-26T10:23:31.000Z",
                "updated_at": "2020-08-26T10:23:31.000Z"
            },
            {
                "id": "b69a5b2d-3fc6-4f4b-ac30-7970b3bf84d2",
                "type": "ID",
                "sub_type": "FRONT",
                "relation_id": "42fa4d1b-60e6-4ce9-b684-953b58a8dca9",
                "link": "link",
                "reference_id": "cd0cbf74-a3a3-4270-98be-75b236dad50b",
                "status": null,
                "comments": null,
                "deleted_at": null,
                "created_at": "2020-08-26T10:22:02.000Z",
                "updated_at": "2020-08-26T10:22:02.000Z"
            },
            {
                "id": "e7118fef-b778-418c-b8cc-3ce906b31482",
                "type": "ID",
                "sub_type": "FRONT",
                "relation_id": "975059dd-69ef-4dd7-92e0-627290d76a9f",
                "link": "link",
                "reference_id": "cd0cbf74-a3a3-4270-98be-75b236dad50b",
                "status": null,
                "comments": null,
                "deleted_at": null,
                "created_at": "2020-08-26T10:22:53.000Z",
                "updated_at": "2020-08-26T10:22:53.000Z"
            }
        ],
        "criminal_statuses": [],
        "sjv_criminal_reports": []
    }
}
```

An employee who has created a profile can use this API tp update their persona details. The updates can be made until the verification process is complete.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| first_name | `string` | The employee's first name. |
| middle_name | `string` | The employee's middle name. |
| last_name | `string` | The employee's last name. |
| dob | `string` | The employee's date of birth in the format DD-MM-YYYY. |
| ssn | `string` | The employee's SSN. |
| email | `string` | The employee's email address - must be the same address to which the verification request was sent. |
| house_number | `integer` | The employee's house number. |
| street_name | `string` | The name of the street where the employee's house is located. |
| address | `string` | The employee's residential address. |
| city | `string` | The city where the employee's house is located. |
| state | `string` | The state where the employee's house is located. |
| zip_code | `string` | The ZIP code of the postal district where the employee's house is located. |
| phone | `string` | The employee's phone number. |

## Provide Consent

```shell
curl --location --request POST 'https://api.us.springverify.com/employee/consent' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: text/plain' \
--data-raw '{
	 "summary_of_rights":true,
     "background_check":true,
     "report_checked":true,
     "full_name":"John James Doe"
}'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/employee/consent', {
    method: 'POST',
    headers: {
        'Content-Type': 'text/plain',
        'Authorization': 'Bearer JWT_TOKEN'
    },
    body: JSON.stringify({ "summary_of_rights":true, "background_check":true, "report_checked":true, "full_name":"John James Doe" })
});

// REQUEST

var request = require('request');

var headers = {
    'Content-Type': 'text/plain',
    'Authorization': 'Bearer JWT_TOKEN'
};

var dataString = '{ "summary_of_rights":true, "background_check":true, "report_checked":true, "full_name":"John James Doe" }';

var options = {
    url: 'https://api.us.springverify.com/employee/consent',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Content-Type' => 'text/plain',
    'Authorization' => 'Bearer JWT_TOKEN'
);
$data = '{ "summary_of_rights":true, "background_check":true, "report_checked":true, "full_name":"John James Doe" }';
$response = Requests::post('https://api.us.springverify.com/employee/consent', $headers, $data);
```

```python
import requests

headers = {
    'Content-Type': 'text/plain',
    'Authorization': 'Bearer JWT_TOKEN',
}

data = '{ "summary_of_rights":true, "background_check":true, "report_checked":true, "full_name":"John James Doe" }'

response = requests.post('https://api.us.springverify.com/employee/consent', headers=headers, data=data)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/employee/consent")
request = Net::HTTP::Post.new(uri)
request.content_type = "text/plain"
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "success": true,
    "data": {
        "id": "c068d1da-9615-4b8e-a7a7-5daf624ed8d1",
        "summary_of_rights_accepted": true,
        "background_check_disclosure_accepted": true,
        "is_report_checked": false,
        "employee_email": "johndoe@gmail.com",
        "consent_link": "s3 link",
        "spring_sign_ref_id": "5f4524b817710d0014423b61",
        "updated_at": "2020-08-25T14:48:27.394Z",
        "created_at": "2020-08-25T14:48:27.394Z"
    }
}
```

After creating their profile, an employee needs to explicitly provide consent to allow background verification using this API. The name provided here should match the full name provided in the Submit Personal Details API.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| summary_of_rights | `boolean` | Indicates that the employee has read the summary of rights. This flag can be "true" or "false". |
| background_check | `boolean` | Indicates that the employee has agreed for a background check. This flag can be "true" or "false". |
| report_checked | `boolean` | Indicates that the employee's report has been checked. This flag can be "true" or "false". |
| full_name | `string` | The employee's full name. |

## Knowledge Based Quiz

```shell
curl --location --request GET 'https://api.us.springverify.com/employee/kba/questions' \
--header 'Authorization: Bearer JWT_TOKEN'
```

> Success Response

```json
{
    "success": true,
    "data": {
        "data": {
            "idm_session_id": "103c7770a327eada",
            "idmkba_response": {
                "kba_question": [
                    {
                        "question_id": 1,
                        "question": "Which of the following addresses have you lived at?",
                        "question_type": "StreetAddress",
                        "options": [
                            {
                                "id": 1,
                                "option": "2601 GORDON AVE"
                            },
                            {
                                "id": 2,
                                "option": "134 MARYLAND RD"
                            },
                            {
                                "id": 3,
                                "option": "1131 CANYON RD"
                            },
                            {
                                "id": 4,
                                "option": "9011 CENTRAL RD"
                            },
                            {
                                "id": 5,
                                "option": "NONE OF THE ABOVE"
                            }
                        ]
                    },
                    {
                        "question_id": 2,
                        "question": "Which of the following people is your relative?",
                        "question_type": "Relatives",
                        "options": [
                            {
                                "id": 1,
                                "option": "Dwight Kurt Schrute"
                            },
                            {
                                "id": 2,
                                "option": "Gabe"
                            },
                            {
                                "id": 3,
                                "option": "Kelly"
                            },
                            {
                                "id": 4,
                                "option": "Toby"
                            },
                            {
                                "id": 5,
                                "option": "NONE OF THE ABOVE"
                            }
                        ]
                    },
                    {
                        "question_id": 3,
                        "question": "Which of the following vehicles have you owned?",
                        "question_type": "Vehicle",
                        "options": [
                            {
                                "id": 1,
                                "option": "1984 NISSAN 300ZX"
                            },
                            {
                                "id": 2,
                                "option": "1997 ISUZU RODEO"
                            },
                            {
                                "id": 3,
                                "option": "2008 HYUNDAI TIBURON"
                            },
                            {
                                "id": 4,
                                "option": "1967 LINCOLN CONTINENTAL"
                            },
                            {
                                "id": 5,
                                "option": "NONE OF THE ABOVE"
                            }
                        ]
                    },
                    {
                        "question_id": 4,
                        "question": "Which of the following streets have you lived in?",
                        "question_type": "StreetName",
                        "options": [
                            {
                                "id": 1,
                                "option": "BANDERO DR"
                            },
                            {
                                "id": 2,
                                "option": "GALEWOOD DR"
                            },
                            {
                                "id": 3,
                                "option": "UNION HALL RD"
                            },
                            {
                                "id": 4,
                                "option": "KNOBHILL DR"
                            },
                            {
                                "id": 5,
                                "option": "NONE OF THE ABOVE"
                            }
                        ]
                    },
                    {
                        "question_id": 5,
                        "question": "Which of these businesses are you associated with?",
                        "question_type": "Businesses",
                        "options": [
                            {
                                "id": 1,
                                "option": "Stark Industries"
                            },
                            {
                                "id": 2,
                                "option": "Avengers LLC"
                            },
                            {
                                "id": 3,
                                "option": "Wayne Enterprises"
                            },
                            {
                                "id": 4,
                                "option": "Dunder Mifflin"
                            },
                            {
                                "id": 5,
                                "option": "NONE OF THE ABOVE"
                            }
                        ]
                    }
                ],
                "kba_status": {
                    "questions_answered": 0,
                    "no_of_questions": 5
                },
                "is_kba_enabled": "1"
            }
        },
        "attempt_number": 1,
        "success": true
    }
}
```

This API is used to generate a knowledge-based quiz about the employee to verify authenticity. A series of questions relating to the employee's profile will be asked to which the employee is required to answer. Based on these answers, a score will be generated.

## Submit KBA Quiz

```shell
curl --location --request POST 'https://api.us.springverify.com/employee/kba/verify' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: text/plain' \
--data-raw '{
"qna":[
    {
      "question_id": 1,
      "answer_id": 5
    },{
      "question_id": 2,
      "answer_id": 5
    },{
      "question_id": 3,
      "answer_id": 5
    },{
      "question_id": 4,
      "answer_id": 5
    },{
      "question_id": 5,
      "answer_id": 5
    }
  ]
}'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/employee/kba/verify', {
    method: 'POST',
    headers: {
        'Content-Type': 'text/plain',
        'Authorization': 'Bearer JWT_TOKEN'
    },
    body: JSON.stringify({ "qna":[ { "question_id": 1, "answer_id": 5 },{ "question_id": 2, "answer_id": 5 },{ "question_id": 3, "answer_id": 5 },{ "question_id": 4, "answer_id": 5 },{ "question_id": 5, "answer_id": 5 } ] })
});

// REQUEST

var request = require('request');

var headers = {
    'Content-Type': 'text/plain',
    'Authorization': 'Bearer JWT_TOKEN'
};

var dataString = '{ "qna":[ { "question_id": 1, "answer_id": 5 },{ "question_id": 2, "answer_id": 5 },{ "question_id": 3, "answer_id": 5 },{ "question_id": 4, "answer_id": 5 },{ "question_id": 5, "answer_id": 5 } ] }';

var options = {
    url: 'https://api.us.springverify.com/employee/kba/verify',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Content-Type' => 'text/plain',
    'Authorization' => 'Bearer JWT_TOKEN'
);
$data = '{ "qna":[ { "question_id": 1, "answer_id": 5 },{ "question_id": 2, "answer_id": 5 },{ "question_id": 3, "answer_id": 5 },{ "question_id": 4, "answer_id": 5 },{ "question_id": 5, "answer_id": 5 } ] }';
$response = Requests::post('https://api.us.springverify.com/employee/kba/verify', $headers, $data);
```

```python
import requests

headers = {
    'Content-Type': 'text/plain',
    'Authorization': 'Bearer JWT_TOKEN',
}

data = '{ "qna":[ { "question_id": 1, "answer_id": 5 },{ "question_id": 2, "answer_id": 5 },{ "question_id": 3, "answer_id": 5 },{ "question_id": 4, "answer_id": 5 },{ "question_id": 5, "answer_id": 5 } ] }'

response = requests.post('https://api.us.springverify.com/employee/kba/verify', headers=headers, data=data)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/employee/kba/verify")
request = Net::HTTP::Post.new(uri)
request.content_type = "text/plain"
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
  "success": true,
  "data": {
    "correct_answers": 5,
    "total_questions": 5,
    "success": true
  }
}
```

> Error Response

```json
{
    "message": "Your answers didn't seem to be correct, please try uploading DL or Passport.",
    "stack": "message"
}
```

This API is used to submit the employee's answers of the [Knowledge Based Quiz](https://docs.us.springverify.com/#knowledge-based-quiz).

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| qna | `array` | This is an array that contains the IDs of the question as well as the ID of the responses to those questions. |

## Upload and Verify ID

```shell
curl --location --request POST 'https://api.us.springverify.com/employee/upload/id' \
--header 'Authorization: Bearer JWT_TOKEN' \
--form 'front=@/path/to/file' \
--form 'back=@/path/to/file' \
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/employee/upload/id', {
    method: 'POST',
    headers: {
        'Authorization': 'Bearer JWT_TOKEN'
    }
});

// REQUEST

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN'
};

var options = {
    url: 'https://api.us.springverify.com/employee/upload/id',
    method: 'POST',
    headers: headers
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Authorization' => 'Bearer JWT_TOKEN'
);
$response = Requests::post('https://api.us.springverify.com/employee/upload/id', $headers);
```

```python
import requests

headers = {
    'Authorization': 'Bearer JWT_TOKEN',
}

response = requests.post('https://api.us.springverify.com/employee/upload/id', headers=headers)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/employee/upload/id")
request = Net::HTTP::Post.new(uri)
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "success": true,
    "data": {
        "success": true,
        "attempts": 1,
        "user_access_code": "6e7d5f15-9456-430d-b9da-9df67a4d9996"
    }
}
```

This API records the ID of the user and verifies its authenticity.

**Important Notes:**

1. The maximum allowed size of the image uploaded (per image) is capped at 2MB. Uploading photos larger than 2MB will throw an exception.
2. If using a mobile device, capturing a clear, non-blurry image is extremely important so that the ID can be processed accurately.
3. At a minimum, a 5MP camera should be used to capture the images for the UploadID parameter.
4. It is mandatory to have a contrast between the ID and the background. The background must be solid colored.
5. It must be ensured that all the edges of the ID are visible and none of the edges are cutoff.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| front | `image` | The image of the front of the ID. |
| back | `image` | The image of the back of the ID. |

## Upload and Verify Passport

```shell
curl --location --request POST 'https://api.us.springverify.com/employee/upload/passport' \
--header 'Authorization: Bearer JWT_TOKEN' \
--form 'front=@/path/to/file'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/employee/upload/passport', {
    method: 'POST',
    headers: {
        'Authorization': 'Bearer JWT_TOKEN'
    }
});

// REQUEST

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN'
};

var options = {
    url: 'https://api.us.springverify.com/employee/upload/passport',
    method: 'POST',
    headers: headers
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Authorization' => 'Bearer JWT_TOKEN'
);
$response = Requests::post('https://api.us.springverify.com/employee/upload/passport', $headers);
```

```python
import requests

headers = {
    'Authorization': 'Bearer JWT_TOKEN',
}

response = requests.post('https://api.us.springverify.com/employee/upload/passport', headers=headers)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/employee/upload/passport")
request = Net::HTTP::Post.new(uri)
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "success": true,
    "data": true
}
```

> Error Response

```json
{
    "message": "The Passport could not be parsed. Please make sure all four sides are visible and the Passport is in front of a dark background."
}
```

This API records the Passport of the user and verifies it authenticity.

This API is used to upload an image of the employee's passport that will be scanned and parsed. The image should have the employee's information on it on the first or the second page. It must be a single picture that captures both pages of the passport. This follows the same logic as the other uploadId endpoints -- the picture needs to be clear with minimal glare and must have sufficient lighting.

There are several variables that are more likely to cause a document to fail:
1. The quality of the captured image - blurred images or images with reflections.
2. Personalization of the document such as documents with especially long names.
3. Variations in manufacturing techniques - for example, a card printed on the wrong side or slight variations in the printing location.
4. Wear and aging of the document - a worn or dirty card can cause failure.
5. Tampering and counterfeiting - unlawful changes or reproduction of documents.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| front | `image` | The image of the passport. |

## Request Manual Review of the ID or Passport

```shell
curl --location --request GET 'https://api.us.springverify.com/employee/id/manual-review' \
--header 'Authorization: Bearer JWT_TOKEN'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/employee/id/manual-review', {
    headers: {
        'Authorization': 'Bearer JWT_TOKEN'
    }
});

// REQUEST

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN'
};

var options = {
    url: 'https://api.us.springverify.com/employee/id/manual-review',
    headers: headers
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Authorization' => 'Bearer JWT_TOKEN'
);
$response = Requests::get('https://api.us.springverify.com/employee/id/manual-review', $headers);
```

```python
import requests

headers = {
    'Authorization': 'Bearer JWT_TOKEN',
}

response = requests.get('https://api.us.springverify.com/employee/id/manual-review', headers=headers)
```

```ruby

require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/employee/id/manual-review")
request = Net::HTTP::Get.new(uri)
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "success": true,
    "data": "success"
}
```

If the uploading of an ID _and_ of the passport fails more than once, a manual review can be requested using this API.

## Get ID verification tries

```shell
curl --location --request GET 'https://api.us.springverify.com/employee/id/try-counts' \
--header 'Authorization: Bearer JWT_TOKEN'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/employee/id/try-counts', {
    headers: {
        'Authorization': 'Bearer JWT_TOKEN'
    }
});

// REQUEST

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN'
};

var options = {
    url: 'https://api.us.springverify.com/employee/id/try-counts',
    headers: headers
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Authorization' => 'Bearer JWT_TOKEN'
);
$response = Requests::get('https://api.us.springverify.com/employee/id/try-counts', $headers);
```

```python
import requests

headers = {
    'Authorization': 'Bearer JWT_TOKEN',
}

response = requests.get('https://api.us.springverify.com/employee/id/try-counts', headers=headers)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/employee/id/try-counts")
request = Net::HTTP::Get.new(uri)
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "success": true,
    "data": {
        "dl": 1,
        "passport": 0,
        "kba": 2
    }
}
```

This API will provide the number of tries that an employee has made for uploading the Driving License ID , the Passport and also the number of tries an employee has made on the KBA. The maximum allowed limit is 2 per method per candidate.

## Get Candidate Info

```shell
curl --location --request GET 'https://api.us.springverify.com/employee/' \
--header 'Authorization: Bearer JWT_TOKEN'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/employee/', {
    headers: {
        'Authorization': 'Bearer JWT_TOKEN'
    }
});


// REQUEST

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN'
};

var options = {
    url: 'https://api.us.springverify.com/employee/',
    headers: headers
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Authorization' => 'Bearer JWT_TOKEN'
);
$response = Requests::get('https://api.us.springverify.com/employee/', $headers);
```

```python
import requests

headers = {
    'Authorization': 'Bearer JWT_TOKEN',
}

response = requests.get('https://api.us.springverify.com/employee/', headers=headers)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/employee/")
request = Net::HTTP::Get.new(uri)
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "success": true,
    "data": {
        "employee": {
            "id": "9ca37df2-1d81-40df-90aa-b88e081b8103",
            "access_id": "ba92f24e-796e-4de4-8907-675f0ee77d58",
            "email": "johndoe@gmail.com",
            "password_hash": "100d5fcb45dc552d1a9011e2707b937904f79df410199fcb7a1e2b3c022d9911",
            "first_name": "John",
            "middle_name": "David",
            "last_name": "Doe",
            "name_verified": null,
            "created_at": "2020-08-25T14:28:47.000Z",
            "updated_at": "2020-08-26T09:56:22.000Z",
            "employer_id": "1d4fb8ba-09ac-412c-aa62-58970b4d7472",
            "payment_id": "6c75e302-ac2a-4efa-9ba9-c5dfc97b941e",
            "email_sent": true,
            "payment": false,
            "status": "PENDING",
            "flow_completed": true,
            "company_created_by": "zed@max.com",
            "employee_limit_id": "e1e2fef4-a1dd-4be2-8dd1-b0ab8ec29ff8",
            "employments": [
                {
                    "id": "d0284d0b-eae8-4f0c-a574-66d39ad9d548",
                    "email": "johndoe@gmail.com",
                    "access_id": null,
                    "employer_name": "Stark Industries pvt ltd",
                    "employer_name_verified": null,
                    "employer_phone": "9911991199",
                    "employer_phone_verified": null,
                    "employer_address": "12, Manhattan Street",
                    "employer_address_verified": null,
                    "employer_town": "New York",
                    "employer_town_verified": null,
                    "state": "New York",
                    "state_verified": null,
                    "zipcode": "129012",
                    "zipcode_verified": null,
                    "employer_country": "USA",
                    "employer_country_verified": null,
                    "job_title": "Senior Manager",
                    "job_title_verified": null,
                    "start_date": "19-11-2000",
                    "start_date_verified": null,
                    "end_date": "19-11-2002",
                    "end_date_verified": null,
                    "supervisor_name": "Nick Fury",
                    "supervisor_contact": "nickfury@starkindustries.com",
                    "current_employment": "0",
                    "current_employment_verified": null,
                    "contract_type": null,
                    "contract_type_verified": null,
                    "source": null,
                    "created_at": "2020-08-26T06:24:25.000Z",
                    "updated_at": "2020-08-26T06:26:07.000Z",
                    "job_type": "Full time",
                    "reason_for_leaving": "xyz",
                    "status": 0,
                    "status_new": null,
                    "super_admin_status": null,
                    "super_admin_status_new": null,
                    "consent": null,
                    "adverse_action": null
                }
            ],
            "education": [
                {
                    "id": "18304027-e78a-426c-aae5-db9c677c1704",
                    "email": "johndoe@gmail.com",
                    "access_id": null,
                    "employee_alias_name": null,
                    "employee_alias_name_verified": null,
                    "school_name": "MIT",
                    "school_name_verified": null,
                    "school_campus": "Boston",
                    "school_campus_verified": null,
                    "phone": null,
                    "phone_verified": null,
                    "address": "New street east block",
                    "address_verified": null,
                    "town": null,
                    "town_verified": null,
                    "city": "Boston",
                    "city_verified": null,
                    "state": "Massachusetts",
                    "state_verified": null,
                    "zipcode": "129012",
                    "zipcode_verified": null,
                    "country": "USA",
                    "country_verified": null,
                    "start_date": "28-12-1991",
                    "start_date_verified": null,
                    "end_date": "12-28-1996",
                    "end_date_verified": null,
                    "degree": "Engineering",
                    "degree_verified": null,
                    "major": "Biotech",
                    "major_verified": null,
                    "school_type": "University",
                    "source": null,
                    "created_at": "2020-08-26T07:15:58.000Z",
                    "updated_at": "2020-08-26T07:16:55.000Z",
                    "currently_attending": 0,
                    "completed_successfully": 1,
                    "status": 0,
                    "status_new": null,
                    "super_admin_status": null,
                    "super_admin_status_new": null,
                    "adverse_action": null
                }
            ],
            "kbaqna": {
                "id": "71f4dda4-2949-4f23-97ae-26d158c082a2",
                "access_id": "6e7d5f15-9456-430d-b9da-9df67a4d9996",
                "email": "johndoe@gmail.com",
                "questions_flag": 1,
                "kba_enabled": 1,
                "question_count": "5",
                "correct_answers": "4",
                "verified": false,
                "created_at": "2020-08-25T15:06:19.000Z",
                "updated_at": "2020-08-25T15:11:49.000Z",
                "idm_session_id": "91b59885191b01f5",
                "employee_email": "johndoe@gmail.com"
            },
            "professional_licenses": [],
            "employee_limit": {
                "id": "e1e2fef4-a1dd-4be2-8dd1-b0ab8ec29ff8",
                "employment": 2,
                "education": 1,
                "professional_license": 0,
                "all_county_criminal_search": true,
                "county_criminal_search": 0,
                "civil_court": 1,
                "driving_license": 0,
                "package_id": null,
                "created_at": "2020-08-25T14:28:47.000Z",
                "updated_at": "2020-08-25T14:28:47.000Z",
                "employee_invite_group_id": "b630d0ec-e8d6-49a0-bbf2-8fd26a791a85"
            },
            "employee_detail": {
                "id": "efa4191a-331d-48c3-8e52-8915ed8167be",
                "access_id": null,
                "address": "236 Avea street",
                "address_verified": null,
                "driving_number": null,
                "driving_number_verified": null,
                "city": "Gotham",
                "city_verified": null,
                "state": "CA",
                "state_verified": null,
                "zipcode": "33433",
                "zipcode_verified": null,
                "country": null,
                "country_verified": null,
                "birthdate": "12-11-1980",
                "birthdate_verified": null,
                "phone": "56-999222992",
                "phone_verified": null,
                "ssn": "6789",
                "ssn_verified": null,
                "created_at": "2020-08-25T14:32:46.000Z",
                "updated_at": "2020-08-26T09:52:33.000Z",
                "employee_email": "johndoe@gmail.com"
            },
            "employee_verification": {
                "id": "c068d1da-9615-4b8e-a7a7-5daf624ed8d1",
                "s3_gov_id": "link",
                "s3_gov_id_back": "link",
                "s3_gov_id_match": false,
                "s3_web_img": null,
                "s3_passport_verified": 1,
                "passport_status": "VERIFIED",
                "s3_dl_verified": 1,
                "dl_status": "VERIFIED",
                "verification_type": "id",
                "address": null,
                "address_verified": null,
                "city": null,
                "city_verified": null,
                "state": null,
                "state_verified": null,
                "zipcode": null,
                "zipcode_verified": null,
                "country": null,
                "country_verified": null,
                "birthdate": null,
                "birthdate_verified": null,
                "criminal_verified": null,
                "global_watchlist_verified": null,
                "created_at": "2020-08-25T14:48:27.000Z",
                "updated_at": "2020-08-26T09:52:59.000Z",
                "is_report_checked": false,
                "summary_of_rights_accepted": true,
                "background_check_disclosure_accepted": true,
                "id_manual_review": null,
                "super_admin_status": null,
                "super_admin_status_new": null,
                "consent_link": "link",
                "spring_sign_ref_id": "5f4630f817710d0014423b74",
                "employee_email": "johndoe@gmail.com"
            },
            "employee_flow": {
                "id": 289,
                "employment_flow": "SUBMITTED",
                "education_flow": "SUBMITTED",
                "professional_license_flow": null,
                "created_at": "2020-08-25T14:32:40.000Z",
                "updated_at": "2020-08-26T07:36:45.000Z",
                "employee_email": "johndoe@gmail.com"
            },
            "criminal_statuses": {
                "national_criminal": "PENDING",
                "sex_offender": "PENDING",
                "global_watchlist": "PENDING",
                "county_criminal_search": "PENDING",
                "civil_court": "PENDING",
                "overall_criminal_status": "PENDING"
            },
            "employer": {
                "id": "1d4fb8ba-09ac-412c-aa62-58970b4d7472",
                "active": true,
                "email": "zed@max.com",
                "domain": "springrole.com",
                "role": "ADMIN",
                "password": "$2b$10$WbyPfKtgYGSgU36arbbqnOpivH9hBFKuORLJ0iF.Dln6f289v3IvW",
                "first_name": "Zed",
                "last_name": "Max",
                "phone_number": "1-2132143213",
                "stripe_id": "cus_Bu8MQZ5BNDiij0",
                "email_sent_time": null,
                "created_at": "2019-07-23T11:32:51.000Z",
                "updated_at": "2020-08-26T08:34:45.000Z",
                "company": {
                    "id": 1,
                    "created_by": "zed@max.com",
                    "name": "Google",
                    "address": "Address",
                    "city": "reqbodycity",
                    "state": "reqbodystate",
                    "zipcode": "12345",
                    "tax_id_number": "reqbodytaxIdNumber",
                    "credits": 496250,
                    "domain": "springrole.com",
                    "employment_limit": null,
                    "education_limit": null,
                    "license_limit": null,
                    "civilcourt_limit": null,
                    "dl_limit": null,
                    "s3_logo": "link",
                    "created_at": "2019-07-23T11:33:04.000Z",
                    "updated_at": "2020-08-20T08:25:09.000Z"
                }
            }
        }
    }
}
```

This API returns all info pertaining to a candidate including all types of verification.

## Add Candidate Employment

```shell
curl --location --request POST 'https://api.us.springverify.com/employee/employment' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: text/plain' \
--data-raw '{
"employer_name": "Stark Industries",
"employer_address": "12, Manhattan Street",
"employer_phone": "9911991199",
"employer_town": "New York",
"state": "New York",
"zip_code": "129012",
"country": "USA",
"job_title": "Senior Manager",
"start_date": "19-11-2000",
"end_date": "19-11-2002",
"supervisor_name": "Nick Fury",
"current_employment": "0",
"job_type": "Contract",
"reason_for_leaving": "xyz",
"supervisor_contact": "nickfury@starkindustries.com"
}'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/employee/employment', {
    method: 'POST',
    headers: {
        'Content-Type': 'text/plain',
        'Authorization': 'Bearer JWT_TOKEN'
    },
    body: JSON.stringify({ "employer_name": "Stark Industries", "employer_address": "12, Manhattan Street", "employer_phone": "9911991199", "employer_town": "New York", "state": "New York", "zip_code": "129012", "country": "USA", "job_title": "Senior Manager", "start_date": "19-11-2000", "end_date": "19-11-2002", "supervisor_name": "Nick Fury", "current_employment": "0", "job_type": "Contract", "reason_for_leaving": "xyz", "supervisor_contact": "nickfury@starkindustries.com" })
});

// REQUEST

var request = require('request');

var headers = {
    'Content-Type': 'text/plain',
    'Authorization': 'Bearer JWT_TOKEN'
};

var dataString = '{ "employer_name": "Stark Industries", "employer_address": "12, Manhattan Street", "employer_phone": "9911991199", "employer_town": "New York", "state": "New York", "zip_code": "129012", "country": "USA", "job_title": "Senior Manager", "start_date": "19-11-2000", "end_date": "19-11-2002", "supervisor_name": "Nick Fury", "current_employment": "0", "job_type": "Contract", "reason_for_leaving": "xyz", "supervisor_contact": "nickfury@starkindustries.com" }';

var options = {
    url: 'https://api.us.springverify.com/employee/employment',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Content-Type' => 'text/plain',
    'Authorization' => 'Bearer JWT_TOKEN'
);
$data = '{ "employer_name": "Stark Industries", "employer_address": "12, Manhattan Street", "employer_phone": "9911991199", "employer_town": "New York", "state": "New York", "zip_code": "129012", "country": "USA", "job_title": "Senior Manager", "start_date": "19-11-2000", "end_date": "19-11-2002", "supervisor_name": "Nick Fury", "current_employment": "0", "job_type": "Contract", "reason_for_leaving": "xyz", "supervisor_contact": "nickfury@starkindustries.com" }';
$response = Requests::post('https://api.us.springverify.com/employee/employment', $headers, $data);
```

```python
import requests

headers = {
    'Content-Type': 'text/plain',
    'Authorization': 'Bearer JWT_TOKEN',
}

data = '{ "employer_name": "Stark Industries", "employer_address": "12, Manhattan Street", "employer_phone": "9911991199", "employer_town": "New York", "state": "New York", "zip_code": "129012", "country": "USA", "job_title": "Senior Manager", "start_date": "19-11-2000", "end_date": "19-11-2002", "supervisor_name": "Nick Fury", "current_employment": "0", "job_type": "Contract", "reason_for_leaving": "xyz", "supervisor_contact": "nickfury@starkindustries.com" }'

response = requests.post('https://api.us.springverify.com/employee/employment', headers=headers, data=data)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/employee/employment")
request = Net::HTTP::Post.new(uri)
request.content_type = "text/plain"
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "success": true,
    "data": {
        "id": "ef9e7612-3789-405a-be37-c7bc1871c1f7",
        "email": "johndoe@gmail.com",
        "employer_name": "Stark Industries",
        "employer_town": "New York",
        "employer_country": "USA",
        "state": "New York",
        "job_title": "Senior Manager",
        "start_date": "19-11-2000",
        "end_date": "19-11-2002",
        "supervisor_name": "Nick Fury",
        "job_type": "Contract",
        "status": "PENDING",
        "employer_address": "12, Manhattan Street",
        "employer_phone": "9911991199",
        "zipcode": "129012",
        "reason_for_leaving": "xyz",
        "supervisor_contact": "nickfury@starkindustries.com",
        "updated_at": "2020-08-25T15:24:03.346Z",
        "created_at": "2020-08-25T15:24:03.346Z"
    }
}
```

This API will be used to submit the Employment records for the Employee.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| employer_name | `string` | Name of the employer. |
| employer_address | `string` | Address of the employer. |
| employer_phone | `string` | Phone number of the employer. |
| employer_town | `string` | Town where the employer is located. |
| state | `string` | State where the employer is located. |
| zip_code | `string` | Zip Code of the postal district where the employer is located. |
| country | `string` | Country where the employer is located. |
| job_title | `string` | Job title of the latest employment. |
| start_date | `string` | Start Date of the employment. |
| end_date | `string` | End Date of the employment. |
| supervisor_name | `string` | The name of the employee's supervisor in this employment. |
| current_employment | `string` | Is this the employee's current employment? |
| job_type | `string` | Is this a contract or a full-time employment? |
| reason_for_leaving | `string` | Reason for leaving the job (optional). |
| supervisor_contact | `string` | Contact details of the supervisor. |

## Delete Employment

```shell
curl --location --request DELETE 'https://api.us.springverify.com/employee/employment?id=ef9e7612-3789-405a-be37-c7bc1871c1f7' \
--header 'Authorization: Bearer JWT_TOKEN'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/employee/employment?id=ef9e7612-3789-405a-be37-c7bc1871c1f7', {
    headers: {
        'Authorization': 'Bearer JWT_TOKEN'
    }
});

// REQUEST

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN'
};

var options = {
    url: 'https://api.us.springverify.com/employee/employment?id=ef9e7612-3789-405a-be37-c7bc1871c1f7',
    headers: headers
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Authorization' => 'Bearer JWT_TOKEN'
);
$response = Requests::get('https://api.us.springverify.com/employee/employment?id=ef9e7612-3789-405a-be37-c7bc1871c1f7', $headers);
```

```python
import requests

headers = {
    'Authorization': 'Bearer JWT_TOKEN',
}

params = (
    ('id', 'ef9e7612-3789-405a-be37-c7bc1871c1f7'),
)

response = requests.get('https://api.us.springverify.com/employee/employment', headers=headers, params=params)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/employee/employment?id=ef9e7612-3789-405a-be37-c7bc1871c1f7")
request = Net::HTTP::Delete.new(uri)
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "success": true,
    "data": 1
}
```

This API is used to delete the employment submitted in the "Add Candidate Employment" API before it goes for verification.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| id | `uuid` | UUID of the employment record. |

## Edit Employment

```shell
curl --location --request POST 'https://api.us.springverify.com/employee/employment' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: text/plain' \
--data-raw '{
    "id": "ef9e7612-3789-405a-be37-c7bc1871c1f7",
	"employer_name": "Stark Industries pvt ltd",
	"employer_address": "12, Manhattan Street",
	"employer_phone": "9911991199",
	"employer_town": "New York",
	"state": "New York",
	"zip_code": "129012",
	"country": "USA",
	"job_title": "Senior Manager",
	"start_date": "19-11-2000",
	"end_date": "19-11-2002",
	"supervisor_name": "Nick Fury",
	"current_employment": "0",
	"job_type": "Contract",
	"reason_for_leaving": "xyz",
	"supervisor_contact": "nickfury@starkindustries.com"
}'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/employee/employment', {
    method: 'POST',
    headers: {
        'Content-Type': 'text/plain',
        'Authorization': 'Bearer JWT_TOKEN'
    },
    body: JSON.stringify({ "id": "ef9e7612-3789-405a-be37-c7bc1871c1f7", "employer_name": "Stark Industries pvt ltd", "employer_address": "12, Manhattan Street", "employer_phone": "9911991199", "employer_town": "New York", "state": "New York", "zip_code": "129012", "country": "USA", "job_title": "Senior Manager", "start_date": "19-11-2000", "end_date": "19-11-2002", "supervisor_name": "Nick Fury", "current_employment": "0", "job_type": "Contract", "reason_for_leaving": "xyz", "supervisor_contact": "nickfury@starkindustries.com" })
});

// REQUEST

var request = require('request');

var headers = {
    'Content-Type': 'text/plain',
    'Authorization': 'Bearer JWT_TOKEN'
};

var dataString = '{ "id": "ef9e7612-3789-405a-be37-c7bc1871c1f7", "employer_name": "Stark Industries pvt ltd", "employer_address": "12, Manhattan Street", "employer_phone": "9911991199", "employer_town": "New York", "state": "New York", "zip_code": "129012", "country": "USA", "job_title": "Senior Manager", "start_date": "19-11-2000", "end_date": "19-11-2002", "supervisor_name": "Nick Fury", "current_employment": "0", "job_type": "Contract", "reason_for_leaving": "xyz", "supervisor_contact": "nickfury@starkindustries.com" }';

var options = {
    url: 'https://api.us.springverify.com/employee/employment',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Content-Type' => 'text/plain',
    'Authorization' => 'Bearer JWT_TOKEN'
);
$data = '{ "id": "ef9e7612-3789-405a-be37-c7bc1871c1f7", "employer_name": "Stark Industries pvt ltd", "employer_address": "12, Manhattan Street", "employer_phone": "9911991199", "employer_town": "New York", "state": "New York", "zip_code": "129012", "country": "USA", "job_title": "Senior Manager", "start_date": "19-11-2000", "end_date": "19-11-2002", "supervisor_name": "Nick Fury", "current_employment": "0", "job_type": "Contract", "reason_for_leaving": "xyz", "supervisor_contact": "nickfury@starkindustries.com" }';
$response = Requests::post('https://api.us.springverify.com/employee/employment', $headers, $data);
```

```python
import requests

headers = {
    'Content-Type': 'text/plain',
    'Authorization': 'Bearer JWT_TOKEN',
}

data = '{ "id": "ef9e7612-3789-405a-be37-c7bc1871c1f7", "employer_name": "Stark Industries pvt ltd", "employer_address": "12, Manhattan Street", "employer_phone": "9911991199", "employer_town": "New York", "state": "New York", "zip_code": "129012", "country": "USA", "job_title": "Senior Manager", "start_date": "19-11-2000", "end_date": "19-11-2002", "supervisor_name": "Nick Fury", "current_employment": "0", "job_type": "Contract", "reason_for_leaving": "xyz", "supervisor_contact": "nickfury@starkindustries.com" }'

response = requests.post('https://api.us.springverify.com/employee/employment', headers=headers, data=data)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/employee/employment")
request = Net::HTTP::Post.new(uri)
request.content_type = "text/plain"
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "success": true,
    "data": {
        "id": "ef9e7612-3789-405a-be37-c7bc1871c1f7",
        "email": "johndoe@gmail.com",
        "access_id": null,
        "employer_name": "Stark Industries pvt ltd",
        "employer_name_verified": null,
        "employer_phone": "9911991199",
        "employer_phone_verified": null,
        "employer_address": "12, Manhattan Street",
        "employer_address_verified": null,
        "employer_town": "New York",
        "employer_town_verified": null,
        "state": "New York",
        "state_verified": null,
        "zipcode": "129012",
        "zipcode_verified": null,
        "employer_country": "USA",
        "employer_country_verified": null,
        "job_title": "Senior Manager",
        "job_title_verified": null,
        "start_date": "19-11-2000",
        "start_date_verified": null,
        "end_date": "19-11-2002",
        "end_date_verified": null,
        "supervisor_name": "Nick Fury",
        "supervisor_contact": "nickfury@starkindustries.com",
        "current_employment": "0",
        "current_employment_verified": null,
        "contract_type": null,
        "contract_type_verified": null,
        "source": null,
        "created_at": "2020-08-26T06:24:25.000Z",
        "updated_at": "2020-08-26T06:26:07.019Z",
        "job_type": "Full time",
        "reason_for_leaving": "xyz",
        "status": "PENDING",
        "status_new": null,
        "super_admin_status": null,
        "super_admin_status_new": null,
        "consent": null
    }
}
```

By adding the ID received in the "Submit Employment" response in this API, it can be used to edit the employment data.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| id | `uuid` | UUID of the employment record. |
| employer_name | `string` | Name of the employer. |
| employer_address | `string` | Address of the employer. |
| employer_phone | `string` | Phone number of the employer. |
| employer_town | `string` | Town where the employer is located. |
| state | `string` | State where the employer is located. |
| zip_code | `string` | Zip Code of the postal district where the employer is located. |
| country | `string` | Country where the employer is located. |
| job_title | `string` | Job title of the latest employment. |
| start_date | `string` | Start Date of the employment. |
| end_date | `string` | End Date of the employment. |
| supervisor_name | `string` | The name of the employee's supervisor in this employment. |
| current_employment | `string` | Is this the employee's current employment? |
| job_type | `string` | Is this a contract or a full-time employment? |
| reason_for_leaving | `string` | Reason for leaving the job (optional). |
| supervisor_contact | `string` | Contact details of the supervisor. |

## Trigger Employment Verification

```shell
curl --location --request GET 'https://api.us.springverify.com/employee/submit/employment' \
--header 'Authorization: Bearer JWT_TOKEN'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/employee/submit/employment', {
    headers: {
        'Authorization': 'Bearer JWT_TOKEN'
    }
});

// REQUEST

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN'
};

var options = {
    url: 'https://api.us.springverify.com/employee/submit/employment',
    headers: headers
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Authorization' => 'Bearer JWT_TOKEN'
);
$response = Requests::get('https://api.us.springverify.com/employee/submit/employment', $headers);
```

```python
import requests

headers = {
    'Authorization': 'Bearer JWT_TOKEN',
}

response = requests.get('https://api.us.springverify.com/employee/submit/employment', headers=headers)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/employee/submit/employment")
request = Net::HTTP::Get.new(uri)
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "success": true,
    "data": {
        "success": true,
        "status": "Completed",
        "institution_name_verified": false,
        "employer_name_verified": false,
        "count": 1
    }
}
```

Once the employment records have been submitted and finalized, an Employment Verification request can be triggered using this API.

## Add Employee's Education

```shell
curl --location --request POST 'https://api.us.springverify.com/employee/education' \
--header 'Content-Type: application/json' \
--data-raw '{
    "school_name": "MIT",
    "school_type": "University",
    "school_campus": "Boston",
    "address": "Asdasd",
    "city":"Boston",
    "state":"Massachusetts",
    "zip_code": "126112"
    "country":"USA",
    "start_date":"28-12-1991",
    "end_date":"12-28-1996",
    "degree":"Engineering",
    "currently_attending":"0",
    "completed_successfully":"1",
    "major":"Biotech"
}'
```
> Success Response

```json
{
    "success": true,
    "data": {
        "id": "de359912-1223-4338-87c4-0783a0ea495b",
        "email": "johndoe@gmail.com",
        "school_name": "MIT",
        "school_type": "University",
        "school_campus": "Boston",
        "address": "Asdasd",
        "city": "Boston",
        "country": "USA",
        "start_date": "28-12-1991",
        "end_date": "12-28-1996",
        "degree": "Engineering",
        "currently_attending": "0",
        "completed_successfully": "1",
        "status": "PENDING",
        "state": "Massachusetts",
        "major": "Biotech",
        "zipcode": "126112",
        "updated_at": "2020-08-26T06:57:12.876Z",
        "created_at": "2020-08-26T06:57:12.876Z"
    }
}
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/employee/education', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json'
    },
    body: '{ "school_name": "MIT", "school_type": "University", "school_campus": "Boston", "address": "Asdasd", "city":"Boston", "state":"Massachusetts", "zip_code": "126112" "country":"USA", "start_date":"28-12-1991", "end_date":"12-28-1996", "degree":"Engineering", "currently_attending":"0", "completed_successfully":"1", "major":"Biotech" }'
});

// REQUEST

var request = require('request');

var headers = {
    'Content-Type': 'application/json'
};

var dataString = '{ "school_name": "MIT", "school_type": "University", "school_campus": "Boston", "address": "Asdasd", "city":"Boston", "state":"Massachusetts", "zip_code": "126112" "country":"USA", "start_date":"28-12-1991", "end_date":"12-28-1996", "degree":"Engineering", "currently_attending":"0", "completed_successfully":"1", "major":"Biotech" }';

var options = {
    url: 'https://api.us.springverify.com/employee/education',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Content-Type' => 'application/json'
);
$data = '{ "school_name": "MIT", "school_type": "University", "school_campus": "Boston", "address": "Asdasd", "city":"Boston", "state":"Massachusetts", "zip_code": "126112" "country":"USA", "start_date":"28-12-1991", "end_date":"12-28-1996", "degree":"Engineering", "currently_attending":"0", "completed_successfully":"1", "major":"Biotech" }';
$response = Requests::post('https://api.us.springverify.com/employee/education', $headers, $data);
```

```python
import requests

headers = {
    'Content-Type': 'application/json',
}

data = '{ "school_name": "MIT", "school_type": "University", "school_campus": "Boston", "address": "Asdasd", "city":"Boston", "state":"Massachusetts", "zip_code": "126112" "country":"USA", "start_date":"28-12-1991", "end_date":"12-28-1996", "degree":"Engineering", "currently_attending":"0", "completed_successfully":"1", "major":"Biotech" }'

response = requests.post('https://api.us.springverify.com/employee/education', headers=headers, data=data)
```

```ruby

require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/employee/education")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

This API will be used to submit the education records of the employee.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| school_name | `string` | The name of the employee's school. |
| school_type | `string` | The type of the employee's school. |
| school_campus | `string` | The campus of the employee's school. |
| address | `string` | The address of the employee's school. |
| city | `string` | The city where the employee's school is located. |
| state | `string` | The state where the employee's school is located. |
| zip_code | `string` | The ZIP code of the postal district where the employee's school is located. |
| country | `string` | The country where the employee's school is located. |
| start_date | `string` | Start date of the course. |
| end_date | `string` | End date of the course. |
| degree | `string` | Official degree of the course. |
| currently_attending | `string` | This flag will be set to 1 if the employee is currently attending the course, else it will be set to 0. |
| completed_successfully | `string` | This flag will be set to 1 if the employee has successfully completed the course, else it will be set to 0. |
| major | `string` | The field in which the employee has majored. |

## Edit Education Entry

```shell
curl --location --request POST 'https://api.us.springverify.com/employee/education' \
--header 'Content-Type: application/json' \
--data-raw '{
    "id":"de359912-1223-4338-87c4-0783a0ea495b",
    "school_name": "MIT",
    "school_type": "University",
    "school_campus": "Boston",
    "address":"New street east block",
    "city":"Boston",
    "state":"Massachusetts",
    "zip_code": "129012",
    "country":"USA",
    "start_date": "28-12-1991",
   "end_date": "12-28-1996",
    "degree":"Engineering",
    "currently_attending":"0",
    "completed_successfully":"1",
    "major":"Biotech"
}'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/employee/education', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json'
    },
    body: JSON.stringify({ "id":"de359912-1223-4338-87c4-0783a0ea495b", "school_name": "MIT", "school_type": "University", "school_campus": "Boston", "address":"New street east block", "city":"Boston", "state":"Massachusetts", "zip_code": "129012", "country":"USA", "start_date": "28-12-1991", "end_date": "12-28-1996", "degree":"Engineering", "currently_attending":"0", "completed_successfully":"1", "major":"Biotech" })
});

// REQUEST

var request = require('request');

var headers = {
    'Content-Type': 'application/json'
};

var dataString = '{ "id":"de359912-1223-4338-87c4-0783a0ea495b", "school_name": "MIT", "school_type": "University", "school_campus": "Boston", "address":"New street east block", "city":"Boston", "state":"Massachusetts", "zip_code": "129012", "country":"USA", "start_date": "28-12-1991", "end_date": "12-28-1996", "degree":"Engineering", "currently_attending":"0", "completed_successfully":"1", "major":"Biotech" }';

var options = {
    url: 'https://api.us.springverify.com/employee/education',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Content-Type' => 'application/json'
);
$data = '{ "id":"de359912-1223-4338-87c4-0783a0ea495b", "school_name": "MIT", "school_type": "University", "school_campus": "Boston", "address":"New street east block", "city":"Boston", "state":"Massachusetts", "zip_code": "129012", "country":"USA", "start_date": "28-12-1991", "end_date": "12-28-1996", "degree":"Engineering", "currently_attending":"0", "completed_successfully":"1", "major":"Biotech" }';
$response = Requests::post('https://api.us.springverify.com/employee/education', $headers, $data);
```

```python
import requests

headers = {
    'Content-Type': 'application/json',
}

data = '{ "id":"de359912-1223-4338-87c4-0783a0ea495b", "school_name": "MIT", "school_type": "University", "school_campus": "Boston", "address":"New street east block", "city":"Boston", "state":"Massachusetts", "zip_code": "129012", "country":"USA", "start_date": "28-12-1991", "end_date": "12-28-1996", "degree":"Engineering", "currently_attending":"0", "completed_successfully":"1", "major":"Biotech" }'

response = requests.post('https://api.us.springverify.com/employee/education', headers=headers, data=data)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/employee/education")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "success": true,
    "data": {
        "id": "18304027-e78a-426c-aae5-db9c677c1704",
        "email": "johndoe@gmail.com",
        "access_id": null,
        "employee_alias_name": null,
        "employee_alias_name_verified": null,
        "school_name": "MIT",
        "school_name_verified": null,
        "school_campus": "Boston",
        "school_campus_verified": null,
        "phone": null,
        "phone_verified": null,
        "address": "New street east block",
        "address_verified": null,
        "town": null,
        "town_verified": null,
        "city": "Boston",
        "city_verified": null,
        "state": "Massachusetts",
        "state_verified": null,
        "zipcode": "129012",
        "zipcode_verified": null,
        "country": "USA",
        "country_verified": null,
        "start_date": "28-12-1991",
        "start_date_verified": null,
        "end_date": "12-28-1996",
        "end_date_verified": null,
        "degree": "Engineering",
        "degree_verified": null,
        "major": "Biotech",
        "major_verified": null,
        "school_type": "University",
        "source": null,
        "created_at": "2020-08-26T07:15:58.000Z",
        "updated_at": "2020-08-26T07:16:55.604Z",
        "currently_attending": "0",
        "completed_successfully": "1",
        "status": "PENDING",
        "status_new": null,
        "super_admin_status": null,
        "super_admin_status_new": null
    }
}
```

This API will be used to edit the education records for the Employee. The id parameter passed will be the same as received at the time of Education Entry submission.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| id | `uuid` | UUID of the education entry. |
| school_name | `string` | The name of the employee's school. |
| school_type | `string` | The type of the employee's school. |
| school_campus | `string` | The campus of the employee's school. |
| address | `string` | The address of the employee's school. |
| city | `string` | The city where the employee's school is located. |
| state | `string` | The state where the employee's school is located. |
| zip_code | `string` | The ZIP code of the postal district where the employee's school is located. |
| country | `string` | The country where the employee's school is located. |
| start_date | `string` | Start date of the course. |
| end_date | `string` | End date of the course. |
| degree | `string` | Official degree of the course. |
| currently_attending | `string` | This flag will be set to 1 if the employee is currently attending the course, else it will be set to 0. |
| completed_successfully | `string` | This flag will be set to 1 if the employee has successfully completed the course, else it will be set to 0. |
| major | `string` | The field in which the employee has majored. |

## Delete Education

```shell
curl --location --request DELETE 'https://api.us.springverify.com/employee/education?id=de359912-1223-4338-87c4-0783a0ea495b' \
--header 'Authorization: Bearer JWT_TOKEN'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/employee/education?id=de359912-1223-4338-87c4-0783a0ea495b', {
    headers: {
        'Authorization': 'Bearer JWT_TOKEN'
    }
});

// REQUEST

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN'
};

var options = {
    url: 'https://api.us.springverify.com/employee/education?id=de359912-1223-4338-87c4-0783a0ea495b',
    headers: headers
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Authorization' => 'Bearer JWT_TOKEN'
);
$response = Requests::get('https://api.us.springverify.com/employee/education?id=de359912-1223-4338-87c4-0783a0ea495b', $headers);
```

```python
import requests

headers = {
    'Authorization': 'Bearer JWT_TOKEN',
}

params = (
    ('id', 'de359912-1223-4338-87c4-0783a0ea495b'),
)

response = requests.get('https://api.us.springverify.com/employee/education', headers=headers, params=params)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/employee/education?id=de359912-1223-4338-87c4-0783a0ea495b")
request = Net::HTTP::Delete.new(uri)
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "success": true,
    "data": 1
}
```

This API is used to delete the education entry submitted in the previous API before it goes for verification.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| id | `uuid` | UUID of the education entry. |

## Trigger Education Verification

```shell
curl --location --request GET 'https://api.us.springverify.com/employee/submit/education' \
--header 'Authorization: Bearer JWT_TOKEN'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/employee/submit/education', {
    headers: {
        'Authorization': 'Bearer JWT_TOKEN'
    }
});

// REQUEST

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN'
};

var options = {
    url: 'https://api.us.springverify.com/employee/submit/education',
    headers: headers
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Authorization' => 'Bearer JWT_TOKEN'
);
$response = Requests::get('https://api.us.springverify.com/employee/submit/education', $headers);
```

```python
import requests

headers = {
    'Authorization': 'Bearer JWT_TOKEN',
}

response = requests.get('https://api.us.springverify.com/employee/submit/education', headers=headers)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/employee/submit/education")
request = Net::HTTP::Get.new(uri)
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "success": true,
    "data": {
        "success": true,
        "status": "Completed",
        "institution_name_verified": false,
        "employer_name_verified": false
    }
}
```

Once the education records have been submitted and finalized, an Education Verification request can be triggered using this API.

## Create Password

```shell
curl --location --request POST 'https://api.us.springverify.com/employee/create-password' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: text/plain' \
--data-raw '{
    "token": "JWT_TOKEN"
	"password_hash":"5c29a959abce4eda5f0e7a4e7ea53dce4fa0f0abbe8eaa63717e2fed5f193d31"
}'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/employee/create-password', {
    method: 'POST',
    headers: {
        'Content-Type': 'text/plain',
        'Authorization': 'Bearer JWT_TOKEN'
    },
    body: '{ "token": "JWT_TOKEN" "password_hash":"5c29a959abce4eda5f0e7a4e7ea53dce4fa0f0abbe8eaa63717e2fed5f193d31" }'
});

// REQUEST

var request = require('request');

var headers = {
    'Content-Type': 'text/plain',
    'Authorization': 'Bearer JWT_TOKEN'
};

var dataString = '{ "token": "JWT_TOKEN" "password_hash":"5c29a959abce4eda5f0e7a4e7ea53dce4fa0f0abbe8eaa63717e2fed5f193d31" }';

var options = {
    url: 'https://api.us.springverify.com/employee/create-password',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Content-Type' => 'text/plain',
    'Authorization' => 'Bearer JWT_TOKEN'
);
$data = '{ "token": "JWT_TOKEN" "password_hash":"5c29a959abce4eda5f0e7a4e7ea53dce4fa0f0abbe8eaa63717e2fed5f193d31" }';
$response = Requests::post('https://api.us.springverify.com/employee/create-password', $headers, $data);
```

```python
import requests

headers = {
    'Content-Type': 'text/plain',
    'Authorization': 'Bearer JWT_TOKEN',
}

data = '{ "token": "JWT_TOKEN" "password_hash":"5c29a959abce4eda5f0e7a4e7ea53dce4fa0f0abbe8eaa63717e2fed5f193d31" }'

response = requests.post('https://api.us.springverify.com/employee/create-password', headers=headers, data=data)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/employee/create-password")
request = Net::HTTP::Post.new(uri)
request.content_type = "text/plain"
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "success": true,
    "data": {
        "id": "9ca37df2-1d81-40df-90aa-b88e081b8103",
        "access_id": "6e7d5f15-9456-430d-b9da-9df67a4d9996",
        "email": "johndoe@gmail.com",
        "password_hash": "yahoo123456789",
        "first_name": "John",
        "middle_name": "David",
        "last_name": "Doe",
        "name_verified": null,
        "created_at": "2020-08-25T14:28:47.000Z",
        "updated_at": "2020-08-26T07:52:04.000Z",
        "employer_id": "1d4fb8ba-09ac-412c-aa62-58970b4d7472",
        "payment_id": "6c75e302-ac2a-4efa-9ba9-c5dfc97b941e",
        "email_sent": true,
        "payment": false,
        "status": null,
        "flow_completed": null,
        "company_created_by": "zed@max.com",
        "employee_limit_id": "e1e2fef4-a1dd-4be2-8dd1-b0ab8ec29ff8",
        "employments": [
            {
                "id": "d0284d0b-eae8-4f0c-a574-66d39ad9d548",
                "email": "johndoe@gmail.com",
                "access_id": null,
                "employer_name": "Stark Industries pvt ltd",
                "employer_name_verified": null,
                "employer_phone": "9911991199",
                "employer_phone_verified": null,
                "employer_address": "12, Manhattan Street",
                "employer_address_verified": null,
                "employer_town": "New York",
                "employer_town_verified": null,
                "state": "New York",
                "state_verified": null,
                "zipcode": "129012",
                "zipcode_verified": null,
                "employer_country": "USA",
                "employer_country_verified": null,
                "job_title": "Senior Manager",
                "job_title_verified": null,
                "start_date": "19-11-2000",
                "start_date_verified": null,
                "end_date": "19-11-2002",
                "end_date_verified": null,
                "supervisor_name": "Nick Fury",
                "supervisor_contact": "nickfury@starkindustries.com",
                "current_employment": "0",
                "current_employment_verified": null,
                "contract_type": null,
                "contract_type_verified": null,
                "source": null,
                "created_at": "2020-08-26T06:24:25.000Z",
                "updated_at": "2020-08-26T06:26:07.000Z",
                "job_type": "Full time",
                "reason_for_leaving": "xyz",
                "status": 0,
                "status_new": null,
                "super_admin_status": null,
                "super_admin_status_new": null,
                "consent": null,
                "adverse_action": null
            }
        ],
        "education": [
            {
                "id": "18304027-e78a-426c-aae5-db9c677c1704",
                "email": "johndoe@gmail.com",
                "access_id": null,
                "employee_alias_name": null,
                "employee_alias_name_verified": null,
                "school_name": "MIT",
                "school_name_verified": null,
                "school_campus": "Boston",
                "school_campus_verified": null,
                "phone": null,
                "phone_verified": null,
                "address": "New street east block",
                "address_verified": null,
                "town": null,
                "town_verified": null,
                "city": "Boston",
                "city_verified": null,
                "state": "Massachusetts",
                "state_verified": null,
                "zipcode": "129012",
                "zipcode_verified": null,
                "country": "USA",
                "country_verified": null,
                "start_date": "28-12-1991",
                "start_date_verified": null,
                "end_date": "12-28-1996",
                "end_date_verified": null,
                "degree": "Engineering",
                "degree_verified": null,
                "major": "Biotech",
                "major_verified": null,
                "school_type": "University",
                "source": null,
                "created_at": "2020-08-26T07:15:58.000Z",
                "updated_at": "2020-08-26T07:16:55.000Z",
                "currently_attending": 0,
                "completed_successfully": 1,
                "status": 0,
                "status_new": null,
                "super_admin_status": null,
                "super_admin_status_new": null,
                "adverse_action": null
            }
        ],
        "cic_criminal_records": [
            {
                "id": "4fb5b9ee-873e-4a7c-b19b-b0ae943f420b",
                "record_type": null,
                "reviewed": false,
                "adverse_action": null
            },
            {
                "id": "f68e75da-a915-4e4c-8159-9e86ca595996",
                "record_type": null,
                "reviewed": false,
                "adverse_action": null
            }
        ],
        "professional_licenses": [],
        "employee_detail": {
            "id": "efa4191a-331d-48c3-8e52-8915ed8167be",
            "access_id": null,
            "address": "236 Avea street",
            "address_verified": null,
            "driving_number": null,
            "driving_number_verified": null,
            "city": "Gotham",
            "city_verified": null,
            "state": "CA",
            "state_verified": null,
            "zipcode": "33433",
            "zipcode_verified": null,
            "country": null,
            "country_verified": null,
            "birthdate": "12-11-1980",
            "birthdate_verified": null,
            "phone": "56-999222992",
            "phone_verified": null,
            "ssn": "6789",
            "ssn_verified": null,
            "created_at": "2020-08-25T14:32:46.000Z",
            "updated_at": "2020-08-25T14:32:46.000Z",
            "employee_email": "johndoe@gmail.com"
        },
        "employee_verification": {
            "id": "c068d1da-9615-4b8e-a7a7-5daf624ed8d1",
            "s3_gov_id": "image link",
            "s3_gov_id_back": "image link",
            "s3_gov_id_match": false,
            "s3_web_img": null,
            "s3_passport_verified": 1,
            "passport_status": "VERIFIED",
            "s3_dl_verified": 1,
            "dl_status": "VERIFIED",
            "verification_type": "id",
            "address": null,
            "address_verified": null,
            "city": null,
            "city_verified": null,
            "state": null,
            "state_verified": null,
            "zipcode": null,
            "zipcode_verified": null,
            "country": null,
            "country_verified": null,
            "birthdate": null,
            "birthdate_verified": null,
            "criminal_verified": null,
            "global_watchlist_verified": null,
            "created_at": "2020-08-25T14:48:27.000Z",
            "updated_at": "2020-08-26T05:01:16.000Z",
            "is_report_checked": false,
            "summary_of_rights_accepted": true,
            "background_check_disclosure_accepted": true,
            "id_manual_review": null,
            "super_admin_status": null,
            "super_admin_status_new": null,
            "consent_link": "link",
            "spring_sign_ref_id": "5f4524b817710d0014423b61",
            "employee_email": "johndoe@gmail.com"
        },
        "employee_limit": {
            "id": "e1e2fef4-a1dd-4be2-8dd1-b0ab8ec29ff8",
            "employment": 2,
            "education": 1,
            "professional_license": 0,
            "all_county_criminal_search": true,
            "county_criminal_search": 0,
            "civil_court": 1,
            "driving_license": 0,
            "package_id": null,
            "created_at": "2020-08-25T14:28:47.000Z",
            "updated_at": "2020-08-25T14:28:47.000Z",
            "employee_invite_group_id": "b630d0ec-e8d6-49a0-bbf2-8fd26a791a85",
            "employee_invite_group": {
                "id": "b630d0ec-e8d6-49a0-bbf2-8fd26a791a85",
                "package": "diamond",
                "active": true,
                "created_at": "2020-08-25T14:28:47.000Z",
                "updated_at": "2020-08-25T14:28:47.000Z",
                "company_created_by": "zed@max.com",
                "package_id": "5"
            }
        },
        "kbaqna": {
            "id": "71f4dda4-2949-4f23-97ae-26d158c082a2",
            "access_id": "6e7d5f15-9456-430d-b9da-9df67a4d9996",
            "email": "johndoe@gmail.com",
            "questions_flag": 1,
            "kba_enabled": 1,
            "question_count": "5",
            "correct_answers": "4",
            "verified": false,
            "created_at": "2020-08-25T15:06:19.000Z",
            "updated_at": "2020-08-25T15:11:49.000Z",
            "idm_session_id": "91b59885191b01f5",
            "employee_email": "johndoe@gmail.com"
        },
        "employee_flow": {
            "id": 289,
            "employment_flow": "SUBMITTED",
            "education_flow": "SUBMITTED",
            "professional_license_flow": null,
            "created_at": "2020-08-25T14:32:40.000Z",
            "updated_at": "2020-08-26T07:36:45.000Z",
            "employee_email": "johndoe@gmail.com"
        },
        "s3_files": [
            {
                "id": "0d81811f-4cb7-4d3e-875f-0e229ab98fa4",
                "type": "ID",
                "sub_type": "BACK",
                "relation_id": "c7509e8b-49bb-40df-bc80-609c4f460af8",
                "link": "link",
                "reference_id": "9ca37df2-1d81-40df-90aa-b88e081b8103",
                "status": null,
                "comments": null,
                "deleted_at": null,
                "created_at": "2020-08-25T15:14:00.000Z",
                "updated_at": "2020-08-25T15:14:00.000Z"
            },
            {
                "id": "3bf55d4f-6570-4d32-93ae-fc101677fce9",
                "type": "ID",
                "sub_type": "FRONT",
                "relation_id": "c7509e8b-49bb-40df-bc80-609c4f460af8",
                "link": "link",
                "reference_id": "9ca37df2-1d81-40df-90aa-b88e081b8103",
                "status": null,
                "comments": null,
                "deleted_at": null,
                "created_at": "2020-08-25T15:14:00.000Z",
                "updated_at": "2020-08-25T15:14:00.000Z"
            }
        ],
        "criminal_statuses": [
            {
                "id": "026c5a13-8112-40c7-8bba-39655a3f987a",
                "type": "NATIONAL_CRIMINAL",
                "status": "PENDING",
                "source": "SYSTEM",
                "employee_email_fk": "johndoe@gmail.com",
                "created_at": "2020-08-25T15:14:36.000Z",
                "updated_at": "2020-08-25T15:14:36.000Z"
            },
            {
                "id": "0e9fb3d3-d464-4752-bc34-8537807482b9",
                "type": "GLOBAL_WATCHLIST",
                "status": "PENDING",
                "source": "SYSTEM",
                "employee_email_fk": "johndoe@gmail.com",
                "created_at": "2020-08-25T15:14:36.000Z",
                "updated_at": "2020-08-25T15:14:36.000Z"
            },
            {
                "id": "2cf42d97-850f-4348-bac9-a7eacbe575b9",
                "type": "SEX_OFFENDER",
                "status": "PENDING",
                "source": "SYSTEM",
                "employee_email_fk": "johndoe@gmail.com",
                "created_at": "2020-08-25T15:14:36.000Z",
                "updated_at": "2020-08-25T15:14:36.000Z"
            },
            {
                "id": "4b8e8f14-db92-476c-b2fb-bf9d054ad96e",
                "type": "COUNTY_CRIMINAL_SEARCH",
                "status": "PENDING",
                "source": "SYSTEM",
                "employee_email_fk": "johndoe@gmail.com",
                "created_at": "2020-08-25T15:14:36.000Z",
                "updated_at": "2020-08-25T15:14:36.000Z"
            },
            {
                "id": "5d16c475-ba47-44fe-962c-a32dc26878c5",
                "type": "CIVIL_COURT",
                "status": "PENDING",
                "source": "SYSTEM",
                "employee_email_fk": "johndoe@gmail.com",
                "created_at": "2020-08-25T15:14:36.000Z",
                "updated_at": "2020-08-25T15:14:36.000Z"
            }
        ],
        "sjv_criminal_reports": [
            {
                "id": "d18c1432-91be-4ce2-8f07-4bff6728850c",
                "employee_email_fk": "johndoe@gmail.com",
                "status": "NOT_INITIATED",
                "sjv_search_type": "CIVIL_COURT_NOTIFICATION",
                "reference_id": null,
                "cic_criminal_record_fk": null,
                "report_link": null,
                "marked_done": 0,
                "marked_reviewed": 0,
                "county_id": null,
                "adverse_action_fk": null,
                "authenticating_unique_identifier": null,
                "created_at": "2020-08-25T15:14:36.000Z",
                "updated_at": "2020-08-25T15:14:36.000Z",
                "county_name": null,
                "adverse_action": null
            },
            {
                "id": "fed50a2f-bc72-4d3b-99dd-5e5053304008",
                "employee_email_fk": "johndoe@gmail.com",
                "status": "NOT_INITIATED",
                "sjv_search_type": "COUNTY_CRIMINAL_NOTIFICATION",
                "reference_id": null,
                "cic_criminal_record_fk": null,
                "report_link": null,
                "marked_done": 0,
                "marked_reviewed": 0,
                "county_id": null,
                "adverse_action_fk": null,
                "authenticating_unique_identifier": null,
                "created_at": "2020-08-25T15:14:36.000Z",
                "updated_at": "2020-08-25T15:14:36.000Z",
                "county_name": null,
                "adverse_action": null
            }
        ]
    }
}
```

After all the previously mentioned checks have been successfully submitted and triggered, the employee can create a password for their profile using this API.

<aside class="notice">
The `Password` fields shouldbe hashed using SHA256 beforehand.
</aside>

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| token | `string` | JWT_TOKEN |
| password_hash | `string` | Hash of the password. (8 characters minimum). |

## Reset Password

```shell
curl --location --request POST 'https://api.us.springverify.com/employee/reset-password' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: text/plain' \
--data-raw '{
	"password":"ab0365e1c40ea17c8d1e7819c45a477ac836080b3b5fd1305ccb281acf24c62e"
}'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/employee/reset-password', {
    method: 'POST',
    headers: {
        'Content-Type': 'text/plain',
        'Authorization': 'Bearer JWT_TOKEN'
    },
    body: JSON.stringify({ "password":"ab0365e1c40ea17c8d1e7819c45a477ac836080b3b5fd1305ccb281acf24c62e" })
});

// REQUEST

var request = require('request');

var headers = {
    'Content-Type': 'text/plain',
    'Authorization': 'Bearer JWT_TOKEN'
};

var dataString = '{ "password":"ab0365e1c40ea17c8d1e7819c45a477ac836080b3b5fd1305ccb281acf24c62e" }';

var options = {
    url: 'https://api.us.springverify.com/employee/reset-password',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Content-Type' => 'text/plain',
    'Authorization' => 'Bearer JWT_TOKEN'
);
$data = '{ "password":"ab0365e1c40ea17c8d1e7819c45a477ac836080b3b5fd1305ccb281acf24c62e" }';
$response = Requests::post('https://api.us.springverify.com/employee/reset-password', $headers, $data);
```

```python
import requests

headers = {
    'Content-Type': 'text/plain',
    'Authorization': 'Bearer JWT_TOKEN',
}

data = '{ "password":"ab0365e1c40ea17c8d1e7819c45a477ac836080b3b5fd1305ccb281acf24c62e" }'

response = requests.post('https://api.us.springverify.com/employee/reset-password', headers=headers, data=data)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/employee/reset-password")
request = Net::HTTP::Post.new(uri)
request.content_type = "text/plain"
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "success": true,
    "data": true
}
```

This API is used to reset the employee profile password.

<aside class="notice">
The `Password` fields shouldbe hashed using SHA256 beforehand.
</aside>

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| password | `string` | Hash of the password. (8 characters minimum). |

## Complete Employee Flow

```shell
curl --location --request GET 'https://api.us.springverify.com/employee/flow-completed' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: text/plain' \
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/employee/flow-completed', {
    headers: {
        'Content-Type': 'text/plain',
        'Authorization': 'Bearer JWT_TOKEN'
    }
});

// REQUEST

var request = require('request');

var headers = {
    'Content-Type': 'text/plain',
    'Authorization': 'Bearer JWT_TOKEN'
};

var options = {
    url: 'https://api.us.springverify.com/employee/flow-completed',
    headers: headers
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Content-Type' => 'text/plain',
    'Authorization' => 'Bearer JWT_TOKEN'
);
$response = Requests::get('https://api.us.springverify.com/employee/flow-completed', $headers);
```

```python
import requests

headers = {
    'Content-Type': 'text/plain',
    'Authorization': 'Bearer JWT_TOKEN',
}

response = requests.get('https://api.us.springverify.com/employee/flow-completed', headers=headers)
```

```ruby

require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/employee/flow-completed")
request = Net::HTTP::Get.new(uri)
request.content_type = "text/plain"
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
  "success": true,
  "data": {
    "id": "9ca37df2-1d81-40df-90aa-b88e081b8103",
    "access_id": "6e7d5f15-9456-430d-b9da-9df67a4d9996",
    "email": "johndoe@gmail.com",
    "password_hash": "yahoo123456789",
    "first_name": "John",
    "middle_name": "David",
    "last_name": "Doe",
    "name_verified": null,
    "created_at": "2020-08-25T14:28:47.000Z",
    "updated_at": "2020-08-26T07:52:04.000Z",
    "employer_id": "1d4fb8ba-09ac-412c-aa62-58970b4d7472",
    "payment_id": "6c75e302-ac2a-4efa-9ba9-c5dfc97b941e",
    "email_sent": true,
    "payment": false,
    "status": null,
    "flow_completed": null,
    "company_created_by": "zed@max.com",
    "employee_limit_id": "e1e2fef4-a1dd-4be2-8dd1-b0ab8ec29ff8",
    "employments": [
      {
        "id": "d0284d0b-eae8-4f0c-a574-66d39ad9d548",
        "email": "johndoe@gmail.com",
        "access_id": null,
        "employer_name": "Stark Industries pvt ltd",
        "employer_name_verified": null,
        "employer_phone": "9911991199",
        "employer_phone_verified": null,
        "employer_address": "12, Manhattan Street",
        "employer_address_verified": null,
        "employer_town": "New York",
        "employer_town_verified": null,
        "state": "New York",
        "state_verified": null,
        "zipcode": "129012",
        "zipcode_verified": null,
        "employer_country": "USA",
        "employer_country_verified": null,
        "job_title": "Senior Manager",
        "job_title_verified": null,
        "start_date": "19-11-2000",
        "start_date_verified": null,
        "end_date": "19-11-2002",
        "end_date_verified": null,
        "supervisor_name": "Nick Fury",
        "supervisor_contact": "nickfury@starkindustries.com",
        "current_employment": "0",
        "current_employment_verified": null,
        "contract_type": null,
        "contract_type_verified": null,
        "source": null,
        "created_at": "2020-08-26T06:24:25.000Z",
        "updated_at": "2020-08-26T06:26:07.000Z",
        "job_type": "Full time",
        "reason_for_leaving": "xyz",
        "status": 0,
        "status_new": null,
        "super_admin_status": null,
        "super_admin_status_new": null,
        "consent": null,
        "adverse_action": null
      }
    ],
    "education": [
      {
        "id": "18304027-e78a-426c-aae5-db9c677c1704",
        "email": "johndoe@gmail.com",
        "access_id": null,
        "employee_alias_name": null,
        "employee_alias_name_verified": null,
        "school_name": "MIT",
        "school_name_verified": null,
        "school_campus": "Boston",
        "school_campus_verified": null,
        "phone": null,
        "phone_verified": null,
        "address": "New street east block",
        "address_verified": null,
        "town": null,
        "town_verified": null,
        "city": "Boston",
        "city_verified": null,
        "state": "Massachusetts",
        "state_verified": null,
        "zipcode": "129012",
        "zipcode_verified": null,
        "country": "USA",
        "country_verified": null,
        "start_date": "28-12-1991",
        "start_date_verified": null,
        "end_date": "12-28-1996",
        "end_date_verified": null,
        "degree": "Engineering",
        "degree_verified": null,
        "major": "Biotech",
        "major_verified": null,
        "school_type": "University",
        "source": null,
        "created_at": "2020-08-26T07:15:58.000Z",
        "updated_at": "2020-08-26T07:16:55.000Z",
        "currently_attending": 0,
        "completed_successfully": 1,
        "status": 0,
        "status_new": null,
        "super_admin_status": null,
        "super_admin_status_new": null,
        "adverse_action": null
      }
    ],
    "cic_criminal_records": [
      {
        "id": "4fb5b9ee-873e-4a7c-b19b-b0ae943f420b",
        "record_type": null,
        "reviewed": false,
        "adverse_action": null
      },
      {
        "id": "f68e75da-a915-4e4c-8159-9e86ca595996",
        "record_type": null,
        "reviewed": false,
        "adverse_action": null
      }
    ],
    "professional_licenses": [
      
    ],
    "employee_detail": {
      "id": "efa4191a-331d-48c3-8e52-8915ed8167be",
      "access_id": null,
      "address": "236 Avea street",
      "address_verified": null,
      "driving_number": null,
      "driving_number_verified": null,
      "city": "Gotham",
      "city_verified": null,
      "state": "CA",
      "state_verified": null,
      "zipcode": "33433",
      "zipcode_verified": null,
      "country": null,
      "country_verified": null,
      "birthdate": "12-11-1980",
      "birthdate_verified": null,
      "phone": "56-999222992",
      "phone_verified": null,
      "ssn": "6789",
      "ssn_verified": null,
      "created_at": "2020-08-25T14:32:46.000Z",
      "updated_at": "2020-08-25T14:32:46.000Z",
      "employee_email": "johndoe@gmail.com"
    },
    "employee_verification": {
      "id": "c068d1da-9615-4b8e-a7a7-5daf624ed8d1",
      "s3_gov_id": "image link",
      "s3_gov_id_back": "image link",
      "s3_gov_id_match": false,
      "s3_web_img": null,
      "s3_passport_verified": 1,
      "passport_status": "FAILED",
      "s3_dl_verified": 1,
      "dl_status": "VERIFIED",
      "verification_type": "id",
      "address": null,
      "address_verified": null,
      "city": null,
      "city_verified": null,
      "state": null,
      "state_verified": null,
      "zipcode": null,
      "zipcode_verified": null,
      "country": null,
      "country_verified": null,
      "birthdate": null,
      "birthdate_verified": null,
      "criminal_verified": null,
      "global_watchlist_verified": null,
      "created_at": "2020-08-25T14:48:27.000Z",
      "updated_at": "2020-08-26T05:01:16.000Z",
      "is_report_checked": false,
      "summary_of_rights_accepted": true,
      "background_check_disclosure_accepted": true,
      "id_manual_review": null,
      "super_admin_status": null,
      "super_admin_status_new": null,
      "consent_link": "link",
      "spring_sign_ref_id": "5f4524b817710d0014423b61",
      "employee_email": "johndoe@gmail.com"
    },
    "employee_limit": {
      "id": "e1e2fef4-a1dd-4be2-8dd1-b0ab8ec29ff8",
      "employment": 2,
      "education": 1,
      "professional_license": 0,
      "all_county_criminal_search": true,
      "county_criminal_search": 0,
      "civil_court": 1,
      "driving_license": 0,
      "package_id": null,
      "created_at": "2020-08-25T14:28:47.000Z",
      "updated_at": "2020-08-25T14:28:47.000Z",
      "employee_invite_group_id": "b630d0ec-e8d6-49a0-bbf2-8fd26a791a85",
      "employee_invite_group": {
        "id": "b630d0ec-e8d6-49a0-bbf2-8fd26a791a85",
        "package": "diamond",
        "active": true,
        "created_at": "2020-08-25T14:28:47.000Z",
        "updated_at": "2020-08-25T14:28:47.000Z",
        "company_created_by": "zed@max.com",
        "package_id": "5"
      }
    },
    "kbaqna": {
      "id": "71f4dda4-2949-4f23-97ae-26d158c082a2",
      "access_id": "6e7d5f15-9456-430d-b9da-9df67a4d9996",
      "email": "johndoe@gmail.com",
      "questions_flag": 1,
      "kba_enabled": 1,
      "question_count": "5",
      "correct_answers": "4",
      "verified": false,
      "created_at": "2020-08-25T15:06:19.000Z",
      "updated_at": "2020-08-25T15:11:49.000Z",
      "idm_session_id": "91b59885191b01f5",
      "employee_email": "johndoe@gmail.com"
    },
    "employee_flow": {
      "id": 289,
      "employment_flow": "SUBMITTED",
      "education_flow": "SUBMITTED",
      "professional_license_flow": null,
      "created_at": "2020-08-25T14:32:40.000Z",
      "updated_at": "2020-08-26T07:36:45.000Z",
      "employee_email": "johndoe@gmail.com"
    },
    "s3_files": [
      {
        "id": "0d81811f-4cb7-4d3e-875f-0e229ab98fa4",
        "type": "ID",
        "sub_type": "BACK",
        "relation_id": "c7509e8b-49bb-40df-bc80-609c4f460af8",
        "link": "link",
        "reference_id": "9ca37df2-1d81-40df-90aa-b88e081b8103",
        "status": null,
        "comments": null,
        "deleted_at": null,
        "created_at": "2020-08-25T15:14:00.000Z",
        "updated_at": "2020-08-25T15:14:00.000Z"
      },
      {
        "id": "3bf55d4f-6570-4d32-93ae-fc101677fce9",
        "type": "ID",
        "sub_type": "FRONT",
        "relation_id": "c7509e8b-49bb-40df-bc80-609c4f460af8",
        "link": "link",
        "reference_id": "9ca37df2-1d81-40df-90aa-b88e081b8103",
        "status": null,
        "comments": null,
        "deleted_at": null,
        "created_at": "2020-08-25T15:14:00.000Z",
        "updated_at": "2020-08-25T15:14:00.000Z"
      }
    ],
    "criminal_statuses": [
      {
        "id": "026c5a13-8112-40c7-8bba-39655a3f987a",
        "type": "NATIONAL_CRIMINAL",
        "status": "PENDING",
        "source": "SYSTEM",
        "employee_email_fk": "johndoe@gmail.com",
        "created_at": "2020-08-25T15:14:36.000Z",
        "updated_at": "2020-08-25T15:14:36.000Z"
      },
      {
        "id": "0e9fb3d3-d464-4752-bc34-8537807482b9",
        "type": "GLOBAL_WATCHLIST",
        "status": "PENDING",
        "source": "SYSTEM",
        "employee_email_fk": "johndoe@gmail.com",
        "created_at": "2020-08-25T15:14:36.000Z",
        "updated_at": "2020-08-25T15:14:36.000Z"
      },
      {
        "id": "2cf42d97-850f-4348-bac9-a7eacbe575b9",
        "type": "SEX_OFFENDER",
        "status": "PENDING",
        "source": "SYSTEM",
        "employee_email_fk": "johndoe@gmail.com",
        "created_at": "2020-08-25T15:14:36.000Z",
        "updated_at": "2020-08-25T15:14:36.000Z"
      },
      {
        "id": "4b8e8f14-db92-476c-b2fb-bf9d054ad96e",
        "type": "COUNTY_CRIMINAL_SEARCH",
        "status": "PENDING",
        "source": "SYSTEM",
        "employee_email_fk": "johndoe@gmail.com",
        "created_at": "2020-08-25T15:14:36.000Z",
        "updated_at": "2020-08-25T15:14:36.000Z"
      },
      {
        "id": "5d16c475-ba47-44fe-962c-a32dc26878c5",
        "type": "CIVIL_COURT",
        "status": "PENDING",
        "source": "SYSTEM",
        "employee_email_fk": "johndoe@gmail.com",
        "created_at": "2020-08-25T15:14:36.000Z",
        "updated_at": "2020-08-25T15:14:36.000Z"
      }
    ],
    "sjv_criminal_reports": [
      {
        "id": "d18c1432-91be-4ce2-8f07-4bff6728850c",
        "employee_email_fk": "johndoe@gmail.com",
        "status": "NOT_INITIATED",
        "sjv_search_type": "CIVIL_COURT_NOTIFICATION",
        "reference_id": null,
        "cic_criminal_record_fk": null,
        "report_link": null,
        "marked_done": 0,
        "marked_reviewed": 0,
        "county_id": null,
        "adverse_action_fk": null,
        "authenticating_unique_identifier": null,
        "created_at": "2020-08-25T15:14:36.000Z",
        "updated_at": "2020-08-25T15:14:36.000Z",
        "county_name": null,
        "adverse_action": null
      },
      {
        "id": "fed50a2f-bc72-4d3b-99dd-5e5053304008",
        "employee_email_fk": "johndoe@gmail.com",
        "status": "NOT_INITIATED",
        "sjv_search_type": "COUNTY_CRIMINAL_NOTIFICATION",
        "reference_id": null,
        "cic_criminal_record_fk": null,
        "report_link": null,
        "marked_done": 0,
        "marked_reviewed": 0,
        "county_id": null,
        "adverse_action_fk": null,
        "authenticating_unique_identifier": null,
        "created_at": "2020-08-25T15:14:36.000Z",
        "updated_at": "2020-08-25T15:14:36.000Z",
        "county_name": null,
        "adverse_action": null
      }
    ]
  }
}
```

This API is used to let the system know that employee form has been submitted successfully.

## Employee Login

```shell
curl --location --request POST 'https://api.us.springverify.com/auth/login' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--data-raw '{
    "email": "john@wick.com",
    "password": "daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5",
    "role":"employee"
}'
```

```javascript
// FETCH

var fetch = require('node-fetch');

fetch('https://api.us.springverify.com/auth/login', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
        'Authorization': 'Bearer JWT_TOKEN'
    },
    body: JSON.stringify({ "email": "john@wick.com", "password": "daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5", "role":"employee" })
});

// REQUEST

var request = require('request');

var headers = {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer JWT_TOKEN'
};

var dataString = '{ "email": "john@wick.com", "password": "daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5", "role":"employee" }';

var options = {
    url: 'https://api.us.springverify.com/auth/login',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Content-Type' => 'application/json',
    'Authorization' => 'Bearer JWT_TOKEN'
);
$data = '{ "email": "john@wick.com", "password": "daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5", "role":"employee" }';
$response = Requests::post('https://api.us.springverify.com/auth/login', $headers, $data);
```

```python
import requests

headers = {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer JWT_TOKEN',
}

data = '{ "email": "john@wick.com", "password": "daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5", "role":"employee" }'

response = requests.post('https://api.us.springverify.com/auth/login', headers=headers, data=data)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.us.springverify.com/auth/login")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"
request["Authorization"] = "Bearer JWT_TOKEN"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

# response.code
# response.body
```

> Success Response

```json
{
    "success": true,
    "data": {
        "success": true,
        "success_msg": "employee logged in successfully",
        "data": {
            "token": "JWT_TOKEN"
        }
    }
}
```

> Error Response:

```json
{
    "errors": [
        {
            "value": "",
            "msg": "Not a valid role",
            "param": "role",
            "location": "body"
        }
    ]
}
```

For logging in, the aim is to generate a JSON web token that is to be used in all subsequent API calls. The JWT generated will be valid for one hour.

To call the subsequent APIs, the user will need to send the JWT successfully in the header of those APIs.

<aside class="notice">
The password should be hashed using SHA256 beforehand.
</aside>

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| email | `string` | The email address used by the employee to register with SpringVerify. |
| password | `string` | The password used by the employee to register with SpringVerify. (Hashed, 8 characters minimum). |
| role | `string` | The role of the user being logged in - in this case, `employee`. |

---
