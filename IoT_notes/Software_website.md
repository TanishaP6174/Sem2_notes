
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

## NETWORKING

