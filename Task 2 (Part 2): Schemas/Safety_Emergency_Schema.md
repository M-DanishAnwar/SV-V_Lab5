# Operation Schemas: Safety, Vibration & Emergency

### Schema 7: Adjust_Humidity
* **Description:** Corrects humidity levels when they drift outside the permitted profile range.
* **Inputs:** `current_humidity`, `target_humidity_range`.
* **Outputs:** Actuator signals (`HUMIDIFIER_ON`, `DEHUMIDIFIER_ON`, or `OFF`).
* **Preconditions:** Active conservation is running; `current_humidity` is outside permitted range.
* **Postconditions:** Moisture-control hardware is activated; recovery timer begins.

---

### Schema 8: Verify_Condition_Restored
* **Description:** Checks that temperature or humidity has actually returned inside safety limits after an adjustment.
* **Inputs:** Sensor telemetry readings, `recovery_timer_limit`.
* **Outputs:** Verification result (`RESTORED` or `RECOVERY_TIMEOUT`).
* **Preconditions:** A correction command was issued (`Adjust_Temperature` or `Adjust_Humidity`).
* **Postconditions:**
  * If readings are inside range before timer expires, system declares conditions normal.
  * If timer runs out and readings are still bad, triggers `Trigger_Protection_Alert`.

---

### Schema 9: Trigger_Protection_Alert
* **Description:** Protects the artifact when normal environmental recovery fails within the allowed period.
* **Inputs:** Recovery timeout signal OR critical sensor failure.
* **Outputs:** `REDUCE_LIGHT` signal, backup control commands, `OPERATOR_ALARM`.
* **Preconditions:** Condition was not restored within allowed recovery period.
* **Postconditions:** Chamber dims lights, engages maximum protective controls, and sends alert to operator.

---

### Schema 10: Suspend_For_Vibration
* **Description:** Halts potentially dangerous actions when ground shaking or physical movement is detected.
* **Inputs:** `vibration_sensor_reading`, `max_vibration_threshold`.
* **Outputs:** `SUSPEND_MOTORS` command, stabilization timer reset.
* **Preconditions:** `vibration_sensor_reading > max_vibration_threshold`.
* **Postconditions:** All vibration-generating components are paused; system waits for movement to stop.

---

### Schema 11: Verify_Vibration_Stability
* **Description:** Confirms vibration has stayed completely below the safety threshold for the full stabilization period.
* **Inputs:** `current_vibration`, `stabilization_clock`, `required_stabilization_time`.
* **Outputs:** Status flag (`VIBRATION_STABILIZED` or `STILL_UNSTABLE`).
* **Preconditions:** Shaking stopped (`current_vibration <= max_vibration_threshold`).
* **Postconditions:**
  * If vibration stays low until timer hits zero, system permits return to normal conservation.
  * If shaking occurs before timer finishes, stabilization timer restarts from zero.

---

### Schema 12: Switch_To_Backup_Power
* **Description:** Swaps power line to internal emergency battery upon main electricity loss.
* **Inputs:** `main_power_sensor == OFF`, `battery_reserve_level`.
* **Outputs:** `POWER_SOURCE_BATTERY` command.
* **Preconditions:** Main power loss detected during active operation.
* **Postconditions:**
  * If emergency battery has charge, system continues operating on backup.
  * If backup battery is dead or missing, triggers `Execute_Safe_Shutdown`.

---

### Schema 13: Execute_Safe_Shutdown
* **Description:** Preserves safety logs and gracefully turns off hardware when all power sources fail.
* **Inputs:** `main_power_sensor == OFF`, `emergency_power_available == FALSE`.
* **Outputs:** System incident log entry, `SHUTDOWN_SIGNAL`.
* **Preconditions:** No main power and no emergency power.
* **Postconditions:** Power-failure event written to permanent flash storage; components set to fail-safe positions.

---

### Schema 14: Authorize_Artifact_Removal
* **Description:** Confirms the chamber environment is safe before allowing an operator to remove the artifact.
* **Inputs:** Operator removal request, `protection_mode_active`, `chamber_safety_status`.
* **Outputs:** Door lock release signal (`UNLOCK_DOOR`) or access denial error.
* **Preconditions:** Removal request received from authorized operator.
* **Postconditions:** Door lock releases only if safety status is verified and no emergency response is active.
