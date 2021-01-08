cmake_minimum_required(VERSION 3.1)
project(icecast-timing C)

add_library(icecast-timing STATIC timing.c)

include(CheckIncludeFile)
include(CheckSymbolExists)

check_include_file(inttypes.h HAVE_INTTYPES_H)
if(HAVE_INTTYPES_H)
    # PUBLIC because it's also used in the header file
    target_compile_definitions(icecast-timing PUBLIC HAVE_INTTYPES_H)
endif()

check_include_file(stdint.h HAVE_STDINT_H)
if(HAVE_STDINT_H)
    target_compile_definitions(icecast-timing PRIVATE HAVE_STDINT_H)
endif()

check_include_file(sys/select.h HAVE_SYS_SELECT_H)
if(HAVE_SYS_SELECT_H)
    target_compile_definitions(icecast-timing PRIVATE HAVE_SYS_SELECT_H)
endif()

check_include_file(sys/time.h HAVE_SYS_TIME_H)
if(HAVE_SYS_TIME_H)
    target_compile_definitions(icecast-timing PRIVATE HAVE_SYS_TIME_H)

    check_symbol_exists(gettimeofday "sys/time.h" HAVE_GETTIMEOFDAY)
    if(HAVE_GETTIMEOFDAY)
        target_compile_definitions(icecast-timing PRIVATE HAVE_GETTIMEOFDAY)
    endif()
endif()

check_include_file(sys/timeb.h HAVE_SYS_TIMEB_H)
if(HAVE_SYS_TIMEB_H)
    target_compile_definitions(icecast-timing PRIVATE HAVE_SYS_TIMEB_H)

    check_symbol_exists(gettimeofday "sys/timeb.h" HAVE_FTIME)
    if(HAVE_FTIME)
        target_compile_definitions(icecast-timing PRIVATE HAVE_FTIME)
    endif()
endif()

check_include_file(time.h HAVE_TIME_H)
if(HAVE_TIME_H)
    target_compile_definitions(icecast-timing PRIVATE HAVE_TIME_H)
endif()

if(HAVE_SYS_TIME_H AND HAVE_TIME_H)
    target_compile_definitions(icecast-timing PRIVATE TIME_WITH_SYS_TIME)
endif()

if(WIN32)
    target_compile_definitions(icecast-timing PUBLIC WIN32)

    # PUBLIC because it's also used in the header file
    target_compile_definitions(icecast-timing PUBLIC _WIN32)
endif()
