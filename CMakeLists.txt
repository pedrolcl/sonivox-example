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
project( sonivox-render
    LANGUAGES C
    VERSION 1.0
)

option(FIND_SONIVOX "Find an installed Sonivox library" OFF)
option(USE_STATICLIB "Use the static library" ON)

message(STATUS "Finding Sonivox: ${FIND_SONIVOX}")
message(STATUS "Using Static Library: ${USE_STATICLIB}")

if (FIND_SONIVOX)
    find_package ( sonivox CONFIG REQUIRED )
    message(STATUS "Found Sonivox Version: ${sonivox_VERSION}")
else()
    if (USE_STATICLIB)
        set(BUILD_SONIVOX_SHARED OFF)
        set(BUILD_SONIVOX_STATIC ON)
    else()
        set(BUILD_SONIVOX_SHARED ON)
        set(BUILD_SONIVOX_STATIC OFF)
    endif()
    set(BUILD_TESTING OFF)
    add_subdirectory( sonivox )
    include(CMakePrintHelpers)
    if (USE_STATICLIB)
        cmake_print_properties(TARGETS sonivox::sonivox-static PROPERTIES VERSION)
    else()
        cmake_print_properties(TARGETS sonivox::sonivox PROPERTIES VERSION SOVERSION)
    endif()
endif()

add_executable ( sonivox-render sonivoxrender.c )

if (USE_STATICLIB)
    target_link_libraries ( sonivox-render sonivox::sonivox-static )
else()
    target_link_libraries ( sonivox-render sonivox::sonivox )
endif()
