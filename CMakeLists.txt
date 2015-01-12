# ================================================================
# Generate a project
# mkdir build
# cd build
# cmake -G "XCode" ../
# cmake -G "Visual Studio 12" ../
# ================================================================

# NOTES - for building for VS2012
# FCEUX requires zlib - needs ZLIB_INCLUDE definition
# FCEUX runtime library needs to be changed to /MT /MTd
# FCEUX character set needs to be changed to Unicode

# ================================================================

CMAKE_MINIMUM_REQUIRED(VERSION 2.8)
PROJECT(fceux)

# ================================================================

# This is where FCEUX finds zlib - and may change from machine to machine
set(ZLIB_INCLUDE "D:\\Work\\active\\laputa\\external\\nimble\\external\\zlib")

# ================================================================

set(CMAKE_C_FLAGS "-g -Wall ${CMAKE_C_FLAGS}")
set(CMAKE_CXX_FLAGS "-g -Wall ${CMAKE_CXX_FLAGS}")

# ================================================================

FILE(GLOB main_src ${CMAKE_CURRENT_SOURCE_DIR}/src-fceumm/*.c)
FILE(GLOB main_inc ${CMAKE_CURRENT_SOURCE_DIR}/src-fceumm/*.h)
FILE(GLOB boards_src ${CMAKE_CURRENT_SOURCE_DIR}/src-fceumm/boards/*.c)
FILE(GLOB boards_inc ${CMAKE_CURRENT_SOURCE_DIR}/src-fceumm/boards/*.h)
FILE(GLOB fir_src ${CMAKE_CURRENT_SOURCE_DIR}/src-fceumm/fir/*.c)
FILE(GLOB fir_inc ${CMAKE_CURRENT_SOURCE_DIR}/src-fceumm/fir/*.h)
FILE(GLOB input_src ${CMAKE_CURRENT_SOURCE_DIR}/src-fceumm/input/*.c)
FILE(GLOB input_inc ${CMAKE_CURRENT_SOURCE_DIR}/src-fceumm/input/*.h)
FILE(GLOB mappers_src ${CMAKE_CURRENT_SOURCE_DIR}/src-fceumm/mappers/*.c)
FILE(GLOB mappers_inc ${CMAKE_CURRENT_SOURCE_DIR}/src-fceumm/mappers/*.h)
FILE(GLOB palettes_src ${CMAKE_CURRENT_SOURCE_DIR}/src-fceumm/palettes/*.c)
FILE(GLOB palettes_inc ${CMAKE_CURRENT_SOURCE_DIR}/src-fceumm/palettes/*.h)
FILE(GLOB snes_src ${CMAKE_CURRENT_SOURCE_DIR}/libsnes-fceumm/*.c)
FILE(GLOB snes_inc ${CMAKE_CURRENT_SOURCE_DIR}/libsnes-fceumm/*.h)

INCLUDE_DIRECTORIES(${CMAKE_CURRENT_SOURCE_DIR}/src)
INCLUDE_DIRECTORIES(${ZLIB_INCLUDE})

set(src ${src} ${main_src})
set(src ${src} ${boards_src})
set(src ${src} ${fir_src})
set(src ${src} ${input_src})
set(src ${src} ${mappers_src})
set(src ${src} ${palettes_src})
set(src ${src} ${snes_src})

set(inc ${inc} ${main_inc})
set(inc ${inc} ${boards_inc})
set(inc ${inc} ${fir_inc})
set(inc ${inc} ${input_inc})
set(inc ${inc} ${mappers_inc})
set(inc ${inc} ${palettes_inc})
set(inc ${inc} ${snes_inc})

source_group(main FILES ${main_src} ${main_inc})
source_group(boards FILES ${boards_src} ${boards_inc})
source_group(fir FILES ${fir_src} ${fir_inc})
source_group(input FILES ${input_src} ${input_inc})
source_group(mappers FILES ${mappers_src} ${mappers_inc})
source_group(palettes FILES ${palettes_src} ${palettes_inc})
source_group(snes FILES ${snes_src} ${snes_inc})

add_definitions(-DNDEBUG)
add_definitions(-D__LIBSNES__)
add_definitions(-DSOUND_QUALITY=0)
add_definitions(-DPATH_MAX=1024)

if(WIN32)
add_definitions(-DINLINE=__inline)
add_definitions(-DPSS_STYLE=2)
else()
add_definitions(-DHAVE_ASPRINTF)
add_definitions(-DINLINE=inline)
add_definitions(-DPSS_STYLE=1)
endif()

add_definitions(-DLSB_FIRST)
add_definitions(-DLOCAL_LE=1)

ADD_LIBRARY(fceux ${src} ${inc})

# ================================================================