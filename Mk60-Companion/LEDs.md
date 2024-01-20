# LEDs

### Wheel LEDs

One LED near each wheel's control circuit, labeled for which corner of the car it controls.

Slow flashing on/off: Not initialized

Off with flicker about twice per second: Initialized, wheel not moving.

Solid on: Wheel moved recently!

_note: wheel LEDs might do funny stuff for about 30 seconds after a firmware update_

### Bootloader

When the bootloader is running, all four main LEDs blink together twice per second.

### Brake SW (red)

Illuminated when the brake pedal is pressed.

### ABS Warn (yellow)

Illuminated when the ABS warning light is on (MK60E1/Mk60E5 only). Matches the state of the ABS warning light output pin to an external light in the instrument cluster.

### CAN OK (green)

Illuminated when valid CAN data is being received from the ABS controller.

### Status (blue)

TBD, currently just blinks to show life.

### Unlabeled near bluetooth module (green)

Flashing when bluetooth isn't connected, on solid when connected.
