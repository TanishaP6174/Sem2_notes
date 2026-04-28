# Links I need to check out

- https://talkingiot.io/which-iot-predictions-came-true/
- https://dl.acm.org/doi/pdf/10.1145/3765516
- Lea, P., 2018. Internet of Things for Architects
- https://link.springer.com/article/10.1007/s10462-025-11342-3 
- https://www.weforum.org/agenda/2017/06/internet-of-things-will-power-the-fourth-industrial-revolution/
- https://www.marinetraffic.com/en/ais/home/centerx:126.9/cente
- https://www.indiawaterportal.org/articles/ngt

DONET I & II and S Net Cable Configurations Underwater communication networks

---

# NOTES

## History of IoT

- 1969 - ARPANET paved way for “Internet”
- 1982 - Coco cola vending machine → connected to the Internet to check availability of soda
- 1990 - smart Toaster
- 1995 – The first GPS satellite program of US government is completed making it possible to get location information required for many IoT devices
- 1999 – A big year for IoT as the term was used for the first time by Kevin Ashton, a cofounder of Auto-ID center at MIT
- 2008 - first international IoT conference

---

## Definition of IoT

→ Network of physical objects that contain embedded technology to communicate and sense or interact with their internal states, the environment or each other

---

## Intro

- Upstream info flow  
  ![[Pasted image 20260427232227.png]]

- Initially did not encompass the local storage and local processing, just had cloud based processing
- Edge computing → involves those
- Data goes from sensors → all the way to the cloud/databases

### Why its popular?

- faster communication tech
- flexibility of IPv6
- Emergence of fog/edge computing
- Advances in Big data, deep learning and AI understanding
- Embedded chips → smaller, cheaper and low power

### Importance

- Gathers useful data
- Focuses on automation and control
- useful in monitoring
- increases efficiency, saves time, improves quality of life

### Challenges/Impediments

- Privacy and security of data
- Huge volume of data → will require advancements in storage tech and processing infrastructure
- increasing complexity → hard to verify the system for safety and correctness
- Intermittent connectivity of a node will reduce the quality of user experience

---

## M2M vs IoT vs CPS

### M2M vs IoT

- **M2M (Machine-to-Machine):**  
  Refers to two or more autonomous devices talking **directly** to each other (point-to-point) using cellular or wired networks. Usually an isolated system designed for a specific task.  
  - *Example:* A vending machine "calling" the warehouse to report it's out of soda.

- **IoT (Internet of Things):**  
  Uses M2M communication but adds a layer where data is sent to a **gateway or edge router**, then to the cloud for large-scale analysis.  
  - *Example:* A smart factory sending data to a central dashboard.

---

### CPS vs IoT

- **CPS (Cyber-Physical Systems):**  
  Highly integrated systems where software and physical components work in a **closed loop**  

  **Loop:**  
  Sensing → Control → Actuation → Feedback  

  - *Example:* Autonomous car

- **IoT:**  
  More focused on **monitoring**, does not require feedback loops  

  - *Example:* Smart thermometer

---
## Transducers:

- Convert one form of energy to another
- A sensor may incorporate several transducers. 
- Example: A chemical sensor produces electrical signal in response to a chemical reaction. It may have two parts: first one converts the energy of a chemical reaction to heat (transducer) and the other part (thermopile) converts the heat into an electrical signal

>Complexity - density of sensors 

# SENSORS

### SENSOR CLASSIFICATION

Sensors are classified across **6 main dimensions**:

---

#### 2.1 By OUTPUT TYPE

**Analog Sensor:**

- Produces a continuous voltage or current output
- The output signal varies smoothly and proportionally with the input stimulus
- Example: a temperature sensor whose output voltage rises gradually as temperature increases
- Needs no additional conversion to represent real-world quantities

**Digital Sensor:**

- Produces a discrete (binary) output — high or low, 0 or 1
- Internally, an **ADC (Analog-to-Digital Converter)** is used to convert the analog signal from the sensing element into digital form
- More noise-resistant and easier to interface with microcontrollers
- Example: a digital thermometer that outputs temperature as a number over I2C protocol

**Extra to know:** ADC resolution matters — a 10-bit ADC can represent 1024 discrete levels (0–1023), while a 12-bit ADC gives 4096 levels. Higher resolution = finer granularity.

---

#### 2.2 By DATA TYPE

**Scalar Sensor:**

- Measures a quantity that has **only magnitude** (no direction)
- Output voltage is proportional to the magnitude of the measured physical quantity
- Examples: temperature, pressure, color, humidity, pH
- A thermometer tells you _how hot_, not _in which direction_

**Vector Sensor:**

