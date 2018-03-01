cmake_minimum_required(VERSION 2.8.3)
project(aruco_catkin)

find_package(catkin_simple REQUIRED)
catkin_simple(ALL_DEPS_REQUIRED)

include(ExternalProject)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)

ExternalProject_Add(aruco_src
  URL https://sourceforge.net/projects/aruco/files/3.0.0/aruco304.zip
  CMAKE_ARGS
    -DCMAKE_INSTALL_PREFIX=${CATKIN_DEVEL_PREFIX}
    -DCMAKE_BUILD_TYPE:STRING=Release
  BUILD_COMMAND make
  INSTALL_COMMAND make install
)

cs_add_library(${PROJECT_NAME} SHARED src/dependency_tracker.cc)
add_dependencies(${PROJECT_NAME} aruco_src)
target_link_libraries(${PROJECT_NAME}
  ${CATKIN_DEVEL_PREFIX}/lib/libaruco${CMAKE_SHARED_LIBRARY_SUFFIX})

cs_install()
cs_export(
  INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include
  CFG_EXTRAS aruco-extras.cmake)
