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

## Links
- Sensor Networks
 - http://roximity.com/
 - https://en.wikipedia.org/wiki/IBeacon
- Ubicomp
 - http://www.ubiq.com/weiser/calmtech/calmtech.htm
 - [Map Bag](http://www.joshbillions.org/post/3974357210/mapbag)
- Examples
 - http://postscapes.com/wifi-light-socket-spark
 - http://www2.meethue.com/en-us/

## Tips
- Substitutions
  - What if I don't have the right capacitor?
    1. Make sure you use the same voltage rating or higher.
    1. Make sure you use the same temperature rating or higher.
    1. If capacitors are placed in parallel, the total equivalent capacitance is the sum of all (e.g. `CTotal = C0 + C1 + C2 + ... + CN`).
    1. If capacitors are placed in series, the total equivalent capacitance is the sum of the inverse of each capacitor (e.g. `1 / CTotal = 1 / C0 + 1 / C1 + 1 / C2 + ... + 1 / CN`.
    1.
  - What if I don't have the right resistor?

## A manual web server using AT commands

This short guide shows how we can manually serve a simple web page without programming anything, by simply issuing AT commands.


First, we need to join an existing network. These chips can act as a WiFi base station, but we'll connect to an existing network for this example.

`AT+CWLAP`  -- scans and lists all access points  
`AT+CWJAP="SAIC-Guest","password"` -- connects to a specific access point, in this case the SAIC network. Replace password with the one we share in the workshop (let's not broadcast this publicly)  

`AT+CIFSR` -- gets the device's IP address once connected  

	AT+CIFSR
	192.168.1.135
	OK

We are now connected. Next, we can to start up a webserver.
`AT+CIPMUX=1` -- enable multiplex mode (this is required to start a server)
`AT+CIPSERVER=1,80`  -- start the server on port 80, the default port for a web server.

Using your computer's web browser, go to the address reported by the `AT+CIFSR` command. For example, http://192.168.1.135.

Note- your laptop must be on the same WiFi network as the ESP8266 device.

Something like this will appear once you connect:

```
+IPD,0,353:GET / HTTP/1.1
Host: 192.168.1.135
Connection: keep-alive
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/40.0.2214.111 Safari/537.36
Accept-Encoding: gzip, deflate, sdch
Accept-Language: en-US,en;q=0.8


OK
```

Your browser will patiently wait, because the ESP8266 chip does not automatically issue a response. We have to do this manually. One important thing to note is that we've put the chip into MUX (Multiplex) mode, allowing the chip to handle multiple simultaneous connections. Because of this, every connection is assigned a unique identifier. To tell the chip which connection to send data on, we need to also tell it the ID of the connection we want to use.

We can read the connection ID from the above response. Following the `+IPD` above, we see a `,0,353`. This tells us that connection ID 0 has just received a packet with a length 0f 353. If we created another connection, that number would increment to 1.

To send a response to connection ID 0, issue the following commands:

`AT+CIPSEND=0,10` -- Prepare the ESP chip for sending data to connection ID 0, with a length of 10 characters (yeah, we have to count it ourselves)  
`helloworld` -- type a 10 character message  
`AT+CIPCLOSE=0`-- close the connection. The browser will wait until the server (our ESP8266 chip in this case) closes the connection.  

That's it! Your browser should now display our helloworld text.
