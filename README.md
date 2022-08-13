# ESP32 Plant waterer, (ESPHome managed device with Home Assistant automation)

This is a simple low-cost automated plant watering system, designed to work with Home Assistant based on ESP32 configuration and Over-The-Air (OTA) updates and management from the ESPHome plugin.

The setup uses MQTT protocol to publish sensor data allowing Home Assistant to monitor and trigger watering automations based on automation patterns described in this setup.

The setup includes the following instructions and (working) code;

- ESPHome code that needs to be flashed onto your ESP32 device
- Home Assistant Dashboard YAML
- Home Assistant watering automation YAML
- historical\_stat sensor YAML (acts as failsafe for watering automation)
- Home Assistant Error & Alerting automation YAML

The project and code is built around low cost M Stack Atom Lite, a low cost minature ESP32-pico based board. I originally tested this with smaller ESP-32 pico based boards, however most of these do not have embedded USB ports so an additional USB/ TTL convertor is required to initially flash these devices and potentially a separate power regulator board would be required.

<img
  src="https://github.com/rasclatt-dot-com/ESP32-Plant-Waterer-for-ESPHome/blob/68ac2869e89305c3aec70a16f734cccb81066467/assets/Screen%20Shot%202022-08-13%20at%201.52.37%20pm.png"
  alt="Home Assistant Dashboard configuration"
  title="Home Assistant Dashboard configuration"
  style="display: inline-block; margin: 0 auto; width: 500px">

The M5 Atom Lite includes USB-C connector, onboard RGB LED, and exposes enough GPIO pins to support this project in a single 24mm x 24mm package and available for around $7 USD at the time of writing.

At the time of writing M5Stack also have a nicely packaged watering addon retailing at arround $8 USD, which combines a capacitive water sensor (capacitive sensors have a longer lifespan than the cheaper conductive sensors out there) and low powered water pump in a single unit which is designed to plug straight into the M5 range via a Grove connection which makes the hardware easy to assemble.
<img
  src="https://github.com/rasclatt-dot-com/ESP32-Plant-Waterer-for-ESPHome/blob/68ac2869e89305c3aec70a16f734cccb81066467/assets/waterer.png"
  alt="Home Assistant Dashboard configuration"
  title="Home Assistant Dashboard configuration"
  style="display: inline-block; margin: 0 auto; width: 500px">

The M5 Atom Lite can provide enough power to run the water pump, (only tested when plugged into use power).

## Bill of materials

- [M5Stack Atom Lite](https://docs.m5stack.com/en/core/atom_lite) (SKU:C008)
- [Watering Unit with Moisture Sensor and Pump](https://shop.m5stack.com/products/watering-unit-with-mositure-sensor-and-pump) (SKU: U101)
- 1 x empty water bottle/ container
- 1 x USB power adaptor

## Hardware

Setup is pretty easy – as the M5 Atom Lite and the watering unit both use a compatible grove connector (cable is provided in the watering kit).

<img
  src="https://github.com/rasclatt-dot-com/ESP32-Plant-Waterer-for-ESPHome/blob/68ac2869e89305c3aec70a16f734cccb81066467/assets/m5%20setup%202.png"
  alt="Home Assistant Dashboard configuration"
  title="Home Assistant Dashboard configuration"
  style="display: inline-block; margin: 0 auto; width: 500px">

## ESP32 setup

The easiest way to set this up is to set up a new device in ESPHome and paste the YAML code and run the installation process.

This guide assumes you are familiar with ESPHome, and have installed the MQTT broker service in Home Assistant.

You need to add the "historical\_stats" code to Home Assistant "configuration.yaml", and restart Home Assistant, this sensor monitors the number of times watering automations have run in the last 4 hours, and is used to prevent the automation getting in a loop and overwatering. Also as this project does not have a water level sensor in the resevoir this is also used to raise alerts that the water reservoir has run out or the water pipe or is not spraying water into the plant pot. (this last one could save a whole container of water being fed to your carpet if things go wrong!).

Once complete you should see the device show up in MQTT

<img
  src="https://github.com/rasclatt-dot-com/ESP32-Plant-Waterer-for-ESPHome/blob/68ac2869e89305c3aec70a16f734cccb81066467/assets/Screen%20Shot%202022-08-12%20at%2011.06.21%20am.png"
  alt="Home Assistant Dashboard configuration"
  title="Home Assistant Dashboard configuration"
  style="display: inline-block; margin: 0 auto; width: 300px">

With the following entities detected;

<img
  src="https://github.com/rasclatt-dot-com/ESP32-Plant-Waterer-for-ESPHome/blob/d142af60731f68d4294a2b4009cb46d3a226c8d4/assets/Screen%20Shot%202022-08-13%20at%202.41.37%20pm.png"
  alt="Home Assistant Dashboard configuration"
  title="Home Assistant Dashboard configuration"
  style="display: inline-block; margin: 0 auto; width: 800px">

The example folder has example YAML for the following;

## Dashboard
The example YAML files include a dashboard with the folloiing features;
- Manual water pump switch
- Moisture percentage sensor value  
- Counter of number of times watering automation has run in the last 4 hours
- Moisture Gauge card

<img
  src="https://github.com/rasclatt-dot-com/ESP32-Plant-Waterer-for-ESPHome/blob/d142af60731f68d4294a2b4009cb46d3a226c8d4/assets/Screen%20Shot%202022-08-13%20at%202.41.04%20pm.png"
  alt="Home Assistant Dashboard configuration"
  title="Home Assistant Dashboard configuration"
  style="display: inline-block; margin: 0 auto; width: 400px">

## Watering automation

The watering automation example YAML provides the following features;
  - Automated watering based on capacitive sensor values
  - Sends push alerts to home assistant app enabled mobile devices / web portal
  - Built in failsafe logic to prevent over watering (watering cannot run for an hour since last watering, and cannot run more than 4 times in a 4 hour window).
  
## Alerting and failsafe automation

The example YAML provides a failsafe automation and alerting of the watering function;
- Alerting and failsafe automation
- Sends alerts if the watering has run 4 times in a 4 hour period, where the soil I still reporting dry.
- Push alerts to mobiles
- TTS/ voice service alerting (uses nabu casa service - but can be changed if you dont subscribe to this service or disabled if this feature gets annoying!)

## Code tested and running

Setup, hardware and code tested and running on;
- Home Assistant 2022.8.3
- Supervisor 2022.08.3
- Operating System 8.4
- Frontend 20220802.0 - latest
- ESPHome 2202.6.3

## Additional information/ instructions

All YAML code in this repository is heavily commented to provide additional instruction/ guidance on customisation/ configuration for your system. This is intended to make is more understandable and re-usable to a wider audience.