- Measures quantities that have **magnitude, direction, AND orientation**
- Output voltage reflects all three properties
- Examples: sound (has amplitude and direction), velocity, acceleration, images (captured by camera sensors)
- A camera sensor captures both the intensity of light and the spatial direction it comes from — that's why it's a vector sensor
- A 3-axis accelerometer (like in your phone) is a vector sensor — it measures acceleration in X, Y, and Z directions simultaneously

---

#### 2.3 By INTERMEDIATE STAGES (Conversion steps)

**Direct Sensor:**

- Converts the stimulus **directly** into an electrical signal in a single step
- Uses a physical effect (thermoelectric, photoelectric, piezoelectric, etc.)
- Most direct sensors are also passive sensors
- Example: **Thermocouple** — two dissimilar metals joined together; when there's a temperature difference between the junctions, a voltage is directly produced (Seebeck effect)

**Indirect Sensor:**

- Requires **one or more intermediate transduction steps** before producing an electrical output
- More complex chain: stimulus → intermediate form → electrical signal
- Example: **Chemical sensors** like a pH sensor — chemical reaction first changes ion concentration, which then affects electrode potential, which is finally measured as voltage

**The Seebeck Effect (important for exams):** When two different metals are joined and the junctions are at different temperatures, a voltage is generated. This is the principle behind thermocouples. The voltage produced is proportional to the temperature difference — making a thermocouple a **relative sensor**.

---

#### 2.4 By POWER REQUIREMENT

**Passive Sensor:**

- Generates electrical output **by itself** from the energy of the stimulus — no external power needed
- The stimulus itself provides the energy for the output signal
- Most passive sensors are direct sensors
- Examples:
    - **Thermocouple** — temperature difference creates voltage
    - **Photodiode** — light creates current (photovoltaic effect)
    - **Piezoelectric sensor** — mechanical pressure creates charge
    - **LDR (Light Dependent Resistor)** — resistance changes with light (note: needs a circuit to measure the resistance change, but the sensing element itself is passive)

**Active Sensor:**

- Requires an **external excitation signal** (power supply) to operate
- The excitation signal interacts with the stimulus, and the output is a modified version of the excitation signal
- Examples:
    - **LiDAR** — emits laser pulses and measures reflected time
    - **GPS** — receives radio signals from satellites (requires receiver power)
    - **Infrared sensor (active type)** — emits IR light and detects reflection
    - **Ultrasonic sensor (HC-SR04)** — transmits ultrasonic pulses and receives echo

**Extra:** Most IoT sensors are active because they need consistent, reliable outputs and their excitation signals can be precisely controlled.

---

#### 2.5 By REFERENCE TYPE

**Absolute Sensor:**

- Measures stimulus relative to an **absolute physical scale** that does not depend on measurement conditions
- The reference is fixed and universal (e.g., absolute zero for temperature)
- Example: **Thermistor** — a semiconductor or polymer resistor whose resistance varies with temperature on an absolute scale. You can always derive the exact temperature from the resistance value using a known equation, regardless of environmental conditions.

**Relative Sensor:**

- Measures stimulus **with respect to a chosen reference** that may not be absolute
- The reference itself can vary or must be established separately
- Example: **Thermocouple** — measures the _difference_ in temperature between a hot junction and a cold (reference) junction. If the cold junction temperature changes, your reading changes. You need to compensate for the reference junction temperature.

**Why this matters:** If you're measuring atmospheric pressure with a relative pressure sensor, it tells you pressure relative to some reference (like atmospheric pressure at sea level). An absolute pressure sensor gives you the actual pressure in Pascals independent of any reference.

---

#### 2.6 By STIMULUS CONTACT

**Contact Sensor:**

- Must physically touch the stimulus to measure it
- Examples: strain gauges (touch the surface to measure deformation), temperature sensors (touch the object to measure its temperature), ECG electrodes

**Non-contact Sensor:**

- Measures the stimulus without any physical contact
- Examples: optical sensors (measure light intensity from a distance), magnetic sensors (measure magnetic fields without touching), IR sensors, ultrasonic sensors, radar sensors

---

#### 2.7 Classification by FIELD OF APPLICATION

From your slides, sensors are used across 17+ application domains: Agriculture, Automotive, Civil engineering, Domestic appliances, Distribution/commerce, Environment/meteorology, Energy/power, Information/telecom, Health/medicine, Marine, Manufacturing, Recreation, Military, Space, Scientific measurement, Transportation, and others.

---

#### 2.8 Classification by STIMULI (from Fraden's Handbook)

Sensors can be classified by the type of stimulus they detect:

|Stimulus Category|Examples|
|---|---|
|Acoustic|Wave amplitude, phase, velocity|
|Biological|Biomass concentration|
|Chemical|Component identity, concentration|
|Electric|Charge, current, voltage, conductivity|
|Magnetic|Magnetic field, flux, permeability|
|Optical|Wave amplitude, refractive index|
|Mechanical|Position, force, pressure, strain, velocity|
|Radiation|Type, energy, intensity|
|Thermal|Temperature, flux, conductivity|

