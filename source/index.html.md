---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - javascript
  - python
  - ruby
  - php

search: true

code_clipboard: true
---

# Relicflare REST API

This is the official documentation for the Relicflare REST API. These APIs provide numerous capabilities for important testing and monitoring methods for websites. For instance, website performance, security, DNS, and SEO score are some of the capabilities offered by these APIs.

# API type

As mentioned earlier, the Relicflare API is a REST API. That means it works over HTTP and accepts and returns data in the JSON (JavaScript Object Notation) format.

# Rate limits

The Relicflare API permits a call rate of **10 API calls per second** for each client having a valid token.  

# Authentication

Relicflare follows the (mostly) standard Bearer Token strategy used in HTTP APIs for authentication/authorization. The API expects the token to be present in a request header called `Authorization`, whose value must be in the format `Bearer <token>`. For example: `Bearer fJrk5RSO3CScRhcs5TaE3d5ecEsjCYzNFskg` is a valid header value. Notice the single space between "Bearer" and the token and make sure it's present (only one space!) in your API calls.

# Obtaining the API token

Coming soon . . .

# Base URL

For all the endpoints listed in this documentation, the base URL is https://relicflare.p.rapidapi.com.

# Code samples

Tested, working code samples are provided for the following languages:

* Bash (the universal "language")
* JavaScript
* Python
* Ruby
* PHP

If your language is not listed here, converting the Bash examples should be trivial as they use the Linux utility `curl` and are fully transparent in their working.

Please note that while these examples can be directly copy-pasted and will work, they don't necessarily represent the recommended way of making API calls. For instance, you might be more familiar with some other method/library, or maybe your workplace insists on a particular library for security reasons. Therefore, chances are you'd use these snippets to understand how to make the calls in your language and then adjust as needed.

# Response formats

The API response follows some common formats for the sake of clarity and consistency. Depending on the request status, the response structures are as given below.

## 200/201 - Success

Response on success

```json
{
    "timestamp": 1610796547300,
    "apiStatus": "success",
    "apiCode": 201,
    "message": "An overview message.",
    "meta": {
        "url": "https://example.com",
        "test": {
            "id": "test_id_to_view_results_later"
        }
    },
    "data": [
        {
					.....
        }
    ]
}
```

## 401 - Unauthorized

When the incoming request doesn't have proper authorization, the following response structure is used.

```json
{
    "timestamp": 1610802049955,
    "apiStatus": "failure",
    "apiCode": 401,
    "message": "You are not authorized to access the API.",
    "meta": {
        "method": "GET",
        "endpoint": "/requested-end-point"
    }
}
```

## 422 - Unprocessable Entity

This error is mostly the result of a validation error and causes the API to return a response in the following format.

```json
{
    "timestamp": 1610802445020,
    "apiStatus": "failure",
    "apiCode": 400,
    "message": "Please fix the errors.",
    "meta": {
        "type": "validation"
    },
    "data": [
        {
            "field": "url",
            "value": "https://the-endpoint.com",
            "error": "Invalid url provided."
        }
    ]
}
```

## 400 - Bad request

In case the server refuses to process the request, it sends response in the following format.

```json
{
  "timestamp": 1607702387460,
  "apiStatus": "failure",
  "apiCode": 400,
  "message": "Unable to reach the host.",
  "meta": {
    "type": "unreachable"
  },
  "data": {
    "host": "127.0.0.1"
  }
}
```

## 404 - Not Found

If a wrong endpoint or wrong request type (POST instead of GET, for example) is provided, the API returns an error in the following format.

```json
{
    "timestamp": 1610803132768,
    "apiStatus": "failure",
    "apiCode": 404,
    "message": "API not found.",
    "meta": {
        "method": "POST",
        "endpoint": "/doesnotexist"
    }
}
```

## 405 - Method not Allowed

If a request is asking for a valid resource but the method isn't supported, the error message is in the following format.

```json
{
    "timestamp": 1610803132768,
    "apiStatus": "failure",
    "apiCode": 405,
    "message": "Invalid method.",
		"allow": "POST",
    "meta": {
        "method": "DELETE",
        "endpoint": "/tests/up"
    }
}
```

# Input parameters

Following are the common inputs used in Relicflare APIs. Their formats and expectations are also documented.

## URL

Often a URL needs to be provided among the incoming request's parameters. The format for this is (part of a JSON structure):

```json
"url": "example.com"
```

As shown in the snippet, the `url` parameter always accept a single string value only.

## Locations

Many of the Relicflare APIs are multi-region; that is, tests can be conducted from different regions/locations across the world. In case you need to use this feature, the locations are to be provided as follows:

```json
"locations": [ "uk", "us" ]
```

As indicated, the `"locations"` parameter is an array. Even for a single location, the input is to be sent as an array:

```json
"locations": [ "uk" ]
```

### List of locations (with codes)

We currently support the following locations:

| Code | Location |
| --- | --- |
| uk | London, United Kingdom |
| us | New York, United States |
| sg | Singapore |

## Follow Redirect

In case you want to control the API's behavior for HTTP redirects, include a `followRedirect` parameter with the request.

```json
"followRedirect": "false"
```

As shown, the value for `followRedirect` is a string, and can be either `"true"` or `"false"`.

# API Endpoints

## Time to First Byte (TTFB)

