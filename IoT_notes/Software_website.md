
[[Software_slides]]
[[Software_notes]]

## Arduino IDE & ESP-IDF

Arduino is an open-source electronics platform with a simple IDE and easy-to-use API. Programs are called "sketches" and use two main functions:

- `setup()` - Runs once at startup
- `loop()` - Runs repeatedly forever

**Advantages**:
	1. Easy to start with
	2. Good for rapid prototyping
	3. Huge community support
	4. Large library ecosystem
	5. Extensive tutorials online

**Disadvantages**:
	1. Abstraction adds overhead
	2. Limited optimization options
	3. Less control over hardware
	4. Hidden complexity makes debugging hard
	5. Not ideal for production

**NOTE**: `delay()` blocks execution - other code cannot run during the delay. This is a hidden limitation of the Arduino approach.

ESP-IDF (Espressif IoT Development Framework) is the official SDK(software development kit) from Espressif. It's built on FreeRTOS and provides full access to all hardware features.

**Components**:
	1. Toolchain(compiler, linker)
	2. Build system (CMake based)
	3. FreeRTOS kernel
	4. Component libraries
	5. Debugging tools (GDB, OpenOCD)

**Advantages**:
	1. Full hardware control
	2. Better performance
	3. Professional features like secure boot and ota
	4. FreeRTOS for real-time tasks
	5. Dual-core utilization

**Disadvantages**:
	1. Steeper learning curve
	2. More setup required
	3. Verbose code
	4. Fewer community tutorials
	5. More documentation reading

**NOTE:** `vTaskDelay()` is non-blocking - other FreeRTOS tasks can run during the delay!

|Feature|Arduino IDE|ESP-IDF|
|---|---|---|
|Setup Time|10 minutes|1-2 hours|
|First Program|5 minutes|30 minutes|
|Code Complexity|Low|Medium-High|
|Performance|Good|Excellent|
|Power Optimization|Limited|Full Control|
|Debugging|Basic Serial|GDB + OpenOCD|
|Multi-tasking|Manual|FreeRTOS Tasks|
|Security Features|Basic|Enterprise-grade|
|OTA Updates|Library-based|Built-in|
|Community Size|Very Large|Large|

#### **Why emulation?**

##### Benefits

- No hardware required
- Better debugging visibility
- CI/CD testing pipelines
- Can't damage virtual hardware
- Same environment for everyone

##### Limitations

- No WiFi / Bluetooth
- No real sensors
- Limited analog (ADC/DAC)
- Some peripherals missing
- Timing not exact

## NETWORKING

### WIFI

Operates in the 2.4 GHz and 5 GHz bands

**PROS**:
	1. Very high bandwidth
	2. Easy to set up, familiar since present in most houses
	3. Widely available infrastructure
	4. Supports a wide range of IoT devices

**CONS:**
	1. High power consumption
	2. Interference and congestion capability in dense environments
	3. LImited device density per access point
	4. Limited range

eg: smart home system
### Bluetooth/BLE

Operates in the 2.4 GHz band. BLE(Bluetooth LOw Energy) is specifically optimized for low power applications

**PROS**:
	1. Low power, especially BLE, good for batter devices
	2. Cheap, widespread adoption
	3. Phones are ubiquitous
	4. short range reduces interference issues


**CONS:**
	1. HIghly limited range
	2. Limited data throughput
	3. Not suitable for large networks without mesh extensions

eg: wearable fitness trackers

### Zigbee and Z-Wave

**Zigbee** (IEEE 802.15.4) operates in the 2.4 GHz band and supports **mesh networking**, where devices relay data through each other to extend range and improve reliability. **Z-Wave** uses a different frequency (sub-GHz, around 900 MHz) to avoid Wi-Fi interference, and is purpose-built for home automation with mesh capabilities.

**PROS**:
	1. Low power consumption
	2. Mesh networking - device relay msgs, extending range and adding redundancy
	3. Self-healing network - automatically reroutes if a node fails
	4. scalable to large networks
	5. Z-Wave: operates on sub-GHz, avoiding Wi-fi interference