---

#### 2.9 Classification by CONVERSION PHENOMENA

**Physical Conversion:**

- Thermoelectric, Photoelectric, Photomagnetic, Magnetoelectric, Electromagnetic, Thermoelastic, Electroelastic, Thermomagnetic, Thermo-optic, Photoelastic

**Chemical Conversion:**

- Chemical transformation, Physical transformation, Electrochemical process, Spectroscopy

**Biological Conversion:**

- Biochemical transformation, Physical transformation, Effect on test organisms, Spectroscopy

---

#### 2.10 Classification by SENSING ELEMENT MATERIAL

- Inorganic/Organic
- Conductor/Insulator
- Semiconductor
- Liquid, gas, or plasma
- Biological substance

---

## Sensor characteristics

### 1. Sensor specifications

- sensitivity
- range/span
- precision
- accuracy
- stability
- selectivity
- speed of response
- environmental conditions
- overload characteristics
- linearity
- hysteresis
- dead band
- operating life
- output format
- cost
- size
- weight

Understanding sensor specifications is critical for selecting the right sensor for an application.

---
#### 3.1 Sensitivity

**Definition:** The ratio of change in output to change in input.

**Formula:** S = dY/dX = ΔY/ΔX

Where Y is the output quantity and X is the input stimulus.

- Sensitivity is also the **slope of the calibration curve**
- For a linear sensor with equation **E = A + Bs**, the sensitivity is **B** (the slope)
- Higher sensitivity means a small change in input produces a large change in output — better for detecting small changes
- Units depend on the sensor: e.g., mV/°C for a temperature sensor, mV/Pa for a pressure sensor

**Sensitivity error:** When the actual sensitivity differs from the ideal — the calibration curve has a slightly different slope than expected.

---

#### 3.2 Range and Span

**Range:** The minimum and maximum values of the physical quantity the sensor can measure.

- Example: An RTD (Resistance Temperature Detector) has a range of **-200°C to 800°C**

**Span (also called Full Scale or Dynamic Range):** The difference between maximum and minimum input values.

- Span = Maximum value − Minimum value
- For the RTD above: Span = 800 − (−200) = **1000°C**

---

#### 3.3 Accuracy

**Definition:** The ability of a sensor to give readings close to the **true value** of the measured quantity.

**Absolute Error = Measured Value − True Value**

**Relative Error = Absolute Error / True Value**

- Relative error is often expressed as a percentage
- A sensor with ±1% accuracy means the reading could be off by up to 1% of the true value

---

#### 3.4 Precision (Repeatability/Reproducibility)

**Definition:** The ability of a sensor to give the **same reading** when measuring the same quantity multiple times under the same conditions.

**Key distinction — Accuracy vs. Precision:**

- **High accuracy + High precision:** All readings are close together AND close to the true value (best case — like arrows clustered at the bullseye)
- **Low accuracy + High precision:** Readings are close together but consistently wrong (systematic error — arrows clustered but away from bullseye)
- **Low accuracy + Low precision:** Readings are scattered and wrong (worst case — arrows scattered far from bullseye)

**Exam tip:** A sensor can be precise without being accurate, but an accurate sensor must also be precise.

---

#### 3.5 Resolution

**Definition:** The smallest change in input that can produce a detectable change in output.

- Example: A sensor with resolution of 0.01°C can detect temperature changes as small as 0.01°C
- Related to the number of bits in an ADC: an n-bit ADC has 2ⁿ levels
- Finer resolution = more granular measurements

---

#### 3.6 Linearity

**Definition:** The degree to which the sensor's output follows a straight-line relationship with the input across its operating range.

**Why linearity matters:**

- A linear sensor is easiest to work with mathematically
- If output E = A + Bs, you can simply calculate the measured quantity (MQ) as: **MQ = Vout/slope + C**
- Non-linear sensors require more complex calibration (polynomial equations, lookup tables, or ML algorithms)

**Linearity error:** The maximum deviation between the actual sensor output and the ideal (best-fit) straight line, usually expressed as a percentage of full-scale output.

**Non-linear sensors:** Examples include photodiodes, thermistors, and pH sensors. The pH sensor follows the Nernst equation: **E = E₀ + 2.303(RT/nF) × log[aH⁺]**, where the sensitivity (slope) is **-2.303RT/nF** and is temperature-dependent.

---

#### 3.7 Sensor Calibration

**Definition:** The process of finding the relationship between sensor output voltage and the actual measured quantity.

For a linear sensor: **MQ = Vout/slope + C**

There are two unknown quantities (slope and C, or intercept). Calibration determines these.

**Process:**

1. Subject the sensor to a known stimulus
2. Measure the output voltage
3. Two data points are sufficient for a linear sensor
4. Plot and determine slope and intercept

