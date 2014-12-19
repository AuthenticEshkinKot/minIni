cmake_minimum_required (VERSION 2.6)

project (minIni)

if("${FS_TYPE}" STREQUAL "DEFAULT")
    file(GENERATE OUTPUT minGlue.h INPUT minGlue-stdio.h)
elseif("${FS_TYPE}" STREQUAL "CCS")
    file(GENERATE OUTPUT minGlue.h INPUT minGlue-ccs.h)
elseif("${FS_TYPE}" STREQUAL "EFSL")
    file(GENERATE OUTPUT minGlue.h INPUT minGlue-efsl.h)
elseif("${FS_TYPE}" STREQUAL "FFS")
    file(GENERATE OUTPUT minGlue.h INPUT minGlue-ffs.h)
elseif("${FS_TYPE}" STREQUAL "MDD")
    file(GENERATE OUTPUT minGlue.h INPUT minGlue-mdd.h)
else()
    file(GENERATE OUTPUT minGlue.h INPUT minGlue-stdio.h)    
endif("${FS_TYPE}" STREQUAL "DEFAULT")

add_library(minini_static STATIC minIni.c)
add_library(minini SHARED minIni.c)

set_target_properties(minini_static PROPERTIES OUTPUT_NAME minini)

include(CheckCXXCompilerFlag)

check_cxx_compiler_flag(-O2 IS_OPTIMIZATION_SUPPORTED)

if(IS_OPTIMIZATION_SUPPORTED)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O2")
endif()
