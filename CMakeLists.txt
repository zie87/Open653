cmake_minimum_required(VERSION 3.10.8)

include(CMakeForceCompiler)
CMAKE_FORCE_C_COMPILER(arm-none-eabi-gcc GNU)


# Name of the OS CMake is building for.
set(CMAKE_SYSTEM_NAME Generic)


# Set a name, version, and enable languages for the entire project.
project(OS C ASM)


# Define directories
set(SRC_DIR ${CMAKE_SOURCE_DIR}/src)
set(ARINC_DIR ${CMAKE_SOURCE_DIR}/third_party/ARINC)
set(LINKER_SCRIPTS_DIR ${CMAKE_SOURCE_DIR}/third_party/linker_scripts)


# Set all the compiler flags
set(gc_flags "-ffunction-sections -fdata-sections -Wl,--gc-sections") # These flags helps with dead code elimination. More info can found at http://stackoverflow.com/a/10809541
set(mcu_flags "-mcpu=cortex-m4 -mtune=cortex-m4 -mthumb -mlittle-endian -mfpu=fpv4-sp-d16 -mfloat-abi=hard -mthumb-interwork")
set(linker_flags "-Wl,-T${LINKER_SCRIPTS_DIR}/STM32F415RG_FLASH.ld -Wl,-gc-sections -Wl,-LTO -nostartfiles -Wl,-Map=./result.map --specs=nosys.specs")
set(WARNINGS "-Wall -Wextra")
set(CFLAGS "-O0 -g -ggdb -LTO -std=c99 -fms-extensions ${WARNINGS} ${gc_flags} ${mcu_flags} ${linker_flags}")
set(CMAKE_C_FLAGS ${CFLAGS})


# Add definition
add_definitions(-DHSE_VALUE=16000000 -DSTM32F415xx)


# Use following dirs for header lookup
include_directories(
	# SRC
	${SRC_DIR}
	# ARINC APEX
	${ARINC_DIR}/APEX
)

add_subdirectory(third_party)
add_subdirectory(src)
