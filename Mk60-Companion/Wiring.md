# Wiring

## Grey "ABS module" Connector

| Pin | Function | Mk60 Pin | MK60E1/e5 Pin |
|--|--|--|--|
| 1 | PT-CAN low | 15 | 15 |
| 2 | Brake switch signal <sup>1</sup> | 14 | 4 |
| 3 | Rear right - | 42 | 42 |
| 4 | Rear left - | 37 | 37 |
| 5 | Front right - | 33 | 33 |
| 6 | Front left - | 46 | 46 |
| 7 | Front left + | 45 | 45 |
| 8 | Front right + | 34 | 34 |
| 9 | Rear left + | 36 | 36 |
| 10 | Rear right + | 43 | 43 |
| 11 | Ignition power | 4 | 17 + 29 <sup>2</sup> |
| 12 | PT-CAN high | 11 | 30 |

#### Notes

1. This is the signal telling the Mk60 when the brake pedal is pressed. Do not connect to anything other than the brake switch input pin on the Mk60.
2. On MK60E5, pins 17 and 29 serve slightly different functions but should both be connected to switched ignition power in standalone/retrofit applications.

#### Mating Connector
- TE/Deutsch DTM06-12SA
- Amphenol ATM06-12SA

## Black "Chassis" Connector

| Pin | Function | Notes |
|--|--|--|
| 1 | Left front VR - | <sup>1</sup> |
| 2 | Right front VR - | <sup>1</sup> |
| 3 | Left rear VR - | <sup>1</sup> |
| 4 | Right rear VR - | <sup>1</sup> |
| 5 | Brake signal | High for "braking", low or floating for "not braking". Connect to ordinary 12v brake light circuit.|
| 6 | Speedometer output | 5V PWM proportional to vehicle speed |
| 7 | Fault light output | Controls ABS/DSC fault light. Only functional on MK60E1 and MK60E5. |
| 8 | VR input shield | Connect to VR input wiring shields. |
| 9 | Right rear VR + | <sup>1</sup> |
| 10 | Left rear VR + | <sup>1</sup> |
| 11 | Right front VR + | <sup>1</sup> |
| 12 | Left front VR + | <sup>1</sup> |

#### Notes

1. While the VR inputs are labeled +/-, VR wheel speed sensor polarity doesn't matter. They are labeled +/- for consistency with vehicle wiring diagrams, not for any functional purpose.

#### Mating Connector
- TE/Deutsch DTM06-12SB
- Amphenol ATM06-12SB