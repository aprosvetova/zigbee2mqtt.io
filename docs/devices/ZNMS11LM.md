---
title: "Xiaomi ZNMS11LM control via MQTT"
description: "Integrate your Xiaomi ZNMS11LM via Zigbee2MQTT with whatever smart home
 infrastructure you are using without the vendors bridge or gateway."
---

*To contribute to this page, edit the following
[file](https://github.com/Koenkk/zigbee2mqtt.io/blob/master/docs/devices/ZNMS11LM.md)*

# Xiaomi ZNMS11LM

| Model | ZNMS11LM  |
| Vendor  | Xiaomi  |
| Description | Xiaomi Aqara smart lock |
| Exposes | state, reverse, action, linkquality |
| Picture | ![Xiaomi ZNMS11LM](../images/devices/ZNMS11LM.jpg) |

## Notes

### Device type specific configuration
*[How to use device type specific configuration](../information/configuration.md)*

* `legacy`: Set to `false` to disable the legacy integration (highly recommended!) (default: true)


## OTA updates
This device supports OTA updates, for more information see [OTA updates](../information/ota_updates.md).


## Exposes

### State (binary)
Value can be found in the published state on the `state` property.
It's not possible to read (`/get`) or write (`/set`) this value.
If value equals `UNLOCK` state is ON, if `LOCK` OFF.

### Reverse (binary)
Value can be found in the published state on the `reverse` property.
It's not possible to read (`/get`) or write (`/set`) this value.
If value equals `UNLOCK` reverse is ON, if `LOCK` OFF.

### Action (enum)
Value can be found in the published state on the `action` property.
It's not possible to read (`/get`) or write (`/set`) this value.
The possible values are: `finger_not_match`, `password_not_match`, `reverse_lock`, `reverse_lock_cancel`, `locked`, `lock_opened`, `finger_add`, `finger_delete`, `password_add`, `password_delete`, `lock_opened_inside`, `lock_opened_outside`, `ring_bell`, `change_language_to`, `finger_open`, `password_open`, `door_closed`.

### Linkquality (numeric)
Link quality (signal strength).
Value can be found in the published state on the `linkquality` property.
It's not possible to read (`/get`) or write (`/set`) this value.
The minimal value is `0` and the maximum value is `255`.
The unit of this value is `lqi`.

## Manual Home Assistant configuration
Although Home Assistant integration through [MQTT discovery](../integration/home_assistant) is preferred,
manual integration is possible with the following configuration:


{% raw %}
```yaml
binary_sensor:
  - platform: "mqtt"
    state_topic: "zigbee2mqtt/<FRIENDLY_NAME>"
    availability_topic: "zigbee2mqtt/bridge/state"
    value_template: "{{ value_json.state }}"
    payload_on: "UNLOCK"
    payload_off: "LOCK"

binary_sensor:
  - platform: "mqtt"
    state_topic: "zigbee2mqtt/<FRIENDLY_NAME>"
    availability_topic: "zigbee2mqtt/bridge/state"
    value_template: "{{ value_json.reverse }}"
    payload_on: "UNLOCK"
    payload_off: "LOCK"

sensor:
  - platform: "mqtt"
    state_topic: "zigbee2mqtt/<FRIENDLY_NAME>"
    availability_topic: "zigbee2mqtt/bridge/state"
    value_template: "{{ value_json.action }}"
    icon: "mdi:gesture-double-tap"

sensor:
  - platform: "mqtt"
    state_topic: "zigbee2mqtt/<FRIENDLY_NAME>"
    availability_topic: "zigbee2mqtt/bridge/state"
    value_template: "{{ value_json.linkquality }}"
    unit_of_measurement: "lqi"
    icon: "mdi:signal"

sensor:
  - platform: "mqtt"
    state_topic: "zigbee2mqtt/<FRIENDLY_NAME>"
    availability_topic: "zigbee2mqtt/bridge/state"
    icon: "mdi:update"
    value_template: "{{ value_json['update']['state'] }}"

binary_sensor:
  - platform: "mqtt"
    state_topic: "zigbee2mqtt/<FRIENDLY_NAME>"
    availability_topic: "zigbee2mqtt/bridge/state"
    payload_on: true
    payload_off: false
    value_template: "{{ value_json.update_available}}"
```
{% endraw %}