**Calibration drift** is the biggest challenge in IoT deployments — the values of slope and C change over time due to aging, environmental effects, and sensor degradation.

**Strategies to handle drift:**

- Know the drift pattern in advance and program corrections into system logic
- Recalibrate remotely ("over the air") using a reference (gold standard) sensor
- Physically bring the sensor to a controlled environment for recalibration

---

#### 3.8 Hysteresis

**Definition:** When a sensor gives **different outputs for the same input** depending on whether the input is increasing or decreasing.

- Caused by mechanical backlash, magnetic domains, or viscoelastic properties of materials
- The output follows a "loop" — different path going up versus going down
- Examples: humidity sensors (absorbing moisture vs. releasing moisture gives different readings), mechanical pressure sensors

**On a graph:** Hysteresis appears as a closed loop — the upward curve and downward curve don't overlap.

---

#### 3.9 Dead Band

**Definition:** A range of input values over which the sensor produces **no change in output**.

- Input must exceed the dead band before the sensor responds
- Appears as a flat region near zero on the input-output curve
- Relevant for sensors with threshold switching behavior

---

#### 3.10 Stability

**Short-term stability:** How consistent the sensor output is over a short time period (minutes to hours).

**Long-term stability (drift):** How much the sensor output changes over extended time (days to months).

---

#### 3.11 Dynamic Characteristics

**Static characteristics** describe sensor behavior when the input is constant or changes very slowly.

**Dynamic characteristics** describe how the sensor responds to **rapidly changing inputs**.

Important dynamic parameters:

- **Response time:** Time for sensor output to settle within a tolerance band after an abrupt change in input
- **Dynamic linearity:** Ability to follow rapid changes in input
- **Bandwidth:** Range of frequencies over which the sensor responds accurately

Key input waveforms used for dynamic testing: impulse, step, ramp, sinusoidal, and random noise.

---

#### 3.12 Other Specifications

- **Selectivity:** Ability to respond only to the intended stimulus and ignore others (important for chemical sensors)
- **Speed of response:** How quickly the sensor reacts to changes
- **Overload characteristics:** Maximum input the sensor can handle without damage or permanent change
- **Operating life:** Expected lifespan under normal operating conditions
- **Output format:** Voltage, current, frequency, digital protocol (I2C, SPI, UART)
- **Environmental conditions:** Operating temperature range, humidity range, pressure rating

---
### 4. SPECIFIC SENSORS IN DETAIL

---

#### 4.1 LDR (Light Dependent Resistor)

**What it is:** A passive sensor made of high-resistance semiconductor material (typically Cadmium Sulfide — CdS).

**Working principle — Photoconductivity:**

- When light falls on the semiconductor, photons transfer energy to electrons in the **valence band**
- If photon energy ≥ bandgap energy, electrons jump to the **conduction band**
- More free electrons = lower resistance = higher conductivity
- In darkness: very high resistance (MΩ range)
- In bright sunlight: very low resistance (kΩ range)
- Resistance is **inversely proportional** to light intensity

**Sensitivity:** Varies with the wavelength of incident light (has a peak sensitivity wavelength).

**Latency:**

- When light is suddenly applied after darkness: responds in a few tens of milliseconds
- When light is removed: can take up to a second to return to high resistance
- This lag makes LDRs unsuitable for applications requiring fast light detection

**Applications:** Outdoor/street lighting automation, night lights, camera light meters.

**Classification:** Passive, analog, scalar, contact (indirectly — responds to light falling on its surface), absolute sensor.

---

#### 4.2 IR (Infrared) Sensor

**Infrared spectrum:** 780 nm to 1 mm wavelength, subdivided as:

- **IR-A (700–1400 nm):** Used in fiber optics and general IR sensors
- **IR-B (1400–3000 nm):** Heat sensing
- **IR-C / Far-IR (3000 nm – 1 mm):** Thermal imaging

**Active IR Sensor:**

- Has two parts: an **IR LED (emitter)** and a **photodiode/phototransistor (receiver)**
- The LED emits IR light; the receiver detects how much is reflected back
- Used for proximity detection, object detection, line following in robots
- Motion detection range: typically 100–500 cm

**Passive IR (PIR) Sensor:**

- Detects IR radiation **emitted** by objects/humans/animals — does not emit anything itself
- Every object above absolute zero emits thermal IR radiation
- Uses **pyroelectric sensors** — materials that generate charge when their temperature changes
- Two pyroelectric elements arranged side by side; when something moves through the field of view, the differential between the two sensors changes, triggering detection
- Uses a **Fresnel lens array** to divide the field of view into zones, widening the detection area
- Detects motion, not just presence — a stationary warm object won't trigger it

