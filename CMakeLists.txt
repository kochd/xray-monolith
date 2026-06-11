cmake_minimum_required(VERSION 3.16)
project(xray-monolith LANGUAGES CXX C)

# Set C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_C_STANDARD 11)

# Build options
set(CMAKE_BUILD_TYPE Release CACHE STRING "Build type")
option(BUILD_SHARED_LIBS "Build shared libraries" ON)
option(XRAY_USE_DX11 "Use Direct3D 11 renderer (Windows only)" OFF)

# Platform detection
if(WIN32)
    message(STATUS "Building for Windows")
    set(XRAY_PLATFORM_WINDOWS TRUE)
elseif(UNIX AND NOT APPLE)
    message(STATUS "Building for Linux")
    set(XRAY_PLATFORM_LINUX TRUE)
    add_compile_options(-fPIC)
elseif(APPLE)
    message(STATUS "Building for macOS")
    set(XRAY_PLATFORM_MACOS TRUE)
endif()

# Set output directories
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib")
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib")

# Add third-party subdirectories
add_subdirectory(sdk)
add_subdirectory(src/3rd\ party)

# Add main source subdirectories
add_subdirectory(src/xrCore)
add_subdirectory(src/xrCDB)
add_subdirectory(src/xrCPU_Pipe)
add_subdirectory(src/xrNetServer)
add_subdirectory(src/xrParticles)
add_subdirectory(src/xrSound)
add_subdirectory(src/xrXMLParser)
add_subdirectory(src/xrPhysics)

# Rendering layers
add_subdirectory(src/Layers)

# Main engine and game
add_subdirectory(src/xrEngine)
add_subdirectory(src/xrGame)
add_subdirectory(src/ReShadeCompat)

# Print build summary
message(STATUS "")
message(STATUS "=== X-Ray Monolith Build Configuration ===")
message(STATUS "Build type: ${CMAKE_BUILD_TYPE}")
message(STATUS "Platform: ${CMAKE_SYSTEM_NAME}")
message(STATUS "C++ Standard: ${CMAKE_CXX_STANDARD}")
message(STATUS "Output directory: ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}")
message(STATUS "==========================================")
