# Sole Sense
## Low-Cost, Edge-Intelligent Pressure-Sensing Sandals for Gait Analysis and Human Activity Recognition in Healthcare

**Project research and explanation document**  
**Prepared:** 3 November 2026

---

## 1. Project in one paragraph

Sole Sense is a proposed smart sandal that measures pressure under the foot while a person stands, walks or performs other activities. Thin pressure sensors are placed at selected locations in the sandal's footbed, such as the heel, midfoot and forefoot/toe region. A small embedded processor reads the sensors, filters the measurements and extracts useful patterns. A locally running algorithm (edge intelligence) can then estimate gait events and classify activities such as standing, walking or running. The results may be shown on a phone or computer for rehabilitation and mobility monitoring.

**Important:** This is a monitoring and research aid, not a standalone medical diagnostic device. A prototype must be calibrated and validated against an appropriate reference system before clinical claims can be made.

## 2. Meaning of every term in the title

| Term | Simple explanation |
|---|---|
| Sole Sense | A sandal/footbed that senses what happens under the sole of the foot. |
| Low-cost | Uses affordable, accessible components and a simple mechanical design. |
| Edge | The computation happens on or close to the wearable device rather than requiring every raw sample to be sent to the internet. |
| Intelligent | The device uses rules or a trained machine-learning model to interpret sensor patterns. |
| Pressure-sensing | Measures changes in force/pressure applied by different parts of the foot. |
| Sandals | The wearable platform that holds the sensors, electronics and battery. |
| Gait | The pattern of walking: how the feet contact the ground, support the body and move forward. |
| Gait analysis | Measuring and studying walking characteristics such as step timing, stance time and pressure distribution. |
| Human Activity Recognition (HAR) | Automatically identifying activities, for example standing, walking, running, sitting-to-standing or climbing stairs. |
| Healthcare | Using movement information to support mobility monitoring, rehabilitation and clinical assessment. |

## 3. How the complete system works

1. **Foot loads the sandal:** During each step, the heel, midfoot and forefoot apply changing forces to the footbed.
2. **Sensors respond:** Each pressure sensor changes its electrical output when force is applied.
3. **Signal conditioning:** A resistor divider or suitable analog front end converts the sensor response into a voltage that can be measured.
4. **Data acquisition:** The microcontroller samples each channel and attaches a timestamp.
5. **Pre-processing:** Software filters noise, checks sensor health and segments the signal into time windows or steps.
6. **Feature extraction:** The software calculates useful quantities such as peak pressure response, contact duration, pressure ratios and step interval.
7. **Inference:** Rules or a trained model estimate gait events and classify the activity.
8. **Output:** A phone/computer dashboard can display activity, step count, trends and warnings about unusual readings.

### System block diagram

```text
Foot pressure
     |
     v
Pressure sensors in sandal
(heel / midfoot / forefoot)
     |
     v
Signal conditioning + ADC
     |
     v
Microcontroller / edge processor
     |
     +--> Filtering and calibration
     |
     +--> Gait-event detection
     |
     +--> Feature extraction
     |
     +--> Activity classification
     |
     v
Local result / Bluetooth / Wi-Fi
     |
     v
Mobile or PC dashboard / data log
```

## 4. Gait cycle: the key biomechanics

A **gait cycle** is one complete cycle for one foot, from one contact of that foot with the ground to the next contact of the same foot.

It is commonly divided into:

- **Stance phase:** The foot is on the ground and supports body weight. It is approximately 60% of a typical walking cycle.
- **Swing phase:** The foot is off the ground and moves forward. It is approximately 40% of a typical walking cycle.

The exact timing varies with walking speed, the person and the definition used. The percentages are typical descriptions, not fixed values for every person.

