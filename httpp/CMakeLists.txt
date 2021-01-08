cmake_minimum_required(VERSION 3.1)
project(icecast-httpp C)

add_library(icecast-httpp STATIC encoding.c httpp.c)
target_include_directories(icecast-httpp PRIVATE ..)
add_subdirectory(../avl "${CMAKE_CURRENT_BINARY_DIR}/avl")
target_link_libraries(icecast-httpp PRIVATE icecast-avl)

include(CheckIncludeFile)
check_include_file(strings.h HAVE_STRINGS_H)
if(HAVE_STRINGS_H)
    target_compile_definitions(icecast-httpp PRIVATE HAVE_STRINGS_H)
endif()

# TODO: Add support for tests
# Currently, it fails to build with:
#    test.c: In function ‘main’:
#    test.c:38:44: error: ‘http_parser_t’ {aka ‘struct http_parser_tag’} has no member named ‘version’
#       38 |         printf("Version was 1.%d\n", parser.version);
#          |                                            ^
