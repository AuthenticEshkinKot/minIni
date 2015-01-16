cmake_minimum_required (VERSION 2.6)

project (minIni)

#FS_TYPE options is responsible for type of target filesystem
option(SHARED "If ON - builds shared library" OFF)

set(GLUE_H "${CMAKE_CURRENT_SOURCE_DIR}/minGlue-stdio.h")

if("${FS_TYPE}" STREQUAL "CCS")
    set(GLUE_H "${CMAKE_CURRENT_SOURCE_DIR}/minGlue-ccs.h")
elseif("${FS_TYPE}" STREQUAL "EFSL")
    set(GLUE_H "${CMAKE_CURRENT_SOURCE_DIR}/minGlue-efsl.h")
elseif("${FS_TYPE}" STREQUAL "FFS")
    set(GLUE_H "${CMAKE_CURRENT_SOURCE_DIR}/minGlue-ffs.h")
elseif("${FS_TYPE}" STREQUAL "MDD")
    set(GLUE_H "${CMAKE_CURRENT_SOURCE_DIR}/minGlue-mdd.h")
endif("${FS_TYPE}" STREQUAL "CCS")

file(GENERATE OUTPUT "${CMAKE_CURRENT_SOURCE_DIR}/minGlue.h" INPUT "${GLUE_H}")

set(SOURCES 
    minIni.c
    minGlue.h
    minIni.h
    wxMinIni.h)

add_library(minIni STATIC ${SOURCES})

install(TARGETS minIni
        ARCHIVE DESTINATION lib)

if(SHARED)
add_library(minini_shared SHARED ${SOURCES})
set_target_properties(minini_shared PROPERTIES OUTPUT_NAME minIni)
install(TARGETS minini_shared ARCHIVE DESTINATION lib LIBRARY DESTINATION lib)
endif(SHARED)

install(FILES 
    minIni.h
    minGlue.h
    DESTINATION include/minIni)