**Pyroelectric effect:** Certain crystals (like lithium tantalate, PVDF polymer) generate an electric charge in response to temperature changes. The output voltage ΔV is proportional to the pulse energy of the incoming IR radiation.

---

#### 4.3 Digital Barometric Pressure Sensor (BMP180)

**Type:** Piezoresistive sensor

**Working principle:**

- A thin silicon diaphragm deflects under pressure
- Piezoresistors on the diaphragm change resistance when they experience mechanical strain (piezoresistive effect)
- A Wheatstone bridge circuit converts resistance changes to voltage
- Measures **both atmospheric pressure and temperature**

**Applications:** Weather stations, altitude measurement (pressure decreases with altitude), indoor navigation.

**Piezoresistive effect:** When a semiconductor is mechanically stressed, its electrical resistance changes — more sensitive than the metallic strain gauge version.

---

#### 4.4 Ultrasonic Sensor (HC-SR04)

**What is ultrasound?** Sound waves with frequency above 20,000 Hz (inaudible to humans).

**Frequency spectrum (from your slides):**

- < 20 Hz: Infrasound (earthquakes, low bass)
- 20 Hz – 20 kHz: Audible range (human hearing)
- > 20 kHz: Ultrasound
    
    - 20 kHz – 2 MHz: Animal hearing, medical therapy
    - 2 MHz – 200 MHz: Medical diagnostic imaging, acoustic microscopy

**Transducer types used:**

1. **Piezoelectric transducers:** Apply voltage → crystal deforms → emits sound. Sound hits crystal → deformation → voltage produced. (Direct and inverse piezoelectric effects)
2. **Capacitive transducers:** Like a microphone — use electrostatic fields between a conductive diaphragm and a backing plate.

**Piezoelectric materials (natural):** Quartz crystals, Topaz, Rochelle Salt, Schorl Tourmaline, Sugar Cane, Tendon, DNA, Bone, Dentine/Enamel.

**Piezoelectric materials (manufactured):** PZT (Lead Zirconate Titanate) — most commonly used.

**HC-SR04 Specifications:**

- Power Supply: DC 5V
- Working Current: 15 mA
- Working Frequency: 40 kHz
- Range: 2 cm – 400 cm
- Resolution: 0.3 cm
- Measuring Angle: 15°

**Working Principle (SONAR-based):**

1. A 10-microsecond pulse applied to **TRIG pin**
2. Sensor emits a burst of **8 ultrasonic pulses at 40 kHz** (the 8-pulse pattern creates a recognizable signature to distinguish from ambient noise)
3. **ECHO pin goes HIGH** as pulses travel
4. If pulses are **not reflected**: ECHO times out after 38 ms → no obstacle in range
5. If pulses are **reflected**: ECHO goes LOW upon receiving the echo
6. Output pulse width (150 µs to 25 ms) corresponds to the distance

**Distance formula:** Distance = (Time × Speed of Sound) / 2 (Speed of sound ≈ 343 m/s at room temperature; divide by 2 because pulse travels to object AND back)

**Acoustic impedance:** The resistance of a medium to the passage of sound waves. Measured in Rayleigh (Pa·s/m). The reflection of ultrasound at boundaries depends on the difference in acoustic impedance between two media.

**Novel application:** Wearable stretchable ultrasonic arrays can non-invasively image body tissue up to 1.5 inches deep (Nature, 2023).

---

#### 4.5 Water Level Sensors

**Resistive type (basic):**

- 10 exposed copper traces: 5 power + 5 sense, alternating
- Water bridges power and sense traces
- Parallel conductors act as a **variable resistor**
- **Resistance is inversely proportional to water level**
- More immersion → lower resistance → higher output voltage
- **Limitation:** Short lifespan due to corrosion in moist environment

**Optical type:**

- Uses infrared LEDs and phototransistors
- Non-contact, high accuracy, fast response
- Limitations: Cannot be used in direct sunlight; water vapor affects accuracy

**Capacitance type:**

- Uses the liquid as a dielectric: C = (A × ε) / d
- Empty container = low capacitance; full container = high capacitance
- Limitation: Electrode corrosion changes capacitance, requires recalibration

**Tuning fork type:**

- Vibrates at a resonant frequency; when liquid reaches the fork, frequency changes
- Unaffected by flow, bubbles, liquid type
- Cannot be used in viscous media

**Diaphragm type:**

- Mechanical diaphragm deflects when liquid pressure acts on it
- No power needed in the tank; works with many liquid types
- Being mechanical, requires maintenance over time

**Float type:**

- Float rises with liquid level, actuating a switch
- Works with any liquid, can operate without power
- Larger and more maintenance-intensive than other types

**Ultrasonic liquid level (top-down or bottom-up):**

- Non-contact measurement
- Virtually unlimited media types
- Accuracy affected by temperature and dust

**Radar level gauge:**