**CONS:**
	1. Lower data rates
	2. Require coordinator/hub for the network
	3. Can be complex to set up and maintain
	4. Z-Wave: limited and proprietary ecosystem

eg: Smart lighting
### LoRaWAN (Long Range Wide Area Network)

LoRaWAN is a low-power, long-range protocol designed for IoT applications. 

It operates in **sub-GHz ISM bands** (433/868/915 MHz) using chirp spread spectrum modulation, achieving ranges of **up to 10-15 km** in rural areas with very low power consumption — battery life of several years on a coin cell.

Trades bandwidth for range and battery life

**PROS**:
	1. Very long range
	2. Low power consumption
	3. Low cost/device and operates on unlicensed spectrum
	4. supports large-scle deployments
	5. Good penetration through buildings and obstacles


**CONS:**
	1. Requires gateway infrastructure
	2. Very low data rate
	3. High latency
	4. Limited bandwidth
	5. Uplink-heavy design - limited downlink capacity

### Cellular Data(4G/5G)

Cellular IoT leverages existing mobile network infrastructure (cell towers) to provide wide-area connectivity. **4G/LTE** offers high bandwidth for data-intensive applications, while **5G** promises ultra-low latency and massive device density, enabling new categories of IoT applications.

**PROS**:
	1. 


**CONS:**
	1.

### NB-IoT (Narrowband IoT)

NB-IoT is a **cellular LPWAN** technology standardized by 3GPP. It operates within existing LTE bands but uses a very narrow bandwidth (200 kHz), providing excellent coverage — including deep indoor and underground penetration — with very low power consumption. It bridges the gap between cellular and LPWAN.

**PROS**:
	1. 


**CONS:**
	1.


eg: wearable fitness trackers

How to choose?

                    ┌──────────────────────────┐
                    │  What range do you need?  │
                    └─────────────┬────────────┘
                      ┌───────────┴───────────┐
                      ▼                       ▼
               Short (<100m)           Long (>1km)
                      │                       │
              ┌───────┴───────┐       ┌───────┴───────┐
              │ High bandwidth│       │ Need cellular │
              │    needed?    │       │ infrastructure?│
              └───┬───────┬───┘       └───┬───────┬───┘
                  ▼       ▼               ▼       ▼
                 Yes      No             Yes      No
                  │       │               │       │
              ┌───┘   ┌───┘           ┌───┘   ┌───┘
              ▼       ▼               ▼       ▼
           Wi-Fi   Battery          NB-IoT  LoRaWAN
                   powered?        or LTE-M  or Sigfox
                   ┌──┴──┐
                   ▼     ▼
                  Yes    No
                   │     │
                  BLE  Zigbee/
                       Z-Wave


|     |     |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- |
|     |     |     |     |     |     |     |
|     |     |     |     |     |     |     |
|     |     |     |     |     |     |     |
|     |     |     |     |     |     |     |
|     |     |     |     |     |     |     |
|     |     |     |     |     |     |     |
|     |     |     |     |     |     |     |
|     |     |     |     |     |     |     |

**NOTE:**
NB-IoT has **better penetration** and _can_ work underground,  **but only if cellular coverage is available there.**

## QUESTIONS 

![[Pasted image 20260428084350.png]]

![[Pasted image 20260428084518.png]]

![[Pasted image 20260428084550.png]]

![[Pasted image 20260428084612.png]]

![[Pasted image 20260428084642.png]]

![[Pasted image 20260428084659.png]]

![[Pasted image 20260428084740.png]]

![[Pasted image 20260428084805.png]]

![[Pasted image 20260428084815.png]]

![[Pasted image 20260428084903.png]]![[Pasted image 20260428085454.png]]
![[Pasted image 20260428085503.png]]
![[Pasted image 20260428085525.png]]


## NOTE: