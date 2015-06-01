cmake_minimum_required(VERSION 3.0)

set(CMAKE_INSTALL_PREFIX "${CMAKE_CURRENT_BINARY_DIR}/install" CACHE PATH "Path where to install binaries")

project(caffe-windows-dependencies)

include (ExternalProject)

find_package(Boost 1.55 REQUIRED COMPONENTS system thread filesystem)

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

set(GFLAGS_DIR ${CMAKE_INSTALL_PREFIX}/gflags/CMake)

# glog
set (GLOG_CMAKE_ARGS 
     -DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX}/glog # set the install prefix per generator     
     -DBUILD_SHARED_LIBS=OFF # build static libs  
     -DCMAKE_DEBUG_POSTFIX=d # add a debug postfix to the debug libs so we can install them with the release ones
     -Dgflags_DIR=${CMAKE_INSTALL_PREFIX}/gflags/CMake
     )

ExternalProject_Add ( glog 
                      DEPENDS gflags
                      SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/glog
                      CMAKE_ARGS ${GLOG_CMAKE_ARGS}
                    )

set(GLOG_INCLUDE_DIR ${CMAKE_INSTALL_PREFIX}/glog/include)
set(GLOG_LIBRARY_RELEASE ${CMAKE_INSTALL_PREFIX}/glog/lib/glog.lib)
set(GLOG_LIBRARY_DEBUG ${CMAKE_INSTALL_PREFIX}/glog/lib/glogd.lib)


set (PROTOBUF_CMAKE_ARGS 
     -DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX}/protobuf # set the install prefix per generator     
     -DBUILD_SHARED_LIBS=OFF # build static libs  
     #-DCMAKE_DEBUG_POSTFIX=d # add a debug postfix to the debug libs so we can install them with the release ones
     )


ExternalProject_Add ( protobuf                       
                      SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/protobuf/vsprojects           
                      CMAKE_ARGS ${PROTOBUF_CMAKE_ARGS}
                    )

set(PROTOBUF_SRC_ROOT_FOLDER ${CMAKE_INSTALL_PREFIX}/protobuf)

set (LEVELDB_CMAKE_ARGS 
     -DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX}/leveldb # set the install prefix per generator     
     -DBUILD_SHARED_LIBS=OFF # build static libs  
     -DCMAKE_DEBUG_POSTFIX=d # add a debug postfix to the debug libs so we can install them with the release ones
     -DBOOST_ROOT=${Boost_ROOT}
     -DBOOST_INCLUDE_DIR=${Boost_INCLUDE_DIRS}
     -DBOOST_LIBRARYDIR=${Boost_LIBRARY_DIRS}
	 -DBoost_INCLUDE_DIR=${Boost_INCLUDE_DIR}
     )


ExternalProject_Add ( leveldb                       
                      SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/leveldb           
                      CMAKE_ARGS ${LEVELDB_CMAKE_ARGS}
                    )

set(LEVELB_DIR ${CMAKE_INSTALL_PREFIX}/leveldb)

set (LMDB_CMAKE_ARGS 
     -DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX}/lmdb # set the install prefix per generator     
     -DBUILD_SHARED_LIBS=OFF # build static libs  
     -DCMAKE_DEBUG_POSTFIX=d # add a debug postfix to the debug libs so we can install them with the release ones
     )

ExternalProject_Add ( lmdb                       
                      SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/lmdb/libraries/liblmdb        
                      CMAKE_ARGS ${LMDB_CMAKE_ARGS}
                    )

set(LMDB_DIR ${CMAKE_INSTALL_PREFIX}/lmdb)

set (SNAPPY_CMAKE_ARGS 
     -DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX}/snappy # set the install prefix per generator     
     -DBUILD_SHARED_LIBS=OFF # build static libs  
     -DCMAKE_DEBUG_POSTFIX=d # add a debug postfix to the debug libs so we can install them with the release ones
     )


ExternalProject_Add ( snappy                       
                      SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/snappy        
                      CMAKE_ARGS ${SNAPPY_CMAKE_ARGS}
                    )

set(SNAPPY_ROOT_DIR ${CMAKE_INSTALL_PREFIX}/snappy)


# todo opencv
find_package(OpenCV QUIET COMPONENTS core highgui imgproc imgcodecs)
if(NOT OpenCV_FOUND) # if not OpenCV 3.x, then imgcodecs are not found
  find_package(OpenCV REQUIRED COMPONENTS core highgui imgproc)
endif()

find_package(HDF5 REQUIRED COMPONENTS HL)

set(OpenBLAS_INCLUDE_DIR "" CACHE PATH "OpenBLAS include dir")
set(OpenBLAS_LIB "" CACHE FILEPATH "OpenBLAS lib")

configure_file(${CMAKE_CURRENT_SOURCE_DIR}/caffe-cache-init.cmakein ${CMAKE_INSTALL_PREFIX}/caffe-cache-init.cmake)



