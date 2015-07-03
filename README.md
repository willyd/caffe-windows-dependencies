# caffe-windows-dependencies
This project sets up a super build of most of the [Caffe](https://github.com/BVLC/caffe) library dependencies on Windows. The dependencies built by this project are:
* gflags
* glog
* leveldb
* lmdb
* protobuf
* snappy
* opencv
* hdf5

It does not build the following dependencies:
* boost

Windows binaries for these dependencies are widely available. Please note that boost is required to build the windows version of leveldb.

The project is setup via git submodules pointing to either the original code repository on github or a fork of the project so that a CMake-based build is available.

When this project is built it will generate binaries and includes (via CMake install) and a cmake cache file that cane be used so configure the CMake based build of Caffe. Here is what an initial setup would look like:

    # clone windows-compatible version of caffe
    cmd> git clone https://github.com/willyd/caffe.git caffe
    cmd> cd caffe
    cmd> git checkout windows
    # clone dependencies
    cmd> git clone --recursive https://github.com/willyd/caffe-windows-dependencies.git dep
    # build dependencies
    cmd> md dependencies
    cmd> cd dependencies
    cmd> cmake ..\dep -DBOOST_ROOT=PATH/TO/BOOST -DBoost_USE_STATIC_LIBS=ON
    cmd> cmake --build . --config Debug
    cmd> cmake --build . --config Release
    cmd> cd ..
    cmd> md build
    cmd> cd build
    cmd> cmake  .. -C ..\dependencies\install\caffe-cache-init.cmake
    cmd> cmake --build . --config Debug
    cmd> cmake --build . --config Release
    
Tested with:
- Visual Studio 2013 64 bits, Visual Studio 2015 32 bits 
- CMake 3.2.3
