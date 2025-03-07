# Compatibility matrix

| Sensor type | Mk60 | Mk60e1 | Mk60e5 |
|--|--|--|--|
| VR | ✅ Yes, with Happy Cactus converter | ✅ Yes, with Happy Cactus converter | ✅ Yes, with Happy Cactus converter |
| Plain Hall | ✅ Directly works, no converter required | ✅ Directly works, no converter required | ❌ (but supported soon on a new converter variant!) |
| AK-protocol Hall | ❌ | ❌ | ✅ Stock E90 sensors |

## Sensor Types

### Sensor type identification

Check the resistance between the two sensor pins using a multimeter. If the resistance in both directions (swap +/-) match, and is in the range of 500-3000 ohms, you have a VR sensor. Anything else is a Hall sensor.

## VR aka "Passive"

VR (variable reluctance) sensors are the oldest type of wheel speed sensor, and detect teeth going by with a coil of wire wrapped around a permanent magnet. Metal teeth traveling by the sensor tip distort the magnetic field and induce a voltage in the coil.

## Hall Effect aka "Active"

Hall effect sensors utilize the [hall effect](https://en.wikipedia.org/wiki/Hall_effect_sensor) to sense the presence and movement of magnetic fields.

### Common-type Hall

By far the most common sensor type on post-2000 cars. These sensors change their current draw in a square wave pattern depending on whether or not a tooth (or magnetic field) is present at the sensor tip.

### AK-protocol Hall

These are the strange sensors found on E8x/E9x cars fitted with an Mk60e5 controller. They send a higher current pulse for timing, then transmit 9 bits of extra data about wheel direction, sensor field strength, and diagnostics. Everybody seems to agree this was a mistake, as they exist possibly only on 2005-2013 BMWs like the E87 and E90.

There's a more in-depth comparison of different sensor types available here: [AN296246-Speed-Sensor-Protocols.pdf](AN296246-Speed-Sensor-Protocols.pdf)

# Mk60 Module Types

| Controller | Pressure sensors | Wheel speed sensor type | Source |
|--|--|--|--|
| Mk60 | 2, external | Plain hall | 2003+ E46 |
| Mk60e1 | 1, internal | Plain hall | 4 cyl RWD E82/E87/E9x |
| Mk60e5 | 5, internal | AK hall | 6/8 cyl RWD E82/E87/E9x |

### Mk60

Mk60 introduced in 2003 on the BMW E46. These controllers use external pressure sensors mounted to the master cylinder (on a BMW) or to a fitting between the master and ABS unit (swaps). There are anecdotal reports of the pressure sensors failing on track, which leaves you with no brake pressure. Proceed with caution!

Found on all RWD (NOT AWD/XDRIVE!) E46 cars, model year 2003 and later. Also found on later Mini R50/52/53 cars, though these have some differences (but should still work with the converter, and are a good option for FWD installations).

Part numbers from the M3 are 813.3, 817.3, and 818.3. All can be coded to CSL parameters, 813.3 and 817.3 are flashable to motorsport firmware.

### Mk60e1

Similar to the Mk60, but includes an internal pressure sensor on the front input. Found on RWD-only E82/87/9x gas and diesel 4 cylinder cars, 116i/120i/116d/120d/320i/320d/etc.

Uses an identical connector pinout, CAN data format, and yaw sensor to the Mk60e5.

### Mk60e5

5 internal pressure sensors, one on the front input, plus one on each wheel output. Also upgrades to analog inlet valves that can partially open/close for finer control of brake pressures. The Mk60e5 only supports the unusual AK-protocol sensors.

Found on RWD-only 6 and 8 cylinder E82/87/9x cars, like 128i/135i/328i/335i/M3.

Uses an identical connector pinout, CAN data format, and yaw sensor to the Mk60e1. Only difference in the CAN format is that it sends an extra frame with all 4 wheel brake pressures.
