cmake_minimum_required(VERSION 2.8.3)
project(orb_slam)




IF(NOT ROS_BUILD_TYPE)
  SET(ROS_BUILD_TYPE Release)
ENDIF()

MESSAGE("Build type: " ${ROS_BUILD_TYPE})

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS}  -Wall  -O3 -march=native -std=c++11")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall  -O3 -march=native -std=c++11")


LIST(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake_modules)

find_package(catkin REQUIRED COMPONENTS roscpp tf sensor_msgs cv_bridge image_transport)

find_package(Boost REQUIRED COMPONENTS thread)

find_package(OpenCV REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(Cholmod REQUIRED)

include_directories(
${PROJECT_SOURCE_DIR}
${EIGEN3_INCLUDE_DIR}
${CHOLMOD_INCLUDE_DIR}
${catkin_INCLUDE_DIRS}
include
)


catkin_package(
   INCLUDE_DIRS include
   LIBRARIES ${PROJECT_NAME}
   CATKIN_DEPENDS roscpp tf sensor_msgs cv_bridge image_transport
   DEPENDS eigen opencv)




add_executable(${PROJECT_NAME}
src/main.cc
src/Tracking.cc
src/LocalMapping.cc
src/LoopClosing.cc
src/ORBextractor.cc
src/ORBmatcher.cc
src/FramePublisher.cc
src/Converter.cc
src/MapPoint.cc
src/KeyFrame.cc
src/Map.cc
src/MapPublisher.cc
src/Optimizer.cc
src/PnPsolver.cc
src/Frame.cc
src/KeyFrameDatabase.cc
src/Sim3Solver.cc
src/Initializer.cc
)


target_link_libraries(${PROJECT_NAME}
${OpenCV_LIBS}
${EIGEN3_LIBS}
${catkin_LIBRARIES}
cholmod
${PROJECT_SOURCE_DIR}/Thirdparty/DBoW2/lib/libDBoW2.so
${PROJECT_SOURCE_DIR}/Thirdparty/g2o/lib/libg2o.so
)
