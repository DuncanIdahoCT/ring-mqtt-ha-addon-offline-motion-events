```
type: grid
square: false
columns: 1
cards:
  - type: entities
    entities:
      - entity: sensor.driveway_info
        name: Info
      - entity: binary_sensor.driveway_motion
        name: Motion
      - entity: sensor.driveway_wireless
        name: Wireless
    title: Driveway
  - card:
      type: iframe
      url: >-
        ${'/local/driveway_latest_event.html?v='+states['input_text.driveway_timestamp'].state}
    entities:
      - input_text.driveway_timestamp
    type: custom:config-template-card
```
