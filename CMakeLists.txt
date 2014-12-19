cmake_minimum_required (VERSION 2.6)

project (minIni)

#FS_TYPE options is responsible for type of target filesystem
option(SHARED "If ON - builds shared library" OFF)

if("${FS_TYPE}" STREQUAL "DEFAULT")
    file(GENERATE OUTPUT "${CMAKE_SOURCE_DIR}/minGlue.h" INPUT "${CMAKE_SOURCE_DIR}/minGlue-stdio.h")
elseif("${FS_TYPE}" STREQUAL "CCS")
    file(GENERATE OUTPUT "${CMAKE_SOURCE_DIR}/minGlue.h" INPUT "${CMAKE_SOURCE_DIR}/minGlue-ccs.h")
elseif("${FS_TYPE}" STREQUAL "EFSL")
    file(GENERATE OUTPUT "${CMAKE_SOURCE_DIR}/minGlue.h" INPUT "${CMAKE_SOURCE_DIR}/minGlue-efsl.h")
elseif("${FS_TYPE}" STREQUAL "FFS")
    file(GENERATE OUTPUT "${CMAKE_SOURCE_DIR}/minGlue.h" INPUT "${CMAKE_SOURCE_DIR}/minGlue-ffs.h")
elseif("${FS_TYPE}" STREQUAL "MDD")
    file(GENERATE OUTPUT "${CMAKE_SOURCE_DIR}/minGlue.h" INPUT "${CMAKE_SOURCE_DIR}/minGlue-mdd.h")
else()
    file(GENERATE OUTPUT "${CMAKE_SOURCE_DIR}/minGlue.h" INPUT "${CMAKE_SOURCE_DIR}/minGlue-stdio.h")
endif("${FS_TYPE}" STREQUAL "DEFAULT")

add_library(minini STATIC "${CMAKE_SOURCE_DIR}/minIni.c")

if(SHARED)
add_library(minini_shared SHARED "${CMAKE_SOURCE_DIR}/minIni.c")
set_target_properties(minini_shared PROPERTIES OUTPUT_NAME minini)
endif(SHARED)

include(CheckCXXCompilerFlag)

check_cxx_compiler_flag(-O2 IS_OPTIMIZATION_SUPPORTED)

if(IS_OPTIMIZATION_SUPPORTED)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O2")
endif()
