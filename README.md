# Smart-Wind-Tunnel-
A fully instrumented subsonic wind tunnel designed from scratch in SolidWorks, 3D printed, and connected to a live aerodynamic data acquisition dashboard!
Demo video: coming soon — currently in fabrication
----
What is this? 
A benchtop wind tunnel that measures real aerodynamic forces and airspeed in real time, displays them on a live web dashboard, and validates results against published
NACA 0012 airfoil data. Built as an independent engineering project to physically validate CFD simulation
work (STAR-CCM+) with real sensor data, this project closes the loop between computational and experimental aerodynamics. Every part of the system was built from scratch:
mechanical structure, sensor wiring, embedded firmware, and the full software stack.

Features:

📊 Live dashboard — real-time velocity, drag force, Reynolds number, and Cd updating every 100ms
🌫️ Flow visualization — fog machine at the inlet makes airflow patterns and separation visible on camera
📁 Session logging — every test run saved automatically with timestamp and angle of attack
📈 Validation — drag polar compared against published NACA 0012 data at Re ≈ 100,000
🔧 Modular design — flanged joints let you swap tunnel sections, airfoil models, and sensors


How the whole system works (plain English)
                        Both sensors live inside here
                                     ↓↓
[Bell Mouth] → [Contraction] → [Test Section] → [Diffuser] → [Fan]
                                ↑          ↑
                          Pitot tube   Load cell
                          (airspeed)  (drag force)
                               ↓          ↓
                              Arduino Uno R3
                            (reads both sensors,
                            sends numbers to Pi
                            via USB cable)
                                 ↓
                            Raspberry Pi 5
                            (tiny computer,
                            does the math,
                            runs the server)
                                 ↓
                            FastAPI backend
                            (Python program that
                            broadcasts live data
                            over your WiFi)
                                 ↓
                            React dashboard
                            (webpage on your laptop
                            that shows everything
                            updating live)
                            
The pitot tube pokes through the top wall of the test section. It has two tiny
holes — one facing into the airflow, one facing sideways. The pressure difference
between them tells you exactly how fast the air is moving. This feeds into the
MPXV7002DP pressure sensor, which sends a voltage to the Arduino.
The load cell sits under the floor of the test section. The airfoil sits on a
rod on top of it. When air pushes on the airfoil, the load cell feels it — the
same way a kitchen scale feels weight. That's your drag force. It feeds into the
HX711 amplifier board, then to the Arduino.
The Arduino reads both sensors 10 times per second and sends the numbers to
the Raspberry Pi over a USB cable, formatted as simple text like 12.4,0.032
(velocity, force).
The Raspberry Pi is a credit-card sized Linux computer. It runs a Python
program that receives those numbers, calculates Reynolds number and drag
coefficient from them, stores everything in a database, and broadcasts it all
live over your WiFi network.
The dashboard is a webpage you open on your laptop. It connects to the Pi
over WiFi and updates itself in real time — no refreshing needed. You see the
airspeed as a big live number, a chart drawing itself in real time, and the drag
coefficient updating as you change the fan speed or tilt the airfoil. You can
also download all your data as a CSV with one click.

Flow visualization 
A mini fog machine feeds glycol haze into the bell mouth inlet. Under the right
lighting the fog makes the boundary layer, wake, and flow separation point around
the airfoil completely visible — to your eyes and on camera. This turns every
test run into a visual demo clip that actually shows the aerodynamics happening.
----
|       Section      |               Dimensions              |                Material(s)                 |
| Inlet (Bell mouth) | 250×250 → 200×200mm entry, 150mm long |              3D printed PLA                |
|     Contraction    |     200×200 → 150×150mm, 200mm long   |              3D printed PLA                |
|    Test section    |            150×150mm × 300mm          | Clear acrylic (laser cut) & 3D printed PLA |
|      Diffuser      |     150×150 → 200×200mm, 200mm long   |              3D printed PLA                |
|     Fan mount      |   200×200mm frame, 122×122mm opening  |              3D printed PLA                |
----

Tech stack
|          Layer        |            Technology            | 
|          CAD          |            SolidWorks            | 
|     Microcontroller   |          Arduino Uno R3          | 
| Single-board computer |       Raspberry Pi 5 (2GB)       | 
|    Airspeed sensor    | MPXV7002DP differential pressure | 
|      Force sensor     |       HX711 + 1kg load cell      | 
|        Backend        |   Python 3, FastAPI, WebSockets  | 
|        Frontend       |          React, Recharts         | 
|      Data storage     |               SQLite             | 

Bill of materials
Full BOM with links and prices in hardware/BOM.csv. 
Total build cost: ~$250.

Running the software
On the Raspberry Pi (the server) 
bash
cd software/backend
pip install -r requirements.txt
uvicorn backend:app --host 0.0.0.0 --port 8000

This starts the Python server. The Pi is now listening to the Arduino and
broadcasting live data over your WiFi on port 8000.

On your laptop (the dashboard)
bash
cd software/dashboard
npm install
npm start

This opens the dashboard in your browser. It finds the Pi automatically
over WiFi and starts showing live data.
----
Build log
Week-by-week progress with photos in docs/build-log.md.
----
Results
Drag polar vs published NACA 0012 data coming to results/ once
fabrication is complete.
