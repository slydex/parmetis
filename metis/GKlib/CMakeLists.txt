cmake_minimum_required(VERSION 2.8)
project(GKlib)

get_filename_component(abs "." ABSOLUTE)
set(GKLIB_PATH ${abs})
unset(abs)
include(GKlibSystem.cmake)

include_directories(BEFORE ".")
add_library(GKlib ${GKlib_sources})
if(UNIX)
  target_link_libraries(GKlib m)
endif()

include_directories(BEFORE "test")
add_subdirectory("test")

install(TARGETS GKlib
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib)
install(FILES ${GKlib_includes} DESTINATION include)
