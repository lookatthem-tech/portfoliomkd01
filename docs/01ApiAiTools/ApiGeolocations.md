# Geolocations API

The application automatically assigns geolocations to systems based on available data, but these defaults might not always reflect your organization's preferred location structure.

The Geolocations API enables you to define and manage geolocations when the default assignments are incomplete or inaccurate. For example, in some cases, VPNs, network architecture, or data gaps might lead to incorrect system placement.

With these endpoints, you can do the following:

- Create, update, and delete custom geolocations.
- View both default and custom geolocation assignments.

Use this API when you need greater control over how systems are geographically represented within your environment.

## <span class="get">GET</span> geolocations

This endpoint returns all available geolocations, including both default and custom entries that can be assigned to systems based on IP ranges.

### Request Syntax

```
GET /api/geolocations
```

### Query Parameters

| Parameter  | Description                                                                                                                                                                                                 | Type   | Required? |
| ---------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | --------- |
| top        | Add this parameter to specify the maximum number of locations to get. Here is an example:<br/>`acme-api/geolocations?top=20`                                                                                | string | Optional  |
| country    | Add this parameter to specify the country (by country code) for which you want to return geolocations. Here is an example:<br/>`acme-api/geolocations?country=US`                                           | string | Optional  |
| region     | Add this parameter to specify the region (by region code) for which you want to return geolocations. For U.S. locations, the region is the state. Here is an example:<br/>`acme-api/geolocations?region=MA` | string | Optional  |
| city       | Add this parameter to specify the city (by name) for which you want to return geolocations. Here is an example:<br/>`acme-api/geolocations?city=Boston`                                                     | string | Optional  |
| postalcode | Add this parameter to specify the postal code for which you want to return geolocations. Here is an example:<br/>`acme-api/geolocations?postalcode=02109`                                                   | string | Optional  |

### Response Attributes

| Element             | Description                                                                                                                                                                                                                                                                                                                          |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| locid               | The specific ID for the geolocation.<br/><br/>**IMPORTANT:** Any locid with a negative value (for example, -1) represents a custom-defined location. You can edit and delete custom-defined locations.<br/><br/>Any locid with a positive value (for example, 1) is a default (non-custom) location and cannot be edited or deleted. |
| country             | The country (by country code) for the geolocation.                                                                                                                                                                                                                                                                                   |
| region              | The region (by region code) for the geolocation.                                                                                                                                                                                                                                                                                     |
| city                | The name of the city for the geolocation.                                                                                                                                                                                                                                                                                            |
| postalcode          | The postal code for the geolocation.                                                                                                                                                                                                                                                                                                 |
| latitude            | The latitude for the geolocation.                                                                                                                                                                                                                                                                                                    |
| longitude           | The longitude for the geolocation.                                                                                                                                                                                                                                                                                                   |
| dmacode             | The Designated Market Area code for the geolocation.                                                                                                                                                                                                                                                                                 |
| areacode            | The telephone area code for the geolocation.                                                                                                                                                                                                                                                                                         |
| ipblocks            | An array of data about the range of IP addresses included in the specific geolocation. This array includes startip, endip, startipnum, and endipnum.                                                                                                                                                                                 |
| ipblocks.startip    | The starting IP address for the range of IP addresses included in the specific geolocation.                                                                                                                                                                                                                                          |
| ipblocks.endip      | The ending IP address for the range of IP addresses included in the specific geolocation.                                                                                                                                                                                                                                            |
| ipblocks.startipnum | The integer ID number for the starting IP address (startip) from the geolocation data provider.                                                                                                                                                                                                                                      |
| ipblocks.endipnum   | The integer ID number for the ending IP address (endip) from the geolocation data provider.                                                                                                                                                                                                                                          |

### Response in JSON

```json
{
  "locid": 67890,
  "country": "US",
  "region": "MA",
  "city": "Boston",
  "postalcode": "02108",
  "latitude": 42.3578,
  "longitude": -71.0618,
  "dmacode": "506",
  "areacode": "617",
  "ipblocks": [
    {
      "startip": "8.8.8.0",
      "endip": "8.8.8.255",
      "startipnum": 134744064,
      "endipnum": 134744319
    },
    {
      "startip": "73.159.0.0",
      "endip": "73.159.255.255",
      "startipnum": 1234560000,
      "endipnum": 1234625535
    }
  ]
}
```

