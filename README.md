                                          Autonomous Defusal Rover 
                                                 By Koray
![image alt](https://github.com/koray9012/Bomb-defusal-robot/blob/main/16026.jpg?raw=true)
An advanced, multi-motor ESP32 rover featuring an onboard mechanical arm with 3 metal-gear servos, LoRa long-range wireless telemetry, and smooth PWM power delivery designed to handle heavy tasks without browning out.
You can let it run custom autonomous sequences or control it remotely over long distances via its LoRa module.

## Key Upgrades & Features
Quad-Motor Differential Drive:

• Powered by a robust 4-motor chassis running through an L298N driver, utilizing gradual software PWM ramping to eliminate voltage sags and system crashes.

3-Axis Metal-Gear Mechanical Arm:

• Integrated robotic arm equipped with three precise metal-gear servos (Spin, Lifter, and Gripper) capable of picking up objects, raising heavy payloads up to 180 degrees, and executing complex automated pickup sequences.

Long-Range LoRa Telemetry:

• Equipped with a 433MHz Ra-02 LoRa module allowing reliable long-distance wireless communication and command transmitting far beyond standard Wi-Fi ranges.

Optimized Dual-Capacitor Power Architecture:

• Built around a custom 2S 18650 (7.4V) battery setup augmented with strategic $470\mu\text{F}$ filtering capacitors across both the motor driver power line and the ESP32 logic rail to ensure total stability under heavy mechanical loads.

## How to use:

To use the defusal rover, first make sure your custom 2S 18650 battery pack is wired securely to the master power switch and the L298N board. Flip the power switch to turn on the system. The onboard ESP32 will pause briefly for 3 seconds to let all power rails stabilize, and then it will automatically execute the programmed defusal/pickup sequence: driving forward, opening the gripper, lifting the arm to clear obstacles, moving forward to approach the target, grabbing it securely, lifting it straight up, and reversing to safety. All movements use smooth ramping and deliberate delays to keep current spikes manageable.

## Operating Instructions
1. Power On
1.Connect your custom 2S 18650 battery pack to the system input.

2.Flip the master power switch to turn on the rover.

3.Wait for the 3-second initialization and stabilization delay.

2. Execution Sequence
1.The rover automatically starts its predefined pickup and object clearance sequence:

• Drives forward for 2 seconds with gradual speed ramping.

• Opens the mechanical gripper.

• Lifts the arm's lifter servo to ~120 degrees.

• Drives forward for another 0.5 seconds.

• Lowers the lifter slightly and closes the gripper to secure the payload.

• Raises the lifter straight up to 180 degrees for 2 seconds.

• Reverses backward to complete the mission.

## Why I made it:

After working on simpler two-wheel edge-detecting robots, I wanted to step up to a heavy-duty mechanical system that could actually interact with its environment. Adding 4 drive motors plus 3 metal-gear servos created a massive challenge with power delivery—every time the arm moved or the motors punched forward, the battery voltage would sag and trigger the ESP32's brownout detector, resetting the board instantly. 

To solve this, I dove deep into hardware architecture: adding dedicated bulk capacitors, separating control logic, writing custom software PWM ramps to gently spool up the motors instead of slamming them to full speed, and utilizing native ESP32 timer allocations. It turned into an incredible masterclass in power management, motor drivers, and multi-servo automation.

### Wiring & Connections:

Below is the complete wiring summary for the Defusal Rover 1.0.

![image](https://github.com/koray9012/Bomb-defusal-robot/blob/main/%D0%95%D0%BA%D1%80%D0%B0%D0%BD%D0%BD%D0%B0%20%D1%81%D0%BD%D0%B8%D0%BC%D0%BA%D0%B0%202026-08-24%20232720.png?raw=true)

### Pinout Breakdown:

| ESP32 Pin | Component | Connected Pin / Note |
| :--- | :--- | :--- |
| **GPIO 16** | L298N Motor Driver | IN1 (Right Motors) |
| **GPIO 27** | L298N Motor Driver | IN2 (Right Motors) |
| **GPIO 26** | L298N Motor Driver | IN3 (Left Motors) |
| **GPIO 25** | L298N Motor Driver | IN4 (Left Motors) |
| **GPIO 13** | L298N Motor Driver | ENA (PWM Speed Control - remove jumper cap) |
| **GPIO 21** | L298N Motor Driver | ENB (PWM Speed Control - remove jumper cap) |
| **GPIO 4** | Spin Servo | Signal Wire (Orange/Yellow) |
| **GPIO 22** | Lifter Servo | Signal Wire (Orange/Yellow) |
| **GPIO 32** | Gripper Servo | Signal Wire (Orange/Yellow) |
| **GPIO 5** | LoRa Module (Ra-02) | NSS |
| **GPIO 23** | LoRa Module (Ra-02) | MOSI |
| **GPIO 19** | LoRa Module (Ra-02) | MISO |
| **GPIO 18** | LoRa Module (Ra-02) | SCK |
| **GPIO 34** | LoRa Module (Ra-02) | DIO0 |
| **GPIO 14** | LoRa Module (Ra-02) | RST |
| **3.3V** | LoRa Module (Ra-02) | 3.3V Power Pin |
| **5V Rail** | All Servos & ESP32 | L298N 5V Output screw terminal |
| **12V Screw** | Power Switch | Switch Pin 2 to L298N 12V input |
| **Battery +** | Master Power Switch | Switch Pin 1 |
| **Battery -** | Shared System GND | Tied to L298N GND and ESP32 GND |

## Code:

The code can be found in repo: Defusal Rover 1.0 Code

## Bill of materials:

| Item | Quantity | Price (USD) | Link |
| :--- | :--- | :--- | :--- |
| Esp32 38 pins | 1 | 8.68 USD | https://www.ardboard.com/index.php?route=product/product&product_id=413 |
| L298N Motor Driver | 1 | 4.60 USD | https://elimex.bg/product/71197-kit-k2010-drayver-za-postoyannotokovi-motori |
| LoRa Ra-02 433MHz | 1 | ~5.00 USD | https://www.ardboard.com/lora-ra-02-433MHz?search=LoRa |
| Metal Gear Servos | 3 | ~15.00 USD | Standard SG90 / MG966R style metal gear servos |
| 4WD Robot Chassis Kit | 1 | 20.93 USD | https://elimex.bg/product/84826-shasi-za-robot-4wd-s-4-motora-i-2-osnovi-kit-za-sglobqvane |
| 18650 Battery | 2 | 5.77 USD x2 = 11.54 USD | https://elimex.bg/product/85664-akumulator-3.7v-3400mah-lc18650-lava |
| Battery holder & 2S BMS | 1 set | ~3.00 USD | Elimex / Local electronics store |
| Power Switch & Wires | 1 | ~1.50 USD | Standard hardware components |

## Very important: Ensure you remove the black jumper caps from ENA and ENB on the L298N so they can be driven cleanly by the ESP32 PWM pins, and double-check that all ground wires are tied into a rock-solid common ground to avoid erratic servo behavior!

## Video for defusal rover demo (https://youtu.be/twnig8tyBAo)

## Credits:

This project uses:

Kicad

Hack Club Macondo

Btw thank you for the pinecil Hack Club :)
