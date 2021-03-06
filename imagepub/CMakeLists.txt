cmake_minimum_required(VERSION 2.8.3)
project(imagepub)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
find_package(catkin REQUIRED COMPONENTS
  cv_bridge
  dynamic_reconfigure
  image_transport
  roscpp
)

find_package(OpenCV)

#################################
## Add Dynamic Reconfigure API ##
#################################
generate_dynamic_reconfigure_options(
  cfg/imgFilter.cfg
)


###################################
## catkin specific configuration ##
###################################
catkin_package(
  INCLUDE_DIRS include
  LIBRARIES imagepub
  CATKIN_DEPENDS cv_bridge dynamic_reconfigure image_transport roscpp
)


###########
## Build ##
###########

# include_directories(include)
include_directories(
  ${catkin_INCLUDE_DIRS}
  ${OpenCV_INCLUDE_DIRS}
)

## add the C++ img_publisher 
add_executable(img_publisher src/img_publisher.cpp)
add_dependencies(img_publisher 
	${catkin_EXPORTED_TARGETS} 
	${${PROJECT_NAME}_EXPORTED_TARGETS}
)
target_link_libraries(img_publisher 
	${catkin_LIBRARIES} 
	${OpenCV_LIBRARIES}
)

## add the C++ img_filter 
add_executable(img_filter src/img_filter.cpp)
add_dependencies(img_filter
  ${catkin_EXPORTED_TARGETS} 
  ${${PROJECT_NAME}_EXPORTED_TARGETS}
  ${PROJECT_NAME}/cfg/imgFilter.cfg
)
target_link_libraries(img_filter 
  ${catkin_LIBRARIES} 
  ${OpenCV_LIBRARIES}
)