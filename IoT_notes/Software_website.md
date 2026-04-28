
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

---

## CODE:

### ESP-IDF Blink Example

This program blinks an LED on GPIO 2 using the ESP-IDF framework with FreeRTOS.

#### Include Headers

```
#include <stdio.h>
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"
#include "driver/gpio.h"
#include "esp_log.h"
```

|Header|Purpose|
|---|---|
|`stdio.h`|Standard C I/O functions (printf, etc.)|
|`freertos/FreeRTOS.h`|FreeRTOS kernel definitions and types|
|`freertos/task.h`|Task functions like `vTaskDelay()`|
|`driver/gpio.h`|GPIO control functions|
|`esp_log.h`|Logging macros (ESP_LOGI, ESP_LOGE, etc.)|

#### Definitions

```
#define LED_PIN GPIO_NUM_2
#define BLINK_PERIOD_MS 1000

static const char *TAG = "BLINK";
```

|Line|Explanation|
|---|---|
|`GPIO_NUM_2`|ESP-IDF enum for GPIO pin 2 (built-in LED on most boards)|
|`BLINK_PERIOD_MS`|Delay between toggles in milliseconds|
|`TAG`|Label for log messages - appears as `I (123) BLINK: message`|

#### GPIO Configuration

```
gpio_config_t io_conf = {
    .pin_bit_mask = (1ULL << LED_PIN),
    .mode = GPIO_MODE_OUTPUT,
    .pull_up_en = GPIO_PULLUP_DISABLE,
    .pull_down_en = GPIO_PULLDOWN_DISABLE,
    .intr_type = GPIO_INTR_DISABLE
};
gpio_config(&io_conf);
```

|Field|Value|Explanation|
|---|---|---|
|`pin_bit_mask`|`(1ULL << LED_PIN)`|Bitmask selecting which pins to configure. `1ULL << 2` = `0b100` = pin 2|
|`mode`|`GPIO_MODE_OUTPUT`|Pin direction: OUTPUT (can also be INPUT, INPUT_OUTPUT)|
|`pull_up_en`|`DISABLE`|Internal pull-up resistor (not needed for output)|
|`pull_down_en`|`DISABLE`|Internal pull-down resistor|
|`intr_type`|`DISABLE`|No interrupt on this pin|

**Why use `gpio_config()`?** It configures multiple settings atomically. You could also use individual functions like `gpio_set_direction()`, but this is cleaner and more efficient.

#### The Main Loop

```
int led_state = 0;
while (1) {
    led_state = !led_state;
    gpio_set_level(LED_PIN, led_state);
    ESP_LOGI(TAG, "LED %s", led_state ? "ON" : "OFF");
    vTaskDelay(pdMS_TO_TICKS(BLINK_PERIOD_MS));
}
```

|Line|Explanation|
|---|---|
|`led_state = !led_state`|Toggle: 0→1 or 1→0|
|`gpio_set_level()`|Set pin HIGH (1) or LOW (0)|
|`ESP_LOGI()`|Log info message (I=Info, E=Error, W=Warning, D=Debug)|
|`vTaskDelay()`|Pause task, let other tasks run (non-blocking)|
|`pdMS_TO_TICKS()`|Convert milliseconds to FreeRTOS ticks|

#### Understanding vTaskDelay()

```
vTaskDelay(pdMS_TO_TICKS(1000));
// Equivalent to:
vTaskDelay(1000 / portTICK_PERIOD_MS);
```

#### How vTaskDelay() Works

|Part|Meaning|
|---|---|
|`1000`|Delay time in milliseconds|
|`portTICK_PERIOD_MS`|Duration of one FreeRTOS tick (typically 10ms)|
|`1000 / 10 = 100`|Number of ticks to wait|

#### vTaskDelay() vs Arduino delay()

|Arduino `delay()`|FreeRTOS `vTaskDelay()`|
|---|---|
|Blocks CPU completely|Yields CPU to other tasks|
|Nothing else can run|Other tasks execute during delay|
|Wastes CPU cycles|Efficient multitasking|

---

### ESP-IDF GPIO Input with Interrupts

This program reads a button and toggles an LED using hardware interrupts.

#### Interrupt Service Routine (ISR)

```
static void IRAM_ATTR gpio_isr_handler(void *arg)
{
    uint32_t gpio_num = (uint32_t)arg;
    xQueueSendFromISR(gpio_evt_queue, &gpio_num, NULL);
}
```

