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

if(UNIX)
set(includedir ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_INCLUDEDIR})
set(libdir ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR})
set(prefix ${CMAKE_INSTALL_PREFIX})
configure_file(uvzmq.pc.in uvzmq.pc @ONLY)

include(GNUInstallDirs)
install(TARGETS uvzmq zsock_test
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
)
# message(STATUS "lib:${CMAKE_INSTALL_FULL_LIBDIR}:${CMAKE_INSTALL_BINDIR}")
install(FILES ${PROJECT_BINARY_DIR}/uvzmq.pc
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/pkgconfig
)

install(FILES uv_zsock.h
TYPE INCLUDE
)
endif()

if(WIN32)
install(FILES uv_zsock.h
TYPE INCLUDE
)


    install(TARGETS uvzmq zsock_test
          RUNTIME DESTINATION bin/$<CONFIG>
          LIBRARY DESTINATION lib/$<CONFIG>
          ARCHIVE DESTINATION lib/$<CONFIG>
  )
endif()
