## How to Use the Modules

### 1. Add the Module to Your West Manifest
Include the following lines in the `remotes` section of your `west.yml` manifest file. 

```yaml
  - name: infera-ai-org
    url-base: git@github.com:infera-AI
```

Include the following lines in the `projects` section of your `west.yml` manifest file. 

```yaml
   - name: infera-gc05a2
     remote: infera-ai-org
     revision: main
     path: modules/lib/infera_gc05a2
   - name: zephyr-stm32-mw-isp
     remote: infera-ai-org
     revision: main
     path: modules/lib/zephyr-stm32-mw-isp
     submodules: true
```

Ensure that the `stm32-mw-isp` and `infera-gc05a2` submodule is initialized and updated by running the following command in your project directory:
```
west update
```


### 2. Download the Example Application

Clone the `tcpserversink` example application to your `zephyrproject` directory:

```bash
cd ~/zephyrproject
git clone git@github.com:Infera-AI/tcpserversink.git
```

Or if you already have the example, ensure it's located at:
```
zephyrproject/
├── tcpserversink/
├── zephyr/
├── modules/
└── ...
```

### 3. Build the Application

Navigate to the `tcpserversink` directory and build the application with H.264 video encoding support:

```bash
cd tcpserversink
west build -p always -b stm32n6570_dk/stm32n657xx --sysbuild . -- -DSHIELD=infera_gc05a2 -DFILE_SUFFIX=venc
```

**Build Parameters Explained:**
- `-p always` - Pristine build (clean before building)
- `-b stm32n6570_dk/stm32n657xx` - Target board
- `--sysbuild` - Enable sysbuild for MCUboot support
- `-DSHIELD=infera_gc05a2` - Use the GC05A2 camera sensor shield
- `-DFILE_SUFFIX=venc` - Enable H.264 video encoder configuration

### 4. Flash the Application

#### Step 1: Set BOOT Pins for Flashing Mode

Before flashing, configure the board's BOOT pins:

**BOOT Configuration for Flashing:**
- **BOOT0 = 0** (Low)
- **BOOT1 = 1** (High)

#### Step 2: Flash the Firmware

```bash
west flash
```

Wait for the flashing process to complete. You should see output indicating successful programming.

#### Step 3: Set BOOT Pins for Normal Operation

After flashing is complete, reconfigure the BOOT pins for normal boot:

**BOOT Configuration for Normal Boot:**
- **BOOT0 = 0** (Low)
- **BOOT1 = 0** (Low)

#### Step 4: Power Cycle the Board

Disconnect and reconnect power to the board, or press the reset button, to start the application.

### 5. Running the Application

Once the board boots, it will:
1. Initialize the GC05A2 camera sensor
2. Start the H.264 video encoder
3. Wait for TCP client connection on port 5000

Connect to the video stream using GStreamer:

```bash
gst-launch-1.0 tcpclientsrc host=<board-ip> port=5000 ! queue ! decodebin ! autovideosink sync=false
```

Replace `<board-ip>` with your STM32N6570-DK board's IP address.

