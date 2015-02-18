cmake_minimum_required (VERSION 3.0.2)

project (minIni)

#FS_TYPE options is responsible for type of target filesystem
option(BUILD_SHARED_LIBS "If ON - builds shared library" OFF)

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

file(WRITE "${CMAKE_CURRENT_SOURCE_DIR}/minGlue.h" "")
file(GENERATE OUTPUT "${CMAKE_CURRENT_SOURCE_DIR}/minGlue.h" INPUT "${GLUE_H}")

set(SOURCES 
    minIni.c
    minGlue.h
    minIni.h
    wxMinIni.h)

add_library(minIni STATIC ${SOURCES})

install(TARGETS minIni
        ARCHIVE DESTINATION lib)

if(BUILD_SHARED_LIBS)
add_library(minini_shared SHARED ${SOURCES})
set_target_properties(minini_shared PROPERTIES OUTPUT_NAME minIni)
if(WIN32)
install(TARGETS minini_shared RUNTIME DESTINATION bin)
else()
install(TARGETS minini_shared LIBRARY DESTINATION bin)
endif(WIN32)
endif(BUILD_SHARED_LIBS)

install(FILES 
    minIni.h
    minGlue.h
    DESTINATION include/minIni)
