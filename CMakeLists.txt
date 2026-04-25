# Copyright (C) 2026 The Embedded Society <https://github.com/embedded-society/armv7m-intrinsics>

# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at

#     http://www.apache.org/licenses/LICENSE-2.0

# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

cmake_minimum_required(VERSION 3.13)

project(armv7m-intrinsics
    LANGUAGES CXX
    VERSION 0.1.0
    DESCRIPTION "C++ wrappers for ARMv7-M instructions not normally emitted by the compiler (Cortex-M3)"
    HOMEPAGE_URL "https://github.com/embedded-society/armv7m-intrinsics"
)

add_library(${PROJECT_NAME} INTERFACE)

target_compile_features(${PROJECT_NAME} INTERFACE cxx_std_20)

target_include_directories(${PROJECT_NAME} INTERFACE
    "${CMAKE_CURRENT_SOURCE_DIR}/include"
)

target_sources(${PROJECT_NAME} INTERFACE
    "${CMAKE_CURRENT_SOURCE_DIR}/include/armv7m-intrinsics/barriers.hpp"
    "${CMAKE_CURRENT_SOURCE_DIR}/include/armv7m-intrinsics/bits.hpp"
    "${CMAKE_CURRENT_SOURCE_DIR}/include/armv7m-intrinsics/exclusive.hpp"
    "${CMAKE_CURRENT_SOURCE_DIR}/include/armv7m-intrinsics/hints.hpp"
    "${CMAKE_CURRENT_SOURCE_DIR}/include/armv7m-intrinsics/interrupts.hpp"
    "${CMAKE_CURRENT_SOURCE_DIR}/include/armv7m-intrinsics/reverse.hpp"
    "${CMAKE_CURRENT_SOURCE_DIR}/include/armv7m-intrinsics/saturation.hpp"
    "${CMAKE_CURRENT_SOURCE_DIR}/include/armv7m-intrinsics/svc.hpp"
)
