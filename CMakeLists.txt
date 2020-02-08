cmake_minimum_required(VERSION 3.15)
project(UVZMQ C)

find_package(PkgConfig REQUIRED)

pkg_check_modules(ZMQ REQUIRED libzmq)
include_directories(${ZMQ_INCLUDE_DIRS})
link_directories(${ZMQ_LIBRARY_DIRS})

pkg_check_modules(LIBUV REQUIRED libuv)
include_directories(${LIBUV_INCLUDE_DIRS})
link_directories(${LIBUV_LIBRARY_DIRS})

set(SRCS uv_zsock.c)

add_library(uvzmq ${SRCS})
# target_link_libraries(uvzmq)

add_executable(zsock_test uv_zsock_test.c)
target_link_libraries(zsock_test uvzmq uv zmq)

# add_executable(zloop_test zloop_compat_test.c)
# target_link_libraries(zloop_test zmq)

