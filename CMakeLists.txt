#[=========================================================================[
  Copyright (c) 2022-2024 Pedro López-Cabanillas

  Licensed under the Apache License, Version 2.0 (the "License");
  you may not use this file except in compliance with the License.
  You may obtain a copy of the License at

  http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
]=========================================================================]

cmake_minimum_required(VERSION 3.14)

project( sonivoxrender
    LANGUAGES C
    VERSION 1.0
)

option(FIND_SONIVOX "Find an installed Sonivox library" ON)
option(sonivox_SHARED_LIBS "Use the static library" ON)
option(USE_44KHZ "Output 44100 Hz audio sample rate (instead of 22050 Hz)" ON)
option(USE_16BITS_SAMPLES "Use 16 bits samples (instead of 8 bit)" ON)
option(NEW_HOST_WRAPPER "Use the new host wrapper" ON)
option(BUILD_MANPAGE "Build the manpage" OFF)

set(MAX_VOICES 64 CACHE STRING "Maximum number of voices")

message(STATUS "Finding Sonivox: ${FIND_SONIVOX}")
message(STATUS "Using Static Library: ${USE_STATICLIB}")

if (FIND_SONIVOX)
    find_package ( sonivox 4 CONFIG REQUIRED )
    message(STATUS "Found Sonivox Version: ${sonivox_VERSION}")
else()
    set(BUILD_TESTING OFF)
    set(BUILD_EXAMPLE OFF)
    add_subdirectory( sonivox )
    include(CMakePrintHelpers)
    cmake_print_properties(TARGETS sonivox::sonivox PROPERTIES VERSION SOVERSION)
endif()

add_executable ( sonivoxrender sonivoxrender.c )

target_link_libraries ( sonivoxrender sonivox::sonivox )

if (BUILD_MANPAGE)
    if(NOT EXISTS ${PANDOC_EXECUTABLE})
        find_program(PANDOC_EXECUTABLE pandoc)
        mark_as_advanced(PANDOC_EXECUTABLE)
    endif()

    if (EXISTS ${PANDOC_EXECUTABLE})
        set(_src ${CMAKE_CURRENT_SOURCE_DIR}/sonivoxrender.1.md)
        if (NOT PROJECT_RELEASE_DATE)
            unset(_date)
            execute_process (
                COMMAND bash -c "LANG=en;date +'%B %d, %Y'"
                OUTPUT_VARIABLE _date
                OUTPUT_STRIP_TRAILING_WHITESPACE
            )
        else()
            set(_date ${PROJECT_RELEASE_DATE})
        endif()
        set(_footer "${PROJECT_NAME} ${PROJECT_VERSION}")
        add_custom_command (
            OUTPUT sonivoxrender.1
            COMMAND ${PANDOC_EXECUTABLE} -s -t man -Vdate=${_date} -Vfooter=${_footer} ${_src} -o sonivoxrender.1
            DEPENDS ${_src}
            WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
            VERBATIM
        )
        add_custom_target(manpage ALL DEPENDS sonivoxrender.1)
    endif()
else()
    configure_file(${CMAKE_CURRENT_SOURCE_DIR}/sonivoxrender.1 ${CMAKE_CURRENT_BINARY_DIR} COPYONLY)
endif()

include(GNUInstallDirs)

install( TARGETS sonivoxrender
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

install(FILES ${CMAKE_CURRENT_BINARY_DIR}/sonivoxrender.1
    DESTINATION ${CMAKE_INSTALL_MANDIR}/man1
)
