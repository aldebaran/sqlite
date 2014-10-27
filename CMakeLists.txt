cmake_minimum_required(VERSION 2.8)
project(sqlite)
find_package(qibuild)

option("WITH_SYSTEM_SQLITE"
  "Set this if you have sqlite installed with rtree support"
  OFF)

if(WITH_SYSTEM_SQLITE)
  return()
endif()

include_directories(".")
add_definitions("-DSQLITE_ENABLE_RTREE=1")

set(_srcs
  sqlite3.c
  sqlite3.h
)

if(APPLE)
  # STATIC is required because some core libraries depend on /usr/lib/libsqlite3
  # on OSX:
  #
  # dyld: Library not loaded: /usr/lib/libsqlite3.dylib
  # Referenced from:
  # /System/Library/Frameworks/CFNetwork.framework/Versions/A/CFNetwork
  #   Reason: Incompatible library version:
  #           CFNetwork requires version 9.0.0 or later,
  #           but libsqlite3.dylib provides version 0.0.0
  qi_create_lib(sqlite3 STATIC ${_srcs})
else()
  qi_create_lib(sqlite3 ${_srcs})
endif()

qi_use_lib(sqlite3 PTHREAD)
if(UNIX AND NOT APPLE)
  qi_use_lib(sqlite3 DL)
endif()

qi_stage_lib(sqlite3)
qi_install_header(sqlite3.h)
