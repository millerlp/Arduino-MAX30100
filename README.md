# Arduino-MAX30100


Arduino library for the Maxim Integrated MAX30100 oximetry / heart rate sensor. 
Branched from https://github.com/oxullo/Arduino-MAX30100


## Disclaimer

The library is offered only for educational purposes and it is not meant for medical uses.
Use it at your sole risk.

## Notes

Maxim integrated stopped the production of the MAX30100 in favor of MAX30101 and MAX30102.
Therefore this library won't be seeing any further improvement, besides fixes.



### Pull-ups

Since the I2C interface is clocked at 400kHz, make sure that the SDA/SCL lines are pulled
up by 4,7kOhm or less resistors.

## Architecture

The library offers a low-level driver class, MAX30100.
This component allows for low level communication with the device.


## Examples

The included examples show how to use the PulseOximeter class:

 * MAX30100_Minimal: a minimal example that dumps human-readable results via serial
 * MAX30100_RawData: demonstrates how to access raw data from the sensor
 * MAX30100_Tester: this sketch helps to find out potential issues with the sensor

## Troubleshooting

Run the MAX30100_Tester example to inspect the state of your rig.
When run with a properly connected sensor, it should print:

```
Initializing MAX30100..Success
Enabling HR/SPO2 mode..done.
Configuring LEDs biases to 50mA..done.
Lowering the current to 7.6mA..done.
Shutting down..done.
Resuming normal operation..done.
Sampling die temperature..done, temp=24.94C
All test pass. Press any key to go into sampling loop mode
```

Pressing any key, a data stream with the raw values from the photodiode sampling red
and infrared is presented.
With no finger on the sensor, both values should be close to zero and jump up when
a finger is positioned on top of the sensor.


Typical issues when attempting to run the examples:

### I2C error or garbage data

In particular when the tester fails with:

```
Initializing MAX30100..FAILED: I2C error
```

This is likely to be caused by an improper pullup setup for the I2C lines.
Make sure to use 4,7kOhm resistors, checking if the breakout board in use is equipped
with pullups.

### Logic level compatibility

If you're using a 5V-based microcontroller but the sensor breakout board pulls SDA and SCL up
to 3.3V, you should ensure that its inputs are compatible with the 3.3V logic levels.
An original Atmel ATMega328p considers anything above 3V as HIGH, so it might work well without
level shifting hardware.

Since the MAX30100 I2C pins maximum ratings aren't bound to Vdd, a cheap option to avoid
level shifting is to simply pull SDA and SCL up to 5V instead of 3.3V.

### Sketchy beat frequency readouts

The beat detector uses the IR LED to track the heartbeat. The IR LED is biased
by default at 50mA on all examples, excluding the Tester (which sets it to 7.6mA).
This value is somewhat critical and it must be experimented with.

The current can be adjusted using PulseOximeter::setIRLedCurrent().
Check the _MAX30100_Minimal_ example.





