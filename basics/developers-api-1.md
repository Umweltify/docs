---
description: API Documentation for Device Climate Status
icon: network-wired
---

# Device ID

### Overview

This documentation provides guidelines for developers to integrate our APIs into their applications or games. These APIs allow developers to retrieve users' device climate status and incentivize climate-friendly actions through in-app or in-game rewards.

***

### Base URL&#x20;

The base URL for all API endpoints is:

`https://api.umweltify.com`&#x20;

***

### API 1: **GetToken**

#### Description

The `GetToken` API is used to obtain an authorization token. This token is required for making requests to other APIs and expires in 24 hours.

#### Endpoint

{% swagger src="../.gitbook/assets/openapi.yaml" path="/v1/Auth/GetToken" method="post" %}
[openapi.yaml](../.gitbook/assets/openapi.yaml)
{% endswagger %}

#### Request Parameters

| Parameter   | Type   | Description                                                                                                 |
| ----------- | ------ | ----------------------------------------------------------------------------------------------------------- |
| `appId`     | string | The unique identifier for the developer's app in our [developers portal](https://developers.umweltify.com). |
| `appSecret` | string | The secret key associated with the app.                                                                     |

#### Request Example

```json
{
  "appId": "your-app-id",
  "appSecret": "your-app-secret"
}
```

#### Response Example

```json
{
  "Data": "your-auth-token",
  "StatusCode": 200,
  "Message": "success"
}
```

#### Notes

* Developers can obtain their `appId` and `appSecret` by registering in our [Developer Program](https://developers.umweltify.com).
* The token must be included as a Bearer token in the Authorization header for the ClimateStatus API request.

***

### API 2: **ClimateStatus**

#### Description

The `ClimateStatus` API retrieves the climate status of a specific device. This status is returned as a number that corresponds to a predefined set of descriptions.

#### Endpoint

{% swagger src="../.gitbook/assets/openapi.yaml" path="/v1/ClimateStatus" method="get" %}
[openapi.yaml](../.gitbook/assets/openapi.yaml)
{% endswagger %}

#### Request Parameters

| Parameter  | Type   | Description                          |
| ---------- | ------ | ------------------------------------ |
| `deviceID` | string | The unique identifier of the device. |

#### Headers

| Header        | Description                         |
| ------------- | ----------------------------------- |
| Authorization | Bearer token obtained from GetToken |

#### Query String Example

```
/v1/ClimateStatus?DeviceID=12345
```

#### Response Example

```json
{
  "Data": {
    "ClimateStatus": 5
  },
  "StatusCode": 200,
  "Message": "Success"
}
```

#### Climate Status Descriptions

<table><thead><tr><th width="214">Climate Status Code</th><th>Description</th></tr></thead><tbody><tr><td>0</td><td><mark style="color:orange;">The device climate status is not yet set.</mark></td></tr><tr><td>1</td><td><mark style="color:red;">Not Green: The device owner's home electricity source is not green.</mark></td></tr><tr><td>2</td><td><mark style="color:red;">Not Green: The device owner's station is not available.</mark></td></tr><tr><td>3</td><td><mark style="color:red;">Not Green: Generally not green.</mark></td></tr><tr><td>4</td><td><mark style="color:red;">Not Green: The device's carbon budget is used up.</mark></td></tr><tr><td>5</td><td><mark style="color:green;">Green: The device owner's home electricity source is green.</mark></td></tr><tr><td>6</td><td><mark style="color:green;">Green: The device owner's station carbon budget is available.</mark></td></tr><tr><td>7</td><td><mark style="color:green;">Green: Generally green.</mark></td></tr><tr><td>8</td><td><mark style="color:green;">Green: The device's carbon budget is still available.</mark></td></tr><tr><td>9</td><td><mark style="color:green;">Green: Certified green electricity source through EACs.</mark></td></tr></tbody></table>

#### Notes

* Ensure that the `deviceID` provided is valid and registered in Umweltify.
* To register a device in Umweltify, users must first install the [ThingsApp](https://climatein.umweltify.com/thingsapp) on their devices.
* This API requires a valid authorization token in the request headers.
* Interpret the `ClimateStatus` number using the description table above.

***

### Authentication

The ClimateStatus API requires a valid authorization token in the request headers.

#### Authorization Header Example

```
Authorization: Bearer your-auth-token
```

***

### Error Codes

<table><thead><tr><th width="269">Error Code</th><th>Description</th></tr></thead><tbody><tr><td>400</td><td>Bad Request: Missing or invalid parameters.</td></tr><tr><td>401</td><td>Unauthorized: Invalid or expired authorization.</td></tr><tr><td>404</td><td>Not Found: Requested resource does not exist.</td></tr><tr><td>500</td><td>Internal Server Error: An unexpected error.</td></tr></tbody></table>

***

### Contact

For additional support or inquiries, please submit a ticket in the developers' portal.
