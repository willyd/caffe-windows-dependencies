cmake_minimum_required(VERSION 3.0)

set(CMAKE_INSTALL_PREFIX "${CMAKE_CURRENT_SOURCE_DIR}/bin" CACHE PATH "Path where to install binaries")

project(caffe-windows-dependencies)


include (ExternalProject)

# gflags
set (GFLAGS_CMAKE_ARGS 
     -DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX}/gflags # set the install prefix per generator
     -DCMAKE_DEBUG_POSTFIX=d # add a debug postfix to the debug libs so we can install them with the release ones
     -DBUILD_SHARED_LIBS=OFF # build static libs       
     )

ExternalProject_Add ( gflags 
                      SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/gflags
                      CMAKE_ARGS ${GFLAGS_CMAKE_ARGS}
                    )