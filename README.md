# yhcsk.github.io

5190 final project
[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/a-5mB3GB)

**Team Number:** 29

**Team Name:** Three Musketeers of Upenn

| Team Member Name | Email Address         |
| ---------------- | --------------------- |
| Yishu Wang       | wang78@seas.upenn.edu |
| Shankai Chen     | skchen@seas.upenn.edu |
| Yonggeng Wu      | wuyg7@seas.upenn.edu  |

**GitHub Repository URL:** https://github.com/upenn-embedded/final-project-f25-future-lab-three-musketeers-of-upenn.git

**GitHub Pages Website URL:** [for final submission]*

## Final Project Proposal

### 1. Abstract

This project presents an **Automatic Fire Fighting System** using an  **MCU** ,  **flame sensors** , and a  **water pump mechanism** . It can automatically detect fire through infrared flame sensors, activate an alarm, and spray water toward the detected flame area. It aims to minimize human intervention during fire emergencies in small indoor spaces.

### 2. Motivation

Fire outbreaks in homes and offices cause severe property damage and loss of life each year. Conventional fire alarm systems only provide alerts without active response. The motivation of this project is to develop a **low-cost, automatic fire suppression system** that can both **detect** and **extinguish** fire immediately using accessible components.

The intended purpose is to provide an **autonomous early-response safety device** suitable for small rooms, labs, or workshops.

### 3. System Block Diagram

<<<<<<< HEAD
*Show your high level design, as done in WS1 and WS2. What are the critical components in your system? How do they communicate (I2C?, interrupts, ADC, etc.)? What power regulation do you need?*

![1761438461446](image/README/图片1.png)

### 4. Design Sketches

*What will your project look like? Do you have any critical design features? Will you need any special manufacturing techniques to achieve your vision, like power tools, laser cutting, or 3D printing?  Submit drawings for this section.*

![1761449165333](image/README/1761449165333.png)

### 5. Software Requirements Specification (SRS)

*Formulate key software requirements here. Think deeply on the design: What must your device do? How will you measure this during validation testing? Create 4 to 8 critical system requirements.*

*These must be testable! See the Final Project Manual Appendix for details. Refer to the table below; replace these examples with your own.*

**5.1 Definitions, Abbreviations**

**PWM:** Pulse Width Modulation — used to control servo motor rotation angle and water pump timing.

**Threshold Value:** A predefined ADC limit used to decide whether a flame is detected.

**Interrupt**: Software mechanism triggered by sensor input to perform immediate response (e.g., fire detected).

**Serial Monitor**: The serial communication interface used for debugging and data output.

**Timer**: Internal counter used for periodic sampling and timing control of fire detection and water spraying.

**5.2 Functionality**

| ID     | Description                                                                                                                                                                                                                                                                                              |
| ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| SRS-01 | The**MCU——ATmega328PB** shall continuously read the flame sensor analog signal every**100 milliseconds (±10 ms)** and convert it <br />to a 10-bit ADC value.                                                                                                                           |
| SRS-02 | **PWM control signals**: The MCU shall send PWM control signals to the servo motors to adjust the nozzle direction toward the flame's <br />estimated position.                                                                                                                                   |
| SRS-03 | **Water pump spraying**: The system shall keep the water pump running for at least 5 seconds or until no flame is detected for  3 <br />consecutive readings .                                                                                                                                   |
| SRS-04 | **Flame detection:** When the analog reading exceeds the predefined flame threshold, the MCU shall trigger a **fire detection event.**<br />When the flame sensor reading returns below the threshold, the MCU shall deactivate the relay and stop the water pump within 1 <br />second. |
| SRS-05 | **Servo angle control**: The system shall have the ability to adjust the servomotors to any angle thus they can detect and extinguish the<br />fire from any direction.                                                                                                                            |
| SRS-06 | The MCU shall output all sensor readings, threshold comparisons, and pump status through the**serial monitor** for debugging <br />and validation.                                                                                                                                                 |

### 6. Hardware Requirements Specification (HRS)

*Formulate key hardware requirements here. Think deeply on the design: What must your device do? How will you measure this during validation testing? Create 4 to 8 critical system requirements.*

