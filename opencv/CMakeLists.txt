set(extproject_name OpenCV)
set(extproject_FILE "opencv-2.4.11.zip")
set(extproject_URL "http://sourceforge.net/projects/opencvlibrary/files/opencv-unix/2.4.11/${extproject_FILE}/download")
set(extproject_GIT_REPOSITORY "https://github.com/Itseez/opencv.git")
set(extproject_GIT_TAG "2.4.11")

if(EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/${extproject_FILE}") 
	set(extproject_URL "${CMAKE_CURRENT_SOURCE_DIR}/${extproject_FILE}")
endif()

set(OpenCV_CMAKE_ARGS 
	-DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX}/${extproject_name}					
	-DBUILD_DOCS=OFF
	-DBUILD_TESTS=OFF
	-DBUILD_PERF_TESTS=OFF
	-DBUILD_PACKAGE=OFF
	-DBUILD_WITH_STATIC_CRT=OFF
	-DBUILD_opencv_apps=OFF
	-DBUILD_opencv_gpu=OFF
	-DBUILD_opencv_python=OFF
	-DBUILD_opencv_ts=OFF
	-DWITH_CUDA=OFF
	)
set(OpenCV_STATIC ON CACHE BOOL "Use static opencv libs")
if(OpenCV_STATIC)
	set(OpenCV_CMAKE_ARGS ${OpenCV_CMAKE_ARGS} -DBUILD_SHARED_LIBS=OFF)
else()
	set(OpenCV_CMAKE_ARGS ${OpenCV_CMAKE_ARGS} -DBUILD_SHARED_LIBS=ON)
endif()

externalproject_add( ${extproject_name} 
					URL ${extproject_URL}							
					CMAKE_ARGS ${OpenCV_CMAKE_ARGS}					
					)