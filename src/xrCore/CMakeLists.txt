cmake_minimum_required(VERSION 3.16)
project(xrCore CXX C)

# xrCore - Core engine library
# This is a static library containing fundamental engine utilities

# Source files (from vcxproj)
set(XRCORE_SOURCES
    # Blackbox/crash handling
    blackbox/BlackBoxUI.cpp
    blackbox/BSUFunctions.cpp
    blackbox/CrashHandler.cpp
    blackbox/GetLoadedModules.cpp
    blackbox/IsNT.cpp
    blackbox/NT4ProcessInfo.cpp
    blackbox/stdafx_.cpp
    blackbox/TLHELPProcessInfo.cpp

    # Core engine utilities
    clsid.cpp
    cpuid.cpp
    crc32.cpp
    dump_string.cpp
    FileSystem.cpp
    FileSystem_borland.cpp
    file_stream_reader.cpp
    FS.cpp
    FTimer.cpp
    LocatorAPI.cpp
    LocatorAPI_auth.cpp
    LocatorAPI_defs.cpp
    Lock.cpp
    log.cpp
    LzHuf.cpp
    lzo_compressor.cpp
    memory_allocation_stats.cpp
    memory_monitor.cpp
    memory_usage.cpp
    mezz_stringbuffer.cpp
    Model.cpp
    NET_utils.cpp
    os_clipboard.cpp
    ppmd_compressor.cpp
    rt_compressor.cpp
    rt_compressor9.cpp
    rt_lzo1x_1.cpp
    rt_lzo1x_9x.cpp
    rt_lzo1x_d1.cpp
    rt_lzo1x_d2.cpp
    rt_lzo1x_d3.cpp
    rt_lzo_init.cpp
    ScopeLock.cpp
    stdafx.cpp
    stream_reader.cpp
    string_concatenations.cpp
    xrCore.cpp
    xrDebugNew.cpp
    xrMemory.cpp
    xrMemory_align.cpp
    xrMemory_debug.cpp
    xrMemory_POOL.cpp
    xrMemory_pso_Copy.cpp
    xrMemory_pso_Fill.cpp
    xrMemory_pso_Fill32.cpp
    xrMemory_subst_borland.cpp
    xrMemory_subst_msvc.cpp
    xrsharedmem.cpp
    xrstring.cpp
    xrSyncronize.cpp
    Xr_ini.cpp
    xr_shared.cpp
    xr_trims.cpp
    _compressed_normal.cpp
    _math.cpp
    _sphere.cpp
    _std_extensions.cpp
)

# Header files
set(XRCORE_HEADERS
    blackbox/BugslayerUtil.h
    blackbox/CrashHandler.h
    blackbox/DiagAssert.h
    blackbox/Internal.h
    blackbox/stdafx_.h
    blackbox/SymbolEngine.h
    blackbox/WarningsOff.h
    blackbox/WarningsOn.h
    buffer_vector.h
    buffer_vector_inline.h
    ChooseTypes.H
    client_id.h
    clsid.h
    Coder.hpp
    compression_ppmd_stream.h
    compression_ppmd_stream_inline.h
    cpuid.h
    dump_string.h
    file_stream_reader.h
    FixedMap.h
    FixedSet.h
    FixedVector.h
    FS.h
    FS_impl.h
    FS_internal.h
    FTimer.h
    intrusive_ptr.h
    LocatorAPI.h
    LocatorAPI_defs.h
    Lock.hpp
    log.h
    lzhuf.h
    lzo_compressor.h
    memory_monitor.h
    mezz_stringbuffer.h
    net_utils.h
    os_clipboard.h
    PPMd.h
    PPMdType.h
    ppmd_compressor.h
    profiler.h
    rt_compressor.h
    rt_config1x.h
    rt_lzo1x.h
    rt_lzoconf.h
    rt_lzodefs.h
    rt_lzo_conf.h
    rt_lzo_config.h
    rt_lzo_dict.h
    rt_lzo_ptr.h
    rt_miniacc.h
    ScopeLock.hpp
    stdafx.h
    stream_reader.h
    stream_reader_inline.h
    string_concatenations.h
    string_concatenations_inline.h
    SubAlloc.hpp
    vector.h
    xrCore.h
    xrCore_platform.h
    xrDebug.h
    xrDebug_macros.h
    xrMemory.h
    xrMemory_align.h
    xrMEMORY_POOL.h
    xrMemory_pso.h
    xrMemory_pure.h
    xrMemory_subst_borland.h
    xrMemory_subst_msvc.h
    xrPool.h
    xrsharedmem.h
    xrstring.h
    xrSyncronize.h
    xr_ini.h
    xr_resource.h
    xr_shared.h
    xr_trims.h
    _bitwise.h
    _color.h
    _compressed_normal.h
    _cylinder.h
    _fbox.h
    _fbox2.h
    _flags.h
    _noncopyable.h
    _math.h
    _matrix.h
    _matrix33.h
    _obb.h
    _plane.h
    _plane2.h
    _quaternion.h
    _random.h
    _rect.h
    _sphere.h
    _std_extensions.h
    _stl_extensions.h
    _thread_types.h
    _types.h
    _type_traits.h
    _vector2.h
    _vector3d.h
    _vector3d_ext.h
    _vector4.h
)

# Create static library
add_library(xrCore STATIC ${XRCORE_SOURCES})

# Include directories
target_include_directories(xrCore PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_SOURCE_DIR}/sdk/include
    ${CMAKE_SOURCE_DIR}/src
)

# Add include dir for 3rd party dependencies
target_include_directories(xrCore PRIVATE
    ${CMAKE_SOURCE_DIR}/src/3rd\ party/optick-git/src
    ${CMAKE_SOURCE_DIR}/src/3rd\ party/robin_hood
    ${CMAKE_SOURCE_DIR}/src/3rd\ party/fastdelegate
    ${CMAKE_SOURCE_DIR}/src/3rd\ party/stackwalker/include
)

# Preprocessor definitions (from vcxproj Release config)
target_compile_definitions(xrCore PRIVATE
    PROFILER_OPTICK
    PURE_ALLOC
    WIN32
    NDEBUG
    _WINDOWS
    _USRDLL
    XRCORE_EXPORTS
    _SECURE_SCL=0
)

# Compiler options
target_compile_options(xrCore PRIVATE
    $<$<CXX_COMPILER_ID:GNU,Clang>:
        -fPIC
        -O3
        -finline-functions
        -fomit-frame-pointer
        -fno-exceptions
        -Wall -Wextra -Wno-unused-parameter
    >
)

# C++ Standard
set_property(TARGET xrCore PROPERTY CXX_STANDARD 17)
set_property(TARGET xrCore PROPERTY CXX_STANDARD_REQUIRED ON)

# Link libraries
target_link_libraries(xrCore PUBLIC
    # 3rd party libs (if compiled separately)
    # optick
    # stackwalker
)

# Windows specific linking
if(WIN32 OR CMAKE_SYSTEM_NAME MATCHES "Windows")
    target_link_libraries(xrCore PRIVATE
        ws2_32
        winmm
        advapi32
        shell32
    )
endif()

# Set output properties
set_target_properties(xrCore PROPERTIES
    ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
    LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
)

message(STATUS "xrCore: Configured as static library")
