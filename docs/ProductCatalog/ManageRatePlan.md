# Manage Rate Plan

This API enables you to manage rate plans in the SynXis CR for a specified hotel.

## What is it?

Robust descriptive information can be retrieved from the SynXis Platform using the Hotel Details or Hotel List methods.

- Hotel Details returns content for a single hotel.
- Hotel List returns content for multiple hotels.

## Why use it?

Use the Hotel Details or Hotel List methods during the shopping and booking process to inform a guest about the detailed information about a hotel and answer questions they may have about the property.

## How to use it

The responses between the two methods are identical with the only exception is the number of hotels.

### Hotel Details

<span class="get">GET</span> https://[environment]/v1/api/hotel/{{hotel_Id}}/details Search for a single hotel.

### Hotel List

<span class="get">GET</span> https://[environment]/v1/api/hotel/list Search for multiple hotels using one of these methods:

- Address - city, state, postal code, country
- Destination - destination id that is defined in SynXis Control Center
- Id - one or more hotel ids
- Amenity - standalone search method or used in combination with any of the above options

### Request Additional Content

Additional Information can be returned in the response using the parameter @include and setting any of the supported values:

- Attributes: Return a list of attributes for the hotel.
- ChannelProperties: Returns Channel properties of the hotel.
- ContactInfo: Return the contact information for the hotel.
- Currency: Return the default currency for the hotel.
- DiningOptions: Return a list of dining options for the hotel.
- Features: Return a list of property features for the hotel.

## Create or update a rate or group rate

Use the following to create or update a rate or group rate:

<span class="post">POST</span> /admin/product/ratePlans

```
  {
    "example": "example",
    "example": "example",
    "example": "example",
    "example": "example",
    "example": "example",
    "example": "example",
  }
```

<swagger-ui src="../manage_rate_plan_10_35.json" />
