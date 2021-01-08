cmake_minimum_required(VERSION 3.1)
project(icecast-log C)

add_library(icecast-log log.c)

include(CheckIncludeFile)
include(CheckSymbolExists)

check_include_file(sys/stat.h HAVE_SYS_STAT_H)
if(HAVE_SYS_STAT_H)
    target_compile_definitions(icecast-log PRIVATE HAVE_SYS_STAT_H)
endif()

check_include_file(HAVE_SYS_TYPES_H sys/types.h)
if(HAVE_SYS_TYPES_H)
    target_compile_definitions(icecast-log PRIVATE HAVE_SYS_TYPES_H)
endif()

check_include_file(unistd.h HAVE_UNISTD_H)
if(HAVE_UNISTD_H)
    target_compile_definitions(icecast-log PRIVATE HAVE_UNISTD_H)
endif()

if(WIN32)
    # PUBLIC because _WIN32 is also used in the header file
    target_compile_definitions(icecast-log PUBLIC _WIN32)
endif()
