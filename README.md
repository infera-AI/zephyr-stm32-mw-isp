# Zephyr STM32 Image Signal Processing (ISP) Module

This module provides the necessary code and libraries to enable Image Signal Processing (ISP) support for Zephyr RTOS. It is compatible with the Zephyr STM32 DCMIPP driver.

## Module Components

The module consists of the following components:

1. **ISP Middleware Stack**
   Located in the `stm32-mw-isp` directory, this stack provides the core ISP functionality.
   > **Note:** This code is provided as a submodule. Ensure that you include `submodules: true` in your `west.cfg` manifest file.

2. **Glue Code**
   Found in the `isp_wrapper` directory, this code bridges the Zephyr DCMIPP driver and the ISP middleware stack.

## How to Use the Module

### 1. Add the Module to Your West Manifest

Include the following lines in the `projects` section of your `west.yml` manifest file. Adjust the `name`, `path`, and `revision` fields as needed for your project:

```yaml
    - name: zephyr-stm32-mw-isp
      url: https://github.com/stm32-hotspot/zephyr-stm32-mw-isp.git
      revision: main
      path: st/lib/zephyr-stm32-mw-isp
      submodules: true
```

Ensure that the `stm32-mw-isp` submodule is initialized and updated by running the following command in your project directory:
```
west update
```

### 2. Enable the Module in Your Project Configuration

Add the following line to your project's `prj.conf` file to enable the ISP module:

```
CONFIG_USE_STM32_MW_ISP=y
```

### 3. Build and Run

After completing the above steps, build your Zephyr project as usual. The ISP module will be included and configured automatically.
For instance, to run `samples/drivers/video/capture` sample on `stm32n6570_dk`, use the following:
```
west build -p always -b stm32n6570_dk/stm32n657xx --sysbuild samples/drivers/video/capture -- -DSHIELD=st_b_cams_imx_mb1854
west flash
```

## Additional Notes

For detailed documentation on the ISP middleware stack, refer to the `stm32-mw-isp` directory.

## Compatibility

This module is designed to work seamlessly with the STM32 DCMIPP driver in Zephyr RTOS starting [release v4.2.0](https://github.com/zephyrproject-rtos/zephyr/tree/v4.2.0).
Ensure that your hardware and software configurations are compatible with the STM32 DCMIPP driver requirements.

---
