# Operation Schemas: Setup & Environmental Control

### Schema 1: Perform_Self_Check
* **Description:** Runs a diagnostic routine on all chamber sensors and control units at boot time.
* **Inputs:** Sensor hardware signals (temperature, humidity, light, vibration, door sensor, power line).
* **Outputs:** Diagnostic status code (`PASS` or `FAIL`).
* **Preconditions:** System is powered on.
* **Postconditions:**
  * If all checks pass, system permits transition to monitoring.
  * If any essential sensor fails, error is logged and conservation cannot start.

---

### Schema 2: Register_Artifact_Profile
* **Description:** Reads and stores the safety requirements for a newly placed artifact.
* **Inputs:** `artifact_id`, `temp_min`, `temp_max`, `humidity_min`, `humidity_max`, `max_vibration`, `max_light`.
* **Outputs:** Confirmation receipt (`PROFILE_LOADED`).
* **Preconditions:** Self-check passed; chamber is in monitoring mode; artifact is placed inside.
* **Postconditions:** Environmental limits are stored in active memory and assigned to current monitoring cycle.

---

### Schema 3: Detect_Door_Status
* **Description:** Monitors whether the chamber door is sealed or open.
* **Inputs:** Door magnetic contact sensor signal (`OPEN` / `CLOSED`).
* **Outputs:** `door_state` flag.
* **Preconditions:** Sensor power is active.
* **Postconditions:**
  * If `CLOSED` and profile is loaded, system allows conservation to run.
  * If `OPEN`, active conservation stays suspended.

---

### Schema 4: Read_Environment_Sensors
* **Description:** Periodically reads current conditions inside the chamber.
* **Inputs:** Analog/digital readings from temperature, humidity, light, and vibration sensors.
* **Outputs:** Current telemetry data (`current_temp`, `current_humidity`, `current_light`, `current_vibration`).
* **Preconditions:** Sensors are calibrated and functional.
* **Postconditions:** Sensor readings are compared against the active artifact profile limits.

---

### Schema 5: Start_Conservation_Cycle
* **Description:** Activates normal conservation control mechanisms.
* **Inputs:** `door_state == CLOSED`, `artifact_profile_valid == TRUE`.
* **Outputs:** Status flag `CONSERVATION_RUNNING`.
* **Preconditions:** Door is closed, self-check passed, artifact profile is loaded.
* **Postconditions:** Active monitoring and environmental regulation start.

---

### Schema 6: Adjust_Temperature
* **Description:** Corrects chamber temperature when it drifts outside allowable profile boundaries.
* **Inputs:** `current_temp`, `target_temp_range`.
* **Outputs:** Actuator commands (`HEATER_ON`, `COOLER_ON`, or `OFF`).
* **Preconditions:** Active conservation is running; `current_temp < temp_min` OR `current_temp > temp_max`.
* **Postconditions:** Environmental cooling or heating unit is triggered; recovery timer starts.