*These must be testable! See the Final Project Manual Appendix for details. Refer to the table below; replace these examples with your own.*

**6.1 Definitions, Abbreviations**

**LM2596**: DC-DC buck converter that steps down 12 V input to a stable 5 V output for logic and servo modules.

**KY-026**: Flame sensor detecting fire presence and intensity via analog voltage and digital threshold outputs.

**6.2 Functionality**

| ID     | Description                                                                                                                                                                                                             |
| ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| HRS-01 | The flame sensor (KY-026) shall detect fire within a range of at least**30 cm** , providing both analog intensity and digital threshold <br />outputs.                                                            |
| HRS-02 | The LM2596 voltage regulator shall convert a**12 V DC input** into a stable **5 V output** (±0.1 V) with a current capacity of  **≥ 2 A** , <br />sufficient for powering servos and logic modules. |
| HRS-03 | Each servo motor shall rotate up to**180°** with a minimum stall torque of  **1.8 kg·cm** , used to adjust nozzle direction or mechanical <br />aiming.                                                   |
| HRS-04 | The relay module shall operate using a**5 V control signal** from the MCU and switch **12 V **DC** to the water pump with a load <br />current up to  **2 A** .**                              |
| HRS-05 | The water pump shall deliver a minimum flow rate of**1 L/min** at  **12 V DC** , sufficient to extinguish small flames within 10 seconds.                                                                   |
| HRS-06 | The 12 V DC power supply shall provide a stable output of 12 V (±0.5 V) and a continuous current of at least 2 A to support all<br />active modules simultaneously.                                                    |

### 7. Bill of Materials (BOM)

*What major components do you need and why? Try to be as specific as possible. Your Hardware & Software Requirements Specifications should inform your component choices.*

*In addition to this written response, copy the Final Project BOM Google Sheet and fill it out with your critical components (think: processors, sensors, actuators). Include the link to your BOM in this section.*

