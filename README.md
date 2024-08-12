# Watering System based on NodeMCU ESP32 using ESPHOME and HomeAssistant
This is a simple watering system for gardens using as stated above, a NodeMCU, ESP32, ESPHome and Homeassistant.

The initial idea was tu use one ESP32 and a capacitive soil moisture sensor to get data abaout the soil and control three separate 5v relays that control three 24vAc water valves.
Due to the dificulty to wire the moisture sensor from the ESP32 to the soil I opted to a BLE Soil sensor like the [Xiaomi MiFlora](https://smarthomescene.com/reviews/xiaomi-miflora-plant-sensor-tuya-version-hhccjcy10-review/) and getting data using the Bluetooth Low Energy (BLE) [ESPHome tracker](https://esphome.io/components/bluetooth_proxy.html).

Later decided to add a BME280 Sensor, it reads humidity, temperature and atmosferic pressure and a rain sensor (Aquaflow DP13) that will help me get more data for the automation and also for another home automations I could use.

The automation is really simple, you can check the "rega_oliveiras.yaml"

# Parts Used 
(I will put links to the parts but im not associated with any of these brands or shops, its just to show the specific parts used)

- 1 NodeMCU ESP32 [this model](https://pt.aliexpress.com/item/1005005564949759.html?spm=a2g0o.order_list.order_list_main.39.62ffcaa4AviRG3&gatewayAdapt=glo2bra)
- 1 BME280 [the 3.3v model](https://pt.aliexpress.com/item/32862421810.html?spm=a2g0o.order_list.order_list_main.34.62ffcaa4AviRG3&gatewayAdapt=glo2bra)
- 1 Rain Sensor [this model](https://www.leroymerlin.pt/produtos/jardim/rega/programadores/sensor-de-chuva-dp13-aquaflow-16338805.html?src=clk), this sensor acts as a swith, when there is rain, small discs inflate and the circuit opens, when its dry the circuit keeps closed
- 1 ESP32 Terminal adapter, be carefull choosing the correct adapter, because there are some NodeMCU boards wider than others. [I used this model](https://www.amazon.es/dp/B0BCWBW4SR?psc=1&ref=ppx_yo2ov_dt_b_product_details)
- 3 relay modules like [this one](https://www.switchelectronics.co.uk/products/5v-1-channel-relay-board-module)
- Jumper wires
  
Later decided to design and order a PCB to make it look better and more "professional" but always in a way that parts would be placed without the less soldering possible to make it easier to replace parts, reflash the ESP32 etc.
  

# wiring diagram
Here is the wiring diagram for the setup:

![alt text](https://github.com/tmsaavedra/wateringsystem/blob/main/wiring.png)

# Code
There are some parts that you should adapt to your setup:
- Board type
- WIFI names
- WIFI passwords
- encription keys
- IP's
- the wait time on the relays to limit the time the valve is open to let water flow
- BME280 temp and humidty offsets

```yaml
esphome:
  name: rega
  platform: ESP32
  board: esp-wrover-kit

# Enable logging
logger:

esp32_ble_tracker:
  scan_parameters:
    interval: 1100ms
    window: 1100ms
    active: true #false

bluetooth_proxy:
  active: true
# Enable Home Assistant API
api:
  encryption:
    key: "yourEncriptionkey"
ota:
  password: "yourOTApassword"

wifi:
  networks:
  - ssid: "WIFI1"
    password: "WIFIPASSWORD"
  - ssid:  "WIFI2"
    password: "WIFIPASSWORD"
  manual_ip:
    static_ip: 192.168.88.46
    gateway: 192.168.88.1
    subnet: 255.255.255.0

  # Enable fallback hotspot (captive portal) in case wifi connection fails
  ap:
    ssid: "Esp32Tests Fallback Hotspot"
    password: "mh182IsHSf15"

captive_portal:

i2c:
    sda: GPIO22
    scl: GPIO23
    scan: true
   
sensor:
  - platform: bme280
    temperature:
      name: "BME280 Temperature"
      filters:
        - offset: -1.0
      oversampling: 16x
    pressure:
      name: "BME280 Pressure"
    humidity:
      name: "BME280 Humidity"
      filters:
        - offset: 11.5
    address: 0x76
    update_interval: 60s

switch:
  - platform: gpio
    name: "Pipe1"
    pin: GPIO15
    id: relay1
    icon: "mdi:gate"
    restore_mode: ALWAYS_OFF
    on_turn_on:
      - delay: 30s
      - switch.turn_off: relay1

  - platform: gpio
    name: "Pipe2"
    pin: GPIO2
    id: relay2
    icon: "mdi:gate"
    restore_mode: ALWAYS_OFF
    on_turn_on:
      - delay: 30s
      - switch.turn_off: relay2

  - platform: gpio
    name: "Pipe3"
    pin: GPIO4
    id: relay3
    icon: "mdi:gate"
    restore_mode: ALWAYS_OFF
    on_turn_on:
      - delay: 30s
      - switch.turn_off: relay3   

     
binary_sensor:
  - platform: gpio
    pin:
      number: 1
      inverted: false
      mode:
        input: true
        pullup: true
    name: "RainSensor"
    filters:
      - delayed_on: 10ms
```
# Final Result:
- Board with the esp32
![alt text](https://github.com/tmsaavedra/wateringsystem/blob/main/IMG_2018.JPG)

- 3 water valves and the esp32
![alt text](https://github.com/tmsaavedra/wateringsystem/blob/main/IMG_2019.JPG)

- Rain sensor and the BME280 inside the 3d printed closure.
![alt text](https://github.com/tmsaavedra/wateringsystem/blob/main/IMG_2020.JPG)
 
# Next Steps:
- Add rain detector [something like this](https://github.com/hugokernel/esphome-rain-detector) since its more precise when it is raining or not. The actual one keeps "on" for rain until the inner parts dry, its good to simulate wet earth.
