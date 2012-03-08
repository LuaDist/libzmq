# Copyright (C) 2012 LuaDist.
# Created by Peter Drahoš
# Redistribution and use of this file is allowed according to the terms of the MIT license.
# For details see the COPYRIGHT file distributed with LuaDist.
# Please note that the package source code is licensed under its own license.

project ( libzmq CXX )
cmake_minimum_required ( VERSION 2.6 )
include ( dist.cmake )

set ( INCLUDE_DIRS src include )
file ( GLOB ZMQ_SRC src/*.cpp )

if (WIN32)
	set ( INCLUDE_DIRS ${INCLUDE_DIRS} builds/msvc )
	set ( LIBS Ws2_32 )
endif ()

include_directories ( ${INCLUDE_DIRS} )
add_library ( zmq SHARED ${ZMQ_SRC} )
target_link_libraries( zmq ${LIBS} )

install_library ( zmq )
install_header ( include/zmq.h include/zmq_utils.h )
install_data ( AUTHORS COPYING README NEWS )