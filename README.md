# freertos-blinky-rp2350
Blink and LED every 250 ms and print hello world (over uart) every 1000 ms with FreeRTOS on the rp2350.

## How to build:
Make sure the following include paths are updated on your system: \
PICO_SDK_PATH \
FREERTOS_KERNEL_PATH

Note for FREERTOS_KERNEL_PATH, you want to point it to the official FreeRTOS Kernel, not the raspberrypi fork of the FreeRTOS Kernel. This is because the RP2350 port has been upstreamed, so the raspberrypi fork is outdated. Official FreeRTOS Kernel: https://github.com/FreeRTOS/FreeRTOS-Kernel 

Also, you'll want to run 
```
git submodule update --init --recursive
```
on the FreeRTOS Kernel that you cloned. This is because the RP2350 FreeRTOS port (RP2350_ARM_NTZ) is within the FreeRTOS-Kernel_Community-Supported-Ports submodule of the FreeRTOS kernel. A link to that subdirectory in the FreeRTOS kernel is below: \
https://github.com/FreeRTOS/FreeRTOS-Kernel-Community-Supported-Ports/tree/bae4c7aa19009825ba48071a8fe25dcb8be84880/GCC/RP2350_ARM_NTZ

Once the include paths are updated and the submodules are updated properly, make and build. 
```
mkdir build
cd build
cmake ..
make
```

The output is build/blinky.uf2 or build/blinky.elf depending on which you want to use to flash to the board.

Note that stdio is over UART, so you will need to listen to the port /dev/ttyACM0 for example, using picocom to see the printf output. (This might be different for you if you aren't using Linux)


## All steps taken to set everything up
- clone FreeRTOS Kernel and update submodules   
- update PICO_SDK_PATH and FREERTOS_KERNEL_PATH   
- copy pico_sdk_import.cmake from your pico SDK
- copy FreeRTOS_Kernel_import.cmake from https://github.com/FreeRTOS/FreeRTOS-Kernel-Community-Supported-Ports/blob/bae4c7aa19009825ba48071a8fe25dcb8be84880/GCC/RP2350_ARM_NTZ/FreeRTOS_Kernel_import.cmake   
- copy FreeRTOS config from https://github.com/gabor-budai/FreeRTOS_PICO2350/blob/master/src/FreeRTOSConfig_examples_common.h
     - it is worth noting the FreeRTOS configEnable_MPU, configENABLE_TRUSTZONE, configRUN_FREERTOS_SECURE_ONLY, configENABLE_FPU, and configMAX_SYSCALL_INTERRUPT_PRIORITY, since they are specifically discussed in the README of the rp2350 FreeRTOS Kernel Port's README.     
- CMakeLists was created, adding the following important lines in order (Note that the FreeRTOS import include should come before pico_sdk_init()):
```
include(FreeRTOS_Kernel_import.cmake)
```
```
target_link_libraries(your_project PRIVATE
pico_multicore
pico_stdlib
FreeRTOS-Kernel
FreeRTOS-Kernel-Heap4
)
```
```
target_include_directories(your_project PRIVATE
include/
)
```
The above include is only necessary if you also put FreeRTOSConfig.h in the include/ directory.   

I got most of these steps from dazzlinggod's comment on this forum: https://forums.raspberrypi.com/viewtopic.php?t=378428   

