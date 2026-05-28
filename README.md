# homeassistant_nullautomation
This Home Assistant automation is a zero-export power control for a balcony solar setup. It monitors the household grid power flow and adjusts the output of a home battery (in my setup a Marstek B2500) to minimize both grid import and export.


## Requirements / Preparation

Before using this automation, the following components and entities must already exist and work correctly in Home Assistant:

### 1. Smart meter + power sensor

A power meter with real-time import/export values is required.

Required entity:

* `sensor.put_your_power_sensor_entity_here`

Additionally, a smoothed version of the power value is recommended:

* `sensor.smoothed_version_of_the_power_value`

The smoothed sensor should average the last few seconds (e.g. 5–10 s) to reduce noise and fast fluctuations.



### 2. Controllable inverter or battery system

The inverter / battery output power must be writable from Home Assistant.

Required entity:

* `number.battery_output_power`

The entity must support:

* `number.set_value`

The automation dynamically changes this value to regulate export/import power.



### 3. Integral helper (`input_number`)

The PI controller stores its integral state in a helper entity.

Create this helper in Home Assistant:

* Type: `input_number`
* Entity:

  * `input_number.balkon_pi_integral`

Recommended settings:

* Min: `-1000`
* Max: `1000`
* Step: `1`



### 4. Correct sign convention

The automation assumes:

* positive values (`netz > 0`) = grid import
* negative values (`netz < 0`) = grid export

If your meter uses the opposite convention, the control logic must be inverted.



### 5. Initial tuning values

The included values are optimized for a typical balcony solar + battery setup:

* `deadband: 15`
* `p_faktor: 0.6`
* `i_faktor: 0.01`
* `integral_limit: 400`
* `max_step: 150`
* `max_power: your maximum input power value, e.g. 800`
* `min_power: 0`
* `cooldown: 5`


Depending on:

* inverter reaction speed,
* household load dynamics,
* and sensor latency,

additional tuning is necessary.

