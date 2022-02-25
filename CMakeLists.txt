# CMakeList.txt : Top-level CMake project file, do global configuration
# and include sub-projects here.
#
cmake_minimum_required (VERSION 3.8)

# Is this a typo??
set(CXX_STANARD_REQUIRED )


project ("CMakeProject1")
 
# Include sub-projects.
#add_subdirectory ("libopeninv")


# The executable....
set(CMAKE_C_STANDARD 99)
set(CMAKE_CXX_STANDARD 20)

#set(CMAKE_C_FLAGS "-Os -Wall -Wextra -mcpu=cortex-m3 -mthumb -nostartfiles -ffunction-sections -fdata-sections -fno-common -fno-builtin  -fno-exceptions ") 
#set(CMAKE_CXX_FLAGS "-Os -Wall -Wextra -mcpu=cortex-m3 -mthumb -nostartfiles -ffunction-sections -fdata-sections -fno-common -fno-builtin -fno-rtti -fno-exceptions -fno-unwind-tables")
add_compile_options (-Os -Wall -Wextra -mcpu=cortex-m3 -mthumb -nostartfiles -ffunction-sections -fdata-sections -fno-common -fno-builtin -fno-rtti -fno-exceptions  -fno-unwind-tables)

add_compile_definitions(STM32F1)

add_executable(car-stuff-output
	src/chademo.cpp
	src/hwinit.cpp
	src/leafbms.cpp
	src/stm32_car.cpp
	src/terminal_prj.cpp
	src/throttle.cpp
	libopeninv/src/anain.cpp
	libopeninv/src/crc8.cpp
	libopeninv/src/digio.cpp
	libopeninv/src/errormessage.cpp
	libopeninv/src/foc.cpp
	libopeninv/src/fu.cpp
	libopeninv/src/linbus.cpp
	libopeninv/src/my_fp.c
	libopeninv/src/my_string.c
	libopeninv/src/params.cpp
	libopeninv/src/param_save.cpp
	libopeninv/src/picontroller.cpp
	libopeninv/src/printf.cpp
	libopeninv/src/sine_core.cpp
	libopeninv/src/stm32scheduler.cpp
	libopeninv/src/stm32_can.cpp
	libopeninv/src/terminal.cpp
	libopeninv/src/terminalcommands.cpp
)

target_include_directories(car-stuff-output PRIVATE . include ./include libopeninv/include libopencm3/include)

target_link_libraries(car-stuff-output "/mnt/j/2021/Household/Passat/EV-Source/libopencm3/lib/libopencm3_stm32f1.a")
#target_link_options(car-stuff-output PUBLIC "LINKER:-nostartfiles")
target_link_options(car-stuff-output PUBLIC "LINKER:--gc-sections,-L/mnt/j/2021/Household/Passat/EV-Source/libopencm3/lib,-T/mnt/j/2021/Household/Passat/EV-Source/stm32_car.ld,-Map=linker.map")

#add_custom_command(
#    TARGET ${EXECUTABLE_NAME}
#    POST_BUILD
#	 COMMAND ${CMAKE_OBJCOPY} ARGS -O ihex ${EXECUTABLE_NAME} ${PROJECT_NAME}.hex
#)

add_custom_command(
    TARGET car-stuff-output
	POST_BUILD
	COMMAND arm-none-eabi-objcopy ARGS -O ihex car-stuff-output car-stuff-output.hex
	COMMAND arm-none-eabi-objcopy ARGS -O binary car-stuff-output car-stuff-output.bin
	COMMAND arm-none-eabi-size car-stuff-output 
	BYPRODUCTS car-stuff-output.hex car-stuff-output.bin
)