[Final Project BOM](https://docs.google.com/spreadsheets/d/1RM9a7PHD03kyyGJe7QMgT_bWAH1gc4iGDQpE_juBU_E/edit?gid=1362749064#gid=1362749064)

When selecting specific components, our primary considerations are: flame detection latency, control stability, water spray coverage effectiveness, electrical safety, cost, and assembly complexity. The following are key components determined based on these factors.

**Control and Processing**

* **stm32L406 (Main Controller)**

Reason: Uniformly distributed for the course, facilitating development and debugging. We run bare-metal C firmware on it, responsible for: multi-channel flame sensor sampling (≥50 Hz), servo scanning control, relay driving, water pump start/stop logic, and serial debugging output. The onboard USB power supply and debugging interface also reduce prototype complexity.

**Sensing and Detection**

* **Infrared Flame Sensor Modules ×3**

Reason: Modular design (with comparator and sensitivity potentiometer) provides direct digital signal output. Three probes arranged in a fan pattern cover approximately 120° of the forward area, delivering stable response to lighter flames within 0.5–1.5 m. Multi-channel redundancy enables simple majority voting filtering to reduce false alarms.

**Actuation and Drive**

* **SG90 Servo** 

Reason: High torque sufficient to drive a rotating platform mounting nozzles and hoses; standard 50 Hz PWM control easily generated by ATmega. Used to achieve 0–180° horizontal scanning and automatic targeting of fire sources.

* **Mini Submersible Pump (12 V, approx. 3–5 W)**

Reason: Compact size and moderate flow rate suitable for desktop demonstrations; forms a continuous water column within 0.5–1 m range, providing sufficient impact to extinguish small alcohol lamps or lighter flames.

* **Dual-Channel Relay Module**

Reason: One channel switches the water pump power supply; the other channel is reserved for future expansion (e.g., controlling backup fans or indicator light strips). The module incorporates optocoupler isolation and flywheel diodes, enhancing electrical isolation and protection for the MCU.

**Power Supply and Conditioning**

* **12 V DC Adapter / Lab Power Supply**

Reason: Provides sufficient operating voltage for the water pump while maintaining adequate power margin. Works with the downstream buck module to deliver stable 5 V power to the control section.

* **LM2596 Buck Module ×2**

Reason: Proven stability and easy regulation. One module steps down 12 V to 5 V for the ATmega, flame sensor, and servos; Another module independently powers future expansions (e.g., additional sensors or indicator lights), enabling power segmentation and reducing interference from high-current loads on logic circuits.

### 8. Final Demo Goals

*How will you demonstrate your device on demo day? Will it be strapped to a person, mounted on a bicycle, require outdoor space? Think of any physical, temporal, and other constraints that could affect your planning.*

The device will be demonstrated  indoors , on a fire-safe testing platform. The system will be mounted on a stationary base that holds the  servo-controlled nozzle ,  flame sensors , and  water reservoir and pump assembly . First, a small controlled flame will be placed at different positions within a 30–50 cm range to the sensor. When the flame is detected by the flame sensors, the system will trigger a  buzzer and LED alarm, use the pan-tilt servos to aim the nozzle toward the detected direction and activate the pump to spray water for at least 5 s or until the flame is extinguished.Once the flame is no longer detected for 3 consecutive samples, the system automatically stops the pump and resets to standby mode.

### 9. Sprint Planning

*You've got limited time to get this project done! How will you plan your sprint milestones? How will you distribute the work within your team? Review the schedule in the final project manual for exact dates.*

| Milestone                     | Functionality Achieved                                                                                                                                                                                                                                   | Distribution of Work                                                                                                                                                                                       |
| ----------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Sprint #1<br />(10/27~11/7)   | Basic sensor & control functionality<br />– Flame sensor (KY-026) reading via ADC<br />– Threshold detection and serial debug output<br />– Buzzer + LED alert logic<br />– Pump on/off control via relay or MOSFET                                  | A: build flame sensor + relay + pump circuit, verify power regulation (LM2596)<br />B: write ADC + threshold detection + serial output code.<br />C: test modules together on breadboard; document results |
| Sprint #2<br />(11/8~11/21)   | Servo aiming subsystem + safety logic<br />– Dual servo pan/tilt control using PWM<br />– Nozzle tracking simulation with LEDs<br />– Automatic extinguish routine (spray 5 s or until no flame × 3)<br />– Emergency stop & overcurrent protection | **A** : mount servos, design mechanical bracket.<br />**B** : integrate PWM servo code with flame detection.<br />**C** : run full-system test (sensing → aim → spray → stop).        |
| MVP Demo<br />(week of 11/22) | Minimal Viable Product working end-to-end:<br />– Detects flame, rotates nozzle, sprays water, stops when extinguished.<br />– Manual reset and safe shutdown verified.                                                                                | **A :** ensures mechanical stability & wiring,<br />**B :** handles firmware tuning (thresholds/timing),<br />**C :** prepares test plan, video, and poster draft.                       |
| Final Demo<br />(12/3~12/4)   |                                                                                                                                                                                                                                                          | **A:** finalize hardware packaging & power wiring.<br />**B:** optimize code (debouncing, servo motion smoothing).<br />C:                                                                     |

| **C:**lead presentation, compile final documentation, manage demo safety & logistics. |
| ------------------------------------------------------------------------------------- |

|  |
| - |

**This is the end of the Project Proposal section. The remaining sections will be filled out based on the milestone schedule.**

## Sprint Review #1

### Last week's progress

During the first week, the project focused on initiation, planning, and architecture design. These tasks established the technical foundation for later hardware and firmware development.

**Project Initiation & Role Assignment**

* Defined team roles and responsibilities.
* Reviewed the complete project scope and the six SRS requirements (SRS-01 to SRS-06).

**System Architecture Design**

* Designed the system architecture consisting of three major modules:

  **Flame Detection → Servo Direction Control → Water Pump Module** .
* Created hardware block diagrams and wiring plans.
* Developed the initial software flowchart (ADC → threshold → servo → pump).

**Preliminary Technical Exploration**

Although Week 1 was mainly for planning, early exploration of the firmware framework began, including:

* Multi-channel ADC + DMA structure for sensor acquisition
* Basic PWM control concepts for servo motors
* Feasibility analysis of scanning algorithms

### Current state of project

At the end of this Week , the project is in the  **pre-hardware-integration stage** , with most efforts focused on design and firmware framework preparation.

**Clear System Structure and Module Responsibilities**

* ADC module: multi-sensor flame data collection
* Servo module: directional control (yaw + pitch)
* Pump module: relay-based water activation
* Software logic defined as:

  Detection → Targeting → Extinguishing → Reset/Patrol

**Initial Firmware Framework Developed**

Already provides prototypes for several core modules:

* 4-channel ADC + DMA acquisition framework
* PWM servo control (Yaw + Pitch)
* Two-dimensional scanning algorithm (pitch + yaw sweep)
* Integrated main loop prototype (scan → target → extinguish

### Next week's plan

Next week we will focus on:

**Hardware Assembly and Verification**

* Assemble flame sensors, servos, relay, and water pump on breadboard.
* Connect STM32 to all modules (signal + power).
* Verify grounding, supply stability, and isolation.

**Flame Sensor ADC Testing**

* Begin reading ADC values from the flame sensor.
* Check stability, noise level, and linear response.
* Validate correctness of ADC conversion on PA0–PA3.

**Servo PWM Control Verification**

* Test basic PWM timing output for two servos.
* Ensure smooth left-right (yaw) and up-down (pitch) movements.
* Verify no jitter or irregular motion.

**Relay + Water Pump Activation Test**

* Test the control chain:

  **MCU → Relay → Water Pump**
* Confirm relay switching reliability.
* Test water pump operation under 12V supply.

**Build the Initial Firmware Framework**

* Create the basic project structure in main.c.
* Add function skeletons for major modules (ADC, servo, pump).
* Implement the main loop framework (logic placeholders only).

## Sprint Review #2

### Last week's progress

During the previous sprint, significant progress was made on the core hardware and software modules of the Smart Fire-Fighting System. Each subsystem was optimized, tested independently, and prepared for integration.

**Servo Motor Drive Module**

* Completed GPIO multiplexing and configuration for PWM output.
* Achieved stable and smooth control of servo angle for directional flame targeting.
* Verified that both upper and lower servos respond correctly to PWM commands.

**Flame Sensor Module**

* Fully implemented the flame sensor data acquisition module.
* Configured **ADC1_IN5 (PA0)** as an analog input and enabled ASCR (the analog switch required for the STM32L4 series).
* Verified correct ADC response:
  * **0V → ~0 ADC** ,
  * **3.3V → ~4091 ADC** ,
  * Stable ADC variation with changes in flame intensity.
* Ensured reliable, noise-free readings for future threshold detection logic.

**Water Pump Driver Module**

* Completed the full circuit design and hardware implementation of the water pump driver.
* Implemented relay-based isolation:
  * STM32 outputs 3.3 V to drive relay coil.
  * Relay closes → 12 V powers the water pump.
* Verified correct actuation:  **MCU → Relay → Pump** , demonstrating reliable activation and shutdown control.

### Current state of project

The project is now transitioning from modular development to  **full system-level integration** . All major hardware blocks—servo control, sensor acquisition, and pump actuation—are functioning independently and have been validated.

The current focus is:

* Integrating all modules into a unified firmware framework.
* Implementing the required SRS items for Week 3 of the project timeline, including:
  * **SRS-01:** ADC sampling every 100 ms
  * **SRS-02/05:** Servo angle determination for directional extinguishing
  * **SRS-03:** Pump activation for 5 seconds or until safe readings detected
  * **SRS-04:** Flame threshold detection logic
  * **SRS-06:** Serial output logging

At this stage, the system can read sensor data, move servos, and drive the pump reliably. The upcoming work ties all of this together into a functional autonomous fire-fighting unit.

### Next week's plan

Next sprint will focus on completing the core system behavior and ensuring smooth interaction among subsystems.

**Planned Tasks**

1. **Full Firmware Integration**
   * Connect flame sensor readings → threshold logic → servo positioning → pump triggering.
   * Implement state machine:  *Idle → Detecting → Aiming → Extinguishing → Safe* .
2. **Directional Flame Localization**
   * Implement servo scanning routine (horizontal + vertical).
   * Bind ADC readings to angle tracking to determine flame position.
3. **Safety and Reliability Logic**
   * Add conditions to avoid false triggers.
   * Use temporal filtering (e.g., consecutive readings) to confirm flame presence.
4. **System Debugging + Serial Output**
   * Print ADC values, servo angles, and pump states for debugging.
   * Verify pump timing and extinguishing logic.
5. **Final Integration Testing**
   * End-to-end test with real flame source.
   * Adjust threshold parameters for stable performance.

## MVP Demo

1. Show a system block diagram & explain the hardware implementation.
2. Explain your firmware implementation, including application logic and critical drivers you've written.
3. Demo your device.
4. Have you achieved some or all of your Software Requirements Specification (SRS)?

   1. Show how you collected data and the outcomes.
5. Have you achieved some or all of your Hardware Requirements Specification (HRS)?

   1. Show how you collected data and the outcomes.
6. Show off the remaining elements that will make your project whole: mechanical casework, supporting graphical user interface (GUI), web portal, etc.
7. What is the riskiest part remaining of your project?

   1. How do you plan to de-risk this?
8. What questions or help do you need from the teaching team?

## Final Project Report

Don't forget to make the GitHub pages public website!
If you’ve never made a GitHub pages website before, you can follow this webpage (though, substitute your final project repository for the GitHub username one in the quickstart guide):  [https://docs.github.com/en/pages/quickstart](https://docs.github.com/en/pages/quickstart)

### 1. Video

[Insert final project video here]

* The video must demonstrate your key functionality.
* The video must be 5 minutes or less.
* Ensure your video link is accessible to the teaching team. Unlisted YouTube videos or Google Drive uploads with SEAS account access work well.
* Points will be removed if the audio quality is poor - say, if you filmed your video in a noisy electrical engineering lab.

### 2. Images

[Insert final project images here]

*Include photos of your device from a few angles. If you have a casework, show both the exterior and interior (where the good EE bits are!).*

### 3. Results

*What were your results? Namely, what was the final solution/design to your problem?*

#### 3.1 Software Requirements Specification (SRS) Results

*Based on your quantified system performance, comment on how you achieved or fell short of your expected requirements.*

*Did your requirements change? If so, why? Failing to meet a requirement is acceptable; understanding the reason why is critical!*

*Validate at least two requirements, showing how you tested and your proof of work (videos, images, logic analyzer/oscilloscope captures, etc.).*

| ID     | Description                                                                                               | Validation Outcome                                                                          |
| ------ | --------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| SRS-01 | The IMU 3-axis acceleration will be measured with 16-bit depth every 100 milliseconds +/-10 milliseconds. | Confirmed, logged output from the MCU is saved to "validation" folder in GitHub repository. |

#### 3.2 Hardware Requirements Specification (HRS) Results

*Based on your quantified system performance, comment on how you achieved or fell short of your expected requirements.*

*Did your requirements change? If so, why? Failing to meet a requirement is acceptable; understanding the reason why is critical!*

*Validate at least two requirements, showing how you tested and your proof of work (videos, images, logic analyzer/oscilloscope captures, etc.).*

| ID     | Description                                                                                                                        | Validation Outcome                                                                                                      |
| ------ | ---------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| HRS-01 | A distance sensor shall be used for obstacle detection. The sensor shall detect obstacles at a maximum distance of at least 10 cm. | Confirmed, sensed obstacles up to 15cm. Video in "validation" folder, shows tape measure and logged output to terminal. |
|        |                                                                                                                                    |                                                                                                                         |

### 4. Conclusion

Reflect on your project. Some questions to address:

* What did you learn from it?
* What went well?
* What accomplishments are you proud of?
* What did you learn/gain from this experience?
* Did you have to change your approach?
* What could have been done differently?
* Did you encounter obstacles that you didn’t anticipate?
* What could be a next step for this project?

## References

Fill in your references here as you work on your final project. Describe any libraries used here.
