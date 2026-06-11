cmake_minimum_required(VERSION 3.16)
project(xray-monolith LANGUAGES CXX C)

# Cross-compile for Windows (PE/COFF executable format)
# This uses MinGW-w64 or equivalent to produce .exe files runnable via Proton on Linux

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_C_STANDARD 11)

# Build configuration
set(CMAKE_BUILD_TYPE Release CACHE STRING "Build type")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib")
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib")

# Windows cross-compilation settings (for Proton)
set(CMAKE_SYSTEM_NAME Windows)
set(CMAKE_SYSTEM_PROCESSOR x86_64)

# MinGW-w64 toolchain (adjust path as needed)
if(NOT CMAKE_C_COMPILER)
    set(CMAKE_C_COMPILER x86_64-w64-mingw32-gcc)
endif()
if(NOT CMAKE_CXX_COMPILER)
    set(CMAKE_CXX_COMPILER x86_64-w64-mingw32-g++)
endif()
if(NOT CMAKE_RC_COMPILER)
    set(CMAKE_RC_COMPILER x86_64-w64-mingw32-windres)
endif()

message(STATUS "=== X-Ray Monolith - Cross-Compile for Proton ===")
message(STATUS "C Compiler: ${CMAKE_C_COMPILER}")
message(STATUS "CXX Compiler: ${CMAKE_CXX_COMPILER}")
message(STATUS "Build type: ${CMAKE_BUILD_TYPE}")
message(STATUS "Output: ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}")

# Include directories for SDK
include_directories(
    ${CMAKE_SOURCE_DIR}/sdk/include
    ${CMAKE_SOURCE_DIR}/src
)

# Link directories
link_directories(
    ${CMAKE_SOURCE_DIR}/sdk/lib/x64
)

# Subdirectories for engine build
add_subdirectory(src/xrCore)
add_subdirectory(src/xrCDB)
add_subdirectory(src/xrPhysics)
add_subdirectory(src/xrSound)
add_subdirectory(src/xrXMLParser)
add_subdirectory(src/xrParticles)
add_subdirectory(src/xrNetServer)
add_subdirectory(src/xrCPU_Pipe)
add_subdirectory(src/3rd\ party)
add_subdirectory(src/Layers)
add_subdirectory(src/xrEngine)
add_subdirectory(src/xrGame)

message(STATUS "==================================================")