## <span class="post">POST</span> a custom geolocation

This endpoint enables you to create a custom geolocation.

### Request Syntax

```
POST /api/geolocations
```

### Body Parameters

| Parameter  | Description                                          | Type   | Required? |
| ---------- | ---------------------------------------------------- | ------ | --------- |
| country    | The country (by country code) for the geolocation.   | string | Required  |
| region     | The region (by region code) for the geolocation.     | string | Required  |
| city       | The name of the city for the geolocation.            | string | Required  |
| postalcode | The postal code for the geolocation.                 | string | Required  |
| latitude   | The latitude for the geolocation.                    | float  | Required  |
| longitude  | The longitude for the geolocation.                   | float  | Required  |
| dmacode    | The Designated Market Area code for the geolocation. | string | Required  |
| areacode   | The telephone area code for the geolocation.         | string | Required  |

### Request Body in JSON

```json
{
  "country": "US",
  "region": "MA",
  "city": "Boston",
  "postalcode": "02108",
  "latitude": "42.3578",
  "longitude": "-71.0618",
  "dmacode": "506",
  "areacode": "617"
}
```

### Response Attributes

| Element | Description                                                                                                                                                                                                                                                            |
| ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| locid   | The specific ID that the application assigns to the custom geolocation you just created.<br/><br/>**NOTE:** This locid will be a negative value (for example, -1) to represent that it is a custom-defined location. You can edit and delete custom-defined locations. |

### Response in JSON

```json
{"Success: Location with locid=-3 created."}
```

## <span class="post">POST</span> a custom IP address block

This endpoint enables you to create a custom IP address block within a custom geolocation.

### Request Syntax

```
POST /api/geolocations/{locid}/ipblocks
```

### Query Parameters

| Parameter | Description                                                                                                                                                                                                                                                                                                    | Type   | Required? |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | --------- |
| locid     | Your post syntax must include the specific ID for your custom geolocation in which you want to create a custom IP address block.<br/><br/>**NOTE:** This locid will be a negative value (for example, -1) to represent that it is a custom-defined location. You can edit and delete custom-defined locations. | string | Required  |

### Body Parameters

| Parameter | Description                                                   | Type   | Required? |
| --------- | ------------------------------------------------------------- | ------ | --------- |
| startip   | Add the starting IP address for your custom IP address block. | string | Required  |
| endip     | Add the ending IP address for your custom IP address block.   | string | Required  |

### Request Body in JSON

```json
{
  "startip": "8.8.8.0",
  "endip": "8.8.8.255"
}
```

### Response Attributes

| Element | Description                                                                       |
| ------- | --------------------------------------------------------------------------------- |
| locid   | The specific ID for the custom geolocation in which you just created an IP block. |

### Response in JSON

```json
{"Success: IPBlock with locid=-5 created."}
```

## <span class="put">PUT</span> an update to a custom geolocation

This endpoint enables you to update one of your custom geolocations.

### Request Syntax

```
PUT /api/geolocations/{locid}
```

### Query Parameters

| Parameter | Description                                                                                                                                                                                                                                                                    | Type   | Required? |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------ | --------- |
| locid     | Your put syntax must include the specific ID for the custom geolocation that you want to update.<br/><br/>**NOTE:** This locid will be a negative value (for example, -1) to represent that it is a custom-defined location. You can edit and delete custom-defined locations. | string | Required  |

### Body Parameters

| Parameter  | Description                                                 | Type   | Required? |
| ---------- | ----------------------------------------------------------- | ------ | --------- |
| country    | Update the country (by country code) for the geolocation.   | string | Required  |
| region     | Update the region (by region code) for the geolocation.     | string | Required  |
| city       | Update the name of the city for the geolocation.            | string | Required  |
| postalcode | Update the postal code for the geolocation.                 | string | Required  |
| latitude   | Update the latitude for the geolocation.                    | float  | Required  |
| longitude  | Update the longitude for the geolocation.                   | float  | Required  |
| dmacode    | Update the Designated Market Area code for the geolocation. | string | Required  |
| areacode   | Update the telephone area code for the geolocation.         | string | Required  |

