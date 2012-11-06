# CMake file for sqlite

cmake_minimum_required(VERSION 2.8)
project(sqlite)
find_package(qibuild)

qi_create_lib(sqlite3
  sqlite3.c
  sqlite3.h
)

add_definitions("-DSQLITE_ENABLE_RTREE=1")

qi_use_lib(sqlite3 PTHREAD)
if(UNIX AND NOT APPLE)
  qi_use_lib(sqlite3 DL)
endif()

qi_stage_lib(sqlite3)
