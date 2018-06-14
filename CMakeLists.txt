cmake_minimum_required(VERSION 3.0)
project(masstree)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_FLAGS "-w")

set(masstree_src
  checkpoint.cc
  compiler.cc
  file.cc
  json.cc
  kvio.cc
  kvrandom.cc
  kvthread.cc
  log.cc
  memdebug.cc
  misc.cc
  msgpack.cc
  perfstat.cc
  query_masstree.cc
  str.cc
  straccum.cc
  string.cc
  string_slice.cc
  testrunner.cc
  value_array.cc
  value_string.cc
  value_versioned_array.cc
  config.h
)

add_custom_command(
  OUTPUT config.h
  COMMAND cp ${PROJECT_SOURCE_DIR}/configure.ac configure.ac
  COMMAND cp ${PROJECT_SOURCE_DIR}/bootstrap.sh bootstrap.sh
  COMMAND cp ${PROJECT_SOURCE_DIR}/GNUmakefile.in GNUmakefile.in
  COMMAND sh bootstrap.sh
  COMMAND ./configure
)

add_library(masstree STATIC ${masstree_src})
target_compile_options(masstree PUBLIC -include config.h)

add_library(clp STATIC clp.c)

add_executable(mttest mttest.cc config.h)
target_link_libraries(mttest masstree clp)
target_compile_options(mttest PUBLIC -include config.h)
