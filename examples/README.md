# Examples
## Remote Control
This is the flagship example. Use this to test that all of the hardware works.

The firmware comes in two parts: gamepad firmware, and robot firmware. The gamepad controls the robot. Features:
- Gamepad joystick controls robot movement.
- Gamepad C/F buttons control servo position.
- Gamepad shake makes robot do a spin.
- Touching the Micro:Bit logo on the gamepad makes the robot beep.
- Robot's LED array shows current direction and speed. LED hue shows data from IR line following sensor (red = light, blue = dark).
- Robot has basic obstacle avoidance. Upon seeing an object less than 30cm away, robot will stop moving and controller will vibrate.

## Line Follow
A rudimentary line follower. Follows the left edge of a line of black electrical tape.

## Wanderer
Robot will aimlessly wander around the room, avoiding obstacles.

## Superquadragon
A game similar to Super Hexagon. Requires the Micro:Bit, and no additional hardware.