The TTFB API endpoint measures how fast a web resource starts loading when connected. It can be thought of as a way to measure response times (in milliseconds), loosely speaking.

| ||
| --- | --- |
| Request type | POST |
| Endpoint | `/ttfb` |
| Multi-region supported? | Yes |

Input parameters accepted:

| Parameter | Required? | Info |
| --- | --- | --- |
| `url` | Yes | URL of the server/website to be tested |
| `followRedirect` | No | Whether to follow HTTP redirects or not |
|  `locations` | No | Array of location strings for running the test. Defaults to `[ "us" ]`. |

### Sample request and response (JSON format)

Assuming that a POST request is sent to `<baseURL>/ttfb` with the following body:

```json
{
  "url": "https://example.com",
  "locations": [ "uk", "us" ]
}
```

This would generate a response similar to the following:

```json
{
    "timestamp": 1613218604015,
    "apiStatus": "success",
    "apiCode": 201,
    "meta": {
      "url": "https://example.com",
      "followRedirect": true,
    },
    "data": [
        {
            "redirectedURL": "https://example.com/",
            "country": "England",
            "city": "London",
            "ttfb": 615
        },
        {
            "redirectedURL": "https://example.com/",
            "country": "United States",
            "city": "New York",
            "ttfb": 884
        }
    ]
}
```

As shown, the `data` part shows the response times (TTFB) captured from both the locations requested.

Next, we've provided examples in various languages on how to make this API call.

```bash
curl -H "Content-Type: application/json" -H "X-RAPIDAPI-KEY: my-rapidapi-key" -X POST -d '{"url":"https://geekflare.com"}' https://relicflare.p.rapidapi.com/ttfb

# Results in the following output
{"timestamp":1613559895297,"apiStatus":"success","apiCode":201,"meta":{"url":"https://ankushthakur.com","followRedirect":true},"data":[{"redirectedURL":"https://ankushthakur.com/","country":"United States","city":"New York","ttfb":876}]}

# Or in the formatted form:
{
   "timestamp":1613559895297,
   "apiStatus":"success",
   "apiCode":201,
   "meta":{
      "url":"https://example.com",
      "followRedirect":true
   },
   "data":[
      {
         "redirectedURL":"https://example.com/",
         "country":"United States",
         "city":"New York",
         "ttfb":876
      }
   ]
}
```

```javascript
// Assuming the environment is Node and that axios is installed already.
const axios = require('axios');

axios.post('https://relicflare.p.rapidapi.com/ttfb', 
            { url: "https://geekflare.com" }, 
            {
                headers: { 'X-RAPIDAPI-KEY': 'my-api-key' }
            }     
).then(response => {
    console.log(response.data); // shows the expected JSON
})
```

```python
import json
import requests

url = "https://relicflare.p.rapidapi.com/ttfb"
headers = {
    "X-RAPIDAPI-KEY": "my-rapidapi-key",
}
data = {"url": "https://geekflare.com"}
req = requests.post(url, json=data, headers=headers)
print(req.text)  # prints the expected stuff
```

```ruby
require 'faraday'

url = "https://relicflare.p.rapidapi.com/ttfb"
resp = Faraday.post(url, '{"url": "https://geekflare.com"}',
  { "Content-Type" => "application/json", "X-RAPIDAPI-KEY" => "my-rapid-api-key" })
puts resp.body # Outputs the expected
```

```php
<?php
require_once "vendor/autoload.php"; // These examples use the WordPress Requests library (https://github.com/WordPress/Requests), but you're free to use your own

$url = 'https://relicflare.p.rapidapi.com/ttfb';
$headers = [
    'Content-Type' => 'application/json',
    'X-RAPIDAPI-KEY' => 'my-api-key'
];
$data = [ 'url' => 'https://geekflare.com' ];
$response = Requests::post($url, $headers, json_encode($data));
print_r($response->body); // prints data as expected
```

## Site Status (Up/Down)

The Site Status API provides info about whether the given URL is up or not. Almost esclusively, this API is meant to work with websites. Checking the status of an infrastructure (whose entry point is the given URL) isn't recommended and may provide wrong results. 

| ||
| --- | --- |
| Request type | POST |
| Endpoint | `/up` |
| Multi-region supported? | Yes |

Input parameters accepted:

| Parameter | Required? | Info |
| --- | --- | --- |
| `url` | Yes | URL of the website to be checked |
| `followRedirect` | No | Whether to follow HTTP redirects or not |
|  `locations` | No | Array of location strings for running the test. Defaults to `[ "us" ]`. |

### Sample request and response (JSON format)

Assuming that a POST request is sent to `<baseURL>/ttfb` with the following body:

```json
{
  "url": "https://example.com",
  "locations": [ "uk", "us" ]
}
```

This would generate a response similar to the following:

```json
{
    "timestamp": 1613218604015,
    "apiStatus": "success",
    "apiCode": 201,
    "meta": {
      "url": "https://example.com",
      "followRedirect": true,
    },
    "data": [
        {
            "redirectedURL": "https://example.com/",
            "country": "England",
            "city": "London",
            "ttfb": 615
        },
        {
            "redirectedURL": "https://example.com/",
            "country": "United States",
            "city": "New York",
            "ttfb": 884
        }
    ]
}
```

As shown, the `data` part shows the response times (TTFB) captured from both the locations requested.