- Uses microwave/radar signals (like ultrasonic but with electromagnetic waves)
- Wide application range, unaffected by temperature/dust/steam
- Can produce interference echoes, affecting accuracy

---

#### 4.6 Photometry and Reflectometry

**Beer-Lambert Law:** Relates the absorption of light to the concentration of an absorbing substance.

**A = ε × l × c**

Where:

- A = Absorbance
- ε = Molar extinction coefficient (M⁻¹ cm⁻¹)
- l = Path length (cm)
- c = Concentration (M)

**Transmittance:** T = It / I₀ (ratio of transmitted to incident light intensity)

**Absorbance:** A = −log(T)

**Reflectometry (remission photometry):** A non-destructive technique using light reflection from surfaces to measure color intensity, film thickness, or refractive index. Used in rapid test strips, paint analysis, etc.

---

#### 4.7 Infrasound Sensors and Ocean Sensors (Context from slides)

**KUT Infrasound Network (Tonga eruption, 2022):**

- Infrasound: sound below 20 Hz (inaudible to humans)
- The SAYA INF01 sensor combines: membrane-type infrasound sensor + barometer + thermometer + MEMS accelerometer
- Detected pressure fluctuations from the Tonga volcanic eruption from 7700–8400 km away, approximately 7 hours after the event

**DONET2 (Dense Oceanfloor Network for Earthquakes and Tsunamis):**

- 27 ocean bottom pressure gauges
- Detects tsunami precursors by measuring vertical sea surface movement through water pressure changes
- Long-term ocean-bottom seismographs designed for >1 year operation
- Includes broadband seismic sensors + differential pressure gauges

---

### 5. KEY FORMULAS TO REMEMBER

|Formula|Meaning|
|---|---|
|S = ΔY/ΔX|Sensitivity|
|E = A + Bs|Linear transfer function (B = sensitivity/slope)|
|MQ = Vout/slope + C|Inverse transfer function (getting measured quantity from voltage)|
|Absolute Error = Result − True Value|Accuracy measurement|
|Relative Error = Absolute Error / True Value|Normalized accuracy|
|Span = Max − Min|Input range|
|A = εlc|Beer-Lambert Law|
|T = It/I₀|Transmittance|
|A = −log(T)|Absorbance from transmittance|
|Distance = (Time × v_sound) / 2|Ultrasonic distance calculation|
|C = (A × ε) / d|Capacitance (for capacitive level sensors)|

---

### 6. EXAM QUICK-REFERENCE TABLE

|Classification|Options|Example|
|---|---|---|
|Output|Analog / Digital|Temperature sensor / Digital thermometer|
|Data type|Scalar / Vector|Pressure sensor / Camera|
|Intermediate stages|Direct / Indirect|Thermocouple / pH sensor|
|Power|Passive / Active|Photodiode / LiDAR|
|Reference|Absolute / Relative|Thermistor / Thermocouple|
|Contact|Contact / Non-contact|Strain gauge / IR sensor|

---

### 7. COMMON EXAM TRAPS

1. **Thermocouple is both Direct AND Relative** — direct because it converts temperature difference to voltage in one step; relative because it measures temperature _difference_, not absolute temperature.
2. **LDR is Passive but Analog** — it doesn't generate electricity itself, but its resistance changes, which requires a circuit with a power source to measure. In circuit context it's often treated as active, but as a sensing element, it's passive.
3. **PIR sensor is Passive despite detecting IR** — the "passive" refers to not emitting IR, not to power consumption. The PIR module still needs power to operate its electronics.
4. **Precision ≠ Accuracy** — you can be precisely wrong (consistent but consistently off).
5. **Resolution ≠ Accuracy** — a sensor can have very fine resolution but still be systematically inaccurate.
6. **Sensitivity is the slope**, not the intercept. Sensitivity error means the slope has changed from ideal.
7. **Hysteresis** means the output at a given input depends on the _history_ (whether you were going up or down). It cannot be corrected by simple calibration.


![[Pasted image 20260427232346.png]]

- **Resolution** → smallest detectable change  

- **Precision** → consistency of repeated readings  

---

### 4. Errors

- **Systematic errors** → interfering variables, loading, attenuation  
- **Random errors** → noise  

---

# LDR sensors

## About

- Made of high resistance semiconductor material (CdS, PbS, InSb)
- Passive sensor
- Resistance changes with light intensity, decreases in bright light
- Used in outdoor lighting, street lights

---

## Working principle

- Based on **photoconductivity**
- Conductivity increases with light

- Electrons move from valence band → conduction band if photon energy sufficient
- Sensitivity depends on wavelength

### Latency

- Time taken to respond
- Tens of ms when light appears
- Up to 1 second when light is removed

---

# Ultrasonic sensor

## About

- Uses **ultrasound waves (>20 kHz)**

