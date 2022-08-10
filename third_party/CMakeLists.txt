# Compile CMSIS lib.
set(CMSIS_DIR ${CMAKE_CURRENT_SOURCE_DIR}/CMSIS)
add_library(CMSIS
	${CMSIS_DIR}/Device/ST/STM32F4xx/Source/startup_stm32f415xx.s
	${CMSIS_DIR}/Device/ST/STM32F4xx/Source/system_stm32f4xx.c
)
target_include_directories(CMSIS PUBLIC ${CMSIS_DIR}/Include ${CMSIS_DIR}/Device/ST/STM32F4xx/Include)
target_compile_options(CMSIS PRIVATE -Wno-extra -Wno-pedantic) # There is a lot of unused variable warnings in some hal function-prototypes
add_library(Open653::CMSIS ALIAS CMSIS)

# Compile HAL lib.
set(STM32f4xx_HAL_Driver_DIR ${CMAKE_CURRENT_SOURCE_DIR}/STM32F4xx_HAL_Driver)
file(GLOB src ${STM32f4xx_HAL_Driver_DIR}/Src/*.c)
add_library(STM32f4xx_HAL_Driver ${src})
target_include_directories(STM32f4xx_HAL_Driver PUBLIC ${STM32f4xx_HAL_Driver_DIR}/Inc)
target_link_libraries(STM32f4xx_HAL_Driver PUBLIC Open653::CMSIS)
target_compile_options(STM32f4xx_HAL_Driver PRIVATE -Wno-extra -Wno-pedantic) # There is a lot of unused variable warnings in some hal function-prototypes
add_library(Open653::STM32HAL ALIAS STM32f4xx_HAL_Driver)
