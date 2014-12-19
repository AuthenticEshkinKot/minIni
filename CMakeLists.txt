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

include_directories(${CMAKE_SOURCE_DIR})

set(SOURCES ${CMAKE_SOURCE_DIR}/minIni.c ${CMAKE_SOURCE_DIR}/minGlue.h ${CMAKE_SOURCE_DIR}/minIni.h ${CMAKE_SOURCE_DIR}/wxMinIni.h)

add_library(minini STATIC ${SOURCES})

install(TARGETS minini
        ARCHIVE DESTINATION lib)

if(SHARED)
add_library(minini_shared SHARED ${SOURCES})
set_target_properties(minini_shared PROPERTIES OUTPUT_NAME minini)
install(TARGETS minini_shared
        LIBRARY DESTINATION lib)
endif(SHARED)

install(FILES ${CMAKE_SOURCE_DIR}/minIni.h ${CMAKE_SOURCE_DIR}/minGlue.h DESTINATION include/minIni)
