# FAQ

## How to set phone next_alarm as target time ?
- First activate next alarm sensor in HA (https://companion.home-assistant.io/docs/core/sensors/#sensor-list):
  - Navigate to [Settings](https://my.home-assistant.io/redirect/config/) > Companion App > Manage Sensors
  - By default, most are disabled -> activate in the list sensor.yourphone_next_alarm
   <img src="img/smartHRT_nextAlarm.png" alt="Alt Text" style="width:50%; height:auto;">
- Copy/paste the name of the sensor (e.g. `sensor.clt_l29_next_alarm`) in the [smartHRT dashboard](https://github.com/ebozonne/SmartHRT?tab=readme-ov-file#interface)
