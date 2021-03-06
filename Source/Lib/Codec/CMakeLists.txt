# 
# Copyright(c) 2019 Intel Corporation
# SPDX - License - Identifier: BSD - 2 - Clause - Patent
# 
# EncoderLib Directory CMakeLists.txt

# Include Encoder Subdirectories
include_directories (${PROJECT_SOURCE_DIR}/Source/API/)
include_directories (${PROJECT_SOURCE_DIR}/Source/Lib/Codec/)
include_directories (${PROJECT_SOURCE_DIR}/Source/Lib/C_DEFAULT/)
include_directories (${PROJECT_SOURCE_DIR}/Source/Lib/ASM_SSE2/)
include_directories (${PROJECT_SOURCE_DIR}/Source/Lib/ASM_SSSE3/)
include_directories (${PROJECT_SOURCE_DIR}/Source/Lib/ASM_SSE4_1/)
include_directories (${PROJECT_SOURCE_DIR}/Source/Lib/ASM_AVX2/)

link_directories (${PROJECT_SOURCE_DIR}/Source/Lib/ASM_SSE2/)
link_directories (${PROJECT_SOURCE_DIR}/Source/Lib/C_DEFAULT/)
link_directories (${PROJECT_SOURCE_DIR}/Source/Lib/ASM_SSSE3/)
link_directories (${PROJECT_SOURCE_DIR}/Source/Lib/ASM_SSE4_1/)
link_directories (${PROJECT_SOURCE_DIR}/Source/Lib/ASM_AVX2/)

set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/Bin/${CMAKE_BUILD_TYPE}/)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/Bin/${CMAKE_BUILD_TYPE}/)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/Bin/${CMAKE_BUILD_TYPE}/)

if(${CMAKE_SYSTEM_NAME} MATCHES "Windows")
   SET(CMAKE_C_FLAGS "/MP")    
endif(${CMAKE_SYSTEM_NAME} MATCHES "Windows")

file(GLOB all_files
    "*.h"
    "*.c")
 
# Encoder Lib Source Files
add_library(SvtAv1Enc SHARED
    ${all_files}
)
    
if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
target_link_libraries(SvtAv1Enc
    C_DEFAULT
    ASM_SSE2
    ASM_SSSE3
    ASM_SSE4_1
    ASM_AVX2
    m)
else()
target_link_libraries(SvtAv1Enc
    C_DEFAULT
    ASM_SSE2
    ASM_SSSE3
    ASM_SSE4_1
    ASM_AVX2)
endif()

if(NOT DEFINED CMAKE_INSTALL_LIBDIR)
    set(CMAKE_INSTALL_LIBDIR "lib")
endif()

if(NOT DEFINED CMAKE_INSTALL_INCLUDEDIR)
    set(CMAKE_INSTALL_INCLUDEDIR "include/svt-av1")
endif()
    
configure_file(../pkg-config.pc.in ${CMAKE_BINARY_DIR}/SvtAv1Enc.pc @ONLY)
install(FILES ${CMAKE_BINARY_DIR}/SvtAv1Enc.pc DESTINATION "${CMAKE_INSTALL_LIBDIR}/pkgconfig")
install(TARGETS SvtAv1Enc DESTINATION "${CMAKE_INSTALL_LIBDIR}")
install(DIRECTORY ../../API/ DESTINATION "${CMAKE_INSTALL_PREFIX}/include/svt-av1" FILES_MATCHING PATTERN "*.h")
