# Task 1: System Operations

*Note: As required, all operation names use action verbs and do not use system state names.*

| Op ID | Operation Name | Description |
| :---: | :--- | :--- |
| **OP-01** | `Perform_Self_Check` | Tests all hardware sensors and environmental control units during system startup. |
| **OP-02** | `Register_Artifact_Profile` | Reads and stores the artifact ID along with its safe limits for temperature, humidity, light, and vibration. |
| **OP-03** | `Detect_Door_Status` | Checks whether the chamber door is fully closed or currently open. |
| **OP-04** | `Read_Environment_Sensors` | Continuously collects live data for temperature, humidity, light, and vibration levels. |
| **OP-05** | `Start_Conservation_Cycle` | Initiates active conservation control once the door is closed and profile is loaded. |
| **OP-06** | `Adjust_Temperature` | Powers on heating or cooling units when temperature goes outside allowed limits. |
| **OP-07** | `Adjust_Humidity` | Activates humidifier or dehumidifier when humidity drifts outside permitted limits. |
| **OP-08** | `Verify_Condition_Restored` | Reads sensors after a correction attempt to confirm environmental values have returned to safe ranges. |
| **OP-09** | `Trigger_Protection_Alert` | Dims lights, turns on extra protective systems, and notifies the museum operator when recovery fails. |
| **OP-10** | `Suspend_For_Vibration` | Immediately pauses risky mechanical tasks when shaking or physical vibration is detected. |
| **OP-11** | `Verify_Vibration_Stability` | Verifies that vibration stays below the safety threshold for the full duration of the stabilization timer. |
| **OP-12** | `Switch_To_Backup_Power` | Detects main electricity loss and switches immediately to emergency battery power. |
| **OP-13** | `Execute_Safe_Shutdown` | Logs an emergency power-failure incident and safely shuts down systems if backup power is dead. |
| **OP-14** | `Authorize_Artifact_Removal` | Confirms the chamber is safe and clear of alarms before unlocking the door for the operator. |
