This section contain all you need to change how the aux fan work on Elegoo Neptune 4 Max (and i think Neptune 4 Plus).

If you want to see the processors (host and MCU) temperature, you can add this in the config.cfg :
[temperature_sensor RockchipHost]
sensor_type: temperature_host
min_temp: 10
max_temp: 80

[temperature_sensor STM32MCU]
sensor_type: temperature_mcu
min_temp: 10
max_temp: 85