|Part|Explanation|
|---|---|
|`IRAM_ATTR`|Places function in RAM (required for ISRs - faster than flash)|
|`void *arg`|Argument passed when handler was registered (the GPIO number)|
|`xQueueSendFromISR()`|Send data to queue from interrupt context (ISR-safe version)|

**ISR Rules:**

- Keep ISRs short - do minimal work
- Don't use `printf()` or `ESP_LOG` in ISRs
- Use `FromISR` versions of FreeRTOS functions
- Send event to a task for complex processing

#### Queue for Communication

```
static QueueHandle_t gpio_evt_queue = NULL;

// In app_main():
gpio_evt_queue = xQueueCreate(10, sizeof(uint32_t));
```

|Parameter|Value|Meaning|
|---|---|---|
|Queue length|`10`|Can hold 10 items before blocking|
|Item size|`sizeof(uint32_t)`|Each item is 4 bytes (GPIO number)|

#### Why Use Queues?

Queues provide safe communication between ISRs and tasks:

- ISR puts event in queue (fast, non-blocking)
- Task waits on queue and processes events
- No shared variable race conditions

#### Task Waiting on Queue

```
static void gpio_task(void *arg)
{
    uint32_t gpio_num;
    while (1) {
        if (xQueueReceive(gpio_evt_queue, &gpio_num, portMAX_DELAY)) {
            int level = gpio_get_level(gpio_num);
            ESP_LOGI(TAG, "GPIO %lu level: %d", gpio_num, level);
        }
    }
}
```

|Function|Explanation|
|---|---|
|`xQueueReceive()`|Wait for item from queue, copy to `gpio_num`|
|`portMAX_DELAY`|Wait forever until item arrives (blocks task)|
|`gpio_get_level()`|Read current pin state (0 or 1)|

#### Setting Up the Interrupt

```
// Configure button pin
gpio_config_t btn_conf = {
    .pin_bit_mask = (1ULL << BUTTON_PIN),
    .mode = GPIO_MODE_INPUT,
    .pull_up_en = GPIO_PULLUP_ENABLE,
    .intr_type = GPIO_INTR_ANYEDGE
};
gpio_config(&btn_conf);

// Install ISR service and add handler
gpio_install_isr_service(0);
gpio_isr_handler_add(BUTTON_PIN, gpio_isr_handler, (void *)BUTTON_PIN);
```

|Setting|Explanation|
|---|---|
|`GPIO_MODE_INPUT`|Pin is input (reading button state)|
|`GPIO_PULLUP_ENABLE`|Internal pull-up resistor (button connects to GND)|
|`GPIO_INTR_ANYEDGE`|Trigger interrupt on both press and release|
|`gpio_install_isr_service()`|Initialize the GPIO interrupt system|
|`gpio_isr_handler_add()`|Register our handler for this specific pin|

#### Interrupt Trigger Types

|Type|When it triggers|
|---|---|
|`GPIO_INTR_POSEDGE`|Rising edge (0→1)|
|`GPIO_INTR_NEGEDGE`|Falling edge (1→0)|
|`GPIO_INTR_ANYEDGE`|Any change|
|`GPIO_INTR_LOW_LEVEL`|While pin is LOW|
|`GPIO_INTR_HIGH_LEVEL`|While pin is HIGH|

---

### Arduino vs ESP-IDF: Same Task, Different Code

Compare how the same LED blink is implemented in both frameworks:

#### Arduino (12 lines)

```
#define LED_PIN 2

void setup() {
    pinMode(LED_PIN, OUTPUT);
}

void loop() {
    digitalWrite(LED_PIN, HIGH);
    delay(1000);
    digitalWrite(LED_PIN, LOW);
    delay(1000);
}
```

#### ESP-IDF (25 lines)

```
#include "driver/gpio.h"
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"

#define LED_PIN GPIO_NUM_2

void app_main(void) {
    gpio_config_t conf = {
        .pin_bit_mask = (1ULL << LED_PIN),
        .mode = GPIO_MODE_OUTPUT,
    };
    gpio_config(&conf);

    int state = 0;
    while (1) {
        state = !state;
        gpio_set_level(LED_PIN, state);
        vTaskDelay(pdMS_TO_TICKS(1000));
    }
}
```

|Aspect|Arduino|ESP-IDF|
|---|---|---|
|Pin setup|`pinMode()` - one line|`gpio_config()` - struct with options|
|Set output|`digitalWrite()`|`gpio_set_level()`|
|Delay|`delay()` - blocking|`vTaskDelay()` - non-blocking|
|Loop|Implicit (`loop()` repeats)|Explicit `while(1)`|

**Key Insight:** Arduino hides complexity to make code shorter. ESP-IDF exposes it to give you control. Both are valid - choose based on your needs!

