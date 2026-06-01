# Smart Wind Tunnel
 
**A fully instrumented subsonic wind tunnel designed from scratch in SolidWorks,
3D printed, and connected to a live aerodynamic data acquisition dashboard.**
 
Demo video coming soon, currently in fabrication_

---
 
## What is this?
 
A benchtop wind tunnel that measures real aerodynamic forces and airspeed in real time, displays them on a live web dashboard, and validates results against published NACA 0012 airfoil data.
 
Built as an independent engineering project to physically validate CFD simulation work (STAR-CCM+) with real sensor data closing the loop between computational and experimental aerodynamics. Every part of the system was built from scratch: mechanical structure, sensor wiring, embedded firmware, and the full software stack.
 
---
 
## Features
 
| Feature | Description |
|---------|-------------|
|  📊 **Live dashboard** | Real-time velocity, drag force, Reynolds number, and Cd — updating every 100ms |
|  🌫️ **Flow visualization** | Fog machine at the inlet makes airflow patterns and separation visible on camera |
|  📁 **Session logging** | Every test run saved automatically with timestamp and angle of attack |
|  📈 **Validation** | Drag polar compared against published NACA 0012 data at Re ≈ 100,000 |
|  🔧 **Modular design** | Flanged joints let you swap tunnel sections, airfoil models, and sensors |
 
---
 
## How the whole system works
 
```
                     Both sensors live inside here 
                  ↓                                  ↓
[Bell Mouth] → [Contraction] → [Test Section] → [Diffuser] → [Fan]
                                  ↑         ↑
                             Pitot tube  Load cell
                             (airspeed) (drag force)
                                  ↓         ↓
                                       ↓
                               Arduino Uno R3
                         (reads sensors 10x/sec,
                          sends to Pi over USB)
                                       ↓
                               Raspberry Pi 5
                          (runs Python, does math,
                           broadcasts over WiFi)
                                       ↓
                             FastAPI + WebSocket
                          (live data pipeline over
                           your local network)
                                       ↓
                             React Live Dashboard
                          (opens in your browser,
                           updates in real time)
``` 
## Flow Visualization
 
A mini fog machine feeds glycol haze into the bell mouth inlet. Under the right lighting the fog makes the boundary layer, wake, and flow separation point around the airfoil completely visible to your eyes and on camera.
 
This turns every test run into a visual demo clip that actually shows the aerodynamics happening in real time.
 
---
 
## Tunnel Sections
 
| Dimensions | Material |
|-----------|----------|
| Inlet (Bell mouth) | 250×250 → 200×200mm, 150mm long | 3D printed PLA |
| Contraction | 200×200 → 150×150mm, 200mm long | 3D printed PLA |
| Test section | 150×150mm × 300mm | Clear acrylic + 3D printed PLA frame |
| Diffuser | 150×150 → 200×200mm, 200mm long | 3D printed PLA |
| Fan mount | 200×200mm frame, 122×122mm opening | 3D printed PLA |
 
> All sections connect via flanged joints: M3 bolts through 4mm holes in each flange, with foam weatherstrip tape between faces for an airtight seal.
 
---
 
## Tech Stack
 
| Layer | Technology | What it does |
|-------|-----------|--------------|
| CAD | SolidWorks | All 5 tunnel sections + airfoil modeled from scratch |
| Microcontroller | Arduino Uno R3 | Reads sensors, streams data to Pi over USB |
| Computer | Raspberry Pi 5 (2GB) | Runs Python server, hosts dashboard backend |
| Airspeed sensor | MPXV7002DP | Differential pressure → airspeed via pitot tube |
| Force sensor | HX711 + 1kg load cell | Measures drag force on the airfoil |
| Backend | Python 3 + FastAPI | Processes data, broadcasts live over WiFi |
| Real-time comms | WebSockets | Keeps dashboard live without page refreshes |
| Frontend | React + Recharts | Live dashboard webpage with charts |
| Database | SQLite | Stores every sensor reading with timestamps |
 
---
 
## Hardware & CAD Files
 
Full bill of materials with links and prices → [`hardware/BOM.csv`](hardware/BOM.csv)
**Total build cost: ~$238**

---
 
## Running the Software
 
### On the Raspberry Pi
```bash
cd software/backend
pip install -r requirements.txt
uvicorn backend:app --host 0.0.0.0 --port 8000
```
> Starts the Python server. Pi now listens to Arduino and broadcasts live data over WiFi on port 8000.
 
### On your laptop
```bash
cd software/dashboard
npm install
npm start
```
> Opens the dashboard in your browser. Finds the Pi automatically over WiFi and starts showing live data.
 
---
 
## Build Log
 
Week-by-week progress with photos - [`Build-log.md`](Build-log.md)
 
---
 
## Results
 
Drag polar vs published NACA 0012 data - *coming once fabrication is complete*
 
---
 