- Human range: 20 Hz – 20 kHz  
- Ultrasonic: > 20 kHz  
- HC-SR04: 40 kHz  

---

## Working principle

> SONAR (Sound Navigation and Ranging)

### Steps

1. Transmitter sends pulse  
2. Pulse travels  
3. Hits object  
4. Echo returns  
5. Time measured  
6. Distance calculated  

---

## Transducers

### Piezoelectric

**Direct Effect:**  
Mechanical pressure → voltage  

- Crystal structure  
- Non-centrosymmetric  

No force → no voltage  
Force → charge separation → voltage  

**Inverse Effect:**  
Voltage → vibration → sound  

---

### Capacitive (CMUT)

Acts like a capacitor  

#### Transmitting

1. AC voltage  
2. Diaphragm vibrates  
3. Ultrasonic waves generated  

#### Receiving

1. Wave hits diaphragm  
2. Vibrates  
3. Capacitance changes  
4. Signal generated  

---

### Comparison

| Feature     | Piezoelectric       | Capacitive          |
| ----------- | ------------------- | ------------------- |
| Principle   | Crystal deformation | Electrostatic force |
| Power       | Moderate            | Needs bias          |
| Bandwidth   | Narrow              | Wide                |
| Sensitivity | High                | High                |
| Fabrication | Bulk crystal        | MEMS                |
| Use         | HC-SR04             | Medical imaging     |

---

Micro-electromechanical Systems == MEMS

---

## Case studies

- Tonga volcanic eruption  
  → infrasound sensors used  

- NF01 sensor includes:
  - membrane-type infrasound sensor  
  - barometer  
  - thermometer  
  - accelerometer (MEMS)

---

- DONET2  
  (Dense Oceanfloor Network system for Earthquakes and Tsunamis)

  - 27 pressure gauges  
  - protective hardhats for deep pressure  

----
## Actuators:

- Devices that take signals in electrical form and transform it to something that can influence the physical world.
- To enact movement actuators need energy. Main kinds of energy:
	- electric
	- hydraulic
	- pneumatic (power from compressed gas used to perform mechanical work)
	- thermal/magnetic

### <u>Electromagnetic drives</u>