----
## OTA updates

![[Pasted image 20260428115939.png]]

Since the 2nd-stage bootloader **cannot be updated via OTA**, any bootloader features you need (rollback, anti-rollback, secure boot) must be enabled **before the device ships**. If you ship without rollback support in the bootloader, you can never add it later through an OTA update. This is a one-way decision.

```
 Key insight: TWO firmware slots (ota_0 and ota_1).
  Device runs from one slot while the other receives the new firmware.
  If the update fails, the old firmware is still intact in the other slot.
```

#### Partition Table CSV (OTA-enabled)

The partition table is defined as a simple CSV file:

```
# Name,    Type, SubType, Offset,   Size,  Flags
nvs,       data, nvs,     ,         0x4000,
otadata,   data, ota,     ,         0x2000,
phy_init,  data, phy,     ,         0x1000,
ota_0,     app,  ota_0,   ,         1M,
ota_1,     app,  ota_1,   ,         1M,
```

**Key Points:**

- `ota_0` and `ota_1` are **identical-size** application partitions (1 MB each)
- `otadata` tracks which partition is active (stores sequence numbers)
- Partition table occupies 4 KiB at offset 0x8000 (up to 95 entries + MD5 checksum)
- OTA partitions align on 64 KiB boundaries (flash cache page size)
- The factory partition is **not used** in OTA mode — it doesn't support anti-rollback

---
## IOT Security

### The CIA Triad — Security's Three Goals

Every security discussion starts with three goals. Let's see what each means for our SmartGlow light bulb:

#### C — Confidentiality

**Definition:** Only authorized parties can read the data.

