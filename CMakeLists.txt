cmake_minimum_required(VERSION 2.8.3)
project(sparsehash_catkin)

find_package(catkin_simple REQUIRED)
catkin_simple()

include(ExternalProject)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)

ExternalProject_Add(sparsehash_src
  DOWNLOAD_COMMAND wget --retry-connrefused --waitretry=1 --timeout=12 --tries 10 https://sparsehash.googlecode.com/files/sparsehash-2.0.2.tar.gz
  UPDATE_COMMAND ""
  PATCH_COMMAND patch -N src/sparsehash/internal/sparsehashtable.h ${CMAKE_CURRENT_SOURCE_DIR}/osx.patch
  CONFIGURE_COMMAND ../sparsehash_src/configure --prefix=${CATKIN_DEVEL_PREFIX}
)

cs_add_library(${PROJECT_NAME} src/dependency_tracker.cc)
add_dependencies(${PROJECT_NAME} sparsehash_src)

cs_install()

cs_export(INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include
  CFG_EXTRAS sparsehash-extras.cmake)
