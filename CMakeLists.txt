# Copyright (C) 2012 LuaDist.
# Created by Peter Drahoš
# Redistribution and use of this file is allowed according to the terms of the MIT license.
# For details see the COPYRIGHT file distributed with LuaDist.
# Please note that the package source code is licensed under its own license.

project ( libzmq CXX C)
cmake_minimum_required ( VERSION 2.6 )
include ( dist.cmake )
include ( CMakePlatformTests.cmake )

#2DO OpenPGM (ZMQ_HAVE_OPENPGM)

include_directories ( src include )
file ( GLOB ZMQ_SRC src/*.cpp )

add_definitions(-D_REENTRANT -D_THREAD_SAFE)
if (WIN32)
  set(ZMQ_PLATFORM ZMQ_HAVE_WINDOWS)
  if (MINGW)
    set(ZMQ_PLATFORM ZMQ_HAVE_MINGW32)
  endif ()
  set ( LIBS Ws2_32 )
elseif (CYGWIN)
  set ( ZMQ_PLATFORM ZMQ_HAVE_CYGWIN)
  add_definitions(-D_DARWIN_C_SOURCE)
elseif (APPLE)
  set ( ZMQ_PLATFORM ZMQ_HAVE_OSX)
  set ( ZMQ_HAVE_UIO true )
  add_definitions(-D_GNU_SOURCE)
elseif (LINUX)  
  set ( ZMQ_PLATFORM ZMQ_HAVE_LINUX)
  add_definitions(-D_GNU_SOURCE)
endif ()

configure_file ( src/platform.hpp.in ${CMAKE_CURRENT_SOURCE_DIR}/src/platform.hpp )

add_library ( zmq SHARED ${ZMQ_SRC} )
target_link_libraries( zmq ${LIBS} )

install_library ( zmq )
install_header ( include/zmq.h include/zmq_utils.h )
install_data ( AUTHORS COPYING README NEWS )