**SmartGlow example:** The bulb sends usage data (when you're home, when you sleep). If an attacker reads this, they know when your house is empty.

**Tool:** Encryption (AES, TLS)

#### I — Integrity

**Definition:** Data has not been tampered with.

**SmartGlow example:** An attacker modifies a firmware update in transit, injecting malware. The bulb installs it thinking it's legitimate.

**Tool:** Hashing (SHA256), Digital Signatures

#### A — Availability

**Definition:** The system is accessible when needed.

**SmartGlow example:** An attacker floods the bulb's WiFi with packets, making it unresponsive. Or a bad OTA update bricks 50,000 devices.

**Tool:** Rollback, Rate limiting, Watchdog timers

---

**What is Wireshark?** — A free, open-source tool that captures every packet flowing through your network interface. Think of it as a security camera for your network traffic. Anyone on the same WiFi can run it.

**What an attacker can _still_ see even with HTTPS (with HTTP, they can see everything):**

- **IP addresses** — they know your ESP32 (192.168.1.42) is talking to a server (104.21.32.1)
- **Packet sizes & timing** — they can guess how often your sensor reports
- **DNS queries** — they may see your device looked up `myiot-server.com` (unless you use DNS-over-HTTPS)

But they **cannot read** the actual data, API keys, or commands — that's the whole point of encryption.

There's a single operation at the heart of nearly all encryption: **XOR** (exclusive OR). You already know AND and OR from your digital logic course. XOR is the third sibling:

**The magic property:** XOR is its own inverse. If you XOR something twice with the same key, you get the original back:

>XOR each byte of your message with a key byte, and you get encrypted output. XOR again with the same key to decrypt. One operation, perfectly reversible.

What if we XOR our message with a key that is **completely random** and **as long as the message itself**? That's called a **One-Time Pad (OTP)**, and it's the only encryption scheme that is _mathematically proven_ to be unbreakable.

**Why it's perfect:**

- Key is truly random — no patterns to exploit
- Key is as long as the message — every byte has its own unique key byte
- Used only once — no repeated patterns across messages

**Why it's impractical:**

- Key must be as long as all data you'll ever send
- Key can never be reused (hence "one-time")
- You still need a secure way to share the key!
- Your ESP32 sending 1 MB/day would need 1 MB of fresh random key every day


**What happens if you reuse a One-Time Pad key?** Disaster. If an attacker captures two messages encrypted with the same key, they can XOR the two ciphertexts together — the key cancels out, and the attacker gets the XOR of the two plaintexts, which is often enough to recover both messages. This actually happened in real history (look up the **Venona project** — the US cracked Soviet spy messages because some key pages were reused).

<u>**Pseudorandom Number Generator (PRNG)**</u>

  Short Key (seed)              Pseudorandom Sequence Generator
  ┌──────────┐                 ┌──────────────────────────────────────┐
  │ "s3cReT"  │────seed──────▶│  Generates a LONG stream of bytes    │
  │ (6 bytes) │                │  that LOOK random but are            │
  └──────────┘                 │  deterministic (same seed = same     │
                               │  sequence every time)                │
                               └───────────────┬──────────────────────┘
                                               │
                                               ▼
                               0x7A 0x2F 0xB1 0x93 0xDE 0x44 0x8C 0xF7 ...
                               (as many bytes as you need!)

  Now XOR this pseudorandom stream with your message — same idea as OTP!

This is called a **stream cipher**. It's the practical version of the One-Time Pad:
- The sequence is _not truly random_ — it's deterministic
- A weak PRNG can be predicted by attackers (e.g., `rand()` in C is NOT cryptographically secure)
- Must use a **CSPRNG** (Cryptographically Secure PRNG) — e.g., `/dev/urandom`, `esp_random()`

### **AES (Advanced Encryption Standard)**

The ESP32 has a **hardware AES accelerator** built into the chip! This means AES encryption runs fast without burning CPU cycles — Espressif designed the chip knowing IoT devices need encryption.

| roperty       | What It Means                                                                                                                |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| **Confusion** | Each bit of the output depends on the key in a complex way (SubBytes + AddRoundKey)                                          |
| **Diffusion** | Changing 1 bit of input changes ~50% of output bits (ShiftRows + MixColumns)                                                 |
| **Key size**  | AES-128 = 128-bit key (2128 possible keys ≈ 3.4 × 1038). Brute-forcing this at 1 billion keys/second would take ~1022 years. |

#### Problem with Symmetric Encryption

**How do Alice and Bob share the key in the first place?**

#### Asymmetric Encryption (Public + Private Key Pair)

The breakthrough that made internet commerce possible. Each person has **two keys**:

![[Pasted image 20260428121642.png]]

The math behind it:

| Algorithm | Based On                                 | Key Size                          | Used For                                 |
| --------- | ---------------------------------------- | --------------------------------- | ---------------------------------------- |
| **RSA**   | Hard to factor large numbers (p × q = n) | 2048 or 4096 bits                 | TLS certificates, secure boot signatures |
| **ECDSA** | Elliptic curve discrete logarithm        | 256 bits (equivalent to RSA-3072) | ESP32 secure boot v1, compact signatures |

**Why not use asymmetric for everything?** Asymmetric encryption is **~1000x slower** than symmetric (AES). The ESP32 has limited CPU. So in practice, we use asymmetric to securely exchange a symmetric key, then use AES for the actual data. This is exactly what TLS does.


### Hashing — Detecting Tampering

- A hash function takes any input and produces a fixed-size "fingerprint" (digest). Like a fingerprint, it's unique to the input.
- Same input always gives the same hash. Run SHA256("Hello") a million times — always the same result.
-  You **cannot** reverse a hash back to the original input. Given a hash, you can't figure out what produced it.
- A tiny change in input produces a **completely different** hash. Flip one bit in a firmware file — the hash is unrecognizable.
- t's practically impossible to find two different inputs with the same hash. SHA256 has 2256 possible outputs — more than atoms in the observable universe.

**NOTE**: Encryption protects _confidentiality_. Hashing protects _integrity_.

### Digital signatures - Authenticity

![[Pasted image 20260428122156.png]]


private keys are stored in **Hardware Security Modules (HSMs)** — tamper-proof chips that never expose the key
#### HTTP vs HTTPS — What Eve Sees

| What's Visible  | HTTP (no TLS)                      | HTTPS (with TLS)                 |
| --------------- | ---------------------------------- | -------------------------------- |
| URL path        | `/firmware/v1.1.bin` (visible)     | Hidden (encrypted)               |
| Request headers | All visible (cookies, auth tokens) | Hidden (encrypted)               |
| Response body   | Entire firmware binary visible     | Hidden (encrypted)               |
| Server IP       | Visible                            | Visible (can't hide destination) |
| Domain name     | Visible                            | Visible (SNI extension)          |
| Data length     | Exact size visible                 | Approximate size visible         |
### TLS & HTTPS — Securing the Connection

**TLS Handshake**

![[Pasted image 20260428122700.png]]


This is a real problem for IoT! If SmartGlow's server certificate expires and the embedded CA cert only trusts the old one, devices can't update anymore. **Solution:** Embed the _root CA_ certificate (valid for 20+ years), not the server certificate directly. The server can renew its certificate from the same CA without needing to update devices.

TLS protects data _in transit_. But what if an attacker physically accesses the ESP32 and flashes malicious firmware via USB? Secure Boot ensures only **signed firmware can execute**.

**Flash encryption is transparent:** Your code doesn't change. The ESP32 hardware automatically decrypts flash contents as they're read into CPU cache. You write normal code; the encryption/decryption is invisible to your application.


| Attack                  | How It Works                                        | Defense                                         | SmartGlow Scenario                                                         |
| ----------------------- | --------------------------------------------------- | ----------------------------------------------- | -------------------------------------------------------------------------- |
| **Default Credentials** | Try admin/admin, root/root on every device          | Unique per-device credentials, no defaults      | Bulbs ship with factory password "smartglow" — attacker controls all bulbs |
| **Man-in-the-Middle**   | Intercept WiFi traffic, read/modify data            | TLS/HTTPS with certificate pinning              | Attacker injects malicious firmware during OTA download                    |
| **Replay Attack**       | Record a valid command, replay it later             | Timestamps + nonces in messages                 | Record "turn on" command, replay at 3 AM                                   |
| **Firmware Extraction** | Read flash chip, reverse-engineer firmware          | Flash encryption + secure boot                  | Competitor extracts SmartGlow firmware, clones the product                 |
| **Downgrade Attack**    | Force device to run an older, vulnerable firmware   | Anti-rollback (eFuse security version)          | Attacker forces rollback to v1.0 with known MQTT vulnerability             |
| **Side-Channel**        | Measure power consumption or timing to extract keys | Constant-time crypto, power noise injection     | Attacker measures ESP32 power draw during AES to guess the key             |
| **Denial of Service**   | Flood device with requests until it crashes         | Rate limiting, watchdog timer, input validation | Attacker sends 10,000 MQTT messages/sec, bulb crashes                      |
![[Pasted image 20260428123207.png]]

![[Pasted image 20260428122936.png]]



---

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

### Cellular Data(LTE/4G/5G)

Cellular IoT leverages existing mobile network infrastructure (cell towers) to provide wide-area connectivity. **4G/LTE** offers high bandwidth for data-intensive applications, while **5G** promises ultra-low latency and massive device density, enabling new categories of IoT applications.

**PROS**:
	1. Wide coverage
	2. 5G: ultra low latency
	3. High data rates
	4. Supports mobility
	5. Reliable and well established infrastructure


**CONS:**
	1. Higher power consumption than BLE, Zigbee, LoRaWan
	2. Ongoing subscription costs
	3. Higher cost per device
	4. Coverage gaps in remote/rural areas

### NB-IoT (Narrowband IoT)

NB-IoT is a **cellular LPWAN** technology standardized by 3GPP. It operates within existing LTE bands but uses a very narrow bandwidth (200 kHz), providing excellent coverage — including deep indoor and underground penetration — with very low power consumption. It bridges the gap between cellular and LPWAN.

**PROS**:
	1. Wide coverage via telecom operators
	2. Very low power consumption
	3. Excellent penetration
	4. Uses existing cellular infrastructure
	5. Reliable and carrier managed


**CONS:**
	1. Depends on cellular network providers
	2. Limited bandwith
	3. Higher latency than WiFi or 4G
	4. Not suitable for real-time or high throughput applications

----
### Other Notable protocols

1. Sigfox
	- An ultra-narrowband LPWAN (Low power WAN) operating on ISM bands with extremely low data rates (100 bps) but ranges up to 40 km.
	- Designed for very simple sensors that send a few messages per day. 
	- Sigfox operates its own network infrastructure.
	- eg: Simple asset tracking, parking sensors, basic environmental monitoring.
	
2. 6LoWPAN
	- An adaptation layer that allows IPv6 packets to be carried over IEEE 802.15.4 networks. 
	- It enables low-power devices to communicate using standard Internet protocols, bridging the gap between constrained IoT devices and the IP world.
3. LTE-M
	- Another cellular LPWAN alongside NB-IoT, but with higher bandwidth (up to 1 Mbps) and support for mobility and voice. 
	- Better suited for applications that need more data throughput or device mobility than NB-IoT can provide.



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


| Protocol             | Range | Power | Data rate | Cost | Topology | Best for |
| -------------------- | ----- | ----- | --------- | ---- | -------- | -------- |
| Wi-Fi                |       |       |           |      |          |          |
| Bluetooth/BLE        |       |       |           |      |          |          |
| Zigbee               |       |       |           |      |          |          |
| Z-Wave               |       |       |           |      |          |          |
| Cellular data(4G/5G) |       |       |           |      |          |          |
| LoRaWAN              |       |       |           |      |          |          |
| NB-IoT               |       |       |           |      |          |          |
| Sigfox               |       |       |           |      |          |          |

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