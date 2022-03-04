# Robot
Documentation for the robot I made for my siblings.

## What is this?
In early 2022, I made a robot to help my siblings learn programming.
The robot is based on a BBC Micro:Bit microcontroller dev board and an Adafruit Crickit robot driver board. In its first iteration, the robot includes:
- Two DC motors, for movement
- Two servo motors
- A 5x8 NeoPixel (individually addressable RGB LED) array
- A laser rangefinder sensor
- A speaker
- An infrared line following sensor
- A lithium-ion battery pack
...And all of the onboard peripherals included on the Micro:Bit and Crickit.

## Reference images
### Front
![front](/images/front.png)
### Back
![back](/images/back.jpg)
### Bottom
![bottom](/images/bottom.jpg)
### Crickit
![crickit](/images/crickit.png)
### Remote
![remote](/images/remote.jpg)

## Getting Started
1. Charge the robot through the charging port on the power management board (see reference images). This is the *only* port that will chage the robot.
2. Open [MakeCode](https://makecode.microbit.org/#editor). Click the "install" button in Chrome, if desired.
3. Start a new project.
4. Click on advanced->extensions to import any desired peripheral libraries (see "Library peculiarities and peripheral APIs" section below).
5. Write a program. The block-based language is somewhat self-explanatory for an experienced programmer.
6. PLug in the Micro:Bit via USB and click "Download".
7. On the first attempt to download, MakeCode will offer to pair the Micro:Bit via WebUSB. Follow the directions to pair the device.
8. After pairing, MakeCode should be able to download programs directly to the Micro:Bit.
9. Insert the Micro:Bit into the robot, with the LED array facing towards the front.
10. Turn on the robot via the toggle switch on top of the chassis. If the robot does not turn on, make sure the small power switch on the Crickit is also in the "on" position.

## Safety
Please do not:
- Turn screws, nuts, bolts, or the motor trim potentiometers (unless fixing something).
- Force any of the motors to turn. Forcing the servos might be okay, but do so very gently.
- Put the Micro:Bit in backwards. This might cause fire.
- Unplug any of the connections, unless you know what you are doing.
- Stick metal objects inside the robot, get the robot wet, or cut wires. All of these will cause fire.
- Short power to ground. If you do not know what this means, do not make any new connections.
- Puncture the battery. This will promptly cause an explosion, followed immediately by fire.

As long as these principles are followed, the robot is safe and there should be a minimal amount of fire.

If the electronics are damaged and/or on fire:
- Unplug any sources of external power.
- Unplug the battery from the power management board.
- Check if the battery is hot or expanded. If so, take the robot outdoors immediately.

Do not worry about:
- Electrocution. No voltages higher than 5V are present within the robot, so electrocution is impossible.
- Being run over.
- Being blinded by the IR laser in the rangefinder.

## Documentation
[Micro:Bit / MakeCode](https://makecode.microbit.org/docs)

[Crickit](https://learn.adafruit.com/adafruit-crickit-creative-robotic-interactive-construction-kit/overview)

### Notes on MakeCode
MakeCode has a few peculiarities:
- All data is saved locally, *in the browser*- even if a Microsoft account is signed in. This means that running "clear data" in the browser will delete all programs.
- The "download" button in a project will download a ".hex" file which contains *both* the compiled firmware and the source code. This file can be opened in MakeCode, or loaded straight onto the Micro:Bit.
- Libraries are installed *per-project*. Make sure to re-import all necessary libraries for each project.
- Block code is compiled to TypeScript, which is then compiled to bytecode for execution on the MCU. TypeScript code is viewable by switching to "JavaScript" mode at the top. The "Python" mode is not real Python - it is TypeScript disguised as Python. Do not use Python from the MakeCode environment.

### Library peculiarities and peripheral APIs
**Micro::Bit**
- Some functions, such as "show icon" or "show number", have an optional delay parameter (set to 600ms by default). This argument is not accessible from block view. To change the delay, switch to TypeScript and add an extra argument to the function call.

**Servos**
- The Crickit library is required. The servos are attached to servo slots 1/2.
- Note that the servo motors are mirror images. 0° on one motor is equivalent to 90° (180° in software) on the other motor.
- The angle requested in the servo function call should be 2x the desired angle. See "known bugs/issues" below.

**DC Motors**
- The Crickit library is required. The "tank motors" function works well.
- Note that motors will not move when driven under ~40% power. This is normal.

**NeoPixel Array**
- The NeoPixel library is required.
- The NeoPixel array is a 40-node, 8x5 array on pin 16.
- Use the "Set (Strip) to (NeoPixel at pin (P16) with 40 LEDs as (RGB))" command to initialize the array.
- Use the "(Strip) set matrix width (8)" to enable addressing LEDs by X/Y coordinates.

**Rangefinder**
- The Rangefinder library is required.
- There is an "initialize" function that must be called before any measurements are made.

**IR Line Following Sensor**
- To read the sensor, use the "Analog read pin (P1)" command.
- This will return a value from 0-1023. A higher value indicates a darker detected color.
- Note that the sensor sees in infrared, and is oblivious to most inks. Electrical tape (or any other black tape) works well as a line material.

**Game Controller**
- The controller requires the Elecfreaks Joystick:Bit library.
- Note that the controller requires an initialization command to be run.
- See the "Reference images" section for button labels and joystick axis directions.

## Known bugs/issues

### Robot doesn't go straight
**Cause**
This behavior is normal for DC motors.
**Mitigation**
- Adjust the motor trim potentiometers to weaken one motor. Note that this wastes energy, and will not completely fix the problem.
- Use some method of closed-loop control. For instance, correct course using the Micro:Bit's compass, or follow a line.

### Servo angles are off
The servo motors will set themselves to an angle equal to half of the requested angle.
**Cause**
This is normal. These servo motors have a 90° range, while the Crickit library assumes motors with a 180° range.
**Mitigation**
Double all servo angles in the program. For example - to turn the servo to 45°, request a 90° angle.

### Rangefiner reports ~120mm distance when actual distance is >2000mm
**Cause**
This is a bug in the rangefinder library.
**Mitigation**
Add code to check for a reported distance under 140mm and replace it with a large value. This does make the rangefiner blind to anything right next to it.

### Crickit analog inputs do not work while rangefinder is initialized
**Cause**
This is a bug in the rangefinder library.
**Mitigation**
Use the Micro:Bit's onboard analog pins instead, or avoid simultaneously using the rangefinder with the Crickit signal pins.

### Speaker makes constant noise, or is not loud enough
**Mitigation**
Adjust the amplifier gain potentiometer on the Crickit board. A high value will cause the speaker to produce constant noise; this is normal.

### The front caster wheel leaves the ground when the robot accelerates.
**Cause**
The robot is quite rear-heavy.
**Mitigation**
Accelerate the robot more slowly, add weight to the front, or add a rear caster wheel.

### Why is there hot glue on the wheels!?!
**Cause**
This is necessary to lock the bolt in place. If the bolt is tightened, the wheels push against the chassis and cause friction. If the bolt is missing, the wheels can fall off. The hot glue is not a load-bearing element.
**Solution**
Just leave it alone.