# Testing

## 1. Test correct speed sensor wiring

Confirm that each wheel is wired correctly both to the companion module and to the ABS module itself:

1. Safely lift the vehicle so each wheel can be rotated by hand.
1. Connect Mk60 Companion to your PC and look at the wheel speed section in the configurator.
1. One wheel at a time, spin the wheel by hand.
1. Check that the corresponding LED on the companion module lights when a wheel is turned (and no others light).
1. Check that the corresponding wheel in the configurator shows non-zero wheel speed. Spinning by hand you should be able to easily reach 5-8km/h. With an MK60E5, confirm that each wheel indicates positive wheel speed. If negative, toggle the respective direction check box for the corresponding wheel. Mk60 and Mk60e1 will always indicate positive wheel speed.

## 2. Test brake switch operation

With the brake pedal released, the red LED labeled "Brake SW" should be extinguished. When the brake pedal is pressed, the LED should light (along with the vehicle's brake lights).

In the configurator, confirm that "Brake switch input" and "ABS brake switch input" match state. "Brake switch input" shows the state of the wire input to the Mk60 companion, and "ABS brake switch input" is the brake switch state as reported by the ABS module over CAN.