-  When current flows through a conductor, a **magnetic field** is produced (Ampere's Law)
- Two magnetic fields experience a **force trying to align** them (Lorentz Force)
- Magnetic fields are typically produced by a **stator** (fixed part, provides magnetic field) and a **rotor** (moving part, responds to the field).
- The fields can be produced using a permanent magnet or electromagnets

#### DC MOTOR:

- Stator is a permanent magnet and rotor is a coil connected to the power supply
- When the current flows through the rotor coils, they become electromagnets and try to align with the stator field, causing rotation.
- A **commutator** reverses current direction to keep the rotation going continuously.

#### STEPPER MOTOR:

- Produces rotational motion in exact increments - what does this mean?
- Has  permanent magnet rotor that aligns to magnetic field of stator coils
- When the next stator coil is energized (energizes in sequence) , the rotor moves one step and locks in place
- This allows for precise movement
- No position sensor needed — steps are counted
- High holding torque when stationary
- Can lose steps under overload (no feedback)

#### SERVO MOTOR

- A servo motor is essentially a **DC motor + gearbox + position sensor (potentiometer) + control circuit** in one package. The control circuit continuously compares actual shaft position to the desired position and corrects any error — this is a **closed-loop feedback system**.

	- Used in robotics & industrial appln where a knowledge of the shaft position (?) is required.
	- gear assembly is used to obtain high torque at low speed
	- uses a position sensor to control the motor (closed loop system)
	- control circuit provides feedback on current position of the motor shaft
	-  Potentiometer gives variable voltage proportional to angle
	- Integrated H-Bridge circuit drives motor in both directions
	- Typically controlled by PWM (Pulse Width Modulation) signal

---

![[Pasted image 20260428012046.png]]


### <u>Classification on type of displacement</u>

1. **Linear actuators** - the shaft of the linear actuators will only move in a linear fashion
2. **Rotary actuators** - the shaft will only rotate in an axis

#### TYPES OF ACTUATORS

##### 1. ELECTRIC ACTUATORS

**Advantages**:
	1. Highest precision
	2. Easy to program and network. Offers immediate feedback for diagnosis and maintenance
	3. Less noise
	4. No fluid leaks, so less hazardous for environment
	5. They provide **complete control** on motion profiles and can include an encoder to control the velocity, position and torque.


**Disadvantages**:
	1. Initial cost is expensive
	2. No suitable for all environments, unlike following 2
	3. Susceptible to overheating, wear and tear issues
	4. Actuator's parameters are fixed so to change torque, speed, etc, actuators should replace
##### 2. HYDRAULIC ACTUATORS

**Advantages**:
	1. Easy to control and accurate
	2. Simpler and easier to maintain
	3. Less noise than pneumatic
	4. Easily detectable fluid leaks
	5. Constant torque or force regardless of speed changes (so preferred in heavy industries)


**Disadvantages**:
	1. Proper maintenance is required
	2. Expensive
	3. Leakage of fluid creates environmental problems
	4. Wrong hydraulic fluid for a system can damage the components.

##### 3. PNEUMATIC ACTUATORS

**Advantages**:
	1. Clean, less pollution to the environment
	2. Safe and easy to operate
	3. Inexpensive


**Disadvantages**:
	1. Loud and noisy
	2. Lack of precision controls
	3. Sensitive to vibrations

##### 4. THERMAL/MAGNETIC  ACTUATORS

- Use shape memory alloys (SMA), bimetallic strips, or magnetostrictive materials. Useful in micro/nano-scale or specialized medical devices. Generally slow response but can generate large forces for their size.

##### 5.SOLENOID VALVES
- an electromagnet pulls a plunger to open/close a valve. Widely used in washing machines, irrigation, fluid control.
##### 6. ELECTRICAL RELAYS
- electrically operated switch. A small current controls a large current circuit. Used in automotive, industrial safety systems.


## Sensors — especially in Legged Robots

### Two Main Categories

**Proprioceptive sensors** — measure the robot's own internal state (like how humans feel their own body position without looking).

**Exteroceptive sensors** — measure the external environment (like eyes and ears).

---
### Proprioceptive Sensors

Joint sensors:

- **Encoder** — measures angular position/speed of a joint. Optical or magnetic. High resolution.
- **Torque sensor** — measures force/torque at joints. Crucial for compliant robots that must interact safely with humans.

Body sensors:

- **IMU (Inertial Measurement Unit)** — combines accelerometer + gyroscope (+ sometimes magnetometer). Measures linear acceleration and angular velocity to estimate body orientation and motion.

These feed into Kinematics & Dynamics → enables fully autonomous local-area reactions.

---
### Exteroceptive Sensors — Physical Information

- **F/T sensor (Force/Torque)** — measures contact forces between robot and environment. Used in feet/hands for safe contact detection.
- **Tactile sensor** — distributed pressure sensing (like skin). Detects contact shape and distribution.

### Exteroceptive Sensors — Geometry / Environment

Visual:

- **Binocular (stereo) camera** — two cameras like human eyes; gives depth from triangulation.
- **RGB-D camera** — color image + per-pixel depth (e.g., Intel RealSense, Microsoft Kinect).

Non-Visual:

- **Radar** — uses radio waves, works in fog/rain, long range.
- **LiDAR** — laser pulses for precise 3D point cloud mapping. Very common in autonomous vehicles.
- **Ultrasonic sensor** — sound waves, short range, cheap. Used for obstacle detection.

These feed into Mapping & Localization → enables long-range global locomotion planning.



![[Pasted image 20260428025554.png]]


## IoT Applications of Actuators & Sensors

### Automobile Industry

Modern cars are packed with both sensors and actuators working together:

- ABS (Anti-lock Braking) — wheel speed sensors + brake actuators
- Throttle control — position sensor + electronic throttle body
- Power steering — torque sensor + electric motor
- Airbag deployment — accelerometer (sensor) + pyrotechnic actuator
- Adaptive cruise control — radar/LiDAR + throttle/brake actuators

### Healthcare Robots

**Da Vinci Surgical Robot** — a remote-controlled surgical robot where a surgeon controls robotic arms from a console. Demonstrated by a Canadian doctor performing surgery 400km away. Uses servo motors and precise actuators to translate hand movements to tiny surgical tools.

**Hospital service robots** — deliver medications, transport items, disinfect rooms. Use wheel motors, navigation sensors (LiDAR, cameras), and gripper actuators.

### Case Study: Nizam Institute of Medical Sciences (NIMS)

A large hospital with complex IoT/robotics challenges:

- 9.2 lakh outpatients in 2024
- 34.6 lakh diagnostic tests in 2024
- ~4,000 beds, ~1,000 ICU beds across 22 acres
- ~5,000 stretchers and ~5,000 wheelchairs to track and manage

This scale illustrates why IoT-connected sensors (for asset tracking) and actuators (for automated delivery robots) are valuable in healthcare settings.

### Legged Robots (Boston Dynamics style)

Combine all the sensor and actuator types studied. Each joint uses a servo/torque-controlled motor. IMUs maintain balance. Cameras and LiDAR provide terrain mapping. The sensor data flows into kinematics solvers and machine learning controllers to achieve stable dynamic walking.

**BionicSoftArm** — uses pneumatic bellows (soft actuators) instead of rigid motors. Allows safe human-robot collaboration since the arm is compliant (gives way under unexpected contact).


