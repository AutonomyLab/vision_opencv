cmake_minimum_required(VERSION 2.8)
project(cv_bridge)

find_package(catkin REQUIRED COMPONENTS rosconsole sensor_msgs)

set(CUSTOM_OPENCV_DIR "/local_home/opencv/opencv-3.0.0/install")
find_package(OpenCV 3 REQUIRED PATHS ${CUSTOM_OPENCV_DIR} NO_DEFAULT_PATH)
message(STATUS "OpenCV Includes: ${OpenCV_INCLUDE_DIRS}")
message(STATUS "OpenCV Libraries: ${OpenCV_LIBRARIES}")
message(STATUS "OpenCV Libs: ${OpenCV_LIBS}")
message(STATUS "OpenCV Lib dir: ${OpenCV_LIB_DIR}")

if(NOT ANDROID)
find_package(Boost REQUIRED python)
else()
find_package(Boost REQUIRED)
endif()

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES ${PROJECT_NAME}
  CATKIN_DEPENDS rosconsole sensor_msgs
  DEPENDS OpenCV3
)

catkin_python_setup()

include_directories(SYSTEM ${Boost_INCLUDE_DIRS})
include_directories(include ${catkin_INCLUDE_DIRS} ${OpenCV_INCLUDE_DIRS})

if(NOT ANDROID)
add_subdirectory(python)
endif()
add_subdirectory(src)
if(CATKIN_ENABLE_TESTING)
  add_subdirectory(test)
endif()

# install the include folder
install(
  DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
)
