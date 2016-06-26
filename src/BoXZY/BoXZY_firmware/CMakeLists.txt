#
# CMake file for BoXZY Firmware
#

cmake_minimum_required(VERSION 3.1.0 FATAL_ERROR)

## ----------------------------------------------------------------------------

## Project:
SET(this_target BoXZY_firmware)
PROJECT(${this_target} C CXX ASM)

#====================================================================#
# Sources
#====================================================================#

set(SOURCE_FILES
	"${this_target}.ino"
	"BedLeveling.cpp"
	"BoXZYLaser.cpp"
	"Commands.cpp"
	"Communication.cpp"
	"Drivers.cpp"
	"Eeprom.cpp"
	"Extruder.cpp"
	"gcode.cpp"
	"HAL.cpp"
	"motion.cpp"
	"Printer.cpp"
	"SDCard.cpp"
	"SdFat.cpp"
	"ui.cpp"
	"uilang.cpp"
)

set(HEADER_FILES
	"BoXZYLaser.h"
	"Commands.h"
	"Communication.h"
	"Configuration.h"
	"Drivers.h"
	"Eeprom.h"
	"Events.h"
	"Extruder.h"
	"fastio.h"
	"FatStructs.h"
	"gameduino2.h"
	"gcode.h"
	"gd2_ext.h"
	"HAL.h"
	"logo.h"
	"motion.h"
	"pins.h"
	"Printer.h"
	"Repetier.h"
	"SdFat.h"
	"u8glib_ex.h"
	"uiconfig.h"
	"ui.h"
	"uilang.h"
	"uimenu.h"
)

#====================================================================#
# Extra Include Paths (for Syntax Highlighter and Code Completion)
#====================================================================#

# The following are redundant, but lets QtCreator code completion and highlighting work:
if (${ARDUINO_PLATFORM} STREQUAL "AVR")
	include_directories(
		"${AVR_CORES_PATH}/arduino"
		${${${BOARD_TAG}.build.variant}.path}
	)
	add_definitions(-D__AVR__)
elseif (${ARDUINO_PLATFORM} STREQUAL "SAM")
	include_directories(
		"${SAM_CORES_PATH}/arduino"
		${${${BOARD_TAG}.build.variant}.path}
		"${SAM_SYSTEM_PATH}/libsam"						# needed for chip.h include
		"${SAM_SYSTEM_PATH}/libsam/include"				# needed for other system includes
		"${SAM_SYSTEM_PATH}/CMSIS/CMSIS/Include"
		"${SAM_SYSTEM_PATH}/CMSIS/Device/ATMEL"
	)
	add_definitions(-D__arm__)
elseif (${ARDUINO_PLATFORM} STREQUAL "SAMD")
	include_directories(
		"${SAMD_CORES_PATH}/arduino"
		${${${BOARD_TAG}.build.variant}.path}
		"${ARDUINO_CMSIS_PATH}/CMSIS/Include"
		"${ARDUINO_CMSIS_PATH}/Device/ATMEL"
	)
	add_definitions(-D__arm__)
endif()

#====================================================================#
# Firmware Generation
#====================================================================#
if (${ARDUINO_PLATFORM} STREQUAL "AVR")
#	set(ARDUINO_CPP_FLAGS "--verbose -fshort-enums -D__PROG_TYPES_COMPAT__ -Wno-deprecated-declarations")
	set(ARDUINO_CPP_FLAGS "--verbose -D__PROG_TYPES_COMPAT__ -Wno-deprecated-declarations")
elseif ((${ARDUINO_PLATFORM} STREQUAL "SAM") OR (${ARDUINO_PLATFORM} STREQUAL "SAMD"))
#	set(ARDUINO_CPP_FLAGS "-Wa,-adhlns=\$(basename \$@).lst --verbose -fshort-enums")
	set(ARDUINO_CPP_FLAGS "-Wa,-adhlns=\$(basename \$@).lst --verbose")
endif()
set(ARDUINO_C_FLAGS "")
set(ARDUINO_CXX_FLAGS "-std=c++11")

if (${ARDUINO_PLATFORM} STREQUAL "AVR")
#	set(ARDUINO_LINKER_FLAGS "-Wl,-u,vfprintf -lprintf_flt -Wl,-u,vfscanf -lscanf_flt")
endif()

if (${ARDUINO_PLATFORM} STREQUAL "AVR")
	generate_arduino_firmware(${this_target}
		SKETCH "${CMAKE_CURRENT_SOURCE_DIR}/${this_target}.ino"
		SRCS ${SOURCE_FILES}
		HDRS ${HEADER_FILES}
#		ARDLIBS SdFat SerialPort
#		NO_AUTOLIBS
	)
elseif (${ARDUINO_PLATFORM} STREQUAL "SAM")
	generate_arduino_firmware(${this_target}
		SKETCH "${CMAKE_CURRENT_SOURCE_DIR}/${this_target}.ino"
		SRCS ${SOURCE_FILES}
		HDRS ${HEADER_FILES}
#		ARDLIBS SdFat SerialPort
#		NO_AUTOLIBS
	)
elseif (${ARDUINO_PLATFORM} STREQUAL "SAMD")
	generate_arduino_firmware(${this_target}
		SKETCH "${CMAKE_CURRENT_SOURCE_DIR}/${this_target}.ino"
		SRCS ${SOURCE_FILES}
		HDRS ${HEADER_FILES}
#		ARDLIBS SdFat SerialPort
#		NO_AUTOLIBS
	)
else()
	message(FATAL_ERROR "Unknown Arduino Platform!")
endif()

set_property(TARGET ${this_target} PROPERTY CXX_STANDARD 11)
set_property(TARGET ${this_target} PROPERTY CXX_STANDARD_REQUIRED ON)

