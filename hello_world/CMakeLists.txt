cmake_minimum_required(VERSION 3.12)

set(NAME hello-world)

set(PICO_SDK_PATH "${CMAKE_SOURCE_DIR}/../vendor/pico-sdk")

set(PICO_BOARD "pico2_w")
set(PICO_PLATFORM "rp2350-arm-s")

# Pull in Raspberry Pi Pico SDK (must be before project)
include(${CMAKE_CURRENT_LIST_DIR}/../cmake/pico_sdk_import.cmake)

project(${NAME} LANGUAGES C CXX ASM)
pico_sdk_init()

add_executable(${NAME}
    hello_world.c
)

# Add pico_stdlib library which aggregates commonly used features
target_link_libraries(${NAME} pico_stdlib)

pico_enable_stdio_usb(${NAME} 1)
pico_enable_stdio_uart(${NAME} 1)

# create map/bin/hex/uf2 file in addition to ELF.
pico_add_extra_outputs(${NAME})

target_compile_definitions(${NAME} PRIVATE
    PICO_DEFAULT_UART_TX_PIN=16
    PICO_DEFAULT_UART_RX_PIN=17
)
