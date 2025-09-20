# ESPHome_Fuji_AC

## Hardware

Totaly inspired by FOSV [Fuji Atom Interface](https://github.com/FOSV/Fuji-Atom-Interface). Great project but I bring these improvements :
- Easier composants to weld (size >= 0603, no QFN nor LGA)
- More 'professional' schematic design : schematic more , and organized, TVS added, decoupling capacitors and datasheet recommandations added.
- More 'professional' layout design : use power plan, 4 layers stackup, and state-of-art routing rules (EMC, DFx rules,...)
- Add minors improvments : 12V/5V leds, testpoints, reverse polarity protection on 12V.

Design tools
- Altium Designer 25.8

## Software

This board can run Omniflux repo [esphome-fujitsu-halcyon](https://github.com/Omniflux/esphome-fujitsu-halcyon)

### ESPHome
This project runs under ESPHome 2025.7.
Not tested with newer version.

### YAML

```
substitutions:
  device_name: "fuji-controller"
  friendly_name: Halcyon Controller
  device_description: Atom Lite + FOSV
  esp_board: m5stack-atom

external_components:
  - source: github://Omniflux/esphome-tzsp
  - source: github://Omniflux/esphome-fujitsu-halcyon

packages:
  #wifi: !include common/wifi.yaml

esphome:
  name: ${device_name}
  friendly_name: ${friendly_name}
  comment: ${device_description}

esp32:
  board: ${esp_board}
  framework:
    type: esp-idf


#logger:
#  level: DEBUG

button:
  - platform: restart
    name: Restart
  - platform: safe_mode
    name: Restart (Safe Mode)

sensor:
- platform: uptime
  name: "Uptime"

uart:
  tx_pin: GPIO22  # Device dependent
  rx_pin: GPIO19  # Device dependent
  baud_rate: 500
  parity: EVEN

climate:
- platform: fujitsu-halcyon
  name: None  # Use device friendly_name

  # Fujitsu devices use 0 and 1, but 2-15 should also work. Must not skip addresses
  controller_address: 1  # 0=Primary, 1=Secondary
  #temperature_controller_address: 0  # Fujitsu controller address to read temperature from

  #temperature_sensor_id: my_temperature_sensor  # ESPHome sensor to read temperature from
  #humidity_sensor: my_humidity_sensor  # ESPHome sensor to read humidity from

  #ignore_lock: true  # Ignore child/part/feature lock set on unit or primary/central remote control

  # To capture communications for debugging / analysis
  # Use Wireshark with https://github.com/Omniflux/fujitsu-airstage-h-dissector
  #tzsp:
  #  ip: 192.168.1.20
  #  protocol: 255




# Enable logging
logger:

# Enable Home Assistant API
api:
  encryption:
    key: "xxx"

ota:
  - platform: esphome
    password: "xxx"

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

  # Enable fallback hotspot (captive portal) in case wifi connection fails
  ap:
    ssid: "Test-Halcyon Fallback Hotspot"
    password: "xxx"

captive_portal:
```


### Screenshot of interface 
![Screenshot of interface](/Documentation/HA_interface.png)


## Related projects
- FOSV's [Fuji-Atom-Interface](https://github.com/FOSV/Fuji-Atom-Interface)
- Omniflux repo [esphome-fujitsu-halcyon](https://github.com/Omniflux/esphome-fujitsu-halcyon)
<!-- -->
- Aaron Zhang's [esphome-fujitsu](https://github.com/FujiHeatPump/esphome-fujitsu)
- Jaroslaw Przybylowicz's [fuji-iot](https://github.com/jaroslawprzybylowicz/fuji-iot)
- Raal Goff's [FujiHeatPump](https://github.com/unreality/FujiHeatPump)
- Raal Goff's [FujiHK](https://github.com/unreality/FujiHK)
<!-- -->
- Myles Eftos's [Reverse engineering](https://hackaday.io/project/19473-reverse-engineering-a-fujitsu-air-conditioner-unit)