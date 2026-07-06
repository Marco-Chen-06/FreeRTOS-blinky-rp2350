# FreeRTOS-blinky-rp2350
Blink and LED every 250 ms and print hello world every 1000 ms with FreeRTOS on the rp2350.

# How to build:
Make sure the following include paths are updated on your system:
PICO_SDK_PATH
FREERTOS_KERNEL_PATH

Note for FREERTOS_KERNEL_PATH, you want to point it to the official FreeRTOS Kernel, not the raspberrypi fork of the FreeRTOS Kernel. This is because the RP2350 port has been upstreamed, so the raspberrypi fork is outdated. 
https://github.com/FreeRTOS/FreeRTOS-Kernel

Also, you'll want to run a 
```
git submodule update --init --recursive
```
on the FreeRTOS Kernel that you cloned. This is because the RP2350 FreeRTOS port (RP2350_ARM_NTZ) is within the FreeRTOS-Kernel_Community-Supported-Ports submodule of the FreeRTOS kernel. A link to that subdirectory in the FreeRTOS kernel is below:
https://github.com/FreeRTOS/FreeRTOS-Kernel-Community-Supported-Ports/tree/bae4c7aa19009825ba48071a8fe25dcb8be84880/GCC/RP2350_ARM_NTZ

Then make and build.
```
mkdir build
cd build
cmake -DPICO_BOARD=pico2 ..
make
```

Then run or flash your code in however method you want to.

Where I got the files from:
FreeRTOS config: https://github.com/gabor-budai/FreeRTOS_PICO2350/blob/master/src/FreeRTOSConfig_examples_common.h

FreeRTOS RP2350 Community-supported Port (NOT rp fork of FreeRTOS kernel): https://github.com/FreeRTOS/FreeRTOS-Kernel-Community-Supported-Ports/tree/bae4c7aa19009825ba48071a8fe25dcb8be84880/GCC/RP2350_ARM_NTZ
