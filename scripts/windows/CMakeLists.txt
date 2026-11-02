cmake_minimum_required(VERSION 3.21)
cmake_policy(SET CMP0091 NEW)
project(sqlite3_cli LANGUAGES C)

if(NOT MSVC)
  message(FATAL_ERROR "The Windows build requires MSVC")
endif()

get_filename_component(REPOSITORY_ROOT "${CMAKE_CURRENT_SOURCE_DIR}/../.." ABSOLUTE)

add_executable(
  sqlite3_cli
  "${REPOSITORY_ROOT}/shell.c"
  "${REPOSITORY_ROOT}/sqlite3.c"
)

target_compile_definitions(
  sqlite3_cli
  PRIVATE
    SQLITE_ENABLE_FTS5
    SQLITE_ENABLE_JSON1
    SQLITE_ENABLE_RTREE
    WINVER=0x0601
    _WIN32_WINNT=0x0601
    NTDDI_VERSION=0x06010000
)
target_compile_options(sqlite3_cli PRIVATE /O1)
if(CMAKE_GENERATOR_PLATFORM STREQUAL "ARM64")
  target_link_options(sqlite3_cli PRIVATE /SUBSYSTEM:CONSOLE)
else()
  target_link_options(sqlite3_cli PRIVATE /SUBSYSTEM:CONSOLE,6.01)
endif()
set_property(TARGET sqlite3_cli PROPERTY OUTPUT_NAME sqlite3)
set_property(TARGET sqlite3_cli PROPERTY MSVC_RUNTIME_LIBRARY MultiThreaded)