### Request Body in JSON

```json
{
  "country": "US",
  "region": "MA",
  "city": "Boston",
  "postalcode": "02108",
  "latitude": "42.3578",
  "longitude": "-71.0618",
  "dmacode": "506",
  "areacode": "617"
}
```

### Response Attributes

| Element | Description                                           |
| ------- | ----------------------------------------------------- |
| locid   | The specific ID for the geolocation that you updated. |

### Response in JSON

```json
{"Success: Custom location with locid=-5 updated."}
```

## <span class="put">PUT</span> an update to a custom IP block

This endpoint enables you to update one of your custom IP address blocks.

### Request Syntax

```
PUT /api/geolocations/{locid}/ipblocks
```

### Query Parameters

| Parameter | Description                                                                                                                                                                                                                                                                                           | Type   | Required? |
| --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | --------- |
| locid     | Your put syntax must include the specific ID for your custom geolocation in which you want to update a custom IP block.<br/><br/>**NOTE:** This locid will be a negative value (for example, -1) to represent that it is a custom-defined location. You can edit and delete custom-defined locations. | string | Required  |

### Body Parameters

| Parameter  | Description                                                                   | Type   | Required? |
| ---------- | ----------------------------------------------------------------------------- | ------ | --------- |
| newstartip | Add a new starting IP address that will replace the startip that you specify. | string | Required  |
| newendip   | Add a new ending IP address that will replace the endip that you specify.     | string | Required  |
| startip    | Add the starting IP address that you want to replace with your newstartip.    | string | Required  |
| endip      | Add the ending IP address that you want to replace with your newendip.        | string | Required  |

### Request Body in JSON

```json
{
  "newstartip": "1.1.1.1",
  "newendip": "2.2.2.2",
  "startip": "3.3.3.3",
  "endip": "4.4.4.4"
}
```

### Response in JSON

```json
{"Success: IPBlock updated."}
```

## <span class="delete">DELETE</span> a custom geolocation

This endpoint enables you to delete one of your custom geolocations.

### Request Syntax

```
DELETE /api/geolocations/{locid}
```

### Query Parameters

| Parameter | Description                                                                                                                                                                                                                                                                        | Type   | Required? |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | --------- |
| locid     | Your delete syntax must include the specific ID for your custom geolocation that you want to delete.<br/><br/>**NOTE:** This locid will be a negative value (for example, -1) to represent that it is a custom-defined location. You can edit and delete custom-defined locations. | string | Required  |

### Response Attributes

| Element | Description                                           |
| ------- | ----------------------------------------------------- |
| locid   | The specific ID for the geolocation that you deleted. |

### Response in JSON

```json
{"Success: Location with locid=-5 deleted."}
```

## <span class="delete">DELETE</span> custom IP blocks

This endpoint enables you to delete your custom IP address blocks from one of your custom geolocations. This deletes every IP address block in the geolocation you specify.

### Request Syntax

```
DELETE /api/geolocations/{locid}/ipblocks
```

### Query Parameters

| Parameter | Description                                                                                                                                                                                                                                                                                                          | Type   | Required? |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | --------- |
| locid     | Your delete syntax must include the specific ID for your custom geolocation in which you want to delete your custom IP address blocks.<br/><br/>**NOTE:** This locid will be a negative value (for example, -1) to represent that it is a custom-defined location. You can edit and delete custom-defined locations. | string | Required  |

### Response Attributes

| Element                       | Description                                                                                          |
| ----------------------------- | ---------------------------------------------------------------------------------------------------- |
| totalMatchingBlocks           | This is the total number of IP blocks from your specified location that matched your delete request. |
| customerDefinedMatchingBlocks | This is the number of IP blocks that were successfully deleted.                                      |

### Response in JSON

```json
{
  "Success: Total matching IP blocks={totalMatchingBlocks},
  Customer defined matching blocks(deleted)=
  {customerDefinedMatchingBlocks}."
}
```
