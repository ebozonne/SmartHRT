# SmartHRT
Smart Heating Recovery Time calculation for Home Assistant

## Concept
> Why do I need it?
The thermal mass of my radiators and walls provides good thermal comfort but makes it hard to predict the heating recovery time in the morning.
Without predicting the correct time to start heating at night, it is either too late (and too cold) or too soon (heating the living room unnecessarily while sleeping).

The principle is simple: while sleeping, heating can be completely turned off and boosted at the correct `recovery time`.

<img src="img/SmartHeatingRecoveryTime_principle.png" alt="Alt Text" style="width:50%; height:auto;">

The challenge is to identify the `recovery time`, neither too soon (to avoid wasting money, emitting CO2, and using excess kWh), nor too late (to obtain thermal comfort). This problem involves:

- 2 variables: interior & exterior temperatures (`Tint` & `Text`)
- 1 objective: set point temperature `Tsp` at wake up time `target_hour`

## Interface

<img src="img/SmartHRT_dashboard.png" alt="Alt Text" style="width:35%; height:auto;">

- `CONFIGURATION` panel (4 inputs required):
  - interior temperature `sensor name` (default: my own indoor sensor -> `sensor.salon_temperature`)
  - time of heating interruption (`recoverycalc_hour`) - e.g. 11PM
  - wake-up time (`target_hour`) - e.g. 6AM
  - target temperature (`tsp`) - e.g. 20°C
  Default adaptive mode (can be adjusted or deactivated for further manual tuning -> see advanced mode):
  - `Self Calibration` (activated by default) which computes coefficients for your own room after each recovery cycle
- `TEST`panel: run the calculation script to check the predicted `recovery time`. The script predicts the decrease in `Tint` before `recovery time` based on room constants
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
- A simple calendar, timetable, and/or automation for heating during the week to know the interruption time at night (and to set the `recoverycalc_hour` correctly)

### Download and install
- Download [thermal_recovery_time.yaml](packages/thermal_recovery_time.yaml) and copy the file to your package folder
- Restart Home Assistant
- Download [dashboard_base.yaml](dashboard_base.yaml)
  - open the file with a text editor
  - in your Home Assistant dashboard, click on the 3 dots in the upper right corner and add a tab
  - on the new blank tab, click on edit > 3 dots in the upper right corner > select YAML edition
  - copy and paste the content of `dashboard_base.yaml` -> save.
- Add the calculated recovery time (`input_datetime.recoverystart_hour`) as a condition to start the radiators (amongst the other calendar rules) in your own automation

e.g. I have this automation to manage my radiatiors

<img src="img/HeatingAutomationExample.png" alt="Alt Text" style="width:35%; height:auto;">

## Testing and using the recovery optimization
- Configure the 4 required inputs
- Test the script for the calculation of `recovery time`
- Check next morning if everything went well - The `Self Calibration` requires a few days to adapt `RCth`and `RPth`constants for better predictions; the first night may not be accurate. Following days should give better predictions with self calibration mode (further manual adjustments are always possible in advanced mode)
