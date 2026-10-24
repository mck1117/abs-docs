# MK60E5 Swap FAQ

Common questions about running a BMW MK60E5 (from E8x/E9x 1 Series, 3 Series and M3) as a standalone ABS unit in a different car.

This page assumes:

- **No steering angle sensor (SZL).** See [What would a steering angle sensor add?](#what-would-a-steering-angle-sensor-add) for what that leaves out.
- **The yaw/acceleration sensor cluster is optional.**
- **The Mk60 Companion is set up and working**, so the controller gets clean wheel speed signals and there are no wheel sensor wiring faults.
- **A 48-tooth tone ring reads exactly like a stock BMW wheel** through the converter. The tooth counts below are the physical teeth on your tone rings. You can run a slightly different number of teeth, with caveats. See ([below](#tyre-size-and-tooth-count-the-speed-factor)) for details.

"M3 software" means the E9x M3 controller software. "Non-M software" means the 335i/135i-style software. Both run on the same MK60E5 hardware family. The ABS logic is the same on both unless this page says otherwise.

## Quick answers

| Question | Short answer |
|--|--|
| M3 or non-M? | Both work. The M3 lets the tyres work harder before it steps in and keeps more rear brake. The non-M steps in a bit earlier, is more careful with the rear axle, and can be coded for smaller brakes. |
| Smaller tyres than an E90? | That's normal for a swap, and it makes the controller read fast. Work out the speed factor S ([below](#tyre-size-and-tooth-count-the-speed-factor)). Up to about 3% fast you won't notice it. Beyond 10%, ABS gets clearly worse, and grippy cars hit a braking ceiling sooner. |
| Different front and rear (stagger)? | Keep the front-to-rear mismatch under about 1.5%. The M3 software also handles rear tyres 1.5–3% bigger than the fronts, like the stock M3. Above 3%, or with bigger fronts, one axle reacts early or late in ABS. At 25% or more the controller flags wheel speed sensor faults. [Details](#different-front-and-rear-stagger-mixed-tyres-mixed-tooth-counts). |
| FWD? | Works. For performance driving, the thing to understand is the select-low rear axle under trail braking, especially when the inside rear lifts. |
| Sensor cluster without a steering sensor? | Almost no ABS benefit. It avoids some fault codes and puts yaw and acceleration on CAN for logging. |
| Mass, CG, weight distribution? | Not inputs. Get the hydraulic brake bias right. Brake torque per bar, wheel inertia and caliper fluid volume matter more. |

## What does the ABS do when running standalone?

It runs as plain four-channel ABS with brake force distribution (EBD):

- The front wheels are controlled individually.
- The rear wheels are **select-low**. When either rear needs a release, both rears are held to the lower of the two pressures.
- No special behavior when cornering.
- On split grip (one side of the car on ice, for example), there is no yaw-based limiting of the high-grip side. A pressure-based coupling between the two fronts still works.
- There is no stability control (DSC) and no traction control. Those need the steering angle sensor, sensor cluster ("yaw" sensor), *and* real engine data on CAN.

The main control loop is the same with or without the sensor cluster and steering sensor: it detects a wheel starting to lock, releases pressure, holds while the wheel spins back up, then steps pressure back up.

## Which module should I buy?

See ([below](#does-m3-vs-non-m-matter)) explanation for the difference of M3 vs. non-M.

### M3

All E9x M3 controllers are acceptable. This includes:

| ATE PN | BMW PN | Notes |
|--|--|--|
| 860.3 | 7841232 |
| 868.3 | 7841954 |
| 877.3 | 7844050 |
| 879.3 | 7844739 |
| 883.3 | 7846816 |

### Non-M

All RWD (non-xdrive), 6 cylinder, E8x/E9x 1-series/3-series cars were fitted with an Mk60e5 DSC. That means 125i/128i/135i/323i/325i/328i/330i/335i/325d/330d/335d.

However, while they will all work, not all are created equal:

Controllers from E8x/E9x LCI (2009+) vehicles are preferred.

| ATE PN | BMW PN | M3 FW? | Notes |
|--|--|--|--|
| 884.3 | 6862873 | Yes | |
| 875.3 | 6789304 | Yes | Somewhat common |
| 870.3 | 6790147 | Yes | Very common | 
| 861.3 | 6784766 | | |
| 850.3 | 6778485 | | |

> [!NOTE]
> Modules marked "M3 FW" are fully functional when cross-flashed to M3 firmware.

Early (pre-LCI, model years 2005-2008) controllers had reliability problems with the brushes in the pump motor failing prematurely. Some were replaced under warranty with the prefered modules above, but many are still out there. Trust the PN on the controller rather than the model year of the donor car. Avoid these:

| ATE PN | BMW PN |
|--|--|
| 820.3 | 6771489 |
| 827.3 | 6774930 |
| 830.3 | 6775387 |
| 831.3 | 6775389 |
| 838.3 | 6778237 |
| 848.3 | 6777150 |
| 851.3 | 6776067 |

## Does M3 vs non-M matter?

Both use the same control loop: the same release, hold and reapply logic and the same step patterns. The differences are in **when** they step in and **how much they trust the rear axle**.

| | M3 software | Non-M software |
|--|--|--|
| When a wheel counts as "starting to lock" | Accepts harder wheel deceleration before releasing. The deceleration threshold is capped at 2.4 g; below 60 km/h it is 1.32 g, and below 20 km/h it is 1.20 to 1.27 g. | Releases earlier. The cap is 1.8 g; below 60 km/h it is 1.28 g, and below 20 km/h it is 1.16 g. |
| Rear axle | Keeps more rear brake and accepts more yaw. | Gives up rear pressure sooner: releases that cover both rears trigger with about 15% less deceleration and slip. When only the rears are in ABS, it rebuilds rear pressure at no more than 100 bar/s (M3: 500 bar/s). |
| Pressure rebuild | Stepped rebuild back toward the last lock pressure. | Same steps, but it stops building sooner at high pressure. It can ramp back faster when the wheel is clearly rolling freely. |
| Vehicle variant coding | Selects the ABS deceleration thresholds. The GTS variants are the most permissive: 1.40 g below 60 km/h, and deeper thresholds above 150 km/h. | No effect on ABS. |
| Caliper model (coding) | Two choices, M3 and M3 GTS, both soft. | Six front and five rear choices, up to about 1.5× stiffer than the M3's ([table](#caliper-fluid-volume)). |
| Reference tyre (fixed, not codable) | 2073 mm rolling circumference, 48 teeth | 2000 mm rolling circumference, 48 teeth |
| Built-in stagger allowance | Yes: rear tyres 1.5–3% bigger than the fronts are fully corrected once learned ([stagger](#different-front-and-rear-stagger-mixed-tyres-mixed-tooth-counts)) | None |
| Pump | Normal | Runs longer during ABS stops. This does not change how much slip ABS allows. |

Which one to use:

- **High-grip car with big brakes, where you want maximum deceleration:** M3 software, coded to a GTS variant.
- **Small brakes, or calipers that take much less fluid than the M3's:** non-M software, coded to the closest caliper ([why this matters](#caliper-fluid-volume)).

These differences come from the software calibrations. Limited real-world, back to back A/B testing has been performed on the same car. Either controller is a huge improvement over no ABS or the ABS on an older car.

## Tyre size and tooth count: the speed factor

The controller turns the time between teeth into a wheel speed using a fixed distance per tooth, taken from the stock BMW tyre. That value is fixed in the software and can't be coded. Every tyre or tone ring change shows up as a scale error on everything the controller measures.

E9x tyres are large for a passenger car. Most swap cars (Miatas, MR2s, older BMWs and similar) run smaller tyres, so **most swaps read fast** unless the tone rings have fewer teeth to compensate.

```
your mm per tooth  = rolling circumference (mm) / teeth
stock mm per tooth = 2073 / 48 = 43.2   (M3 software)
                     2000 / 48 = 41.7   (non-M software)

speed factor  S = stock mm per tooth / your mm per tooth
```

- S = 1.00 is perfect.
- S = 1.10 means the controller reads every speed 10% too high.

**Rolling circumference** is the distance the car moves per wheel revolution with the car's weight on it. Get it from the tyre maker's revolutions per km (circumference in mm = 1,000,000 / revs per km), or mark the tyre and roll the car one full turn. As a rough estimate, use 0.97 × π × the tyre's outside diameter.

When all four wheels have the same S:

| S | What to expect |
|--|--|
| up to 1.03 | Within normal tyre wear and pressure variation. Nothing to do. |
| 1.03 to 1.10 | Works. The effects below grow with the error. Check the braking ceiling. |
| more than 1.10 | Clearly worse ABS. Change the tooth count, the tyre size, or the software (the non-M reference tyre is 3.5% smaller). |

The controller can't correct this by itself. Its tyre learning only compares the wheels with each other, so it never learns the overall scale.

### What reading fast does (S > 1)

- Speed, slip and wheel deceleration all read S times too large.
- ABS steps in earlier, with less real slip, and releases more on each cycle. That is safe, but stops get longer on a grippy surface.
- **The braking ceiling drops** (next section). On a grippy car this is the main cost.
- Low-speed thresholds switch over at lower real speeds.

Reading slow (S < 1, which needs bigger tyres or fewer teeth than stock) does the opposite. The wheels go deeper into slip before the first release, and the releases are too small. That is worst on low grip. Avoid more than a few percent slow.

### The braking ceiling

In a hard stop with all four wheels working, the controller cannot work out the car's speed from the wheels. It estimates the car slowing at no more than about 1.25 g in its own units. In real terms that limit is:

```
max real deceleration it can follow ≈ 1.25 / S  (g)
```

If the car actually brakes harder than that, the controller thinks the car is going faster than it is, sees slip that isn't there, and releases too much at the top of the stop. Keep the ceiling above your car's real braking limit with some margin:

- Street tyres reach about 1.0–1.1 g. An S of up to about 1.1 is fine.
- 200-treadwear tyres, R-compounds, slicks or aero can pass 1.2 g. That is at or above the ceiling even at S = 1.00, so expect some over-release at the top of the stop. A slightly slow reading (S of 0.97–1.00) helps here.

### Examples

| Setup | Software | S | Reads | Ceiling | Verdict |
|--|--|--|--|--|--|
| 44 teeth, 1900 mm | M3 | 1.00 | correct | 1.25 g | ideal |
| 48 teeth, 1990 mm | M3 | 1.04 | 4% fast | 1.20 g | fine for street |
| 48 teeth, 1840 mm | non-M | 1.09 | 9% fast | 1.15 g | OK for street, marginal on track |
| 48 teeth, 1840 mm | M3 | 1.13 | 13% fast | 1.11 g | over-releases in hard stops |
| 96 teeth, 1950 mm | either | 2.0 or more | far off | — | not usable |

## Different front and rear (stagger, mixed tyres, mixed tooth counts)

The front and rear axles use the same distance per tooth. Any front-to-rear difference in mm per tooth looks to the controller like one axle turning at a different speed from the other.

```
mismatch = (larger S / smaller S) − 1
```

The axle with bigger tyres or fewer teeth reads slower. Most staggered cars have bigger rear tyres, so the fronts read fast.

### What the controller does about it

While you drive steadily (not braking), the controller learns a correction for each wheel so that all four agree with the slowest one. That correction has limits:

- **It lines up the car speed estimate, but most ABS slip checks still use the uncorrected wheel speeds.** So on its own, learning moves the error from one axle to the other instead of removing it.
- **The M3 software has one exception, built for the stock M3's own stagger.** The stock M3 runs rear tyres about 2.4% bigger than the fronts on a reference tyre that is the same front and rear. When it has learned that the fronts read **1.5–3% faster** than the rears, and each axle's left and right wheels agree within 0.5%, it switches the front ABS slip over to the corrected speeds. The rears are the slowest wheels and need no correction, so all four wheels are then effectively corrected.
- **The non-M software has no such exception.** Its ABS slip always uses the uncorrected speeds.
- Only about 3% of the learned correction is kept from one drive to the next. The rest is relearned once you are driving steadily, which usually takes seconds rather than minutes.
- Right after power-on, the M3 front correction stays off until the controller is confident in what it has learned. We haven't measured how long that takes.

### What to expect (bigger rear tyres)

| Mismatch | M3 software | Non-M software |
|--|--|--|
| under 1.5% | Fine. A small offset, which BMW's calibration accepts. | Fine. |
| 1.5–3% | **Corrected.** This is what the M3 software was designed for. | Fronts lock slightly deeper before the first release. Usable. |
| over 3% | Fronts lock deeper before the first release, so the car briefly loses steering at the start of hard braking. At the start of a drive, the rears may also release early (the "unlearned" case below). | Same. |

Before the controller has learned (or when the mismatch is more than it remembers between drives), the error goes the other way: the slower-reading axle looks like it is slipping when it isn't, and ABS releases it early. With bigger rear tyres, select-low then drops both rears, so you lose rear braking. That is stable, but stops are longer.

Real example: a car on M3 software with 3.3% bigger rear tyres, just outside the window, had rear-only ABS events at about 1.1 g on track, with no real rear slip.

**Bigger front tyres** are never corrected, on either software. Before learning the fronts release early (longer stops). After learning the rears lock deeper before the first release, which risks instability. Avoid it.

A mismatch of 25% or more above 40 km/h (50% below 20 km/h) is flagged as a wheel speed sensor fault (5Dx5 or 5Dx3).

**Recommendation:**

- **Either software:** match mm per tooth front to rear within about 1.5%.
- **M3 software only:** rear tyres 1.5–3% bigger than the fronts (in mm per tooth) also work well. Aim for the middle, about 2–2.5%. The left and right tyres on each axle should match closely (same size, similar wear and pressure), or the correction switches off.
- Never more than 3%, and never bigger fronts. Choose tone rings or tyre sizes to make that work.

## What does installing the sensor cluster get you?

Without a steering sensor: **almost nothing for the ABS itself.**

- **Lateral acceleration and yaw rate are thrown out.** Without the steering sensor, the controller marks both as invalid, so all the cornering and split-grip logic stays off. It is the same as having no cluster.
- **Longitudinal acceleration is read but never trusted.** Before trusting the accelerometer, the controller checks it against the wheels while driving straight and steady, and that check needs valid lateral and yaw signals. Untrusted, the reading only feeds a few minor secondary checks. The car speed estimate and the main deceleration estimate still come from the wheels.
- **It avoids the sensor cluster fault codes** (D370–D372).
- **Data logging:** the controller keeps broadcasting the cluster's lateral acceleration, yaw rate and longitudinal acceleration on CAN, even while its own ABS logic ignores them. That makes them available to a logger.

It also brings back no DSC and no traction control. Both still need the steering sensor and engine data.

### What would a steering angle sensor add?

With both the cluster and a steering sensor, the ABS gains its cornering features:

- **Rear axle:** when driving straight and steady, the rears can be controlled individually instead of select-low.
- **Corners:** at light pedal the rears can go independent. At heavier pedal, rear pressure is cut by an amount set by lateral g, and release steps are halved or cut to a third. This is the fix for the [inside rear lift problem](#when-the-inside-rear-lifts).
- **Split grip:** yaw-based limiting of the high-grip side.
- **Hard stops:** the accelerometer becomes trusted. The car speed estimate and the deceleration thresholds can then use measured deceleration.
- **Fault codes:** the steering sensor fault code (D373) goes away.

These features need the sensors, not the car's geometry. What matters, in order:

1. The steering sensor and cluster are on the bus and report valid.
2. Their signs are right: mounted the same way round as in the BMW.
3. The steering sensor reads close to zero with the wheels straight. Mount it centred within a degree or two; the controller only learns out a small offset.

The steering ratio has almost no effect on ABS.

#### Turn DSC off when you add a steering sensor

**Adding a steering sensor wakes up DSC.** The converter emulates the ECU's CAN frames, so with the sensor and cluster present the controller has everything it needs for stability control and traction control, and it tries to use them. It won't do it well in a swap:

- **The vehicle model is still the M3's**: its mass, CG, wheelbase and cornering stiffness. That model sets the yaw rate the controller thinks the car should have, so it brakes individual wheels against a car it doesn't match. The M3 software has no coding option to change the model.
- **Traction control sees a constantly idling engine** in the emulated engine frames. Its torque requests go nowhere.
- **The engine can't respond**: The DSC will make torque cut/apply requests to the engine, but the converter won't react, leading to a fault.

**Tick the "0x398 DSC disable" option in the Companion.** It sends the same request as holding the DSC button, so the controller switches DSC off at every start.

- **ABS is not affected:** it keeps all the cornering features above, because those depend on the sensors, not on DSC being on.
- **Most of DSC goes quiet:** stability control and the brake-based traction control stop. The one part that stays enabled only asks the engine for more torque when you lift off, and with emulated engine frames nothing acts on that.

## Does vehicle mass, brake gain or CG position matter?

Not really, no. **Mass, CG, wheelbase and weight distribution are not inputs to the ABS.**

- The controller has a vehicle model and brake-size settings, but the ABS loop uses neither. They serve stability control and torque-based brake requests, and those are disabled in a car without a steering sensor.
- On the M3 software, the vehicle variant coding changes the ABS only through the deceleration thresholds, not through mass. Deceleration thresholds are fixed in the non-M software.
- The thresholds are in g and slip, which describe what tyres can do, not anything about the car's weight or brakes.
- The pressure each wheel last locked at is measured on every cycle, and the base release is a percentage of it. So the overall pressure level adjusts itself to your car.

### Hydraulic brake bias

The controller doesn't need 50/50 weight distribution like an M3. **What it needs is hydraulic brake bias that suits your car.**

- **Get the bias close to ideal**, so both axles approach lock at about the same time.
- **Slightly rearward of ideal is fine.** ABS stops the rears locking, and the rears do more of the work. The non-M software is the more forgiving of the two here.
- **Strongly rear-biased:** the rears go into ABS on every hard stop, and because they are select-low, both give up pressure together. You lose rear braking instead of gaining it.
- **Strongly front-biased:** the rears never contribute much, and ABS works only on the fronts.
- **A proportioning valve or bias setting** ahead of the unit still applies. In ABS the controller can only lower a wheel's pressure below what reaches it, never raise it above.

The brake hardware matters in three ways.

### Wheel deceleration per bar

How hard one extra bar slows the wheel depends on brake torque per bar × rolling radius ÷ (wheel + tyre + rotor inertia). Releases are sized from how violently the wheel decelerates, and that sizing was tuned for an M3.

- **More torque per bar or lighter wheels than an M3:** releases overshoot. Expect bigger pressure swings and more pulsing.
- **Less torque per bar or heavier wheels:** releases are too small. The wheel stays in slip longer and the controller needs more cycles.

### Lock pressure versus fixed step sizes

Some release and rebuild steps are fixed in bar, for example 10 to 20 bar and 300 bar/s. They were sized for M3 brakes on a 1.7 tonne car. On a light car with big brakes the wheels lock at much lower line pressure (40–55 bar is typical), so each fixed step is relatively bigger. This is the one place where mass enters: mass and brake torque per bar together set the lock pressure.

### Caliper fluid volume

While it releases pressure, the controller estimates wheel pressure from a model of how much fluid the caliper takes per bar.

- **Calipers that take less fluid than the model** (small pistons, very stiff) **lose more pressure than intended on every release.** One car with small front calipers on M3 software released 1.3 to 3.5 times the intended amount.
- **Calipers that take more fluid** release less than intended.
- **A sign of mismatch:** a pressure step when the release ends and the measured pressure takes over again.

The caliper model is chosen in coding, separately for each axle. For ABS, the only thing that changes between models is the caliper stiffness: how many bar each unit of fluid adds. The piston sizes BMW lists for each code don't affect ABS.

Pick the stiffest model that is still no stiffer than your calipers. The table gives each model's stiffness as a percentage of the stiffest model on that axle, measured over the 10–60 bar range. Lower means softer, so more fluid per bar. Counterintuitively, larger calipers typically consume more fluid per bar, not because they're more flexible, but because their larger pistons apply more force for the same pressure, consuming more fluid in the process.

| Front model | Fitted to | Stiffness |
|--|--|--|
| non-M 0 | 1 Series base | 100% |
| non-M 1 | 1 Series upper, 4-cylinder 3 Series | 96% |
| non-M 5 | sport brakes (fixed caliper, 135i) | 74% |
| non-M 2 | 6-cylinder 3 Series | 71% |
| non-M 3 | 335i / 335d / 330d | 68% |
| non-M 4 | mid 3 Series (320d, US 328i) | 68% |
| M3 | M3 | 68% |
| M3 GTS | M3 GTS | 68% |

| Rear model | Fitted to | Stiffness |
|--|--|--|
| non-M 0 | 1 Series base hatch | 100% |
| non-M 2 | most 4-cylinder 3 Series, 325i, 328i | 90% |
| non-M 1 | 118d/120d, E82/E88 below 135i | 73% |
| non-M 3 | 330i / 335i / 335d | 70% |
| M3 | M3 | 67% |
| non-M 4 | sport brakes (fixed caliper) | 65% |
| M3 GTS | M3 GTS | 57% |

- **The M3 software only offers its two models** (M3 and M3 GTS). They are nearly identical on the front, and both are at the soft end of the range. If your calipers are much stiffer than an E9x's, there may be value to using the non-M software for its stiffer models.
- Even the stiffest non-M model may be softer than very small swap calipers. The over-releasing car above was stiffer than any BMW model.

## FWD vs RWD

The ABS doesn't know which wheels are driven. It never reads engine torque, rpm, gear or clutch, and no coding setting tells it the drive layout. Its control is split by axle position:

- The fronts are controlled individually and the rears are select-low.
- Each axle has its own thresholds and release sizes.
- Outside ABS, the car speed estimate follows the wheels, which works for any drive layout.

For performance driving (spirited street, autocross, track), drive layout matters much less than **how the select-low rear axle behaves under trail braking**. That applies to every layout, but FWD cars often run stiff rear anti-roll bars and have little weight on the rear, so they lift the inside rear easily.

### Trail braking: the select-low rear axle

Without a steering sensor, the rears are always select-low. Under trail braking, the unloaded inside rear reaches its limit first, and the loaded outside rear is held to the same pressure. So **rear braking in a corner is limited to what the inside rear can take.** On a FWD car that also means less rear-brake help with rotation on corner entry.

### When the inside rear lifts

When the inside rear lifts off the ground, it takes the other rear with it:

1. The unloaded rear locks almost immediately, and ABS releases it.
2. That wheel now has the lowest pressure on the axle, so the loaded outside rear is pulled down to match it.
3. In the air, nothing spins the lifted wheel back up, so both rears stay released. There's no time limit on this.
4. At touchdown the wheel spins up, pressure rebuild starts, and the outside rear comes back with a pressure step.

While the wheel is in the air, **the rear axle loses about all of its braking**. It comes back suddenly at touchdown, which can unsettle the car mid-corner.

At risk are FWD cars with a stiff rear setup, mid-engine cars, short-wheelbase cars, and anything that picks up an inside rear when trail braking into tight corners. Fixing it means chassis setup that keeps the inside rear on the ground, or adding a steering angle sensor ([below](#what-would-a-steering-angle-sensor-add)).

In a straight line on split grip, select-low keeps the yaw small, which is good for stability, but it costs rear braking.

**Spotting it in a log.** Watch the `ST_CLCTR` byte in the controller's DSC status frame (0x19E, `ST_DSC` in the [DBC](../MK60e5_CAN.dbc)):

- **0x01 set:** a front wheel is in an ABS cycle. This takes priority; check it first.
- **0x01 clear and 0x20 set:** a rear wheel is in ABS control and no front is.

On corner entry with light pedal, a burst of "0x20 without 0x01" is the select-low rear axle at work, usually an inside rear that is unloaded or in the air. Under hard straight-line braking it more often means the rears read slow from tyre stagger ([above](#different-front-and-rear-stagger-mixed-tyres-mixed-tooth-counts)).

### Engine braking on slippery roads (FWD)

The ABS has one built-in RWD assumption: **the front wheels roll freely and are the best reference for the car's speed.** On a dry road this doesn't matter. On a slippery road with the clutch engaged, engine drag can slow both driven fronts below road speed together. The controller then underestimates front slip, and front pressure rebuild starts late. **Pressing the clutch during a hard stop on a slippery surface removes the difference.**

## Other things to know

### CAN and fault codes

- **ABS and EBD run without any CAN input.** Missing frames are logged as fault codes and DSC reports itself as failed or off, but the ABS function stays on.
- **The Mk60 Companion can emulate the PT-CAN frames the controller expects** (ignition, engine, gearbox and instrument cluster), so it doesn't log CAN fault codes. Each frame can be turned off separately. In a "large transplant", where an E9x-era engine and gearbox come over along with the MK60E5, the real modules send their own frames, so turn those off and use the Companion only for wheel speed conversion.
- **Without a steering sensor you will always have its fault code** (D373), and DSC stays unavailable. This doesn't affect ABS.
- **Vehicle speed broadcast:** the speed the controller sends on CAN is the average of the two front wheels, scaled by the stock tyre size, without the per-wheel tyre correction. It is off by the same factor S, it dips while the fronts are in ABS, and it reads zero with both fronts locked.

### Coding

- **Code hill-start assist off** unless real engine data is on the bus. It decides when the car is pulling away from engine data, and emulated engine frames show a constantly idling engine. It may hold the brakes longer than you expect.
- **Code the tyre pressure warning (RPA) off.** It compares wheel speeds against each other, so it gives false puncture warnings on cars with stagger or odd tooth counts.
- **Soft-stop assist** also reads engine data. Code it off if you're unsure.
- **M3 software:** the vehicle variant coding selects the ABS deceleration thresholds. The GTS variants allow the deepest wheel deceleration, which suits high-grip cars.

### Wiring and setup

- **Wheel inputs must match brake lines.** Each wheel speed input must come from the same corner as the brake line on the matching output, and the front channels must be on the front axle. A swapped pair makes ABS release the wrong wheel. The [Testing](Testing.md) steps check the speed side. Check the hydraulic side the same way.
- **The brake light switch has to agree with pressure.** The controller checks that the brake switch is on during brake applications above about 5 bar and off when pressure drops below about 4 bar. A switch that never opens or never closes sets a brake switch plausibility fault (5E5E).
