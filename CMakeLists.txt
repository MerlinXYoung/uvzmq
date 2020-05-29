cmake_minimum_required(VERSION 3.15)
project(UVZMQ)
message(STATUS "CMAKE_PREFIX_PATH:${CMAKE_PREFIX_PATH}")

find_package(ZeroMQ REQUIRED)
include_directories(${ZeroMQ_INCLUDE_DIR})

find_package(PkgConfig REQUIRED)

pkg_check_modules(LIBUV REQUIRED 
IMPORTED_TARGET 
GLOBAL libuv)
include_directories(${LIBUV_INCLUDE_DIRS})





set(SRCS uv_zsock.c)

add_library(uvzmq ${SRCS})
# target_link_libraries(uvzmq)

add_executable(zsock_test uv_zsock_test.c)
target_link_libraries(zsock_test uvzmq libzmq-static PkgConfig::LIBUV)

# add_executable(zloop_test zloop_compat_test.c)
# target_link_libraries(zloop_test zmq)

install(TARGETS uvzmq zsock_test
        RUNTIME DESTINATION bin
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib
)

install(FILES uv_zsock.h 
DESTINATION include
)