| Gait event / phase | Simple meaning | What a pressure insole may observe |
|---|---|---|
| Initial contact / heel strike | Heel first touches the ground. | Heel sensor begins responding. |
| Loading response | Body weight transfers onto the foot. | Pressure rises as the foot accepts load. |
| Mid-stance | Body moves over the supporting foot. | Pressure is distributed across the foot. |
| Terminal stance / heel rise | Heel lifts while the body moves forward. | Heel pressure falls; forefoot loading becomes more prominent. |
| Pre-swing | The foot prepares to leave the ground. | Forefoot/toe pressure may remain before toe-off. |
| Toe-off | Toes leave the ground. | Pressure falls towards the unloaded state. |
| Swing | Foot moves forward without ground contact. | Most plantar pressure sensors show little or no ground-contact load. |

**Why this matters:** The order and timing of heel loading, forefoot loading and unloading help identify steps and describe a person's walking pattern. Pressure alone does not measure every joint angle or the full movement of the leg.

Reference: [Biomechanics of Normal Gait – AAPM&R KnowledgeNow](https://now.aapmr.org/biomechanics-normal-gait/) and [Gait-cycle phases – open-access review](https://pmc.ncbi.nlm.nih.gov/articles/PMC13418446/).

## 5. Sensor technologies

### 5.1 Force Sensitive Resistor (FSR) / piezoresistive sensor

**What it is:** A thin sensor whose electrical resistance changes when force is applied. In common FSRs, resistance generally decreases as applied force increases.

**How it works:** Pressure changes the contact between conductive layers inside the sensor. The resistance changes, and a voltage-divider circuit turns that change into a measurable voltage.

**Possible use in Sole Sense:** Place one sensor under the heel, one or more under the metatarsal/forefoot region and optionally one under the toe or midfoot.

**Advantages**
- Thin and relatively easy to integrate.
- Simple analog interface.
- Useful for detecting contact and relative changes in loading.
- Commonly used in prototype smart insoles.

**Limitations**
- Output is not automatically an accurate pressure value in kPa.
- Response can be nonlinear and affected by loading history, temperature, mounting and sensor-to-sensor variation.
- Requires calibration in the actual sandal structure.
- A small sensing area does not measure the pressure distribution over the entire foot.

**Example part:** Interlink FSR 402. Check its actual operating range and mechanical dimensions before selecting it; the manufacturer lists a nominal sensing range of 0.2–20 N for the 400 series.

Photo / product details: [Interlink FSR 402 product page](https://www.interlinkelectronics.com/products/fsr-model-402)  
Datasheet: [FSR 400 Series datasheet (PDF)](https://www.interlinkelectronics.com/downloads/datasheets/fsr-400-series-datasheet.pdf)

### 5.2 Capacitive pressure sensor

**What it is:** A sensor that detects a change in capacitance when pressure changes the distance or geometry between conductive layers.

**Possible use:** Measuring pressure at selected foot regions. Capacitive sensors can be suitable for thin, flexible sensing structures, but the electronics and mechanical construction can be more involved.

Photo / technical background: [Plantar pressure measurement in diabetic foot disease – scoping review](https://pmc.ncbi.nlm.nih.gov/articles/PMC11292392/).

### 5.3 Piezoelectric sensor

**What it is:** A material that generates an electrical charge when mechanically stressed.

**Possible use:** Detecting dynamic events such as impact or a changing load during a step.

**Limitation:** Many piezoelectric sensors are better suited to changing/dynamic force than to measuring a constant static load for a long time. The interface circuit must be designed for the sensor type.

Background: [In-shoe plantar pressure measurement technologies – systematic review](https://pmc.ncbi.nlm.nih.gov/articles/PMC11064085/).

### 5.4 Which sensor should a low-cost prototype start with?

A small FSR-based prototype is a practical starting point because it is relatively easy to interface. Treat the first version as a **pressure-pattern/contact detector**, not a medical-grade pressure map. If accurate plantar-pressure measurement is required, compare calibrated sensor technologies and validate them against a reference pressure system.

## 6. Suggested sensor locations

A basic prototype may start with three to five sensing zones per foot:

| Region | Why place a sensor there? |
|---|---|
| Heel | Detects initial contact and heel loading. |
| Midfoot | Helps observe midfoot loading and pressure transfer. |
| First metatarsal / ball of foot | Detects forefoot loading during push-off. |
| Fifth metatarsal / outer forefoot | Helps compare inner and outer forefoot loading. |
| Big toe / toe region | Helps detect the final push-off before toe-off. |

These are suggested prototype locations, not a universal clinical layout. Sensor placement should be based on the research question, sandal geometry and reference measurements.

Visual references:
- [Smart-insole pressure sensor placement and technologies – review](https://pmc.ncbi.nlm.nih.gov/articles/PMC10452488/)
- [In-shoe pressure measurement technologies – review](https://pmc.ncbi.nlm.nih.gov/articles/PMC11064085/)

## 7. Electronics and embedded components

| Component | Function in the project | Notes |
|---|---|---|
| FSR / pressure sensor | Converts applied force into a resistance change. | One channel per sensing zone. |
| Fixed resistor / voltage divider | Converts resistance change into voltage change. | Select resistor value after checking the sensor and ADC range. |
| ADC | Converts analog voltage into a digital number. | Check resolution, input range, noise and sampling rate. |
| ESP32 or similar MCU | Reads sensors, timestamps data and runs processing. | ESP32 includes ADC and wireless connectivity; exact features depend on chip/module variant. |
| Battery and regulator | Powers the wearable. | Include safe charging/protection and a stable regulated supply. |
| Bluetooth Low Energy (BLE) | Sends selected results to a phone. | Useful when low-power wireless communication is needed. |
| Wi-Fi | Sends data to a local network or server. | Usually consumes more power than BLE during transmission. |
| Enclosure / flexible footbed | Holds and protects sensors and wiring. | Must avoid uncomfortable pressure points and wire fatigue. |

### ESP32 notes

The classic ESP32 datasheet documents a 12-bit SAR ADC and Wi-Fi/Bluetooth capabilities. ADC readings can be affected by nonlinearity, attenuation settings, noise and board design. Calibrate the complete measurement chain; do not assume raw ADC counts equal pressure.

Official documentation: [Espressif ESP32 Series Datasheet](https://documentation.espressif.com/esp32_datasheet_en.html).

### Basic sensor interface concept

A common FSR circuit uses a voltage divider:

```text
3.3 V
  |
 [FSR]
  |
  +------ ADC input
  |
 [Fixed resistor]
  |
 GND
```

The voltage-divider output changes as the FSR resistance changes. Swapping the FSR and fixed resistor changes the direction of the voltage response. Use the correct circuit for the selected sensor and keep the ADC input within its permitted voltage range.

## 8. Data acquisition and signal processing

### Sampling

Sampling means reading each sensor repeatedly at a chosen rate. For example, 50 samples per second means each channel is read 50 times every second. The correct rate depends on the fastest event to capture and the sensor/processor capability. A prototype can test several rates and compare missed events, noise and power use.

### Filtering

Filtering reduces unwanted fluctuations:
- **Moving average:** averages recent samples; simple, but can smooth sharp events.
- **Median filter:** rejects isolated spikes.
- **Low-pass filter:** reduces rapid noise while retaining slower pressure changes.

Do not over-filter: heel strike and toe-off are relatively quick events and may become blurred by excessive smoothing.

### Calibration

Calibration maps sensor output to a known applied load or reference measurement.

A simple per-sensor calibration procedure:
1. Record the unloaded sensor value.
2. Apply several known loads using a repeatable fixture.
3. Record the output at each load.
4. Repeat loading and unloading to check repeatability and hysteresis.
5. Fit an appropriate calibration curve or lookup table.
6. Repeat after mounting the sensor in the actual sandal.

For an FSR, a single straight-line conversion may not be sufficiently accurate over the whole range. If the project only needs activity recognition, normalized sensor features may be more useful than claiming absolute pressure.

### Data logging

Store records with timestamps and sensor labels, for example:

```csv
timestamp_ms,heel,midfoot,forefoot,toe
0,120,45,20,10
20,150,50,25,12
40,210,75,40,18
60,260,110,75,30
```

The values above are **illustrative ADC-like readings only**, not real measurements or calibrated pressure values.

## 9. Gait parameters that can be estimated

| Parameter | Meaning | Basic estimation idea |
|---|---|---|
| Step count | Number of detected steps. | Detect repeated foot-contact patterns. |
| Step time | Time between consecutive steps. | Difference between detected contact timestamps. |
| Stance time | Time the foot is loaded/on the ground. | Contact start to toe-off/unloading. |
| Swing time | Time the foot is off the ground. | Toe-off to next initial contact of same foot. |
| Stride time | Time between two successive contacts of the same foot. | Difference between same-foot initial contacts. |
| Left-right loading difference | Difference between corresponding left/right sensor features. | Compare calibrated or normalized measurements. |
| Pressure distribution pattern | How load is shared among sensor locations. | Compare sensor responses across foot regions. |

**Caution:** Step length, walking speed and joint kinematics cannot be reliably calculated from a few pressure sensors alone without additional assumptions or sensors (for example, inertial sensors) and validation.

## 10. Human Activity Recognition (HAR)

HAR means identifying an activity from sensor data.

### Example activity classes

- Standing
- Walking
- Running
- Sitting
- Sit-to-stand transition
- Stair ascent / descent (if sufficient data and validation are available)

### How a machine-learning pipeline works

1. **Collect labelled data:** Record sensor data while participants perform known activities.
2. **Label the activity:** Mark which time intervals correspond to walking, standing, etc.
3. **Segment into windows:** Split continuous data into short overlapping or non-overlapping chunks.
4. **Extract features:** Calculate mean, standard deviation, maximum, minimum, range, signal energy, contact duration and sensor ratios.
5. **Train a model:** Try a simple classifier such as Decision Tree, Random Forest or Support Vector Machine (SVM).
6. **Evaluate:** Test on participants or sessions not used for training.
7. **Deploy:** Convert the selected model into a format suitable for the edge device.
8. **Run locally:** The device classifies new windows without needing a constant cloud connection.

A published smart-insole study investigated pressure and inertial sensors for six activities. It reported that combining pressure and inertial sensors achieved its best reported accuracy of 94.66% in that study's experimental setup. This is **not a guaranteed accuracy for Sole Sense**; performance depends on participants, sensor placement, data collection and test methodology.

Research reference: [Assessing Impact of Sensors and Feature Selection in Smart-Insole-Based Human Activity Recognition (2022)](https://pmc.ncbi.nlm.nih.gov/articles/PMC9230734/).

## 11. What does Edge Intelligence mean here?

**Cloud-based approach:** The sandal sends raw sensor readings to a remote server; the server analyses them and returns results.

**Edge-based approach:** The sandal's microcontroller or nearby processor performs at least some processing and activity recognition locally.

Benefits:
- Can work without continuous internet access.
- May reduce the amount of raw personal movement data transmitted.
- Can provide quicker local feedback.
- Can reduce cloud data and connectivity requirements.

Challenges:
- Limited processor memory and computing power.
- Battery-life constraints.
- Model size and inference time.
- Wireless communication and data synchronisation.
- Need to update and validate models safely.

Research background: [Machine Learning Applied to Edge Computing and Wearable Devices for Healthcare – systematic mapping (2024)](https://www.mdpi.com/1424-8220/24/19/6322).

## 12. Optional inertial sensors

A pressure-only sandal can detect loading patterns and some activities. Adding an **IMU (Inertial Measurement Unit)** can improve motion information.

An IMU usually contains:
- **Accelerometer:** measures linear acceleration, including gravity.
- **Gyroscope:** measures angular velocity (how quickly the device rotates).

An IMU can help distinguish dynamic movements and estimate foot orientation/motion. A research smart-insole study found that pressure and inertial sensors provided complementary information for activity recognition.

Do not add an IMU automatically if the project's scope or budget is limited. First decide whether pressure-only sensing meets the required objectives.

## 13. Healthcare applications

| Application | How Sole Sense may help | Important limitation |
|---|---|---|
| Rehabilitation | Track changes in step timing and loading patterns during exercises. | Clinician must interpret results. |
| Elderly mobility monitoring | Observe activity levels and changes in walking patterns. | Not a guaranteed fall detector. |
| Gait assessment support | Provide repeatable pressure/contact measurements. | Must be validated against accepted gait-analysis equipment. |
| Diabetic foot research | Observe pressure patterns that may be relevant to foot-loading studies. | Not a substitute for clinical foot examination or a validated ulcer-risk system. |
| Activity monitoring | Estimate standing, walking and other trained activities. | Classification errors are possible. |
| Left-right comparison | Compare the two feet over time. | Requires matched sensors, calibration and consistent placement. |

## 14. Main engineering challenges

1. **Sensor drift and hysteresis:** Output can change with time and loading history.
2. **Nonlinearity:** FSR resistance does not generally change linearly with force.
3. **Mechanical integration:** The sensor must not create discomfort or a pressure point.
4. **Repeatable placement:** Small placement changes can alter measurements.
5. **Sensor durability:** Repeated bending, sweat and impact can damage sensors or connections.
6. **Power consumption:** Wireless transmission and continuous sampling use battery energy.
7. **Individual differences:** Foot size, body mass, footwear and walking style vary.
8. **Data quality:** Poor labels or too few participants can make a model unreliable.
9. **Generalisation:** A model trained on one person may not work well for another person.
10. **Clinical validation:** Prototype performance must be compared with an appropriate reference method.

## 15. Suggested prototype plan

### Phase 1 — Bench testing
- Test one FSR with a voltage divider.
- Read the output using an ADC.
- Apply repeatable known loads.
- Check repeatability, hysteresis and drift.

### Phase 2 — Single-sandal prototype
- Install three sensors: heel, midfoot and forefoot.
- Add an ESP32, battery and protected wiring.
- Log raw readings with timestamps.
- Check comfort and mechanical stability without risky or prolonged wear tests.

### Phase 3 — Gait event detection
- Record standing and walking trials.
- Identify contact, loading and unloading patterns.
- Estimate step and stance timing.
- Compare against video annotation or a suitable reference device.

### Phase 4 — Activity recognition
- Collect labelled samples from multiple participants and sessions, with informed consent.
- Start with simple features and a baseline classifier.
- Split training and testing by participant where possible to avoid overly optimistic results.
- Report a confusion matrix, precision, recall and F1-score, not accuracy alone.

### Phase 5 — Edge deployment
- Select a model that fits the processor.
- Measure inference time, RAM/flash use and battery life.
- Send summaries to a phone or dashboard.
- Test offline behaviour and data-loss recovery.

## 16. How to evaluate the project

| Evaluation item | What to measure |
|---|---|
| Sensor repeatability | Variation when the same load is applied repeatedly. |
| Calibration error | Difference between estimated and reference load/pressure. |
| Hysteresis | Difference between loading and unloading outputs at the same load. |
| Drift | Change in output over time under a stable load. |
| Step detection | Precision, recall and missed/false steps compared with reference labels. |
| HAR performance | Confusion matrix, precision, recall, F1-score and accuracy. |
| Latency | Time from a sensor window becoming available to a classification result. |
| Battery life | Operating time under the intended sampling and wireless settings. |
| Comfort/durability | User feedback, sensor damage and wiring failures during controlled testing. |

## 17. Photo, diagram and visual-reference links

The following links lead to pages containing relevant product photographs, technical drawings, research figures or diagrams. They are provided as **visual references**, not as proof that the exact pictured hardware is the final Sole Sense design.

1. **FSR 402 sensor photo and product information:**  
   https://www.interlinkelectronics.com/products/fsr-model-402

2. **FSR construction diagram and sensor series information:**  
   https://www.interlinkelectronics.com/force-sensing-resistor

3. **FSR 400-series mechanical drawing and datasheet (PDF):**  
   https://www.interlinkelectronics.com/downloads/datasheets/fsr-400-series-datasheet.pdf

4. **Gait-cycle phases and explanatory figure:**  
   https://pmc.ncbi.nlm.nih.gov/articles/PMC13418446/

5. **Smart-insole sensor technologies and figures:**  
   https://pmc.ncbi.nlm.nih.gov/articles/PMC10452488/

6. **In-shoe pressure-sensing technologies and comparison table:**  
   https://pmc.ncbi.nlm.nih.gov/articles/PMC11064085/

7. **ESP32 chip block diagram, pin and ADC information (official datasheet):**  
   https://documentation.espressif.com/esp32_datasheet_en.html

8. **Smart-insole activity-recognition research:**  
   https://pmc.ncbi.nlm.nih.gov/articles/PMC9230734/

## 18. References and further reading

The sources below support the technical background in this document. Read the full paper or datasheet for detailed specifications and experimental conditions.

1. **Interlink Electronics — FSR Model 402 product page.** Manufacturer information about the sensor, operating principle and product details.  
   https://www.interlinkelectronics.com/products/fsr-model-402

2. **Interlink Electronics — FSR 400 Series Datasheet.** Mechanical dimensions and technical information for the FSR 400 family.  
   https://www.interlinkelectronics.com/downloads/datasheets/fsr-400-series-datasheet.pdf

3. **Espressif Systems — ESP32 Series Datasheet.** Official documentation for processor, ADC, GPIO, Wi-Fi and Bluetooth features.  
   https://documentation.espressif.com/esp32_datasheet_en.html

4. **In-shoe plantar pressure measurement technologies for the diabetic foot: A systematic review.** Review of in-shoe pressure measurement devices, sensor types, sensor placement and acquisition characteristics.  
   https://pmc.ncbi.nlm.nih.gov/articles/PMC11064085/

5. **Insole Systems for Disease Diagnosis and Rehabilitation: A Review.** Overview of insole sensing methods and gait-related measurements.  
   https://pmc.ncbi.nlm.nih.gov/articles/PMC10452488/

6. **Plantar pressure measurement in diabetic foot disease: A scoping review.** Explains plantar-pressure measurement technologies including capacitive, resistive and piezoelectric approaches.  
   https://pmc.ncbi.nlm.nih.gov/articles/PMC11292392/

7. **Assessing Impact of Sensors and Feature Selection in Smart-Insole-Based Human Activity Recognition (2022).** Research on smart-insole sensor combinations, feature selection and activity classification.  
   https://pmc.ncbi.nlm.nih.gov/articles/PMC9230734/

8. **Machine Learning Applied to Edge Computing and Wearable Devices for Healthcare: Systematic Mapping of the Literature (2024).** Review of machine learning, edge computing and wearable healthcare systems.  
   https://www.mdpi.com/1424-8220/24/19/6322

9. **Biomechanics of Normal Gait — AAPM&R KnowledgeNow.** Background on stance, swing and normal gait.  
   https://now.aapmr.org/biomechanics-normal-gait/

10. **Step-by-step insight into gait analysis: A narrative review unlocking knee biomechanics (2026).** Open-access discussion and illustration of gait-cycle phases.  
    https://pmc.ncbi.nlm.nih.gov/articles/PMC13418446/

---

## 19. Short explanation for a presentation or interview

"Sole Sense is a low-cost smart sandal designed to monitor foot-pressure patterns during human movement. Pressure sensors placed at the heel, midfoot and forefoot collect data while a person stands or walks. A microcontroller reads and processes the signals locally. Signal-processing and machine-learning algorithms are then used to identify gait events and recognise activities such as standing, walking and running. The results can be sent to a mobile or computer application to support gait monitoring and rehabilitation research."

## 20. Key takeaway

The project has three core technical parts:

1. **Sensor and hardware design:** Reliable pressure measurement inside a comfortable sandal.
2. **Gait and signal analysis:** Converting sensor readings into meaningful step and loading information.
3. **Edge machine learning:** Recognising activities locally and reporting the results.

A successful project is not just a sandal with sensors. It must demonstrate repeatable measurements, useful gait/activity recognition, comfortable mechanical integration, acceptable battery life and validation against a reference.
