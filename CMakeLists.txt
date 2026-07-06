cmake_minimum_required(VERSION 3.13)
include(pico_sdk_import.cmake)

project(FreeRTOS-blinky-rp2350-project C CXX ASM)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

set(PICO_PLATFORM rp2350-arm-s)
set(PICO_BOARD pico2)

include(FreeRTOS_Kernel_import.cmake)

pico_sdk_init()

# force no optimizations
set(PICO_DEOPTIMIZED_DEBUG 1)

add_executable(blinky
    src/main.c
)

target_link_libraries(blinky
    pico_stdlib
    hardware_gpio
    hardware_timer
    hardware_resets
    hardware_clocks

    pico_multicore
    FreeRTOS-Kernel
    FreeRTOS-Kernel-Heap4
)

pico_enable_stdio_usb(blinky 0)
pico_enable_stdio_uart(blinky 1)

pico_add_extra_outputs(blinky)

target_compile_definitions(blinky PRIVATE
    PICO_RP2350=1
    PICO_DEBUG=1
)

# be explicit about using rp2350
target_include_directories(blinky PRIVATE
    include/
)

# remove optimization for debugging
target_compile_options(blinky PRIVATE
    -g3
    -O0
    -fno-inline
)
