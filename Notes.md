## IoT

## Technology
- Existing technologies for connecting small objects to a network.
  - Arduino + [Ethernet Shield](https://www.adafruit.com/products/201) / Wifi Shield
    - Advantages
       - Well-documented.
       - Fairly easy to use.
       - Users already know how to use it.
    - Disadvantages
       - Cost in large numbers.
  - [Spark Core](http://spark.io) (Photon $19.00 - in May)
    - Advantages
      - Huge community.
      - Great documentation.
      - Fast!
      - Easy to set up.
      - Build in "Arduino"
      - Online infrastructure in place.
      - New Photon chipset is robust, used in the Nest system, and elsewhere.
      - FCC/CE/IC certified (you can put it in real products that you sell)
      - Fits in a Breadboard
    - Considerations
      - Requires a Wifi connection nearby.
    - Disadvantages
      - Cost can be prohibitive in large numbers.
  - GSM Modules ([example](https://www.adafruit.com/product/1946))
    - Advantages
      - Anywhere with cell reception.
    - Disadvantages
      - Cost can be prohibitive in large numbers.
      - Requires paid data plan (though, this is not too expensive).
    - Considerations
      - Power consumption.
  - Small Linux Computer (Raspberry Pi, Arduino Tre, etc)
    - Advantages
      - Inexpensive all-in-one computer.
    - Disadvantages
      - Relatively large.
      - No built-in wifi or cellular data.
      - Power consumption is relatively high.
    - Considerations
  - ESP8266 Series
    - Advantages
      - Price ($3.00)
      - Size (Tiny)
    - Disadvantages
      - Documentation is still lacking.
      - While core chip is FCC/CE/IC certified, the modules are not.
      - Firmware is still somewhat buggy under certain circumstances.

## Data Sheet
- https://nurdspace.nl/images/e/e0/ESP8266_Specifications_English.pdf
  - Only external BOM items, resistors, capacitors and crystal.
  - Max current consumption when transmitting 200-300 mA.
  - 32bit processor
  - 16 GPIO pins
  -
## Applications
Smart power plugs
- Home automation
- Mesh network
- Industrial wireless control
- Baby monitors
- IP Cameras
- Sensor networks
- Wearable electronics
- Wi-Fi location-aware devices
- Security ID tags
- Wi-Fi position system beacons


## Tips
- Substitutions
  - What if I don't have the right capacitor?
    1. Make sure you use the same voltage rating or higher.
    1. Make sure you use the same temperature rating or higher.
    1. If capacitors are placed in parallel, the total equivalent capacitance is the sum of all (e.g. `CTotal = C0 + C1 + C2 + ... + CN`).
    1. If capacitors are placed in series, the total equivalent capacitance is the sum of the inverse of each capacitor (e.g. `1 / CTotal = 1 / C0 + 1 / C1 + 1 / C2 + ... + 1 / CN`.
    1.
  - What if I don't have the right resistor?
