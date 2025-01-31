
cmake_minimum_required(VERSION 3.12)

set(NAME freertos_blinky)
set(PICO_SDK_PATH "${CMAKE_SOURCE_DIR}/../vendor/pico-sdk")
set(FREERTOS_KERNEL_PATH "${CMAKE_SOURCE_DIR}/../vendor/rpi-freertos-kernel")

set(PICO_BOARD "pico2_w")
set(PICO_PLATFORM "rp2350-arm-s")

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# Pull in Raspberry Pi Pico SDK (must be before project)
include(${CMAKE_CURRENT_LIST_DIR}/../cmake/pico_sdk_import.cmake)

project(${NAME} LANGUAGES C CXX ASM)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 26)
set(PICO_CXX_ENABLE_EXCEPTIONS 1)

pico_sdk_init()

include(${CMAKE_CURRENT_LIST_DIR}/port/pico_config/pico_config.cmake)

include(${CMAKE_CURRENT_LIST_DIR}/port/freertos_config/freertos_config.cmake)
include(${CMAKE_CURRENT_LIST_DIR}/../cmake/FreeRTOS_Kernel_import.cmake)

add_executable(${NAME}
    src/main.cpp
    src/Agent.cpp
    src/BlinkWAgent.cpp
)

# Add pico_stdlib library which aggregates commonly used features
target_link_libraries(${NAME} pico_stdlib)
target_link_libraries(${NAME} FreeRTOS-Kernel-Heap4)
target_link_libraries(${NAME} pico_cyw43_arch_none)
target_link_libraries(${NAME} pico_config)
target_link_libraries(${NAME} freertos_config)

target_compile_definitions(${NAME} PRIVATE
    PICO_DEFAULT_UART_TX_PIN=16
    PICO_DEFAULT_UART_RX_PIN=17
)

pico_enable_stdio_usb(${NAME} 1)
pico_enable_stdio_uart(${NAME} 1)

# create map/bin/hex/uf2 file in addition to ELF.
pico_add_extra_outputs(${NAME})
