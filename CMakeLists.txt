cmake_minimum_required(VERSION 3.0.0)
project(ParMETIS)

add_definitions(-w)

set(GKLIB_PATH ${CMAKE_CURRENT_SOURCE_DIR}/metis/GKlib)
set(METIS_PATH ${CMAKE_CURRENT_SOURCE_DIR}/metis)
message(STATUS "GKLIB_PATH=${GKLIB_PATH}")
message(STATUS "METIS_PATH=${METIS_PATH}")

# Search for MPI.
include(FindMPI)
if(NOT MPI_FOUND)
  message(FATAL_ERROR "mpi is not found")
endif()
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${MPI_C_COMPILE_FLAGS} -Wno-unused-label -Wno-misleading-indentation")

include(${GKLIB_PATH}/GKlibSystem.cmake)

# List of paths that the compiler will search for header files.
# i.e., the -I equivalent
include_directories(BEFORE include)
include_directories(BEFORE ${MPI_C_INCLUDE_PATH})
include_directories(BEFORE ${GKLIB_PATH})
include_directories(BEFORE ${METIS_PATH}/include)

# List of directories that cmake will look for CMakeLists.txt
add_subdirectory(metis/libmetis)
add_subdirectory(metis/include)
add_subdirectory(include)
add_subdirectory(libparmetis)
if(NOT DISABLE_PARMETIS_PROGRAMS)
  add_subdirectory(programs)
endif()

# This is for testing during development and is not being distributed
#add_subdirectory(test)
