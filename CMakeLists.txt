# CMakeLists.txt for event package. It creates a library with dictionary and a
# main program
cmake_minimum_required(VERSION 3.0 FATAL_ERROR)
project(converter)

# You need to tell CMake where to find the ROOT installation. This can be done
# in a number of ways:
#   - ROOT built with classic configure/make use the provided
#   $ROOTSYS/etc/cmake/FindROOT.cmake
#   - ROOT built with CMake. Add in CMAKE_PREFIX_PATH the installation prefix
#   for ROOT
if (ROOT_CMAKE)
  list(APPEND CMAKE_PREFIX_PATH $ENV{ROOTSYS})
else (ROOT_CMAKE)
  set(ROOT_PREINSTALLED "/usr/lib64/Geant4-10.3.0/Modules")
  list(APPEND CMAKE_MODULE_PATH ${ROOT_PREINSTALLED})
endif (ROOT_CMAKE)

#---Locate the ROOT package and defines a number of variables (e.g. ROOT_INCLUDE_DIRS)
if (ROOT_CMAKE)
  find_package(ROOT REQUIRED COMPONENTS MathCore RIO Hist Tree Net
	  HINTS ENV{PATH})
else (ROOT_CMAKE)
  find_package(ROOT REQUIRED COMPONENTS MathCore RIO Hist Tree Net)
endif (ROOT_CMAKE)

#---Define useful ROOT functions and macros (e.g. ROOT_GENERATE_DICTIONARY)
if (ROOT_CMAKE)
  include(${ROOT_USE_FILE})
endif (ROOT_CMAKE)

include_directories(${CMAKE_SOURCE_DIR} ${ROOT_INCLUDE_DIRS})
add_definitions(${ROOT_CXX_FLAGS})

set(CMAKE_BUILD_TYPE Debug)
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O2")

#---Create  a main program
add_executable(convert create_tree.cpp)
target_link_libraries(convert ${ROOT_LIBRARIES})
