cmake_minimum_required(VERSION 2.8.3)
project(sparsehash_catkin)

find_package(catkin_simple REQUIRED)
catkin_simple()

include(ExternalProject)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)

ExternalProject_Add(sparsehash_src
  URL https://sparsehash.googlecode.com/files/sparsehash-2.0.2.tar.gz
  UPDATE_COMMAND ""
  CONFIGURE_COMMAND ../sparsehash_src/configure --prefix=${CATKIN_DEVEL_PREFIX}
)

cs_add_library(${PROJECT_NAME} src/dependency_tracker.cc)
add_dependencies(${PROJECT_NAME} sparsehash_src)

cs_install()

cs_export(INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include
  CFG_EXTRAS sparsehash-extras.cmake)
