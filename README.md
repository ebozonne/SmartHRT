# SmartHRT
Smart Heating Recovery Time calculation for Home Assistant

## Concept
> Why I need it ?
Thermal mass of my radiators and walls are good for thermal comfort but hard to predict for heating recovery time in the morning.
Without prediction of the correct time to start heating at night, it is either too late (and too cold) or too soon (heating living room for nothing while sleeping).

The principle is quite simple, while sleeping heating can be completely turned off, and boosted at the correct `recovery time`.

<img src="img/SmartHeatingRecoveryTime_principle.png" alt="Alt Text" style="width:50%; height:auto;">

The difficulty is to identify `recovery time`, neither too soon (money, emitted CO2 and kWh issue), nor too late (thermal comfort issue). This problem has:

- 2 variables (interior & exterior temperatures, `Tint` & `Text`)
- 1 objective (set point temperature `Tsp` at wake up time `target_hour`)

## Interface

<img src="img/SmartHRT_dashboard.png" alt="Alt Text" style="width:35%; height:auto;">

- `CONFIGURATION` panel (4 inputs are required):
  - interior temperature sensor name: a simple `name` (by default my own indoor temperature sensor name -> `sensor.salon_temperature`)
  - time of heating interruption (`recoverycalc_hour`) - e.g. 11PM
  - wake-up time (`target_hour`) - e.g. 6AM
  - target temperature (`tsp`) - e.g. 20°C
  Default adaptive mode (can be adjusted or deactivated for further manual tuning -> see advanced mode):
  - `Self Calibration` (activated by default) which allows to compute coefficients of your room after each recovery cycle
- `TEST`panel: run the calculation script to check the predicted `recovery time`. The script tries to predict `Tint` decrease before `recovery time` from room constants.
- `OUTPUTS` panel:
  - predicted `recovery time`
  - interior temperature graph
 
## Installation

### Requirements
- For script and automation package:
  - Set up a package folder if not already done (see https://www.home-assistant.io/docs/configuration/packages/)
- For dashboard interface:
  - [HACS](https://www.home-assistant.io/blog/2024/08/21/hacs-the-best-way-to-share-community-made-projects/#how-to-install)
  - [Lovelace-Mushroom](https://github.com/piitaya/lovelace-mushroom)
  - [Bubble-Card](https://github.com/Clooos/Bubble-Card)
  - [Mini-Graph-Card](https://github.com/kalkih/mini-graph-card)
- A very simple calendar, timetable, and/or automation for heating during the week in order to now the interruption time at night (and to set correctly the `recoverycalc_hour`)

### Download and install
- Download [thermal_recovery_time.yaml](packages/thermal_recovery_time.yaml) and copy the file to your package folder
- Restart HA
- Download [dashboard_base.yaml](dashboard_base.yaml)
  - open the file with notepad
  - in your Home Assistant dashboard, click on the 3 dots upside right and add a tab
  - on the new blank tab click on edit > 3 dots upside right, and select YAML edition
  - copy + paste the content of `dashboard_base.yaml` -> save.
- Adding the calculated recovery time as a condition to start the radiators (amongst the other calendar rules) in your own automation

e.g. I have this automation to manage my radiatiors

...img of automation...

### Testing and using the recovery optimization
- Configure the 4 required inputs
- Test the script for the calculation of `recovery time`
- See next morning if everything went well (the `Self Calibration` requires few days to adapt `RCth`and `RPth`constants to get better predictions, first night cannot be really correct or would need a manual adjustment, see advanced mode)
