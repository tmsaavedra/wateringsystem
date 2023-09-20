# Watering System based on NodeMCU ESP32 using ESPHOME and HomeAssistant
This is a simple watering system for gardens using as stated above, a NodeMCU, ESP32, ESPHome and Homeassistant.

The initial idea was tu use one ESP32 and a capacitive soil moisture sensor to get data abaout the soil and control three separate 5v relays that control three 24vAc water valves.
Due to the dificulty to wire the moisture sensor from the ESP32 to the soil I opted to a BLE Soil sensor like the [Xiaomi MiFlora](https://smarthomescene.com/reviews/xiaomi-miflora-plant-sensor-tuya-version-hhccjcy10-review/) and getting data using the Bluetooth Low Energy (BLE) [ESPHome tracker](https://esphome.io/components/bluetooth_proxy.html).

Later decided to add a BME280 Sensor, it reads humidity, temperature and atmosferic pressure and a rain sensor (Aquaflow DP13) that will help me get more data for the automation and also for another home automations I could use.

# Parts Used 
(I will put links to the parts but im not associated with any of these brands or shops, its just to show the specific parts used)

- 1 NodeMCU ESP32 [this model](https://pt.aliexpress.com/item/1005005564949759.html?spm=a2g0o.order_list.order_list_main.39.62ffcaa4AviRG3&gatewayAdapt=glo2bra)
- 1 BME280 [the 3.3v model](https://pt.aliexpress.com/item/32862421810.html?spm=a2g0o.order_list.order_list_main.34.62ffcaa4AviRG3&gatewayAdapt=glo2bra)
- 1 Rain Sensor [this model](https://www.leroymerlin.pt/produtos/jardim/rega/programadores/sensor-de-chuva-dp13-aquaflow-16338805.html?src=clk)
- 1 ESP32 Terminal adapter, be carefull choosing the correct adapter, because there are some NodeMCU boards wider than others. [I used this model](https://www.amazon.es/dp/B0BCWBW4SR?psc=1&ref=ppx_yo2ov_dt_b_product_details)
- 3 relay modules like [this one](https://www.switchelectronics.co.uk/products/5v-1-channel-relay-board-module)
- jumper wires
  

# wiring diagrams

here will be the wiring diagrams

# code

here will be the code
