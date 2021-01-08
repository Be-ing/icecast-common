cmake_minimum_required(VERSION 3.1)
project(icecast-net C)

add_library(icecast-net STATIC resolver.c sock.c)

# TODO: Add support for thread flags
target_compile_definitions(icecast-net PUBLIC NO_THREAD)

include(CheckIncludeFile)
include(CheckSymbolExists)

check_include_file(sys/uio.h HAVE_SYS_SELECT_H)
if(HAVE_SYS_SELECT_H)
    target_compile_definitions(icecast-net PRIVATE HAVE_SYS_SELECT_H)
endif()

check_include_file(sys/uio.h HAVE_SYS_UIO_H)
if(HAVE_SYS_UIO_H)
    # PUBLIC because it is also used in the header file
    target_compile_definitions(icecast-net PUBLIC HAVE_SYS_UIO_H)
endif()

check_include_file(unistd.h HAVE_UNISTD_H)
if(HAVE_UNISTD_H)
    # PUBLIC because it is also used in the header file
    target_compile_definitions(icecast-net PUBLIC HAVE_UNISTD_H)
endif()

check_include_file(winsock2.h HAVE_WINSOCK2_H)
if(HAVE_WINSOCK2_H)
    # PUBLIC because it is also used in the header file
    target_compile_definitions(icecast-net PUBLIC HAVE_WINSOCK2_H)
endif()

check_symbol_exists(endhostent "netdb.h;ws2tcpip.h" HAVE_ENDHOSTENT)
if(HAVE_ENDHOSTENT)
    target_compile_definitions(icecast-net PRIVATE HAVE_ENDHOSTENT)
endif()

check_symbol_exists(getaddrinfo "netdb.h;ws2tcpip.h" HAVE_GETADDRINFO)
if(HAVE_GETADDRINFO)
    target_compile_definitions(icecast-net PRIVATE HAVE_GETADDRINFO)
endif()

check_symbol_exists(getnameinfo "netdb.h;ws2tcpip.h" HAVE_GETNAMEINFO)
if(HAVE_GETNAMEINFO)
    target_compile_definitions(icecast-net PRIVATE HAVE_GETNAMEINFO)
endif()

check_symbol_exists(inet_aton "arpa/inet.h;ws2tcpip.h" HAVE_INET_ATON)
if(HAVE_INET_ATON)
    # PUBLIC because it is also used in the header file
    target_compile_definitions(icecast-net PUBLIC HAVE_INET_ATON)
endif()

check_symbol_exists(inet_pton "arpa/inet.h;ws2tcpip.h" HAVE_INET_PTON)
if(HAVE_INET_PTON)
    # PUBLIC because it is also used in the header file
    target_compile_definitions(icecast-net PUBLIC HAVE_INET_PTON)
endif()

check_symbol_exists(poll "poll.h" HAVE_POLL)
if(HAVE_POLL)
    target_compile_definitions(icecast-net PRIVATE HAVE_POLL)
endif()

check_symbol_exists(sethostent "netdb.h;ws2tcpip.h" HAVE_SETHOSTENT)
if(HAVE_SETHOSTENT)
    target_compile_definitions(icecast-net PRIVATE HAVE_SETHOSTENT)
endif()

check_symbol_exists(writev "sys/uio.h" HAVE_WRITEV)
if(HAVE_WRITEV)
    target_compile_definitions(icecast-net PRIVATE HAVE_WRITEV)
endif()

if(WIN32)
    # PUBLIC because it is also used in the header file
    target_compile_definitions(icecast-net PUBLIC _WIN32)
endif()

include(CTest)
if(BUILD_TESTING)
    enable_testing()
    add_executable(icecast-net-tests test_resolver.c)
    target_link_libraries(icecast-net-tests PRIVATE icecast-net)
    add_test(NAME test_resolver COMMAND $<TARGET_FILE:icecast-net-tests>)
endif()
