cmake_minimum_required(VERSION 3.1)
project(icecast-thread C)

add_library(icecast-thread STATIC thread.c)
target_include_directories(icecast-thread PUBLIC ..)

option(DEBUG_MUTEXES "Turn on mutex debugging. this will log locks/unlocks." OFF)
if(DEBUG_MUTEXES)
    target_compile_definitions(icecast-thread PRIVATE DEBUG_MUTEXES)
endif()

option(CHECK_MUTEXES "Turn on checks to make sure mutex operations make sense, i.e. multi_mutex is locked when locking multiple mutexes, etc." OFF)
if(CHECK_MUTEXES)
    target_compile_definitions(icecast-thread PRIVATE DEBUG_MUTEXES CHECK_MUTEXES)
endif()

set(THREAD_DEBUG "0" CACHE STRING "Turns on the thread.log logging (define to 1-4)")
if(THREAD_DEBUG GREATER "0")
    if(THREAD_DEBUG GREATER "4")
        set(THREAD_DEBUG 4)
    endif()
    target_compile_definitions(icecast-thread PRIVATE THREAD_DEBUG=${THREAD_DEBUG})
endif()

include(CheckIncludeFile)
include(CheckSymbolExists)

check_include_file(sys/time.h HAVE_SYS_TIME_H)
if(HAVE_SYS_TIME_H)
    check_symbol_exists(nanosleep "sys/time.h" HAVE_NANOSLEEP)
    if(HAVE_NANOSLEEP)
        add_compile_definitions(HAVE_NANOSLEEP)
    endif()
endif()

check_symbol_exists(pthread_spin_init "sys/time.h" HAVE_PTHREAD_SPIN_LOCK)
if(HAVE_PTHREAD_SPIN_LOCK)
    add_compile_definitions(HAVE_PTHREAD_SPIN_LOCK)
endif()

if(WIN32)
    target_compile_definitions(icecast-thread PRIVATE _WIN32)
endif()

find_path(pthread_INCLUDE_DIR NAMES pthread.h REQUIRED)
target_include_directories(icecast-thread PRIVATE "${pthread_INCLUDE_DIR}")
