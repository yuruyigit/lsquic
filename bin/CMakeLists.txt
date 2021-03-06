# Copyright (c) 2017 - 2021 LiteSpeed Technologies Inc.  See LICENSE.
INCLUDE(CheckFunctionExists)
CHECK_FUNCTION_EXISTS(sendmmsg HAVE_SENDMMSG)
CHECK_FUNCTION_EXISTS(recvmmsg HAVE_RECVMMSG)
CHECK_FUNCTION_EXISTS(open_memstream HAVE_OPEN_MEMSTREAM)


INCLUDE(CheckSymbolExists)

CHECK_SYMBOL_EXISTS(
    IP_MTU_DISCOVER
    "netinet/in.h"
    HAVE_IP_MTU_DISCOVER
)

CHECK_SYMBOL_EXISTS(
    IP_DONTFRAG
    "netinet/in.h"
    HAVE_IP_DONTFRAG
)

CHECK_SYMBOL_EXISTS(
    preadv
    "sys/uio.h"
    HAVE_PREADV
)

INCLUDE(CheckIncludeFiles)

IF (MSVC AND PCRE_LIB)
FIND_PATH(EVENT_INCLUDE_DIR NAMES pcreposix.h)
IF (EVENT_INCLUDE_DIR)
    MESSAGE(STATUS "found pcreposix.h")
    SET(HAVE_REGEX 1)
ELSE()
    MESSAGE(FATAL_ERROR "event2/event.h was not found")
ENDIF()
ELSE()
CHECK_INCLUDE_FILES(regex.h HAVE_REGEX)
ENDIF()

CONFIGURE_FILE(${CMAKE_CURRENT_SOURCE_DIR}/test_config.h.in ${CMAKE_CURRENT_SOURCE_DIR}/test_config.h)
