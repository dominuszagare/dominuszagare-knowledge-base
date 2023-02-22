# Individually addressable LED strip

ws2812b LED strip is a very popular type of LED strip. It is a strip of LEDs that can be controlled individually. The LEDs are controlled by a single data line. The data line is a digital signal that is sent to the LED strip. The LED strip then interprets the data and lights up the LEDs accordingly.

## How to drive the LED strip

In the ws2812b dataset we can see what signal is needed to drive the LED strip. With the precise timings specified as folows:

- 0: 0.4us high, 0.85us low
- 1: 0.8us high, 0.45us low
- Reset: above 50us low





