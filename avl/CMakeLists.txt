cmake_minimum_required(VERSION 3.1)
project(icecast-avl C)

add_library(icecast-avl avl.c)

# TODO: Add support for thread flags (NO_THREAD, HAVE_AVL_NODE_LOCK)
# PUBLIC because NO_THREAD is also used in the header file
target_compile_definitions(icecast-avl PUBLIC NO_THREAD)

include(CTest)
if(BUILD_TESTING)
    enable_testing()
    add_executable(icecast-avl-tests test.c)
    target_link_libraries(icecast-avl-tests PRIVATE icecast-avl)
    add_test(NAME avl-tests COMMAND $<TARGET_FILE:icecast-avl-tests>)
endif()
