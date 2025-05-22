# Generated Cmake Pico project file

cmake_minimum_required(VERSION 3.13)

if(PROFILE STREQUAL "OFF") 
 set(PROFILE PICO)
endif()

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# Initialise pico_sdk from installed location
# (note this can come from environment, CMake cache etc)

# == DO NOT EDIT THE FOLLOWING LINES for the Raspberry Pi Pico VS Code Extension to work ==
if(WIN32)
    set(USERHOME $ENV{USERPROFILE})
else()
    set(USERHOME $ENV{HOME})
endif()
set(sdkVersion 2.1.1)
set(toolchainVersion 14_2_Rel1)
set(picotoolVersion 2.1.1)
set(picoVscode ${USERHOME}/.pico-sdk/cmake/pico-vscode.cmake)
if (EXISTS ${picoVscode})
    include(${picoVscode})
endif()
# ====================================================================================
set(PICO_BOARD pico CACHE STRING "Board type")

# Pull in Raspberry Pi Pico SDK (must be before project)
#include(${CMAKE_CURRENT_LIST_DIR}/pico-sdk/pico_sdk_init.cmake)
include(pico_sdk_import.cmake)

project(samwise-adcs C CXX ASM)

# Initialise the Raspberry Pi Pico SDK
pico_sdk_init()

file(GLOB top CONFIGURE_DEPENDS "src/*.cpp")
file(GLOB gnc CONFIGURE_DEPENDS "src/gnc/*.cpp")
file(GLOB drivers CONFIGURE_DEPENDS "src/drivers/*.cpp")
file(GLOB drivers_external CONFIGURE_DEPENDS "src/drivers/external/*.[c|cpp]")

add_executable(samwise-adcs ${top} ${gnc} ${drivers} ${drivers_external})

pico_set_program_name(samwise-adcs "samwise_adcs")
pico_set_program_version(samwise-adcs "0.1")

# Modify the below lines to enable/disable output over UART/USB
pico_enable_stdio_uart(samwise-adcs 0)
pico_enable_stdio_usb(samwise-adcs 1)

# Add the standard library to the build
target_link_libraries(samwise-adcs
  hardware_i2c
  hardware_spi
  hardware_uart
  pico_stdlib 
  pico_util
)

# Add the standard include files to the build
target_include_directories(samwise-adcs PRIVATE
  ${CMAKE_CURRENT_LIST_DIR}
  ${CMAKE_CURRENT_LIST_DIR}/.. # for our common lwipopts or any other standard includes, if required
)

target_include_directories(samwise-adcs PRIVATE ${CMAKE_CURRENT_LIST_DIR}/src)

pico_add_extra_outputs(samwise-adcs)