# AMQP API

This document specifies the messages expected by the connector through a publish/subscriber middleware that supports the AMPQ protocol. These messages can be listed in two main categories:

### Southbound traffic (commands):

- Update data
- Request data

### Northbound traffic (control, measurements):

- Register thing
- Unregister thing
- Update thing's schema
- Publish thing's data

## Message types (Southbound)

### Update data

Updates a thing's sensor value.

#### Exchange

* `fog`

#### Binding Key

* `data.update`

#### Data

JSON in the following format:
  * `id` **String** thing's ID
  * `data` **Array (Object)** updates for sensors/actuators, each one formed by:
    * `sensorId` **Number** ID of the sensor to update
    * `data` **Number|Boolean|String** data to be written

#### Example

```json
{
  "id": "fbe64efa6c7f717e",
  "data": [{
      "sensorId": 1,
      "data": true
  }]
}
```

### Request data

Request a thing's sensor value.

#### Exchange

* `fog`

#### Binding Key

* `data.request`

#### Data

JSON in the following format:
  * `id` **String** thing's ID
  * `sensorIds` **Array (Number)** IDs of the sensor to send last value

#### Example

```json
{
  "id": "fbe64efa6c7f717e",
  "data": [1]
}
```

<!-- #### Response

Replies with `ready` when successful or `error` otherwise.

#### Example

```json
{
  "type": "identity",
  "data": {
    "id": "78159106-41ca-4022-95e8-2511695ce64c",
    "token": "d5265dbc4576a88f8654a8fc2c4d46a6d7b85574"
  }
}
``` -->

## Message types (Northbound)

### Register thing

Registers a thing on the cloud.

#### Exchange

* `cloud`

#### Binding Key

* `device.register`

#### Data

JSON in the following format:
  * `id` **String** thing's ID
  * `name` **String** thing's name

#### Example

```json
{
  "id": "fbe64efa6c7f717e",
  "name": "KNoT Thing"
}
```

### Unregister thing

Unregisters a thing on the cloud.

#### Exchange

* `cloud`

#### Binding Key

* `device.unregister`

#### Data

JSON in the following format:
  * `id` **String** thing's ID

#### Example

```json
{
  "id": "fbe64efa6c7f717e"
}
```

### Update schema

Updates thing's schema on the cloud.

#### Exchange

* `cloud`

#### Binding Key

* `schema.update`

#### Data

JSON in the following format:
  * `id` **String** thing's ID
  * `schema` **Array** schema items, each one formed by:
    * `sensorId` **Number** sensor ID
    * `valueType` **Number** semantic value type (voltage, current, temperature, etc)
    * `unit` **Number** sensor unit (V, A, W, W, etc)
    * `typeId` **Number** data value type (boolean, integer, etc)
    * `name` **String** sensor name

#### Example

```json
{
  "id": "fbe64efa6c7f717e",
  "schema": [{
    "sensorId": 1,
    "valueType": 0xFFF1,
    "unit": 0,
    "typeId": 3,
    "name": "Door lock"
  }]
}
```

### Publish data

Publishes thing's data.

#### Exchange

* `cloud`

#### Binding Key

* `data.publish`

#### Data

JSON in the following format:
  * `id` **String** thing's ID
  * `data` **Array** data items to be published, each one formed by:
    * `sensorId` **Number** sensor ID
    * `value` **Number|Boolean|String** sensor value

#### Example

```json
{
  "id": "fbe64efa6c7f717e",
  "data": [
    {
      "sensorId": 1,
      "value": false
    },
    {
      "sensorId": 2,
      "value": 1000
    }
  ]
}
```