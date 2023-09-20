# Watering System based on NodeMCU ESP32 using ESPHOME and HomeAssistant
This is a simple watering system for gardens using as stated above, a NodeMCU, ESP32, ESPHome and Homeassistant.

The initial idea was tu use one ESP32 and a capacitive soil moisture sensor to get data abaout the soil and control three separate 5v relays that control three 24vAc water valves.
Due to the dificulty to wire the moisture sensor from the ESP32 to the soil I opted to a BLE Soil sensor like the [Xiaomi MiFlora](https://smarthomescene.com/reviews/xiaomi-miflora-plant-sensor-tuya-version-hhccjcy10-review/) and getting data using the Bluetooth Low Energy (BLE) [ESPHome tracker](https://esphome.io/components/bluetooth_proxy.html).


