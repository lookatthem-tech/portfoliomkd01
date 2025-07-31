---
toc:
  depth: 2
---

# Sensors API

The sensors endpoints return information from your Acme sensors.

## <span class="get">GET</span> all sensors that activated (from master)

This endpoint gets a list of all sensors that activated in your environment (from the master).

### Request Syntax

```
GET /acme-api/sensors
```

### Query Parameters

| Parameter           | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                         | Type     | Required? |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | --------- |
| startTime           | Add this parameter if you want to filter the response to records within a start and end time frame. If you add a startTime, the best practice is to add an endTime.<br/>Here is an example:<br/>`acme-api/sensors?startTime=2025-06-01T00:00:00.0000000Z`                                                                                                                                                                                                           | dateTime | Optional  |
| endTime             | Add this parameter if you want to filter the response to records within a start and end time frame. If you add an endTime, the best practice is to add a startTime.<br/>Here is an example:<br/>`acme-api/sensors?endTime=2025-06-01T00:00:00.0000000Z`                                                                                                                                                                                                             | dateTime | Optional  |
| tag                 | Add this parameter to filter the response to return only the sensors with a specific metadata tag.<br/>Sensor metadata tags represent general categories into which sensors are placed (such as alarm, Windows, printer).<br/>If you enter a string that is part of multiple tags, the response will include all sensors with a tag that includes that string.<br/>Here is an example:<br/>`acme-api/sensors?tag={tagName}`                                         | string   | Optional  |
| includeDescriptions | Use True or False to specify whether the response will include the description of the sensor itself.<br/>Here is an example:<br/>`acme-api/sensors?includeDescriptions={bool}`                                                                                                                                                                                                                                                                                      | boolean  | Optional  |
| includePayloads     | Use True or False to specify whether to include sensor payloads (if the sensor supports them). Payloads are used to retrieve specific information about a sensor to supplement the sensor's result in the sensor's details and actions. A Payload is a data set containing rows.<br/>Here is an example:<br/>`acme-api/sensors?includePayloads={bool}`                                                                                                              | boolean  | Optional  |
| sensorId            | Add this parameter if you want to filter the response to return data about only one sensor (by its sensorId).<br/>The string you enter must match a sensorId exactly. Partial matches will not be returned.<br/>If you enter a sensorId and a sensorName, Acme applies AND logic to return only the sensor that has both the specified ID and the specified name.<br/>Here is an example:<br/>`acme-api/sensors?sensorId={sensorId}`                                | string   | Optional  |
| sensorName          | Add this parameter if you want to filter the response to return data about only one sensor (by its sensorName).<br/>If you enter a string that is part of multiple sensor names, the response will include all sensor names that include that string. For example, if you use status as the sensorName, the response will include sensors named Device Manager Status and Encryption Status.<br/>Here is an example:<br/>`acme-api/sensors?sensorName={sensorName}` | string   | Optional  |
| SensorCategory      | Add this parameter if you want to filter the response to return sensors from only one SensorCategory. See the Response Attributes for more details.<br/>Here is an example:<br/>`acme-api/sensors?SensorCategory={SensorCategory}`<br/>You must specify the exact category name to get results. Partial matches are not supported for this parameter. You can see the categories for sensors in Configure (under Sensor Configuration >                             | string   | Optional  |
| Visibility          | Add this parameter if you want to filter the response to return sensors that have a specific Visibility value. See the Response Attributes for more details.<br/>Here is an example:<br/>`acme-api/sensors?Visibility={Visibility}`<br/>You can specify one or more of the following, and your response will return sensors that include one or more of the visibilities that you specify:                                                                          | string   | Optional  |

### Response Attributes

| Element        | Description                                                                                                                                                                                                                                                                                                                       |
| -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| status         | Not used.                                                                                                                                                                                                                                                                                                                         |
| data           | An array of the items returned.                                                                                                                                                                                                                                                                                                   |
| SensorId       | The identification number of the sensor.                                                                                                                                                                                                                                                                                          |
| Name           | The common name of the sensor.                                                                                                                                                                                                                                                                                                    |
| Triggered      | True or False for whether the sensor has activated in the last 30 days.                                                                                                                                                                                                                                                           |
| TriggerCount   | The number of times the sensor has activated in the last 30 days.                                                                                                                                                                                                                                                                 |
| Severity       | A numerical representation of the importance of this sensor's activation. Lower numbers are less severe. Higher numbers are more severe. Scale is 1 to 9. For more information, see                                                                                                                                               |
| IsHidden       | Indicates where this sensor will be displayed and where it will be hidden:                                                                                                                                                                                                                                                        |
| ResultTime     | The time stamp when the sensor was activated for this particular instance.                                                                                                                                                                                                                                                        |
| SystemId       | The unique identifier of the system that has this sensor and that it has been activated on.                                                                                                                                                                                                                                       |
| SensorCategory | SensorCategory is the category that Acme assigned to the sensor, such as Applications, Disk, or Security. You can see the categories for sensors in Configure (under Sensor Configuration >                                                                                                                                       |
| Visibility     | Visibility indicates the settings that are configured for the sensor in Configure (under Sensor Configuration > Therefore, you might see one or more of the following values for Visibility in the API response:<br/>If the sensor has no Visibility specified in Acme, the Visibility array will not appear in the API response. |

### Response in JSON

```json
{
  "status": [],
  "data": [
    {
      "SensorId": "5d305703-7ac8-47ec-98bd-28ca99709368",
      "Name": "CPU Usage High",
      "Triggered": true,
      "TriggerCount": 1,
      "Severity": 1,
      "IsHidden": 0,
      "ResultTime": "2025-05-17T00:00:00.0000000Z",
      "SystemId": "e2f222a2-222c-22e2-a22d-22d22d2ccea2",
      "SensorCategory": "CategoryName",
      "Visibility": ["self-help", "assist-detected"]
    }
  ]
}
```

## <span class="get">GET</span> all sensors that activated for a system (from master)

This endpoint gets a list of all sensors that activated for the specified system (from the master).

### Request Syntax

```
GET /acme-api/sensors/{systemId}
```

### Path Parameters

| Parameter | Description                                                                                               | Type   | Required? |
| --------- | --------------------------------------------------------------------------------------------------------- | ------ | --------- |
| systemId  | The unique identifier of the specified system. The system ID must be a globally unique identifier (GUID). | string | Required  |

### Query Parameters

| Parameter           | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Type     | Required? |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------- | --------- |
| startTime           | Add this parameter if you want to filter the response to records within a start and end time frame. If you add a startTime, the best practice is to add an endTime.<br/>Here is an example:<br/>`acme-api/sensors/{systemId}?startTime=2025-06-01T00:00:00.0000000Z`                                                                                                                                                                                                           | dateTime | Optional  |
| endTime             | Add this parameter if you want to filter the response to records within a start and end time frame. If you add an endTime, the best practice is to add a startTime.<br/>Here is an example:<br/>`acme-api/sensors{systemId}?endTime=2025-06-01T00:00:00.0000000Z`                                                                                                                                                                                                              | dateTime | Optional  |
| tag                 | Add this parameter to filter the response to return only the sensors with a specific metadata tag.<br/>Sensor metadata tags represent general categories into which sensors are placed (such as alarm, Windows, printer).<br/>If you enter a string that is part of multiple tags, the response will include all sensors with a tag that includes that string.<br/>Here is an example:<br/>`acme-api/sensors/{systemId}?tag={tagName}`                                         | string   | Optional  |
| includeDescriptions | Use True or False to specify whether the response will include the description of the sensor itself.<br/>Here is an example:<br/>`acme-api/sensors/{systemId}?includeDescriptions={bool}`                                                                                                                                                                                                                                                                                      | boolean  | Optional  |
| includePayloads     | Use True or False to specify whether to include sensor payloads (if the sensor supports them). Payloads are used to retrieve specific information about a sensor to supplement the sensor's result in the sensor's details and actions. A Payload is a data set containing rows.<br/>Here is an example:<br/>`acme-api/sensors/{systemId}?includePayloads={bool}`                                                                                                              | boolean  | Optional  |
| sensorId            | Add this parameter if you want to filter the response to return data about only one sensor (by its sensorId).<br/>The string you enter must match a sensorId exactly. Partial matches will not be returned.<br/>If you enter a sensorId and a sensorName, Acme applies AND logic to return only the sensor that has both the specified ID and the specified name.<br/>Here is an example:<br/>`acme-api/sensors/{systemId}?sensorId={sensorId}`                                | string   | Optional  |
| sensorName          | Add this parameter if you want to filter the response to return data about only one sensor (by its sensorName).<br/>If you enter a string that is part of multiple sensor names, the response will include all sensor names that include that string. For example, if you use status as the sensorName, the response will include sensors named Device Manager Status and Encryption Status.<br/>Here is an example:<br/>`acme-api/sensors/{systemId}?sensorName={sensorName}` | string   | Optional  |
| SensorCategory      | Add this parameter if you want to filter the response to return sensors from only one SensorCategory. See the Response Attributes for more details.<br/>Here is an example:<br/>`acme-api/sensors/{systemId}?SensorCategory={SensorCategory}`<br/>You must specify the exact category name to get results. Partial matches are not supported for this parameter. You can see the categories for sensors in Configure (under Sensor Configuration >                             | string   | Optional  |
| Visibility          | Add this parameter if you want to filter the response to return sensors that have a specific Visibility value. See the Response Attributes for more details.<br/>Here is an example:<br/>`acme-api/sensors/{systemId}?Visibility={Visibility}`<br/>You can specify one or more of the following, and your response will return sensors that include one or more of the visibilities that you specify:                                                                          | string   | Optional  |

### Response Attributes

| Element        | Description                                                                                                                                                                                                                                                                                                                       |
| -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| status         | Not used.                                                                                                                                                                                                                                                                                                                         |
| data           | An array of the items returned.                                                                                                                                                                                                                                                                                                   |
| SensorId       | The identification number of the sensor.                                                                                                                                                                                                                                                                                          |
| Name           | The common name of the sensor.                                                                                                                                                                                                                                                                                                    |
| Triggered      | True or False for whether the sensor has activated in the last 30 days.                                                                                                                                                                                                                                                           |
| TriggerCount   | The number of times the sensor has activated in the last 30 days.                                                                                                                                                                                                                                                                 |
| Severity       | A numerical representation of the importance of this sensor's activation. Lower numbers are less severe. Higher numbers are more severe. Scale is 1 to 9. For more information, see                                                                                                                                               |
| IsHidden       | Indicates where this sensor will be displayed and where it will be hidden:                                                                                                                                                                                                                                                        |
| ResultTime     | The time stamp when the sensor was activated for this particular instance.                                                                                                                                                                                                                                                        |
| SensorCategory | SensorCategory is the category that Acme assigned to the sensor, such as Applications, Disk, or Security. You can see the categories for sensors in Configure (under Sensor Configuration >                                                                                                                                       |
| Visibility     | Visibility indicates the settings that are configured for the sensor in Configure (under Sensor Configuration > Therefore, you might see one or more of the following values for Visibility in the API response:<br/>If the sensor has no Visibility specified in Acme, the Visibility array will not appear in the API response. |

### Response in JSON

```json
{
  "status": [],
  "data": [
    {
      "SensorId": "5d305703-7ac8-47ec-98bd-28ca99709368",
      "Name": "CPU Usage High",
      "Triggered": true,
      "TriggerCount": 1,
      "Severity": 1,
      "IsHidden": 0,
      "ResultTime": "2025-05-17T00:00:00.0000000Z",
      "SensorCategory": "CategoryName",
      "Visibility": ["self-help", "assist-detected"]
    }
  ]
}
```

## <span class="get">GET</span> all sensors that activated for a system (from child)

This endpoint gets a list of all sensors that activated for the specified system (from the child).

### Request Syntax

```
GET /acme-api/sensors/child/{systemId}
```

### Path Parameters

| Parameter | Description                                                                                               | Type   | Required? |
| --------- | --------------------------------------------------------------------------------------------------------- | ------ | --------- |
| systemId  | The unique identifier of the specified system. The system ID must be a globally unique identifier (GUID). | string | Required  |

### Query Parameters

| Parameter           | Description                                                                                                                                                                                                                                                                                                                                                                                                                                  | Type     | Required? |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | --------- |
| startTime           | Add this parameter if you want to filter the response to records within a start and end time frame. If you add a startTime, the best practice is to add an endTime.<br/>Here is an example:<br/>`acme-api/sensors/child/{systemId}?startTime=2025-06-01T00:00:00.0000000Z`                                                                                                                                                                   | dateTime | Optional  |
| endTime             | Add this parameter if you want to filter the response to records within a start and end time frame. If you add an endTime, the best practice is to add a startTime.<br/>Here is an example:<br/>`acme-api/sensors/child/{systemId}?endTime=2025-06-01T00:00:00.0000000Z`                                                                                                                                                                     | dateTime | Optional  |
| tag                 | Add this parameter to filter the response to return only the sensors with a specific metadata tag.<br/>Sensor metadata tags represent general categories into which sensors are placed (such as alarm, Windows, printer).<br/>If you enter a string that is part of multiple tags, the response will include all sensors with a tag that includes that string.<br/>Here is an example:<br/>`acme-api/sensors/child/{systemId}?tag={tagName}` | string   | Optional  |
| includeDescriptions | Use True or False to specify whether the response will include the description of the sensor itself.<br/>Here is an example:<br/>`acme-api/sensors/child/{systemId}?includeDescriptions={bool}`                                                                                                                                                                                                                                              | boolean  | Optional  |
| includePayloads     | Use True or False to specify whether to include sensor payloads (if the sensor supports them). Payloads are used to retrieve specific information about a sensor to supplement the sensor's result in the sensor's details and actions. A Payload is a data set containing rows.<br/>Here is an example:<br/>`acme-api/sensors/child/{systemId}?includePayloads={bool}`                                                                      | boolean  | Optional  |
| SensorCategory      | Add this parameter if you want to filter the response to return sensors from only one SensorCategory. See the Response Attributes for more details.<br/>Here is an example:<br/>`acme-api/sensors/child/{systemId}?SensorCategory={SensorCategory}`                                                                                                                                                                                          | string   | Optional  |
| Visibility          | Add this parameter if you want to filter the response to return sensors that have a specific Visibility value. See the Response Attributes for more details.<br/>Here is an example:<br/>`acme-api/sensors/child/{systemId}?Visibility={Visibility}`<br/>You can specify one or more of the following, and your response will return sensors that include one or more of the visibilities that you specify:                                  | string   | Optional  |

### Response Attributes

| Element        | Description                                                                                                                                                                                                                                                                                                                       |
| -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| status         | Not used.                                                                                                                                                                                                                                                                                                                         |
| data           | An array of the items returned.                                                                                                                                                                                                                                                                                                   |
| SensorId       | The identification number of the sensor.                                                                                                                                                                                                                                                                                          |
| Name           | The common name of the sensor.                                                                                                                                                                                                                                                                                                    |
| Triggered      | True or False for whether the sensor has activated in the last 30 days.                                                                                                                                                                                                                                                           |
| TriggerCount   | The number of times the sensor has activated in the last 30 days.                                                                                                                                                                                                                                                                 |
| Severity       | A numerical representation of the importance of this sensor's activation. Lower numbers are less severe. Higher numbers are more severe. Scale is 1 to 9. For more information, see                                                                                                                                               |
| IsHidden       | Indicates whether or not this sensor is hidden in the Self Help app. The possible values are:                                                                                                                                                                                                                                     |
| ResultTime     | The time stamp when the sensor was activated for this particular instance.                                                                                                                                                                                                                                                        |
| SensorCategory | SensorCategory is the category that Acme assigned to the sensor, such as Applications, Disk, or Security. You can see the categories for sensors in Configure (under Sensor Configuration >                                                                                                                                       |
| Visibility     | Visibility indicates the settings that are configured for the sensor in Configure (under Sensor Configuration > Therefore, you might see one or more of the following values for Visibility in the API response:<br/>If the sensor has no Visibility specified in Acme, the Visibility array will not appear in the API response. |

### Response in JSON

```json
{
  "status": [],
  "data": [
    {
      "SensorId": "5d305703-7ac8-47ec-98bd-28ca99709368",
      "Name": "CPU Usage High",
      "Triggered": true,
      "TriggerCount": 1,
      "Severity": 1,
      "IsHidden": 0,
      "ResultTime": "2025-05-27T00:00:00.0000000Z"
    }
  ]
}
```

## <span class="get">GET</span> descriptions of sensors

This endpoint gets descriptions and other metadata about your sensors.

### Request Syntax

```
GET /acme-api/sensors/definitions
```

### Path Parameters

| Parameter              | Description | Type | Required? |
| ---------------------- | ----------- | ---- | --------- |
| None for this endpoint |             |      |           |

### Query Parameters

| Parameter  | Description                                                                                                                                                                         | Type   | Required? |
| ---------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | --------- |
| sensorName | Add this parameter if you want to filter the response to display the metadata for one specific sensor.<br/>Here is an example:<br/>`acme-api/sensors/definitions?sensorName={name}` | string | Optional  |

### Response Attributes

| Element      | Description                                                                                                                                                               |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| data         | An array of the items returned.                                                                                                                                           |
| sensorId     | The ID number that Acme assigned to the sensor.                                                                                                                           |
| sensorName   | The common name of the sensor.                                                                                                                                            |
| description  | The description of the sensor.                                                                                                                                            |
| category     | The category that Acme assigned to the sensor, such as Applications, Disk, or Security. You can see the categories for sensors in Configure (under Sensor Configuration > |
| instanceName | If the sensor includes a defined instance, this is the name of the instance for which the sensor was activated.                                                           |

### Response in JSON

```json
{
  "data": [
    {
      "sensorId": "1a4348eb-8d15-47bf-ac51-cd3e665533bb",
      "sensorName": "Acrobat - Hangs",
      "description": "Adobe Acrobat Hangs",
      "category": "UHG",
      "instanceName": ""
    },
    {
      "sensorId": "b7ff0dab-240c-4171-980e-61a8d20f49b9",
      "sensorName": "Active Transactions",
      "description": "The number of active transactions on the SQL server is observed to be high.",
      "category": "System",
      "instanceName": ""
    },
    {
      "sensorId": "f694a299-4d48-45d3-bee0-e32086aa16d2",
      "sensorName": "AD Password Expiration",
      "description": "Your AD password belongs to the primary Windows account that is used to login to your computer.",
      "category": "Security",
      "instanceName": ""
    }
  ]
}
```
